> Electron与Jquery相冲突

#### jquery文件底部加 if(typeof module === 'object') {window.jQuery = window.$ = module.exports;};


> electron-packager 安装 npm install electron-packager -g

> electron-packager 打包
#### "packet": "electron-packager ./ cloudapp --platform=win32 --arch=x64 --out ./cloudapp-version --version 1.0.0 --icon=./static/images/favicon.ico --overwrite"
