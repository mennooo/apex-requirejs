# apex-requirejs
Sample project to demonstrate the usage of RequireJS in APEX 5.1+

## How it works in APEX

APEX 5.1 uses Oracle JET as their chart engine. Oracle JET uses RequireJS as their module dependency loader.

This is how it works.

The first step is to create separate modules.

```javascript
// module1.js
define([], function() {

  // return public objects of this module
  return {
    text: "Hell world!"
  }
};
```

The define function is part of RequireJS. Now use this module inside another module.

```javascript
// module2.js

// Load module1 as a dependency to this module
define(['module1'], function(module1) {

  function helloWorld() {
    console.log(module1.text);
  };

  // return public objects of this module
  return {
    helloWorld: helloWorld
  }
};
```

To glue thing together we create our main file which uses the modules.

```javascript
// main.js

// Require modules and execute some code
require(['module2'], function(module2) {

  module2.helloWorld();
  
};
```

To use main.js in our page, we have to load a script.

1. require.js
This is the external RequireJS library

Finally we have to configure RequireJS and specify where our main.js file can be found.

```javascript
require.config({
  baseUrl: "/another/path"
});
```

The baseUrl must refer to the path where main.js, module1.js and module2.js can be found.

### Loading files



### PLSQL packages for RequireJS

APEX_JAVASCRIPT

## JavaScript functions for RequireJS

apex.server.loadScript

Purpose: loading an external script. 
http://docs.oracle.com/database/apex-5.1/AEAPI/apex-server-namespace.htm#AEAPI-GUID-BE3207FA-E766-4F0D-9665-7CF6631E7430

This can load a module and attach it to the global scope.

If the optimizer is used, then no attachment to global.

apex.widget.jet (undocumented)
Purpose: Initializing a JET chart

Not needed in general.

### 

## Creating an Oracle JET plugin

Take a look at the Cookbook to browse the components.

### Loading Oracle JET
- use file prefix: [require jet]/myjs/main.js

### Loading CSS
- Add CSS See: https://apex.oracle.com/i/themes/theme_42/1.1/css/core/BarChart.css

## Creating your own modules

### First module

Wrapping everything into define().

You can set your module dependencies there.

### Setting require.config

Do not change the BasePath: this will break JET charts.

Instead, get the current path of your main script and use that to load the depending modules.

```javascript
function getCurrentScriptUrl() {
  var scripts = document.getElementsByTagName("script");
  // Current script is always the last one
  return scripts[scripts.length-1].src;
}
```

### Changing $(document).ready

An important feature of RequireJS is that modules are loaded asynchronous. This means the modules might not be loaded at $(document).ready. We can delay this event with https://api.jquery.com/jquery.holdready/

