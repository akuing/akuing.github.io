---
layout: post
title: "Wechat on Ubuntu 22.04"
---

Getting Wechat installed on Linux is always a problem. In 2022,Tencent released the official Wechat Linux build on Ubuntu Kylin.

In this post, I would show how to install the Wechat Linux build in Ubuntu 22.04.

    $ lsb_release -a
    No LSB modules are available.
    Distributor ID:	Ubuntu
    Description:	Ubuntu 22.04.1 LTS
    Release:	22.04
    Codename:	jammy

Update software package information. 

    apt-get update 

Install fcitx,dbus,locales etc.

    apt-get install -y --no-install-recommends \
            build-essential \
            ca-certificates \
            locales \
            locales-all \
            gnupg \
            locales \
            locales-all \
            fcitx-libs-dev \
            fcitx-bin \
            fcitx-googlepinyin \
            fcitx \
            fcitx-ui-qimpanel \
            fcitx-sunpinyin \
            dbus-x11 \
            im-config

Clear the apt cache.

    apt-get clean

[UbuntuKylin](https://www.ubuntukylin.com/) is a Chinese Ubuntu derivative.

Edit /etc/apt/sources.list.d/wechat.list file, add ubuntukylin server in the list. Create the file,if the file does not exist.

    deb http://archive.ubuntukylin.com/ubuntukylin focal-partner main



Update software package information. 

    apt-get update 

Install weixin from UbuntuKylin server.

    apt-get install -y --no-install-recommends \
            weixin \
            language-pack-zh* \
            chinese* \
            fonts-wqy-microhei \
            fonts-wqy-zenhei \
            xfonts-wqy

Clear the apt cache.

    apt-get clean

Edit .bashrc file in you user's home folder. Add follow lines at the end of the file.

    GTK_IM_MODULE=fcitx
    QT_IM_MODULE=fcitx
    XMODIFIERS=@im=fcitx
    DefaultIMModule=fcitx

Run weixin with --no-sandbox

    weixin --no-sandbox"

Notice: **Don't run Wechat with sudo.** If you got the error message of no permission to access the .config file etc, chmod command is what you need.

Got the idea from [Lei Mao's blog](https://leimao.github.io/blog/Docker-WeChat//).

