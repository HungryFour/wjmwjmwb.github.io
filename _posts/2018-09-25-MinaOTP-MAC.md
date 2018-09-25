---
layout: post
title:  "MAC二步验证神器，谁用谁知道"
date:   2018-09-25 15:27:21 +0800
tags: MinaOTP-MAC 二步验证 GoogleAuth 
color: RGBA(150, 96, 240, 0.3)
cover: '../assets/source/minaotpmac.png'
subtitle: '自开发工具之MinaOTP-MAC'
--- 

# [MinaOTP-MAC](https://github.com/MinaOTP/MinaOTP-MAC)

MinaOTP-MAC is a two-factor authentication tray app that runs at macOS. It's based on [RFC6238](https://tools.ietf.org/html/rfc6238), and the algorithm was implement by `Objective-C`

The program will generate secure dynamic 2FA tokens for you, and the `add`, `edit`, `remove`,`import`, `export` are pretty convenient.

## Requirements

- macOS 10.10+
- Xcode 9.4.1+
- Swift 4.1

### Software Screenshot

Home

![screenshot](https://raw.githubusercontent.com/wjmwjmwb/GitImage/master/MinaOtp-Guide/guide_en_10.png)

right click the item to delete or edit

![screenshot](https://raw.githubusercontent.com/wjmwjmwb/GitImage/master/MinaOtp-Guide/guide_en_09.jpeg)

Edit

![screenshot](https://raw.githubusercontent.com/wjmwjmwb/GitImage/master/MinaOtp-Guide/guide_en_05.png)

right click the statusbar to get help

![screenshot](https://raw.githubusercontent.com/wjmwjmwb/GitImage/master/MinaOtp-Guide/guide_en_07.png)

Add Token

![screenshot](https://raw.githubusercontent.com/wjmwjmwb/GitImage/master/MinaOtp-Guide/guide_en_08.jpeg)

Scan Qr Code

![screenshot](https://raw.githubusercontent.com/wjmwjmwb/GitImage/master/MinaOtp-Guide/guide_en_02.png)

![screenshot](https://raw.githubusercontent.com/wjmwjmwb/GitImage/master/MinaOtp-Guide/guide_en_03.png)


### Feature

* Generate the 2FA token
* Choose a qrcode_image to add a new token
* Scan a qrcode_image to add a new token
* Add a new token mannually
* Edit the issuer and remark info
* Remove a existed token
* Backup datas to local json files
* Import datas from local json files
* Hotkey:   ⌘⌘   (double click ⌘) 

### Todo
* [ ] sync to icloud
* [ ] add hotkey to copy token 

### Acknowledgements
[FlatButton](https://github.com/OskarGroth/FlatButton)
[Magnet](https://github.com/Clipy/Magnet)
Thanks for their great work. 

