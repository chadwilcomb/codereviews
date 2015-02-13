**Summary**
- Most of the issues with this code are things that I wrote :confused:. The major improvement that can be mad is caching the Permit Views instead of creating them every time (see :boom: below) and then not removing them. This could potentially be using up a lot of resources in the browser. 

**General**
- Remove commented code.
- Add comments to describe what each function does and when/how it is called.
- [Cache jquery selectors](https://github.com/nycdot/javascript#jquery) when using more than once, and use following style for variable name: `var $selector = $(‘.selector’);`
- Use _.each or for loop instead of $.each [for better performance](http://jsperf.com/jquery-each-vs-for-loop/35)
- Use .js- prefixes for class names used exclusively in the JavaScript. Most event selectors in the views would then start with js-

**Models**
App.Models.Permittee
- 746: var deferred not used
- 765: combine with previous if statement instead of nesting

App.Models.PermitType
- 1165: no quotes around object properties - `‘checked’`
- 1230: probably better to move the `propogateDateRange()` logic into a new method in `App.Collections.Permits` since it is actually performing updates on those models in the collection (not on a PermitType model).
- 1238: move all variable definitions to first line of validate function.

App.Models.Permit
- 1804: do we need these defaults?

App.Models.Location
- 2181: remove url attribute

App.Models.Application
- :thumbsup:

**Collections**

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
- 2334: Use `App.getBoroName()` instead

**Views**

App
- 733: scope the App view to `$('#content')` instead of `$('body')`
- 768: add `$('.modal').appendTo('body')` to move all the modals at once, remove the individual `appendTo('body')` from modal() calls.
- 48: cache the jQuery selectors, especially if using in a loop, ie- `$('#submit-modal-body')`
- 157-159: just set the values of these variables in the same line as declaring them.
- 48, 89, 111: All of this static text should be in a constant at the top of the App object or in a seperate object so it can be edited easier and/or reused where possible.
- 177: It seems like `enabled` should be false by default and we should be checking `App.AllPermits.length` rather than `App.LocationList` and `App.SelectedPermitTypes`
- 151, 729: duplicate function, `close()` never called.

App.Views.Permittee
- 776: Best practice is to set the `el` property only when we initialize the view. It's possible the element is not available when the JS file is first parsed. We should wait until `document.ready` (or `App.start()`) to assign the el to a view.
- 801: Move the autocomplete initialization into it's own function.
- 883: Remove this check for placeholder, we started doing that when we were trying to support legacy browsers, not necessary for modern browsers.
- 919, 937, 970: replace $.each with _.each or for loop
- 973, 978: I don't like how we have `[value="2"]` hard coded in this selector. What happens if this ProjectTypeID changes in future? 
- 947, 1095: scope all selectors with `this`
- 1006: App.PermitTypes should be listening to changes in App.Permittee to call it's own fetch function instead of calling it here.
- 1013: We should not be creating a new instance of this view every time, instead we should have on instance and trigger a reset method and/or swap out the collection and re-render. Also, then this single view could be listening for the reset/sync event on App.PermitTypes and reset itself accordingly.
- 1053: Caching $('#showStops') is good, but we should instead be caching $('#showStops tbody') instead of using find() every time.
- 1071: no reason to concat strings or declare this as var, just insert in in html() in line 1084.
- 1154: We should store this projectTypeId (2) as a constant (ie App.PROJECTYPE_GOVT = 2) so that if it ever were to change we could easily update this code (see comment above re line 973, 978).

App.Views.PermitTypeCheckbox
- :thumbsup:

App.Views.PermitTypeCheckboxes
- 1345: just set the value when you declare the var (line 1343).

App.Views.PermitTypeContainer
- :thumbsup:

App.Views.PermitType
- 1473: no need to declare `_this`, remove this line.
- 1483: see general comment above on event selector naming convention (`.js-` class names)
- 1513: no need to declare `_this`, remove this line and change line 1515 to use `this`.
- 1645, 1474: just have the listener call `remove()` and get rid of this `destroy` function altogether.
- 1653: this if statement and `pt` variable seem unnecessary, just use `permitTypeId`
- 1652, 1656, 1657: `alertText` variable is unnecessary, just insert the string into `jConfirm()`
- 1677-1681: use the variables we declared in lines 1665-1669.
- 1721: `'PermitTypeID'` repeated in `_.omit()`

App.Views.Permits
- :boom: 1765: When we create all of these child views, we should cache them by adding references to an array so that we don't just keep adding multiple views and using memory. Use [this template](https://gist.github.com/chadwilcomb/18037a4ce0be22ec67fa). We should refactor all of our PermitApplication pages to use this pattern. And when we remove the parent view, we should loop through the cache and remove all of the child views as well.

App.Views.Permit
- 1932: Backbone models don't have a 'remove' event (only destroy)
- 2066: I don't think this will work, we probably need to do this instead: `this.$('input.permit[name="HoursOfOperation"][value="' + this.model.get('HoursOfOperation') + '"]').prop('checked', true)`
- 2085: cache all these selectors so that we can use them inside the `.on('keyup', function (e)`

App.Views.Location
- 2235: Remove all the escapes before quotes `\”` from template.
- 2244: remove commented code. move `var _this = this;` to first line of function
- 2258: change to `this.model.set(‘marked’, !this.model.get(‘marked’));`

App.Views.LocationWidget
- 2556: [Cache jQuery selectors](https://github.com/nycdot/javascript#jquery), ie- `var $toStreet = $(‘#toStreet’);`) for better performance, especially when used in a loop.
- 2625: `var notReqLocs` is never used, remove it.
- : Remove this, I don't know what it does.nction.
- 2668: Use $name variable name format for storing jQuery selectors and use this before selector to scope to view, ie `var $onStreet = this.$(‘#onStreet’);`
- 2805: When using setTimeout, we should always check for it and [clear the timeout before setting another one](http://www.w3schools.com/jsref/met_win_cleartimeout.asp). We may even be able to remove this setTimeout altogether.
- 2840: use this to scope selector
- 2688: This switch repeats alot of the same code (jQuery ajax call). It would be better to just use the switch to set the url and data properties and then call the ajax function after the switch, [like this](https://gist.github.com/chadwilcomb/c9ddf626883b3d0bd312)

**HTML**
- :100:

**SASS**
- :100:
