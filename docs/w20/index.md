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

<div class="pull-right margin-top-20">
    <a href="/docs/w20/masterpage" class="btn btn-u">Next: Understanding the masterpage</a>
</div>
<div class="clearfix"></div>