# gitbook编译方法

由于gitbook官方依赖了旧版本 nodejs，无法在最新版本 nodejs 上工作，所以我使用docker镜像编译本书的。镜像id为`lonegunmanb/gitbook:ubuntu`

但是由于使用了`sitemap-general`插件无法在`lonegunmanb/gitbook:ubuntu`镜像使用的旧版本 nodejs 上工作，所以要先使用`lonegunmanb/gitbook:nvm`镜像切换 nodejs 版本，安装对应插件后重新执行`gitbook install`才能工作。流程如下：

1. `docker run -ti --rm -v $(pwd):/book --entrypoint=bash lonegunmanb/gitbook:nvm`
2. 进入容器后执行：`nvm use 16`，切换到 nodejs 16
3. 然后执行`npm i gitbook-plugin-sitemap-general`，安装插件依赖的文件
4. `exit`推出当前容器
5. `docker run --rm -v $(pwd):/book lonegunmanb/gitbook:ubuntu gitbook install`，安装所需要的插件

随后就可以工作了

编译命令如下：

```shell
docker run --rm -v $(pwd):/book lonegunmanb/gitbook:ubuntu gitbook build
```

本地预览：

```shell
docker run --rm -v $(pwd):/book -p 4000:4000 lonegunmanb/gitbook:ubuntu gitbook serve
```

输出pdf：

```shell
docker run --rm -v $(pwd):/book lonegunmanb/gitbook:ubuntu gitbook pdf .
```
