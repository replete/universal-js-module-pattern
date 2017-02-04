# Universal module loader

* AMD, like RequireJS
* CommonJS, like Node
* Browser globals

## Example:

```javascript
(function(deps, factory, root, name) {
  if (typeof define === 'function' && define.amd) define(deps,factory);
  else if (typeof module ==='object' && module.exports) module.exports=factory.apply(root,deps.map(function(_){return require(_.split(':')[0])}));
  else root[name]=factory.apply(root, deps.map(function(_){_=_.split(':');return eval(_[_.length-1?0:1])}));
} (
// Dependencies. If colon present, next substring used to find global object
['otherModule', 'underscore:_', 'jQuery:$'],
function(otherModule, _, jQuery) {

  // Write your module code here:

  function constructor() {
	var response = 'response from' + otherModule();

	if (underscore.isUndefined(window.thing)) {
		$('body').append($('<div> thing! </div>'));
	} else {
		return false;
	}
    return response;
  }

  return constructor; // must return a function
 
}, this, 'myModuleName')); // Exported module name

```
