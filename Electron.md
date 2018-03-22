#### Electron与Jquery相冲突

> jquery文件底部加 if(typeof module === 'object') {window.jQuery = window.$ = module.exports;};
> https://stackoverflow.com/questions/32621988/electron-jquery-is-not-defined


#### electron-packager 安装

> npm install electron-packager -g

#### electron-packager 打包

> "packet": "electron-packager ./ cloudapp --platform=win32 --arch=x64 --out ./cloudapp-version --version 1.0.0 --icon=./static/images/favicon.ico --overwrite"

#### electron:vue项目中加入electron实现exe打包

* 将electron中的main.js文件将入到vue中的build目录下，改名为electron.js
* 在vue项目中安装electron和electron-packager,npm install electron --save-dev 和 npm install electron-packager --save-dev
* 在package.json中添加 "electron_dev": "npm run build && electron build/electron.js
* 在package.json中添加 "electron_packager": "electron-packager <项目目录> <应用名称> --platform=win32 --arch=x64 --out <打包后的目录> --version 1.0.0 --icon=./static/images/favicon.ico --overwrite"
* 在vue build的目录里添加package.json和electron项目的main文件，在这即重命名的electron.js
* npm run electron_packager完成打包

#### electron 内嵌 db

* electron可使用的db : https://www.jianshu.com/p/8d3e2c5baec4

#### electron发源人的自述

* https://www.zhihu.com/question/36292298/answer/102418523

#### 禁用系统菜单
* 安装ffi与ref模块
* let hwnd = win.getNativeWindowHandle() //获取窗口句柄。
* user32.GetSystemMenu(hwnd.readUInt32LE(0), true); //禁用系统菜单.
* win.show()

#### electron中使用node原生模块 ffi ref等
1.安装sqlite3  npm i --save sqlite3 --build-from-source --target=1.7.6 --runtime=electron --dist-url=https://atom.io/download/electron
2.安装官方推荐的electron-rebuild模块, ./node_modules/.bin/electron-rebuild --module-dir=<package.json位置>
