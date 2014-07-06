meteor-phaser
=============

Project skeleton for developing Phaser games with Meteor.

## How to use

### Windows

You need the [PhaserIO](https://github.com/thinkong/meteor-phaser/) package from Atmosphere. Follow these steps:

1. Clone the [PhaserIO](https://github.com/thinkong/meteor-phaser/) project from github into the `packages` folder.
2. Rename the folder from `meteor-phaser` to `phaserio`
3. Go inside the `phaserio` folder and run `git submodule update --init`.
	- It will now download the latest Phaser to the `phaserio` folder.
4. Once finished, run `meteor add phaserio`

**Important:** The [Lo-Dash](https://github.com/alethes/meteor-lodash/) dependency works exactly like PhaserIO, but you don't have to use it. Instead run `meteor remove lodash` to remove it. You can get `underscore` instead if you wish, it's part of the Meteor packages and requires no setup.

### Non-Windows

Support for Meteorite should already exist, therefore you can delete the packages folder and `mrt add phaserio`. Otherwise, follow the steps for Windows and you should be fine.

### Post-PhaserIO

If all goes as planned, you should be able to start the app by running `meteor` from the base directory. If not, double-check that the `coffeescript`, `lodash`, and `jquery` packages have been added. This SHOULD have happened automagically, but who knows what computers actually do when you're not looking.

## Dependencies

meteor-phaser uses

- CoffeeScript
- [PhaserIO](https://github.com/thinkong/meteor-phaser/)
- [Lo-Dash](https://github.com/alethes/meteor-lodash/)
- jQuery

**Warning:** The packages `autopublish` and `insecure` are still active. Don't forget to remove them when going into production.

## Project Layout

Most of the magic happens in the `client` folder. Assets except for CSS files are stored in `public`.

~~~
client/
	game/
		sprites/
		states/
			BootState.coffe        # Loads the Preload-Sprite and configues game/screen
			PreloaderState.coffee  # Loads all assets and displays Preload-Sprite
			MainMenuState.coffee   # Menu, just shows a sprite for now
			InGameState.coffee     # Code for the game can go here
		game.coffee	 			   # Starts the Game

	stylesheets/
		phaser-meteor.css

	game.html					   # Loads the game through a helper defined in game.coffee
	header.html					   # Title of the page currently defined here
	phaser-meteor.html			   # Glues the templates together and is the current index page.
~~~

## In Depth

Let's take a look at `phaser-meteor.html` It defines the rough site layout and has two Handlebars templates to do so, `header`, which layouts everything above the game, and `game`, which is the game itself.

~~~HTML
<head>
	<title>phaser-meteor</title>
</head>

<body>
	{{> header}}

	{{> game}}

	{{> footer}}
</body>
~~~

The header template is not interesting at the moment, and the code is straightforward, so let's skip to the game template. It has an id instead of a class because it will serve as the parent for the canvas.

~~~HTML
<template name="game">
	<div id="game">
		{{game}}
	</div>
</template>
~~~

`game.coffee` defines the template helper for this template. Feel free to adjust the size, convert it to full screen, or even make the game responsive (check out this [interesting article from mobi.fm](https://mobi.fm/blog/responsive-full-screen-phaser-test-html5-canvas-phaser/)). The explicit return is so we don't have any code output to our browser window.

~~~CoffeeScript
Template.game.game = ->
	game = new Phaser.Game(800, 600, Phaser.AUTO, "game")
	game.state.add "Boot", new BootState, false
	game.state.add "Preloader", new PreloaderState, false
	game.state.add "MainMenu", new MainMenuState, false
	game.state.add "InGame", new InGameState, false

	game.state.start "Boot"

	return
~~~

**Aside:** I'm an HTML and CSS noob, there's no other way of putting it. I did, however, manage to center all of the elements and you can review or throw away the code here:

~~~CSS
.centered {
	padding-left: 0;
	padding-right: 0;
	margin-left: auto;
	margin-right: auto;
	width: 1024px;
	text-align: center;
}

canvas {
	padding-left: 0;
	padding-right: 0;
	margin-left: auto;
	margin-right: auto;
	display: block;
	width: 800px;
}

.footer {
	width: 800px;
}

.footer pre {
	margin-left: 0;
	margin-right: 0;
	text-align: left;
}
~~~

## Ending Remarks

If you want, you can install an HTML templating engine such as Jade that works with Handlebars. The [Atmosphere](https://atmospherejs.com) plugin page for Meteor and Meteorite has [just such a plugin](https://atmospherejs.com/package/jade-handlebars). Stylus, Less and SASS should be supported by Meteor directly, see `meteor list`.

For how I knew how to install Meteorite packages under windows, see [this helpful post by Tom Coleman](https://www.discovermeteor.com/blog/using-meteor-and-atmopshere-on-windows/).

For any remarks, criticisms or suggestions, feel free to email me at `christian.broomfield@posteo.de`