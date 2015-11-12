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

W20 is a Web solution designed to allow the developer to quickly and simply create enterprise-grade **Single Page
Application** (SPA). It is **server agnostic** which means it can work with any HTTP capable server technology. In fact,
it can even work without any server.

# Modular architecture

W20 provides a **modular programming model for Web applications**, which allow entire parts of Web frontend to be reused
between applications. These reusable parts are called fragments and can be published on any HTTP server as static resources.
Creating a frontend application becomes as easy as referencing the fragment URLs from a configuration file and providing 
some parameters. W20 itself is distributed as several fragments which are all optional, aside from W20 Core. 

# Features

While this modularity is at the heart of W20, it doesn't stop there. A carefully chosen set of open-source frameworks
are integrated with each other and augmented with features you'll need in enterprise software like:

* Internationalization,
* Security, 
* Sophisticated navigation, 
* UI components,
* Graphical theming,
* ...

# Application structure

A W20 application is a Single Page Application (SPA) composed of:

* A master page (usually `index.html`, but can be dynamically generated). It is the entry point of the application. 
More details [here](/docs/w20/masterpage).
* One or more fragment(s). A fragment is a bundle of Web resources described by a JSON manifest which must be accessible 
by HTTP from the browser. More details [here](/docs/w20/fragments).
* A configuration (usually `w20.app.json`, but can be dynamically generated). More details
[here](/docs/w20/configuration).


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

# Masterpage

The masterpage is the only entry point of a W20 application. It is the only full page served when a user access the
application. Its responsibility is to load W20 with a `<script>` tag and define the application top-level layout. 
A minimal master page looks like this:

    <!doctype html>
    <html data-w20-app>
    <head>
        <title>Application title</title>
        <script type="text/javascript" 
                data-main="bower_components/w20/core/modules/w20" 
                src="bower_components/requirejs/require.js">
        </script>
    </head>

    <body>
        <div data-ng-view></div>
    </body>
    </html>

## Head

Two things are required to load a W20 application:

* a `<script>` tag to load RequireJS and specify the w20 module as the main module, 
* a `data-w20-app` attribute on the `html` tag.

Any other tag can be added in the head but be aware that due to the asynchronous nature of W20 initialization the loading 
order between masterpage-loaded and RequireJS-loaded resources is undefined. To be on the safe side, rely on the RequireJS loader to load any of the application dependencies.

## Body

As a W20 application is also an [AngularJS](http://angularjs.org) application, so you should add a `<div>` tag with the 
`data-ng-view` attribute to display the AngularJS current view contents. If you need any additional markup in the body, 
feel free to add it. As this is a Single Page Application, all tags defined in the body are present on all application 
pages.

# Fragments

The central concept in W20 is the **fragment**, which is a reusable bundle of Web resources described by a manifest. A
W20 application is composed of one or more fragment(s), which can be configured specifically. W20 itself is packaged
and distributed as multiple fragments, W20 Core being the first and most important.

## Fragment manifest

A fragment is simply a collection of Web resources. To organize them within a fragment and describe how they can be 
configured, each fragment has its own manifest.
It is a JSON data structure which is in its purest form :

    {
        "id" : "fragment-identifier"
    }
    
Each fragment loaded in a W20 application must be identified by a unique identifier. It is the only required attribute
of a fragment manifest and the only constraint. Naturally a fragment with such a limited amount of information will not
provide any useful behavior, so additional data structures can be added at will and will eventually trigger additional
behavior. 
 
### Modules section

The W20 loader recognize an additional section in the fragment manifest, under the `modules` attribute: 
 
    "modules": {
        "module1": {
            "path": "{fragment-identifier}/modules/module1",
            "config" : {
                ...
            },
            "schemaConfig": {
                ...
            },
            "autoload" : true|false
        }
    }
    
This section describes the AMD modules contained in the fragment with:

* The path which will be used by RequireJS to load the module if it is required by the application (**mandatory**), 
* The default configuration of the module which can be any object (optional),
* The JSON schema of the configuration options (optional),
* If the module will be loaded automatically or if it must be required explicitly by the application.

The modules described here are standard AMD modules. To know more about modules, please see the 
[corresponding section](#!/w20-doc/core/modules). 

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

# Start a new project

You can generate a W20 project archetype using the W20 generator. To do so, you will need to :

1. Install [Node.js](https://nodejs.org)
2. Install [Yeoman](http://yeoman.io/), [Bower](http://bower.io/), [Grunt](http://gruntjs.com/) globally using npm (which comes bundled with nodejs) :
```
    npm install -g yo bower grunt-cli
```
3. Install the generator-w20 :
```
    npm install -g generator-w20
```
4. Create a directory for your project, cd into it and launch the generator with Yeoman :
```
    yo w20
```
5. Run your static server with Grunt :
```
    grunt connect
```
6. That's it !
