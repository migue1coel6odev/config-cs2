# config-cs2

Change directory until you're at `Steam\steamapps\common\Counter-Strike Global Offensive\game\csgo\cfg`!

Then:

```shell
$ git clone https://github.com/migue1coel6odev/config-cs2.git custom_cfg
```

In CS2:

```
exec custom_cfg/main.cfg
```

Game launch options: 

```
gamescope -w 1280 -h 960 -W 2560 -H 1440 -r 144 -f --force-grab-cursor --scaler stretch -- %command% -novid -high -nojoy
```
