# Universal module loader

* AMD, like RequireJS
* CommonJS, like Node
* Browser globals

## Example:

```javascript
(function(deps, factory, root, global) {
  if (typeof define === 'function' && define.amd) define(deps,factory);
  else if (typeof module ==='object' && module.exports) module.exports=factory.apply(root,deps.map(function(_){return require(_.split(':')[0])}));
  else root[global]=factory.apply(root,deps.map(function(_){_=_.split(':');return root[_[_.length-1]]}));
} (
['otherModule', 'underscore:_', 'jQuery:$'], // Dependencies. Optional browser global property name
function(other, under, $) {

  function constructor() {
    var response = 'response from' + other();
    if (!under.isUndefined(window.thing)) {
      $('body').append($('<div> thing! </div>'));
    }
    return response;
  }

  return constructor;
 
}, this, 'myModuleName')); // Exported module name

```
