###Active Permits

**Summary**
- The View is missing the basic HTML elements required (DOCTYPE and <html>). I'm surprised this page even loads! Is there a (good) reason not to use the Layout view for this page?
- We need a container class to put all of our Bootstrap rows and columns inside.
- Put the Location Description in a large font at the top of the page.
- Don't write custom collapse functionality, instead use the Bootstrap collapse functions with the standard classes and data- attributes. Here is a [Gist](https://gist.github.com/chadwilcomb/4251efa111a081cd2119) that has the basic outline of all you need (no JavaScript code required except including bootstrap.js).
- Remove close button. Users should use the standard browser functionality to close a tab/window.


**JavaScript**
- Scope the App View to `{ el: $('#content') }`.
- Use [single quotes for strings](https://github.com/nycdot/javascript-style-guide#strings).
- Remove `closeWindow` event/function.
- Remove `toggleBody` event/function and instead use the standard Bootstrap classes, attributes to handle this.
- Add/remove the `.in` class to the `.collapse` elements to Collapse All/Expand All, like this:
```javascript
    collapseAll: function () {
      this.$('.collapse').addClass('in');
    },
    expandAll: function () {
      this.$('.collapse').removeClass('in');
    }
``` 

**HTML**
- Missing `<!DOCTYPE html>`
- Missing `<html></html>` 
- Missing `<div class="container-fluid">`. From [Boostrap Docs](http://getbootstrap.com/css/#grid-intro): "Rows must be placed within a .container (fixed-width) or .container-fluid (full-width) for proper alignment and padding."
- Add Location Description in large font at top of page to make it obvious these permits all fall on the same location.
- See this [Gist](https://gist.github.com/chadwilcomb/4251efa111a081cd2119) for the basic structure for the collapse sections. Replace the ids with something using PermitID for uniqueness, ie- `id="permit@'(permit.PermitID)'"` in the Razor rendering.


**SASS**
- Make title row (collapse all, expand all) button positioning consistent by using this CSS:
```css
  .title-row button {
    margin-top: 7px;
  }
```
