# Obscen, A simple scene manager

## Overview

Obscen is intended to be an easy-to-use scene manager for the smaller projects.

There are two classes: SceneManager and Scene.

### class SceneManager

Your gameloop only need to know about a scene manager, because it will handle all your scenes.
Your gameloop calls ```update``` and ```draw``` on the scene manager and it will pass it down to the current scene.
Obscen is agnostic against the arguments to the ```update``` and ```draw``` calls, so it doesn't matter if you pass down deltaTime or tick multiplier or renderingContext or whatever, the scene manager will just pass it on down to the scenes.

```js
const sceneManager = new Ob.SceneManager()

// the setScenes call will add a wrapped changeScene method to all scenes
sceneManager.setScenes([
  myScene,
  myOtherScene,
  ])
```

### class Scene

The scenes consist of just one property and a few methods.

```js
const myScene = Ob.Scene({
	
	// this scene's name, this is what you give to changeScene to change to this scene
	name: 'myScene',

	// this will be called when changing to this scene with changeScene('myScene', nextSceneParams)
	// the previousSceneParams is the second argument to changeScene([scene name], nextSceneParams)
	create: function (previousSceneParams) {},

	// this will be called when changing from this scene with changeScene('anyOtherScene')
	destroy: function () {},

	// will be called from this scene's scene managers update
	update: function () {},

	// will be called from this scene's scene managers draw
	draw: function () {},
})
```

## Code

Written in ES5.

Only exports to CommonJS (as of now).

## Usage

```js
const Ob = require('obscen') // import obscen (as Ob, because that was its intended name, which was taken)

localStorage.debug = 'obscen:*' // enable debug output

const splash = new Ob.Scene({ // create new scene - 'splash'
  name: 'splash', 
  create: function () {
    console.log('splash create', this.sharedObject)
    this.sharedObject.hej = 'hejjj'
  },
  destroy: function () {
    console.log('splash destroy')
  },
  update: function () {
    console.log('splash update')
  },
  draw: function () {
    console.log('splash draw')
  },
})

const game = new Ob.Scene({
  name: 'game',
  create: function () {
    console.log('game create', this.sharedObject)
  },
  destroy: function () {
    console.log('game destroy')
  },
  update: function () {
    console.log('game update')
  },
  draw: function () {
    console.log('game draw')
  },
})

const sceneManager = new Ob.SceneManager()

sceneManager.setScenes([
  splash,
  game,
  ])

sceneManager.changeScene('splash')

// of course update and draw will be done in your gameloop
sceneManager.update()
sceneManager.draw()
sceneManager.update()
sceneManager.draw()

splash.changeScene('game')

sceneManager.update()
sceneManager.draw()
sceneManager.update()
sceneManager.draw()

game.changeScene('splash')
```

### Debugging

Obscen uses [debug](https://www.npmjs.com/package/debug). Obscen follows debug conventions.

```js
localStorage.debug = 'ob-scene:*'
```

## Dependencies

- [debug](https://www.npmjs.com/package/debug)

## To do

- Add tests (prolly mocha-chai)
- Add AMD export
