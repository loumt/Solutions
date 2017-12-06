# Solutions

### Electron与Jquery相冲突

#### jquery文件底部加 if(typeof module === 'object') {window.jQuery = window.$ = module.exports;};
