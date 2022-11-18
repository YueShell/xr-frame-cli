# xr-frame-cli

用于微信小程序内置的`xr-frame`系统的CLI，提供以下两个功能：

1. 通过环境贴图，生成`xr-frame`专用的`env-data`，包含`skybox`、`diffuse sh`和`specular map`，支持打包成单二进制文件。
2. 对`gltf`模型文件进行预处理，优化为`xr-frame`友好的数据结构，同时支持打包为`glb`，能大幅提升加载速度。

## 使用

首先安装：
成功安装的系统：Ubuntu 20.04 python 2.7.5 node 14
```sh
1. apt-get install curl
2. curl --silent -k --location https://deb.nodesource.com/setup_14.x | bash -
3. apt-get update
4. apt-get install -y nodejs libvips-dev build-essential libxi-dev libgl1-mesa-dev libglu1-mesa-dev libglew-dev pkg-config
5. npm i xr-frame-cli -g
```

之后可以使用下列指令：

### env-data

运行：

```sh
xr-frame env-data -h
```

可以看到所有支持的操作。

目前支持的输入格式为`['hdr', 'exr', 'png', 'jpg', 'jpeg', 'bmp', 'gif', 'tiff']`，要求输入图片比例近似`2:1`。

以`xr-frame env-data -i test/exr.exr`为例，原始图片和产物如下（`specular map`为`rgbd`编码）：

![](./doc/env-data.jpg)


### gltf

运行：

```sh
xr-frame gltf -h
```

可以看到所有支持的操作。

>如果需要更进阶的优化，可以尝试使用[SeinJSUnityToolkit](https://github.com/hiloteam/SeinJSUnityToolkit)。

## 开发测试

项目提供了一张图和一个模型用于开发测试。

### env-data

执行：

```sh
npm run dev && xr-frame env-data -i test/exr.exr
```

输出将会在`test/exr`目录下。

### gltf

执行：

```sh
npm run dev && xr-frame gltf -i test/gltf-sources/gltf/index.gltf -o test/gltf-dist
```

输出将会在`test/gltf-dist/gltf`目录下。

ISSUE

出现类似报错解决方法/opt/newdata/EnterpriseNucleusServer/xr-cli/node_modules/xr-frame-cli/dist/env-data/renderer.js:25
        this._resizeExt = this._gl.getExtension('STACKGL_resize_drawingbuffer');                               ^
TypeError: Cannot read property 'getExtension' of null

1. Clone this repo: `git clone git@github.com:stackgl/headless-gl.git`
1. Switch to the headless gl directory: `cd headless-gl`
1. Initialize the angle submodule: `git submodule init`
1. Update the angle submodule: `git submodule update`
1. Install npm dependencies: `npm install`
1. Run node-gyp to generate build scripts: `npm run rebuild`
1. cp headless-gl/build/Release/webgl.node node_modules/gl/build/Release/webgl.node
