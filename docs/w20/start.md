---
title: "Start a new project"
type: "home"
zones:
    - "W20"
sections:
    - "W20Introduction"
menu:
    W20Introduction:
        weight: 50
---

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
