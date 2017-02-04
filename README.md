# Universal module loader

* AMD, like RequireJS
* CommonJS, like Node
* Browser globals

## Example:

```javascript
(function(root, deps, factory, name) {
  if (typeof define === 'function' && define.amd) define(deps||[],factory);
  else if (typeof module ==='object' && module.exports) module.exports=factory.apply(root,deps.map(function(_){return require(_.split(':')[0])}));
  else root[name]=factory.apply(root, deps.map(function(_){_=_.split(':');return eval(_[_.length-1?0:1])}));
} (this, 
['otherModule', 'underscore:_', 'jQuery:$'], // <- Dependencies. If colon present, next substring used for browser global
function(otherModule, _, jQuery) {

  function constructor() {

	var response = 'response from' + otherModule();

	if (underscore.isUndefined(window.thing)) {
		$('body').append($('<div> thing! </div>'));
	} else {
		return false;
	}
    return response;
  }

  return constructor;
}, 'myModuleName')); // <-- Exported module name

```
