The first step of integrating GameFuse with your project, is to make an account at https://www.gamefuse.co. After creating your account, add your first game and note the ID and API Token. With this setup, you can now connect via your game client.

## PixiJs, BabylonJs, etc

If your game engine has an editable HTML file, you can paste the following tag in the header of the HTML document:

```
<script src="https://cdn.jsdelivr.net/gh/game-fuse/game-fuse-js@main/V2/gameFuseFull.js"></script>

```

## Playcanvas

Playcanvas has a different method, in your scene, go to the settings wheel on the top bar, then click on "external scripts" on the left panel that is opened, then paste the following:

```
"https://cdn.jsdelivr.net/gh/game-fuse/game-fuse-js@main/V2/gameFuseFull.js".

```

This effectivly does the same thing as the prior method on build.

## Examples

If you would like to see an example in action, check out here

- [Phaser.io Flappy Bird Example](https://github.com/game-fuse/game-fuse-phaser-example)

- [PixiJS Flappy Bird Example](https://github.com/game-fuse/game-fuse-pixijs-example)

- [Babylon JS Example](https://github.com/game-fuse/game-fuse-babylonjs-example)

- [Playcanvas Example](https://playcanvas.com/project/1143168/overview/gamefuseplaycanvasexample)