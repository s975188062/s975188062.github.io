---
title: "工具 - Pythonista 3"
excerpt: "iPad，买前生产力，买后游戏机。如何在iPad上面coding，是一个困扰了我很久的问题，直到我发现了Pythonista 3。"
read_time: false
header:
  overlay_image: /assets/images/tool_pythonista_header.jpeg
  overlay_filter: 0.5
  teaser: /assets/images/tool_pythonista_header.jpeg
  actions:
    - label: "Pythonista 3"
      url: "https://apps.apple.com/cn/app/pythonista-3/id1085978097"
categories:
  - Tools
comments: true
author_profile: true
classes: wide
---

---

# 1. 简介

日常出差，有很多时间要在飞机和火车上度过。这些时间说长不长，说短也不短。拿出电脑来处理工作不现实，同时仅凭手机似乎也干不了什么。这个时候就需要让iPad的生产力最大化。

目前已经做到用iPad看书、读Paper、查手册和写文章。在Charlie的诸多需求中，还差一项Coding不知如何下手。

曾经试过类似Termius的工具，通过SSH在电脑上工作，这需要有一台主机在开机状态，且可以通过网络连接到。显然这在路途中是不可能的。

![tool_oythonista_appstore_page](/assets/images/tool_oythonista_appstore_page.png)

这就需要一个本地的IDE来实现在iPad上Coding的需求，而`Pythonista 3`是目前已知的唯一的一个功能和颜值都在线的移动端IDE。

![tool_pythonista_overview](/assets/images/tool_pythonista_overview.jpeg)

上图是`Pythonista 3`的主界面，左侧是编辑器，右侧是Console。代码绘制的图形也会显示在Console中。

价格的话是68元RMB，如果有这方面需求的话，这个价格并不高。

---

# 2. 功能简介

## 2.1 Python版本

在`Pythonista 3`中，内置了两个版本的Python编译器，分别是`2.7`和`3.6`。

这基本满足了对于`Python 2`和`Python 3`的各类需求了。

## 2.2 Packages

虽然没有pip等包管理工具，但无论从Package的数量还是质量上来看，`Pythonista 3`都是一个很用心的软件。
{: .notice}

有了Python环境，只是九牛一毛而已，在真实的应用环境中，少不了各种各样的Package。

`Pythonista 3`并没有开放包管理的功能，用户无法自己安装或卸载Package，这可以说是`pythonista 3`唯一的缺点了吧。

其实这也好理解，毕竟是在iPad种ARM构架上运行代码，所有函数和指令需要经过重新编译。所以只能运行经过开放商处理过的函数和库，而不能开放给客户随意安装Package。那么我们还是来看一看`Pythonista 3`中准备了哪些Package。

详细内容您可以访问官方文档[Pythonista Modules](http://omz-software.com/pythonista/docs/ios/index.html)

### 2.2.1 点名表扬的几个Package

* numpy

    > Numpy可以说是基础中的基础了，仅靠Python自带的语法处理数据，怕不是脑子都要烧开了。借助Numpy中的array，我们可以更加灵活地对数组进行运算。
    
* matplotlib

    > 当然也没有少了matplotlib，matplotlib已经是应用最广泛的数据可视化库了。满足基本的科研绘图需求。

* SymPy

    > 高阶的科学运算仅靠Numpy无法满足，这个时候就需要用到SymPy这个函数库。借由SymPy，可以通过简单的函数组合完成复杂的微积分等高级运算。

### 2.2.2 其他Packages

* The following modules have been created for Pythonista and provide iOS-specific functionality.

    > [appex — Utilities for Pythonista’s App Extensions](http://omz-software.com/pythonista/docs/ios/appex.html)
    > [canvas — Vector Graphics](http://omz-software.com/pythonista/docs/ios/canvas.html)
    > [cb — Connecting to Bluetooth LE Peripherals](http://omz-software.com/pythonista/docs/ios/cb.html)
    > [clipboard — Copy and paste](http://omz-software.com/pythonista/docs/ios/clipboard.html)
    > [console — Utilities for Console Output and Various System Services](http://omz-software.com/pythonista/docs/ios/console.html)
    > [dialogs — Easy-to-use UI Dialogs](http://omz-software.com/pythonista/docs/ios/dialogs.html)
    > [contacts — Access the iOS Contacts Database](http://omz-software.com/pythonista/docs/ios/contacts.html)
    > [editor — Functions for scripting Pythonista’s text editor](http://omz-software.com/pythonista/docs/ios/editor.html)
    > [keyboard — Utilities for the Pythonista Keyboard](http://omz-software.com/pythonista/docs/ios/keyboard.html)
    > [keychain — Secure Password Storage](http://omz-software.com/pythonista/docs/ios/keychain.html)
    > [linguistictagger — Linguistic analysis](http://omz-software.com/pythonista/docs/ios/linguistictagger.html)
    > [location — Geo-Location Services on iOS](http://omz-software.com/pythonista/docs/ios/location.html)
    > [motion — Motion Sensor Data on iOS](http://omz-software.com/pythonista/docs/ios/motion.html)
    > [notification — Notifications on iOS](http://omz-software.com/pythonista/docs/ios/notification.html)
    > [objc_util — Utilities for bridging Objective-C APIs](http://omz-software.com/pythonista/docs/ios/objc_util.html)
    > [photos — Photo Library Access on iOS](http://omz-software.com/pythonista/docs/ios/photos.html)
    > [reminders — Access to the iOS Reminders Database](http://omz-software.com/pythonista/docs/ios/reminders.html)
    > [scene — 2D Games and Animations](http://omz-software.com/pythonista/docs/ios/scene.html)
    > [shortcuts — Pythonista URLs and Utilities](http://omz-software.com/pythonista/docs/ios/shortcuts.html)
    > [sound — Sound effects and music playback](http://omz-software.com/pythonista/docs/ios/sound.html)
    > [speech — Text-to-Speech on iOS](http://omz-software.com/pythonista/docs/ios/speech.html)
    > [twitter — iOS Twitter Accounts and API Access](http://omz-software.com/pythonista/docs/ios/speech.html)
    > [ui — Native GUI for iOS](http://omz-software.com/pythonista/docs/ios/ui.html)

* The following popular third-party modules are also included with Pythonista. They are not part of the Python Standard Library, but they are not specific to iOS.

    > [bs4 — BeautifulSoup 4](http://omz-software.com/pythonista/docs/ios/beautifulsoup.html)
    > [Bottle: Python Web Framework](http://omz-software.com/pythonista/docs/ios/bottle/index.html)
    > [Dropbox for Python](http://omz-software.com/pythonista/docs/ios/dropbox.html)
    > [evernote](http://omz-software.com/pythonista/docs/ios/evernote.html)
    > [faker](http://omz-software.com/pythonista/docs/ios/faker.html)
    > [feedparser — Universal Feed Parser](http://omz-software.com/pythonista/docs/ios/feedparser.html)
    > [Markdown](http://omz-software.com/pythonista/docs/ios/markdown.html)
    > [markdown2 — A fast and complete implementation of Markdown in Python](http://omz-software.com/pythonista/docs/ios/markdown2.html)
    > [paramiko](http://omz-software.com/pythonista/docs/ios/paramiko.html)
    > [Python Imaging Library](http://omz-software.com/pythonista/docs/ios/PIL.html)
    > [PyPDF2](http://omz-software.com/pythonista/docs/ios/PIL.html)
    > [pyminizip — Create password-protected Zip files](http://omz-software.com/pythonista/docs/ios/pyminizip.html)
    > [qrcode — Pure python QR Code generator](http://omz-software.com/pythonista/docs/ios/qrcode.html)
    > [Requests – HTTP for Humans](http://omz-software.com/pythonista/docs/ios/requests.html)
    > [xmltodict](http://omz-software.com/pythonista/docs/ios/xmltodict.html)

* The following third-party modules are included with Pythonista, but full documentation is currently not available within the app.

    > [arrow](http://omz-software.com/pythonista/docs/ios/index.html)
    > [certifi](http://omz-software.com/pythonista/docs/ios/undocumented/certifi.html)
    > [cssselect](http://omz-software.com/pythonista/docs/ios/undocumented/cssselect.html)
    > [dateutil](http://omz-software.com/pythonista/docs/ios/undocumented/dateutil.html)
    > [ecdsa](http://omz-software.com/pythonista/docs/ios/undocumented/ecdsa.html)
    > [flask](http://omz-software.com/pythonista/docs/ios/undocumented/flask.html)
    > [html2text](http://omz-software.com/pythonista/docs/ios/undocumented/html2text.html)
    > [html5lib](http://omz-software.com/pythonista/docs/ios/undocumented/html5lib.html)
    > [httplib2](http://omz-software.com/pythonista/docs/ios/undocumented/httplib2.html)
    > [images2gif](http://omz-software.com/pythonista/docs/ios/undocumented/images2gif.html)
    > [itsdangerous](http://omz-software.com/pythonista/docs/ios/undocumented/itsdangerous.html)
    > [jinja2](http://omz-software.com/pythonista/docs/ios/undocumented/jinja2.html)
    > [markupsafe](http://omz-software.com/pythonista/docs/ios/undocumented/markupsafe.html)
    > [midiutil](http://omz-software.com/pythonista/docs/ios/undocumented/midiutil.html)
    > [oauth2](http://omz-software.com/pythonista/docs/ios/undocumented/oauth2.html)
    > [openpyxl](http://omz-software.com/pythonista/docs/ios/undocumented/openpyxl.html)
    > [parsedatetime](http://omz-software.com/pythonista/docs/ios/undocumented/parsedatetime.html)
    > [py](http://omz-software.com/pythonista/docs/ios/undocumented/py.html)
    > [pycparser](http://omz-software.com/pythonista/docs/ios/undocumented/pycparser.html)
    > [pygments](http://omz-software.com/pythonista/docs/ios/undocumented/pygments.html)
    > [pyparsing](http://omz-software.com/pythonista/docs/ios/undocumented/pyparsing.html)
    > [pytest](http://omz-software.com/pythonista/docs/ios/undocumented/pyparsing.html)
    > [pytz](http://omz-software.com/pythonista/docs/ios/undocumented/pytz.html)
    > [reportlab](http://omz-software.com/pythonista/docs/ios/undocumented/reportlab.html)
    > [simpy](http://omz-software.com/pythonista/docs/ios/undocumented/simpy.html)
    > [six](http://omz-software.com/pythonista/docs/ios/undocumented/six.html)
    > [sqlalchemy](http://omz-software.com/pythonista/docs/ios/undocumented/sqlalchemy.html)
    > [urllib3](http://omz-software.com/pythonista/docs/ios/undocumented/urllib3.html)
    > [wavebender](http://omz-software.com/pythonista/docs/ios/undocumented/wavebender.html)
    > [werkzeug](http://omz-software.com/pythonista/docs/ios/undocumented/werkzeug.html)
    > [yaml](http://omz-software.com/pythonista/docs/ios/undocumented/yaml.html)

---

# 3. Examples



---

以上。
