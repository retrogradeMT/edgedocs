---
title: NPM Package Creation
description: "A Guide to create NPM Packages"
position: 1
category: "mbrvault"
author: Dustin Stewart


---

## NPM Package Creation Steps. 

1)  Create a blank repo on GitHub with the basic settings. 
2)  Clone it into local env
3) If this is the first time you have created a package you will need to install the `vue-sfc-rollup` package globally by running the command: `npm install -g vue-sfc-rollup`
4) Navigate to the empty repo folder on your local env and run the command `sfc-init` and then follow the setup instructions in the commandline.

** NOTE ** We should probably create a base package folder that can be forked so that we don't need to set up the package every time.   Until then...
 
5)  Add in all of the files for code consistency
       - .vscode/settings
6) run `npm Install`

## Package Development

At this point you should have a repo setup for package development.   Navigate to the `/src/lib-components` folder and you should see a sample SFC package setup.  This can be deleted and replaced with the SFC that you want to use, e.g EdgeForm or EdgeTable.  

This is a now a vue package and can be developed like any other vue project.  You can navigate to  `src/lib-components` and run `npm run serve` to start a vue server. 

Be sure to add any of your SFCs that you put into the `src/lib-components` to the `index.js` file just as you would do in a normal vue project.
    
    export { default as EdgeForm } from './EdgeForm.vue'

Optionally, you can use the `dev` folder to create a working demo/dev version of the component package within the package. I have not really used this option yet, but it is available.   My preferred option, is to demo the component on this site. I am doing it this way because I want a single location for all of the packages.  This is also a Nuxt project, so I can ensure it is working properly in a format that mirrors the real development env.    The development process would be much better using the dev folder within the package itself if you need to make adjustments or test it out. 

## Building and Publishing

Now that we have built a package we can build and deploy it.   

1) Navigate to your package.json file add or adjust the version of the package.  `0.0.1`
2) Run `npm run build` which will create several files inside of the `dist` folder.  The default setup is to create a `main` a `browser` and a `module`.   You can see these options near the top of your `package.json` file.    There may be some situations where the file outputs need to be changed but I won't get into the weeds here. 
3) if you are adding an npm package for the first time, you will want to create an npm user with the command `npm adduser`
4) Run `npm publish` to publish the built packages to npm.   The package will be available almost instantly and can be installed within your working projects. 

## Using it in Nuxt

In addition to installing the package into your Nuxt project you will want to create a plugin. If you have a global.js plugin it can be added there, or it can be added to its own plugin.  If you are going create a new plugin, be sure to register it in the `nuxt.config.js` file

### global.js use

Import it
```
import Vue from "vue";
import EdgeForm from "edge-form";
```

Then use it
```
Vue.use(_);
Vue.use(EdgeForm);
```



