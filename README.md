# Universal module pattern

* AMD, like RequireJS
* CommonJS, like Node
* Browser globals

## Example:

```javascript
(function(global, deps, factory, root) {
  if(typeof define==='function'&&define.amd)define(deps,factory);
  else if(typeof module==='object'&&module.exports)module.exports=factory.apply(root,deps.map(function(_){return require(_.split(':')[0])}));
  else root[global]=factory.apply(root,deps.map(function(_){_=_.split(':');return root[_[_.length-1]]}));
} (
'myModuleName', ['otherModule', 'underscore:_', 'jQuery:$'], // jQuery:$ = optional global name. "jQuery is the $ global"
function(other, under, $) {

  function constructor() {
    var response = 'response from' + other();
    if (!under.isUndefined(window.thing)) {
      $('body').append($('<div> thing! </div>'));
    }
    return response;
  }

  return constructor;

},this));
```



## Example where loader is globalised:

```javascript
// In each environment in your project (e.g., Node + browser)
this.unimodule = function(global, deps, factory, root) {
  if(typeof define==='function'&&define.amd)define(deps,factory);
  else if(typeof module==='object'&&module.exports)module.exports=factory.apply(root,deps.map(function(_){return require(_.split(':')[0])}));
  else root[global]=factory.apply(root,deps.map(function(_){_=_.split(':');return root[_[_.length-1]]}));
};

// some-module.js
(unimodule('myModule',
['otherModule', 'underscore:_', 'jQuery:$'],
function(other, under, $) {

  function constructor() {
    var response = 'response from' + other();
    if (!under.isUndefined(window.thing)) {
      $('body').append($('<div> thing! </div>'));
    }
    return response;
  }

  return constructor;

},this));
```
