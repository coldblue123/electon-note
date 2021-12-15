## preload.js 在开发环境中正常使用, 生产环境中找不到文件?
在页面加载前注入一些自定义API, 那preload肯定是一个首选方式, 问题也接踵而来, 开发环境好好的, 怎么生产环境找不到注入文件了呢?开发环境下可调用具体的静态目录, 
进行静态文件注入 比如: file:/Users/admin/Desktop/project/static/preload.js
在生产环境下, 我们的静态文件被打包到electron静态文件管理模块中, 继续使用本地静态文件显然不太现实. 
```bash
生产环境electron静态文件模块的地址
win: C:/Users/Administrator/AppData/Local/Programs/应用名/resources/app.asar/dist/electron
mac: /Applications/应用名.app/Contents/Resources/app.asar/dist/electron
```
app.asar里放的是我们编译之后的静态文件, 看到dist你应该就很熟悉了, 没错就是项目编译后的目录,但是app.asar文件无法像正常文件夹一样打开,仅支持在electron软件中使用, 这也是源码的保护机制</br>
### 使用 __dirname 拿到文件目录
```bash
import path from 'path'
const filePath = `file:${path.resolve(__dirname, 'preload.js')}`
```
根据项目具体的打包目录进行文件拼接即可, 不管开发环境还是生产环境, 要注入某个js文件, 都必须要找到文件的完整路径, 只要找到preload文件的存放目录就可以解决这个问题了!</br>
