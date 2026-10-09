# ztype-trainer
Trainer for the famous typing game ZType (http://zty.pe/)

## How to use

### Online
1. Head to [zty.pe](http://zty.pe/) to play the game.
2. In Chrome, open ```Developer Tool```, (press F12 on windows), go to ```Console``` tab, paste and execute the following code:
```
var script = document.createElement("script");script.type = "text/javascript";script.src = "https://cdn.jsdelivr.net/gh/KevinWang15/ztype-trainer@master/ztype-trainer.js";document.getElementsByTagName("head")[0].appendChild(script);
```

### Offline
Just clone this repo and use an http-server to serve the website.

e.g. Using [http-server](https://www.npmjs.com/package/http-server)

	git clone https://github.com/KevinWang15/ztype-trainer
	cd ztype-trainer
	http-server

and visit ```http://localhost:8080```

No Node.js? Python works too:

	cd ztype-trainer
	python3 -m http.server 8080

Opening `index.html` directly as a `file://` URL does not work: the game loads its images and sounds over HTTP.

The trainer still downloads jQuery from `apps.bdimg.com` at startup, so you need an internet connection even in offline mode.

## Debugging
1. Open Chrome DevTools (<kbd>F12</kbd>, or <kbd>Cmd</kbd>+<kbd>Option</kbd>+<kbd>I</kbd> on macOS).
2. If the "Trainer Activated" banner is missing above the game, check the ```Network``` tab: the jQuery request to `apps.bdimg.com` must succeed before any cheat installs.
3. Inspect the game and the trainer from the ```Console``` tab:
```
ig.version               // must be '1.24'
ig.game.mode             // 0 title, 1 playing, 2 game over
ig.game.currentTarget    // enemy being shot (should never be the player ship)
ig.game.emps             // EMPs left
trainer                  // all cheat functions, e.g. trainer.machineGun(), trainer.deactivateAll()
```
4. To set breakpoints, use the ```Sources``` tab. `ztype.js` is not minified, so you can break inside the game itself, e.g. in `shoot` in the `game.main` module (line 5389). Breakpoints in `ztype-trainer.js` work the same way.
5. After editing `ztype-trainer.js`, reload the page (<kbd>Cmd</kbd>/<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>R</kbd> skips the cache). Don't load the trainer a second time into the same page: every hotkey then fires twice and cancels itself.

See [ARCHITECTURE.md](ARCHITECTURE.md) for how the trainer hooks into the game.

## Key Bindings
|Shortcut|Function|
|----|----|
|<kbd>Alt</kbd>+<kbd>1</kbd>|Toggle machine gun (automatic shooting). None->slow->fast->none..|
|<kbd>Alt</kbd>+<kbd>2</kbd>|Toggle manual machine gun (press anykey to shoot! impress your friends)|
|<kbd>Alt</kbd>+<kbd>3</kbd>|Toggle instant kill (one bullet kill)|
|<kbd>Alt</kbd>+<kbd>4</kbd>|Unlimited EMP (press <kbd>enter</kbd> to use)|
|<kbd>Alt</kbd>+<kbd>5</kbd>|God Mode (Can be used with <kbd>Alt</kbd>+<kbd>0</kbd>)|
|<kbd>Alt</kbd>+<kbd>6</kbd>|Shotgun (kills every enemy)|
|<kbd>Alt</kbd>+<kbd>7</kbd>|A lot of enemies (spawn 80 enemies)|
|<kbd>Alt</kbd>+<kbd>8</kbd>|A lot of fast moving enemies (spawn 80 fast-moving enemies)|
|<kbd>Alt</kbd>+<kbd>9</kbd>|Deactivate all|
|<kbd>Alt</kbd>+<kbd>0</kbd>|Disable screen shake|
|<kbd>Alt</kbd>+<kbd>-</kbd>|Distraction free mode (removes everything other than the game)|
