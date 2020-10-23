# vue-fontawesome-autogen
> This script is created to make managing imported fontawesome icons in VueJS uncumbersome. It implements simple parsing techniques that would generate a file that imports all of your used icons without having to manage them every single time.

## Installation
``` s
$ npm install --save-dev vue-fontawesome-autogen
```

## Requirements
- Make sure your source code exists within the "src" folder of the project's main directory.
- Make sure you have installed vue-fontawesome dependencies, if not, please check https://github.com/FortAwesome/vue-fontawesome#installation

## Setting up

Add these definitions to your entry point such your "main.js" file
``` js
// Import autogenerated fontawesome imports
import "@/plugins/fontawesome-autogen.js";

// Import shim component for fontawesome
import Fa from "vue-fontawesome-autogen/Fa.vue";
Vue.component("fa", Fa);
```

## Usage
The usage is almost identical to how you would noramlly use with vue-fontawesome but instead, it alleviates the headache of having to deal with the arrays drama when wanting to use a different icon type, such as:

```html
<font-awesome-icon :icon="['far', 'video']" />
```

instead, you would just have write the following, which is much more practical:

```html
<fa icon="far-video" />
```

## Examples
Basic usage:
``` html
<fa icon="far-video" />
```

There's also support for duotone's primary and secondary color attributes, in example:
``` html
<fa icon="fad-video" primaryColor="red" secondaryColor="white" /> 
```

You can also use the advanced attributes, as you would normally with vue-fontawesome, in example:
``` html
<fa icon="fas-check" transform="shrink-6" :style="{ color: 'white' }" />
```

## Generating icon imports
There's basically two ways to do this, either manually or automatically.

### Manually
Executing the following npm command would run the script:
``` sh
$ npm explore vue-fontawesome-autogen -- npm run gen
```
And you should see the success output message such as:
```
- Fontawesome treeshaking list generated. (took 10 ms)
```


### Automatically upon build-time
This is achieved by hooking into webhook's events, so an additonal library is required, in our case, we'll be using [before-build-webpack](https://github.com/artemdudkin/before-build-webpack)
``` sh
$ npm install --save-dev before-build-webpack
```

Configuring webpack, via vue.config.js or alternative, you can check the example provided.
``` js
var WebpackBeforeBuildPlugin = require('before-build-webpack');
// ...
  module: {
    plugins: [
        new WebpackBeforeBuildPlugin(function(stats, callback) {
            const {execSync} = require('child_process');
            console.log(execSync('npm explore vue-fontawesome-autogen -- npm run gen').toString());
            callback();
        }, ['run'])
    ]
  },
// ...
```

then build the project as you normally would
``` sh
$ npm run build
```

The output of build should include the following line
```
- Fontawesome treeshaking list generated. (took 10 ms)
```

### If you're confused, you can check the [example project](https://github.com/GTANAdam/vue-fontawesome-autogen/tree/main/example) above.