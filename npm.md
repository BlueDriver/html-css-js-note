## NPM笔记

### node命令

#### node版本信息

```shell
node -v
```

#### 运行文件

```shell
node file.js
```

> 注意须在文件所在目录执行该命令

---

### NPM

> NPM（Node Package Manager）是一个包管理工具，允许用户从NPM服务器下载别人编写的第三方包到本地使用、下载并安装别人编写的命令行程序到本地使用以及允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

> 安装node.js后，因其已经集成了npm，所以直接使用即可
#### 查看npm版本
```shell
npm -v
```

#### 更新npm版本

```shell
npm install npm -g
```
####  安装模块（包）

```shell
npm install <Module Name>
# -g 参数：全局（global）安装
# 安装express模块
npm install express
```

> 安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 **require('express')** 的方式就好，无需指定第三方包路径。
>
> ```javascript
> var express = require('express');
> ```

##### 全局安装与本地安装

```shell
npm install express          # 本地安装
npm install express -g  	 # 全局安装
# 如果出现以下错误：
npm err! Error: connect ECONNREFUSED 127.0.0.1:8087
# 解决办法为：
npm config set proxy null	 # 不使用代理
```

###### 全局安装VS本地安装

| 安装方式 | 安装位置                                                     | 使用                                  |
| -------- | ------------------------------------------------------------ | ------------------------------------- |
| 全局安装 | 将安装包放在 /usr/local 下或者你 node 的安装目录             | 可以直接在命令行里使用                |
| 本地安装 | 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录 | 可以通过 require() 来引入本地安装的包 |

##### 查看所有模块的信息

```shell
npm list -g		# -g全局
# 或
npm ls -g
# 列出当前项目中的包
npm ls
```

##### 查看某个模块的版本号

```shell
npm list <module_name>
# example:
npm list express
```

> `package.json`文件位于模块所在目录下，用于定义包的属性

#### 卸载模块

```shell
npm uninstall express
```

> 执行完后，express包对应的目录将被删除，可在 /node_modules/ 目录下查看express包目录是否还存在

#### 更新模块

```shell
npm update express
# 更新全局包
npm update express -g
```

#### 搜索模块

```shell
npm search express
```
#### 查看全局的包安装的位置

```shell
npm prefix -g
```
####  创建全局链接

```shell
# 假设我们将 express安装到了全局环境，使用下面的命令可以将其链接到本地环境
npm link express
```
> 使用 npm link命令还可以将本地的包链接到全局。使用方法是在包目录**（package.json）** 所在目录)中运行命令 `npm link` ，如果项目不再需要该模块，可以在项目目录内使用npm unlink命令，删除符号链接。

#### npm常用命令

| 命令                      | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| `npm -l`                  | 列出各命令                                                   |
| `npm help`                | 可查看所有命令的帮助信息                                     |
| `npm help <command>`      | 可查看某条命令的详细帮助，如：`npm help install`             |
| `npm update <package>`    | 可以把当前目录下`node_modules`子目录里边的对应模块更新至最新版本 |
| `npm update <package> -g` | 可以把全局安装的对应命令行程序更新至最新版                   |
| `npm cache clean`         | 可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人 |
| `npm config list -l`      | 列出npm的配置                                                |
| `npm bin`                 | 列出bin目录                                                  |
| `npm set`                 | 设置环境变量，例子：`npm config set prefix <目录>`，更改全局安装目录 |

#### 使用淘宝 NPM 镜像

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

这样就可以使用 cnpm 命令来安装模块了：

```shell
cnpm install <module_name>
```

> [更多淘宝MPM相关的信息](<http://npm.taobao.org/>)