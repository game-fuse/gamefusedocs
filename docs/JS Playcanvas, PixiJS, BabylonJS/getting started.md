Use GameFuse C# in your Unity project to easily add:
<div class="flex-row-wrap" markdown>
- [Authentication](https://github.com/game-fuse/game-fuse-phaser-example){ .md-button }
- [User Data](https://github.com/game-fuse/game-fuse-pixijs-example){ .md-button }
- [Leaderboards](https://github.com/game-fuse/game-fuse-babylonjs-example){ .md-button }
- [In Game Store](https://playcanvas.com/project/1143168/overview/gamefuseplaycanvasexample){ .md-button }
- [Friends](https://playcanvas.com/project/1143168/overview/gamefuseplaycanvasexample){ .md-button }
- [Messages](https://playcanvas.com/project/1143168/overview/gamefuseplaycanvasexample){ .md-button }
- [Groups](https://playcanvas.com/project/1143168/overview/gamefuseplaycanvasexample){ .md-button }

</div>


The first step of integrating GameFuse with your project, is to make an account

[Sign Up](https://gamefuse.co/users/sign_up){ .md-button }

After creating your account, add your first game and note the ID and API Token.

With this setup, you can now connect via your game client

## PixiJs, BabylonJs, etc

If your game engine has an editable HTML file, you can paste the following
tag in the header of the HTML document:

```html
<script src="https://cdn.jsdelivr.net/gh/game-fuse/game-fuse-js@main/V2/gameFuseFull.js"></script>
```

## Playcanvas

Playcanvas has a different method. In your scene, go to the settings wheel
on the top bar, then click on *external scripts* on the opened left panel.
Finally paste the following:

```plaintext
"https://cdn.jsdelivr.net/gh/game-fuse/game-fuse-js@main/V2/gameFuseFull.js".
```

This does the same thing as the previous method on build.

## Examples

If you would like to see examples in action, check them out here:

<div class="flex-row-wrap" markdown>
- [Phaser.io Flappy Bird Example](https://github.com/game-fuse/game-fuse-phaser-example){ .md-button }
- [PixiJS Flappy Bird Example](https://github.com/game-fuse/game-fuse-pixijs-example){ .md-button }
- [Babylon JS Example](https://github.com/game-fuse/game-fuse-babylonjs-example){ .md-button }
- [Playcanvas Example](https://playcanvas.com/project/1143168/overview/gamefuseplaycanvasexample){ .md-button }
</div>
