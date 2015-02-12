##General##
- Remove commented code
- Cache jquery selectors when using more than once, and use following style for variable name: `var $selector = $(‘.selector’);`
- Use _.each or for loop instead of $.each (performance is better)