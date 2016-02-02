# Blessing Skin Server

Just a simple open-source Minecraft skin server write in PHP.

Live demo is now unavailable because of my fault that typing in wrong path when excute `chsh` on my VPS :(

![screenshot](https://img.prinzeugen.net/image.php?di=TASP)

Features:
-----------

- Support latest [UniSkinAPI](https://github.com/RecursiveG/UniSkinServer/blob/master/doc/UniSkinAPI_zh-CN.md)
- Also support legacy link style for UniSkinMod before 1.3
- Nothing else...

Installation:
-----------

1. Configure your database information in config.php
2. Run `./admin/install.php`
3. Configure your rewrite rules in your `nginx.conf`
3. Register or login with `admin/123456`
4. Upload your skin/cape
5. Configure your url in `UniSkinMod.cfg` of Minecraft client
6. Enjoy~

Server configure:
------------

Add rewrite rules to your nginx.conf:

```
if (!-e $request_filename) {
	rewrite ^/([^/]*).json$ /get.php?type=json&uname=$1 last;
	rewrite ^/skin/([^/]*).png$ /get.php?type=skin&uname=$1 last;
	rewrite ^/cape/([^/]*).png$ /get.php?type=cape&uname=$1 last;
	break;
}
```

Well, I need more time to add a fancy route instead of these shit :(

Now you can access `/path/to/root/username.json` to get your json profile. After uploading skins, you can also access `/path/to/root/skin/username.png` for skin image or `/path/to/root/cape/username.png` for cape.

Client configure:
------------

#### For UniSkinMod version >= 1.3

Put your root directory of skin server( like `/path/to/root` above ) into your `/config/UniSkinMod.cfg` of Minecraft client

Sample:

```
# Line starts with '#' is a commit line
# Line starts with 'Root: ' indicates a server
# All servers will be queried in that order.
# Server in front has higher priority
# Official server has the lowest priority
# No more legacy style link support!

# SkinMe Default
Root: http://www.skinme.cc/uniskin
# Your Server
Root: http://fuck.io/path/to/root
```

#### For UniSkinMod version < 1.3 or other

Also put your url into `/config/UniSkinMod.cfg`, but you need to fill 2 urls for skin and cape

Sample:

```
Version: 1
# Do not edit the line above

Skin: http://skins.minecraft.net/MinecraftSkins/%s.png
Cape: http://skins.minecraft.net/MinecraftCloaks/%s.png
# Your Server
Skin: http://fuck.io/path/to/root/skin/%s.png
Cape: http://fuck.io/path/to/root/cape/%s.png
```

FAQ
------------

If everything works well, you will get an awesome skin in game:

![screenshot2](https://img.prinzeugen.net/image.php?di=EV1E)

And the log will be like this:

```
[16:23:56] [launcher/INFO] [UniSkinMod]: Loading configuration ... @D:\Minecraft\1.8\.minecraft\config\UniSkinMod.cfg
[16:23:56] [launcher/INFO] [UniSkinMod]: Root Url Added: http://127.0.0.1/blessing-skin-server

[16:24:23] [Client thread/INFO] [UniSkinMod]: Filling profile for player: 621sama
[16:24:23] [Client thread/INFO] [UniSkinMod]: Fetching URL: http://127.0.0.1/blessing-skin-server/621sama.json
[16:24:23] [Client thread/INFO] [UniSkinMod]: Player Skin Selected: 621sama default 3f14a21723023642b9e8d2bc008b443780698aaedbb7d4e29960e8a2c754a771
[16:24:23] [Client thread/INFO] [UniSkinMod]: Player Cape Selected: 621sama aed8c3fc67aae4906b72fa74c27e15866c89752f0838f6b2a1c44bb4d59cec1e
```

If not, please check your log to locate where error occured.
