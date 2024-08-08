Use GameFuse C# in your Unity project to easily add:
<div class="flex-row-wrap" markdown>
[Authentication](/JS%20Playcanvas%2C%20PixiJS%2C%20BabylonJS/signing%20game%20users%20up/){ .md-button }
[User Data](/JS%20Playcanvas%2C%20PixiJS%2C%20BabylonJS/custom%20user%20data/){ .md-button }
[Leaderboards](/JS%20Playcanvas%2C%20PixiJS%2C%20BabylonJS/in%20game%20leaderboard/){ .md-button }
[In Game Store](/JS%20Playcanvas%2C%20PixiJS%2C%20BabylonJS/creating%20store%20items%20on%20the%20web/){ .md-button }
[Friends](https://google.com){ .md-button }
[Groups](https://google.com){ .md-button }
[Messages](https://google.com){ .md-button }
[Server Keys](/generic/server_key_access/){ .md-button }
[Cloud Code](/generic/cloud_code/){ .md-button }

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
[Phaser.io Flappy Bird Example](https://github.com/game-fuse/game-fuse-phaser-example){ .md-button }
[PixiJS Flappy Bird Example](https://github.com/game-fuse/game-fuse-pixijs-example){ .md-button }
[Babylon JS Example](https://github.com/game-fuse/game-fuse-babylonjs-example){ .md-button }
[Playcanvas Example](https://playcanvas.com/project/1143168/overview/gamefuseplaycanvasexample){ .md-button }
</div>
