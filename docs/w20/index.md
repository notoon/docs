---
title: "Introduction"
type: "home"
zones:
    - "W20"
sections:
    - "W20Introduction"
menu:
    W20Introduction:
        weight: 10
---

W20 is a web solution designed to allow developers to quickly and simply create enterprise-grade **Single Page
Application** (SPA). It is **server agnostic** which means it can work with any HTTP capable server technology. In fact,
it can even work without any server.

# Benefits

W20 provides a **modular programming model for web applications**, allowing entire parts of web frontend to be reused
between applications. These parts are called fragments and can be published on any HTTP server as static resources.
Creating a frontend application with W20 becomes as easy as choosing the fragments you want to load and how you want 
them to be configured from a single configuration file a.k.a the application manifest.
In fact, W20 itself is distributed as several fragments which are, aside from its core, all optional.


# Organization of an application

A W20 application is a Single Page Application (SPA) composed of:

* A **master page** (usually `index.html`, but can be dynamically generated). It is the entry point of the application.
* A **configuration file** (usually `w20.app.json`, but can be dynamically generated).
* One or more **fragment(s)**. A fragment is a bundle of web resources described by a JSON manifest which must be 
accessible by HTTP from the browser.

```
    (docroot)
        |-index.html
        |-w20.app.json
        |-fragments
            |-fragment1
                |-fragment1.w20.json
                ...
            |-fragment2
                |-fragment2.w20.json
                ...
            ...
```

## The master page

A single page application is a web application that fits on a single web page called the master page (usually 
`index.html`). Assuming you keep your static resources in a bower_components directory, a sample code of a 
minimalist master page in W20 would be :

    <!doctype html>
    <html data-w20-app>
    <head>
        <title>Application title</title>
        <script type="text/javascript" 
                data-main="bower_components/w20/modules/w20" 
                src="bower_components/requirejs/require.js">
        </script>
    </head>

    <body>
        <div data-ng-view></div>
    </body>
    </html>

**Things worth noting :**

* The `data-w20-app` attribute on the `html` tag that will load the configuration of your W20 application.
* The `<script>` tag, where we reference [RequireJS](http://requirejs.org/) and instruct it to load W20.
* A W20 application is also an [AngularJS](http://angularjs.org) application. Therefore you should add a `<div>` tag 
with the `data-ng-view` attribute. This will include rendered templates into the master page.

## The configuration file

The configuration file `w20.app.json` is where you set-up your application. As mentioned earlier, a W20 application is 
basically composed of a set of fragments (a fragment is a bundle of web resources). All are optional except one : the
core fragment of W20, thus it has to be referenced in the `w20.app.json`.
This is done by specifying the path to its manifest :

    "bower_components/w20/w20-core.w20.json": {}

For further details, please refer to the manual.

## The fragments

An important concept in W20 is the **fragment**. A W20 application is composed of one or more fragment(s). A fragment is 
a bundle of web resources (templates, css, javascript, ...). Each one is intended to serve a purpose and can be reused 
across applications. For example, W20 provides an optional fragment with native AngularJS implementations of UI 
components such as _datagrid_ and _select_.

### Fragment manifest

To reference web resources and how they are to be configured, each fragment has its own manifest file located at the 
root of the fragment. The only mandatory property is the fragment unique identifier :

    {
        "id" : "fragment-identifier"
    }

By convention, the manifest file is called after the fragment identifier and suffixed by `.w20.json`. In this example, 
it is `fragment-identifier.w20.json`.
 
### Fragment modules

Now suppose we want to use the fragment discussed above but only the _datagrid_ implementation. W20 offers a finer 
granularity to configure your application. Within a single fragment, there can be several units called **modules**. 
Thereby, you can load a fragment without being forced to load all of its resources.
The sample code below shows how to declare a fragment's module :

    {
        "id" : "fragment-identifier";
        
        "modules": {
            "module1": {
                "path": "{fragment-identifier}/modules/module1",
                "autoload" : true|false,
                "configSchema": {
                    ...
                }
            }
        }
    }
    
**Things worth noting :**

* `{fragment-identifier}` is used by W20 as a placeholder to target the fragment path. This ensures paths are always 
relative to the fragment manifest location. It is particularly useful if the fragment is intended to be used across 
applications
* The `path` attribute is mandatory for RequireJS to load the module when it is required by the application
* Fragments modules are [AMD compliant](http://requirejs.org/docs/whyamd.html#amd)
* If a configuration JSON schema is provided for a specific module in the fragment manifest, the configuration specified
here will be validated against it.

# How it works

The master page is the one and only entry point of your web application. By adding the following `<script>` tag in it, 
to things happen :

1. RequireJS is loaded
2. Once RequireJS is loaded, it loads `bower_components/w20/modules/w20.js`
```
    <script type="text/javascript" 
            data-main="bower_components/w20/modules/w20" 
            src="bower_components/requirejs/require.js">
    </script>
```
3. `w20.js` is the core of W20. Once loaded, it will automatically parse your application configuration file 
`w20.app.json` to detect :

    * which fragments to load
    * which modules to load for each fragment
    * how fragments and modules should be configured 

# Start a new project

You can generate a W20 project archetype using the W20 generator. To do so, you will need to :

1. Install [Node.js](https://nodejs.org)
2. Install [Yeoman](http://yeoman.io/), [Bower](http://bower.io/), [Grunt](http://gruntjs.com/) globally using npm (which comes bundled with nodejs) :

    ` npm install -g yo bower grunt-cli`
    
3. Install the generator-w20 :

    `npm install -g generator-w20`
    
4. Create a directory for your project, cd into it and launch the generator with Yeoman :

    `yo w20`
    
5. Run your static server with Grunt :

    `grunt connect`
    
6. That's it !





# TODO REMOVE BELOW

While this modularity is at the heart of W20, it doesn't stop there. A carefully chosen set of open-source frameworks
are integrated with each other and augmented with features you'll need in enterprise software like:

* Internationalization,
* Security, 
* Sophisticated navigation, 
* UI components,
* Graphical theming,
* ...


Any other tag can be added in the head but be aware that due to the asynchronous nature of W20 initialization the loading 
order between masterpage-loaded and RequireJS-loaded resources is undefined. To be on the safe side, rely on the RequireJS loader to load any of the application dependencies.

## Body

As a W20 application is also an [AngularJS](http://angularjs.org) application, so you should add a `<div>` tag with the 
`data-ng-view` attribute to display the AngularJS current view contents. If you need any additional markup in the body, 
feel free to add it. As this is a Single Page Application, all tags defined in the body are present on all application 
pages.



### Other sections

When all the required modules of an application are loaded, each module can examine each fragment manifest to examine
it. Additional sections in the manifest can then be processed to trigger any additional behavior. By convention, each 
module detect its own section(s) and process them, though all the manifest is accessible and scoping is not enforced.

In W20 various modules are using this capability. For instance the `culture` module of `w20-core` will detect the
`i18n` sections in all manifests to register translation bundles to load. Another example is the `application` module
which will detect `routes` sections to register application routes in AngularJS.

If the module corresponding to a specific section is not loaded, it is simply ignored. This allows to declare capabilities
(or potential features) in a reusable fragment manifest, but decide at the application level (in configuration) if it
should be enabled or not.


## Loading 

A fragment is loaded when its manifest URL is referenced from an application configuration. At this point, an alias is
created between the fragment identifier enclosed with curly braces and the location *containing* the fragment manifest.
As an example, if the manifest of the `fragment1` fragment is loaded from `http://myserver.org/w20-fragments/fragment1/fragment1.w20.json`,
the `{fragment1}` alias will resolve to URL `http://myserver.org/w20-fragments/fragment1`.
 
This behavior allows to use the alias not only in the application but also in all fragment resources, including the
manifest. By doing this, you ensure that the paths are always relative to the fragment manifest location, so even if 
the fragment is moved, only a change in application configuration will be needed. **This is particularly important
if a fragment is intended to be reused across applications.**

# Configuration

The application configuration `w20.app.json` is one of the first things loaded by the W20 loader. Its role is to reference fragments
through their manifest URL and configure them specifically for the application. Here is a minimal configuration:

    {
        "resources/w20-core/w20-core.w20.json" : {}
    }
    
This configuration will trigger the load of the `w20-core` fragment and its modules defined as automatically loaded in 
the manifest. An alias will be bound from `{w20-core}` to the `resources/w20-core` URL.

## Configuring modules

Adding a fragment to the configuration like previously shown can be enough, although you often need to specify additional
information like the modules to load and their configuration. To do so, add a `modules` section to the empty object:

    {
        "resources/w20-core/w20-core.w20.json" : {
            "modules": {
                "application": {
                    "id": "my-app"
                }
            }
        }
    }

In this configuration, the `application` module of `w20-core` will be configured with the corresponding object (defining
the unique identifier of the application in this case). This module is normally defined as automatically loaded so this
definition will only serve to configure it. To load a module that is not automatically loaded without configuration, just 
specify it with an empty object:
 
    {
        "resources/w20-core/w20-core.w20.json": {
            "modules": {
                "application": {
                    "id": "my-app"
                }
            }
        },
        
        "resources/other-fragment/other-fragment.w20.json": {
            "modules": {
                "my-module": {}
            }
        }
    }

Note that:

* If a configuration JSON schema is provided for a specific module in the fragment manifest, the configuration specified
here will be validated against it.
* If a default configuration is provided for a specific module in the fragment manifest, the configuration specified here
will be merged with it, overriding it. If no default configuration is provided, the configuration is provided as-is to
the module.