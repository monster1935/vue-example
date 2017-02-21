
### 前言

Github作为目前优秀的同性交友平台，其上维护了众多优秀的开源项目。目前Github上关于前端的项目也是数不胜数，Vue、React、Angular等等。自己也是通过官方文档+github的方式来学习一些新的技术和框架。在github上搜索相关项目时会发现，有的项目不光写了一手好文档并且还给出了项目的在线运行Demo。事实胜于雄辩，一个在线演示可能给项目带来更好的印象分。如何在github上维护自己个人项目源代码的同时并生成项目主页呢？

### Github项目主页

Github给用户提供了运行静态页面的地址，如何展示个人项目的静态页面？以下是创建项目主页的关键：

- gh-pages分支
- 访问地址：[github用户名].github.io/[项目仓库名]，如：[monster1935.github.io/vue-example]()

生成项目主页首先是将欲展示的静态页面推送的Github个人项目仓库的gh-pages分支下，然后通过上述的访问形式访问。

### 如何在维护源代码的同时并同时生成项目主页

以下以Vue的单页应用为例，给出完整的项目维护以及生成项目主页的步骤。

**一、Github上创建远程仓库**

在github上为个人项目创建远程仓库，如下所示:

![](http://oe7c74ud3.bkt.clouddn.com/createRepository.png)

**二、clone远程仓库到本地**

创建好远程仓库后，使用git工具将远程仓库clone到本地，如下所示:
![](http://oe7c74ud3.bkt.clouddn.com/clone.png)


**三、使用vue-cli生成vue单页应用项目**

进入项目根目录，使用vue-cli生成vue的项目的初始结构。步骤如下：


```bash
# 以webpack模板生成项目原型
vue init webpack vue-example
```
在使用vue-cli脚手架工具生成vue项目过程中会提示是否安装一些辅助工具库，可根据自己项目要求酌情安装，或者生成项目后安装。

项目生成完毕后，进入package.json所在目录执行npm install命令，安装项目运行需要的依赖。

依赖安装完成后，即可执行npm run dev命令启动本地的webpack-dev-server进行开发调试。
如下图所示，出现如下画面代表vue项目初始化完毕。后期可在该基础上进行自己项目的开发。
![](http://oe7c74ud3.bkt.clouddn.com/localhost.png)

**四、将项目推送到远程仓库**

项目开发过程中，可以将项目源码推送至github远程仓库中管理。

```
git add --all

git commit -m 'Initial the vue project'

git push 
```



**五、执行项目构建命令，并将构建后的静态页面推送至gh-pages分支**

项目开发完毕可以执行``` npm run build ``` 打包文件，进行文件的打包发布流程。

1. 切换到gh-pages分支 ``` git checkout -b gh-pages ```

2. 执行``` npm run build ```命令，构建代码，如下所示，项目文件结构中出现了dist文件夹
3. 将dist目录下的所有文件夹推送至远程仓库的gh-pages分支，执行以下命令：
```
# 强制添加dist文件夹，因为.gitignore文件中定义了忽略该文件
git add -f dist

# 提交到本地暂存区
git commit -m 'Initial the page of project'

# 部署dist目录下的代码
git subtree push --prefix dist origin gh-pages
```
**注：使用git subtree命令可以在同一分支上维护源代码以及构建代码，在部署时仅仅推送dist目录下的内容。**

### 小结

以上所述的在github上gh-pages分支上生成项目主页主要是利用了github提供的静态页解析功能，因此本文中所属的范围仅使用于静态页面的部署。在将Vue应用部署到gh-pages分支后，可能会出现部分资源无法加载的问题，原因就在于vue中的webpack配置在打包时其publicPath为根路径，如果该静态页在服务器中被访问则不会出现以上问题。在github解析时如果按照根路径解析会出错，因此在github上部署静态页时可以考虑将publicPath设置为当前目录，即``` publicPath: './' ```。

使用Vue-cli webpack模板生成的vue项目，出现上述问题应设置config/index.js中build对象下的```assetsPublicPath```字段为```assetsPublicPath: './'```,原理都是设置publicPath字段。