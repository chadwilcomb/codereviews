**Summary**
- This is constructive criticism of code that was mostly written by myself, so hopefully you can learn from my mistakes :grimacing:


**General JavaScript Comments**
- Move templates into the View as `<script type="text/html"></script>`
- Add the AJAX error logging module (\logging\logging.js) to this bundle and throw Errors from inside all AJAX error functions `error: function (xhr) { throw new Error(xhr.statusText); }`
- Use [single quotes `''` for strings](https://github.com/nycdot/javascript-style-guide#strings)
- Use [_this instead of self to copy `this`](https://github.com/nycdot/javascript-style-guide#naming-conventions)
- Use [class names prefixed with js-](https://github.com/nycdot/javascript-style-guide#jquery) for selectors used only for JavaScript purposes.
- Use [shortcut](https://github.com/nycdot/javascript-style-guide#conditional-expressions--equality) `if (contracts.length)` rather than `if (contracts.length > 0)` [68, 379, 569, 969, 1068, 1094, 1180, 1250, 1501, 2073, 2360, 2472]
- Use _.each or for loop instead of $.each [for better performance](http://jsperf.com/jquery-each-vs-for-loop/35) [74, 953, 971, 1003, 1075, 1096, 1632, 1816, 1823, 1836]

**Models**

App.Models.CanopyInfo
- 1351-1354: declare all variables in first line of function scope. [Further reading](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/) on variable hoisting and function scope.

App.Models.Application
- 2459: `validate` function has `attrs` as input parameters. Therefore, we can use `attrs.PermitteeDetails` instead of `this.get("PermitteeDetails")` (like we did on line 2463 for attrs.ValidStops).

App.Models.Permittee :thumbsup:

App.Models.PropertyOwnerInfo :thumbsup:

App.Models.Location :thumbsup:

App.Models.RelatedAgencyPermit :thumbsup:

App.Models.Insurance :thumbsup:

App.Models.Document :thumbsup:

**Collections**

App.Collections.Permits :thumbsup:

App.Collections.RelatedAgencyPermits :thumbsup:

App.Collections.Insurances :thumbsup:

App.Collections.UploadedDocuments :thumbsup:

**Views**

App
- 102, 118: Remove commented code.
- 594: Cache `this.$('#submitModal')` or use a single selector, like `this.$('#submitModal .modal-header')` rather than using `.find()`
- 738: Remove this Modernizr placeholder workaround, it's for legacy browsers only

App.Views.Permittee
- 1069, use a better var name, ie `var = $ul`
- 1135: There is no `clearHomeOwnerInfo();` function, this throws an error :open_mouth:. Just remove it.
- 1117: use `this` prefix for selectors wherever possible.

App.Views.PropertyOwnerInfo
- Add mask to phone number input field. Check with Manhole Application to see implementation. This also needs to be added to Vault Application

App.Views.LocationWidget
- 1682: Cache the `$('#FromStreet')` and `$('#ToStreet')` selectors before using it in a loop.
- 1691: I doubt that this is a necessary way to parse the JSON, just use `var myData = JSON.parse(data.fromstreets);`.
- 1702: This is like a really old-school way to append `<option>`s to a `<select>`. We should use this syntax instead: `$select.append($('<option>', { value : key }).text(value));`
- 1726: Remove this check for placeholder, not necessary for modern browsers.
- 1811: Cache the `$('#ToStreet')` selector before using it in a loop.

App.Views.RelatedAgencyPermit :thumbsup:

App.Views.RelatedAgencyPermits :thumbsup:

App.Views.Insurance :thumbsup:

App.Views.Insurances
- If you add some insurances and then remove them all, you lose the top row. 

App.Views.Document :thumbsup:

App.Views.UploadedDocuments
- 2428: there is no `removeRow` method.

App.Views.CanopyInfo :thumbsup:

**HTML**


**SASS**
:thumbsup:
