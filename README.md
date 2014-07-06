meteor-phaser
=============

Project skeleton for developing Phaser games with Meteor.

## Dependencies

Meteor uses

- CoffeeScript
- [PhaserIO](https://github.com/thinkong/meteor-phaser/)
- Underscore
- jQuery

**Warning:** The packages `autopublish` and `insecure` are still active. Don't forget to remove them when going into production.

## How to use

### Windows

I packaged the Atmosphere package **PhaserIO** into the folder `packages`. This should run without a problem, just call it with `meteor add phaserio`.

### Non-Windows

Support for Meteorite should already exist, therefore you can delete the packages folder and `mrt add phaserio`.

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
</body>
~~~

The header template is not interesting at the moment, and the code is straightforward, so let's skip to the game template.

~~~HTML
<template name="game">
	<div class="game">
		{{game}}
	</div>
</template>
~~~

`game.coffee` defines the template helper for this template. Feel free to adjust the size, convert it to full screen, or even make the game responsive (check out this [interesting article from mobi.fm](https://mobi.fm/blog/responsive-full-screen-phaser-test-html5-canvas-phaser/)). The explicit return is so we don't have any code output to our browser window.

~~~CoffeeScript
Template.game.game = ->
	game = new Phaser.Game(800, 600, Phaser.AUTO)
	game.state.add "Boot", new BootState, false
	game.state.add "Preloader", new PreloaderState, false
	game.state.add "MainMenu", new MainMenuState, false
	game.state.add "InGame", new InGameState, false

	game.state.start "Boot"

	return
~~~

## Ending Remarks

If you want, you can install an HTML templating engine such as Jade that works with Handlebars. The [Atmosphere](https://atmospherejs.com) plugin page for Meteor and Meteorite has [just such a plugin](https://atmospherejs.com/package/jade-handlebars). Stylus, Less and SASS should be supported by Meteor directly, see `meteor list`.

For any remarks, criticisms or suggestions, feel free to email me at `christian.broomfield@posteo.de`