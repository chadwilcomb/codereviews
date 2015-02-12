#####Summary


#####General
- Remove commented code
- Cache jquery selectors when using more than once, and use following style for variable name: 
```
    var $selector = $(‘.selector’);
```
- Use _.each or for loop instead of $.each (performance is better)


#####Models
App.Models.Permittee
- 746: var deferred not used
- 765: combine with previous if statement instead of nesting

App.Models.PermitType
- 1165: no quotes around object properties - `‘checked’`
- 1230: probably better to move the `propogateDateRange()` logic into a new method in `App.Collections.Permits` since it is actually performing updates on those models in the collection (not on a PermitType model).
- 1238: move all variable definitions to first line of validate function.

App.Models.Permit

App.Models.Location
- 2181: remove url attribute

App.Models.Application


#####Collections

App.Collections.PermitTypes
- 1330, 1331: combine these two listeners since they call the same function. Also can remove the permitteeCleared function and just have the listener call reset directly, ie `this.listenTo(App.Permittee, 'change:id change:PermitteeTypeID', this.reset);`

App.Collections.SelectedPermitTypes
- 1364, 1365, 1369, 1372: just call reset directly from combined listeners, remove `permitteeTypeChanged` and `permitteeCleared` functions `this.listenTo(App.Permittee, 'change:id change:PermitteeTypeID', this.reset);`
- 1375: the PermitType model has an updateDateRange function already, so we should just loop through the collection and call that function on each PermitType model, rather than have another implementation here.
- 1409-1413: not using `ptArr` for anything, remove this unnecessary loop.
- 1437: `ptArr`, PermitTypeIDs not being used anywhere, remove this unnecessary loop.

App.Collections.Permits
- 1775, 1776: combine these two listeners since they call the same function. Also can remove the permitteeCleared function and just have the listener call reset directly, ie `this.listenTo(App.Permittee, 'change:id change:PermitteeTypeID', this.reset);`
- 1781: `getLocationIds` function not called anywhere, delete it.

App.Collections.LocationList
- 2311, 2335: move these switches to some utility functions. It is repeated at least one place (line 199).
- 2331, 2332: presentation code should not be in a Collection, move it to a View.

#####Views

App

App.Views.Permittee

App.Views.PermitTypeCheckbox

App.Views.PermitTypeCheckboxes

App.Views.PermitTypeContainer

App.Views.PermitType

App.Views.Permits

App.Views.Permit

App.Views.Location
- 2235: Remove all the escapes before quotes `\”` from template.
- 2244: remove commented code. move `var _this = this;` to first line of function
- 2258: change to `this.model.set(‘marked’, !this.model.get(‘marked’));`

App.Views.LocationWidget
- 2556: cache the jQuery selectors (ie `var $toStreet = $(‘#toStreet’);`) for better performance, especially when used in a loop.
- 2668: Use $name variable name format for storing jQuery selectors and use this before selector to scope to view, ie `var $onStreet = this.$(‘#onStreet’);`

#####HTML


#####SASS
