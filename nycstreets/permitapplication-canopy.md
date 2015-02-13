**Summary**
- Again, this is constructive criticism of code that was mostly written by myself, so hopefully you can learn from my mistakes :grimacing:
- Move templates into the View as `<script type="text/html"></script>`
- Use [single quotes `''` for strings](https://github.com/nycdot/javascript-style-guide#strings)
- Use [_this instead of self to copy `this`](https://github.com/nycdot/javascript-style-guide#naming-conventions)
- Use [class names prefixed with js-](https://github.com/nycdot/javascript-style-guide#jquery) for selectors used only for JavaScript purposes.

**Models**

App.Models.Permittee :thumbsup:

App.Models.PropertyOwnerInfo :thumbsup:

App.Models.CanopyInfo
- 1351-1354: declare all variables in first line of function scope. [Further reading](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/) on variable hoisting and function scope.

App.Models.Location :thumbsup:

App.Models.RelatedAgencyPermit :thumbsup:

App.Models.Insurance

App.Models.Document :thumbsup:

App.Models.Application
- 2459: `validate` function has `attrs` as input parameters. Therefore, we can use `attrs.PermitteeDetails` instead of `this.get("PermitteeDetails")` (like we did on line 2463 for attrs.ValidStops).

**Collections**

App.Collections.Permits :thumbsup:

App.Collections.RelatedAgencyPermits :thumbsup:

App.Collections.Insurances :thumbsup:

App.Collections.UploadedDocuments :thumbsup:

**Views**

App

App.Views.Permittee

App.Views.CanopyInfo :thumbsup:

App.Views.LocationWidget

App.Views.RelatedAgencyPermit

App.Views.RelatedAgencyPermits

App.Views.Insurance

App.Views.Insurances

App.Views.Document

App.Views.UploadedDocuments
- 2428: there is no `removeRow` method.

**HTML**




**SASS**
