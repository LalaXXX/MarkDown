# WebXR

## A-Frame 框架

### 引入

- 新建 html 文件，引入 \<script src="https://aframe.io/releases/1.0.3/aframe.min.js">\</script> 即可

```html
<html>
  <head>
    <script src="https://aframe.io/releases/1.0.3/aframe.min.js"></script>
  </head>
  <body>
    <a-scene>
      <!--球体-->
      <a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9"></a-box>
      <!--正方体-->
      <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E"></a-sphere>
      <!--圆柱体-->
      <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D"></a-cylinder>
      <!--地面-->
      <a-plane position="0 0 -4" rotation="-90 0 0" width="4" height="4" color="#7BC8A4"></a-plane>
      <a-sky color="#ECECEC"></a-sky>
    </a-scene>
  </body>
</html>
```

<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702114310156.png" style="zoom:95%;" /> 

### Game Demo

> 注意，aframe 中的资源(assets)、纹理贴图(textures) 以及模型 (models) 通常需要远程加载，如果是直接通过本地绝对路径打开 html 页面会因为跨域而无法正常访问资源，所以需要通过 host 或者 localhost 访问 html 文件进行开发
>
> - 直接运行本地**文件系统中**的文件
>
> ```
> file:///yourFile.html
> ```
>
> - 访问运行在**本地服务器上**的文件
>
> ```
> http://localhost/yourFile.html
> ```

- 坐标系：右手坐标系

<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702141521708.png" alt="image-20220702141521708" style="zoom:50%;" /> 

- **ECS 架构**：**Entity - component - system（实体 - 组件 - 系统）**

  - **实体** **\<a-entity>**，是指存在于游戏中的一个物体，实际上它是一系列组件的集合
  - **组件**是一个可重用和模块化的数据块，一个组件可以看作是 html 标签的一个属性，registerComponent

  ```html
  <script>
  AFRAME.registerComponent('very-high', {
    init: function () {
      this.el.setAttribute('position', '0 9999 0')
    }
  });
  </script>
  
  <a-entity very-high></a-entity>
  ```

  - **系统**为组件类提供全局范围，服务和管理 **，** 它为组件类提供公共 API （方法和属性），registerSystem

  ```html
  <script>
  AFRAME.registerSystem('car', {
    getSpeed: function (type) {
      if (type === 'tractor') {
        return 40;
      } else if (type === 'sports car') {
        return 300;
      } else {
        return 100;
      }
    }
  })
  
  AFRAME.registerComponent('car', {
    schema: {
      type: { default: 'tractor' },
    },
    init: function () {
      this.el.setAttribute('speed', this.system.getSpeed(this.data.type))
    },
  })
  </script>
  ```

- **原语**

  - 对 **实体 - 组件** 的封装
  - \<a-box>\</a-box>

  ```html
  <a-scene environment= preset: forest; >
    <a-entity geometry= primitive: box; ></a-entity>
  </a-scene>
  ```

  - \<a-scene>\</a-scene>
  - ........

1. 引入资源库，[aframe-environment-component](https://github.com/supermedium/aframe-environment-component)

   ```html
   <script src= https://unpkg.com/aframe-environment-component@1.3.1/dist/aframe-environment-component.min.js ></script>
   ```

   ```html
   <a-entity environment="preset: forest; groundColor: #445; grid: cross"></a-entity>
   ```

2. 添加场景，WASD 可左右移动

   - \<a-scene>

     - 设置画布（canvas），渲染器（renderer）以及渲染循环

     - 缺省相机和光照

     - 设置 webvr-polyfill，VREffect

     - 添加进入 VR 的界面，来启动WebXR API

       - 页面的右下角能看到一个 VR 的图标，表示添加成功了

       ![image-20220702115349855](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702115349855.png) 

   ```html
   <a-scene environment = "preset: forest; grid: cross"; ></a-scene>
   ```

   <img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702140417593.png" style="zoom:80%;" />

3. 添加墙体

   - 添加`<a-assets>`到场景中
   - 将纹理定义为`<a-assets>`下面的`<img>`
   - 给`<img>`一个HTML ID (e.g. `id= wallImg`)
   - 以DOM选择器格式使用ID来引用资源（`src= #wallImg`）

   ```htm
   <a-assets timeout= 30000 >
       <img
           id= wallImg 
           src= https://image-1300099782.cos.ap-beijing.myqcloud.com/wall.jpeg />
   </a-assets>
   
   <a-box scale= "30 20 4"  position= "0 0 -20"  src= #wallImg ></a-box>
   ```

   <img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702144940775.png" alt="image-20220702144940775" style="zoom:80%;" />

4. 添加文字

   - WebGL 有多种方法来处理文字的渲染，各有优缺点，aframe 采用 SDF 文本实现方案，使用了`three-bmfont-text`，简单且性能好。通过添加 `<a-text>` 原语，实现文本的渲染

   ```html
   <!-- 文本 -->
    <a-text
       id= start-text 
       value= Start 
       color= #BBB 
       position= "-3 6 -18 "
       scale= "10 10 10" 
       font= mozillavr 
   ></a-text>
   ```

   ![image-20220702145421270](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702145421270.png)

5. 添加光标、武器

   - 光标原语 `<a-cursor>` 既可以用于基于注视的交互，也可以用于基于控制器的交互。默认的外观是一个环形几何图形，它通常作为相机的子对象放置
   - 如下我们通过 `<a-camera>` 添加一个自己的相机去代替 `<a-scene>` 设置的缺省相机，并将 `<a-cursor>` 作为相机的子对象挂载，这样无论我们的相机如何旋转和移动，我们始终能看到光标
   - 引入 gltf 模型来加载一个武器，在资源管理系统中，通过 `<a-asset-item>` 原语引入一个 gltf 资源，然后在相机下挂载一个实体子对象，将实体的 `src` 设置为 `<a-asset-item>` 原语的 id：

   ```html
    <a-assets timeout= 30000 >
       <img
           id= wallImg 
           src= https://image-1300099782.cos.ap-beijing.myqcloud.com/wall.jpeg />
   
       <a-asset-item
           id= weapon 
           src= https://vazxmixjsiawhamofees.supabase.co/storage/v1/object/public/models/blaster/model.gltf ></a-asset-item>
   				
   </a-assets>
   
   <a-camera>
       <a-cursor color= #FAFAFA ></a-cursor>
   
       <a-gltf-model
           id= _weapon 
           src= #weapon 
           position= "0.5 -0.5 -0.8" 
           scale= "1 1 1" 
           rotation= "0 180 0" 
       ></a-gltf-model>
   
   
   </a-camera>
   ```

   ![image-20220702150246202](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702150246202.png)

6. 实现当光标聚焦了 start 文字时，文字变大并且变色；光标失焦是还原的效果

   - 注册一个 `start-focus` 组件，它在 `init()` 生命周期时给对应的实体注册了 `mouseenter` 和 `mouseleave` 事件监听
   - 当触发 `mouseenter` 事件时，start 字体会变大并且变成橙色
   - 当触发 `mouseleave` 事件时，start 字体会复原

   ```html
   <script>
   	AFRAME.registerComponent('start-focus', {
   		init: function () {
   			this.el.addEventListener('mouseenter', function () {
   				if (window.startLeaveTimer) {
   					clearTimeout(window.startLeaveTimer);
   					window.startLeaveTimer = null;
   				}
   				window.CursorFocusEntity = 'start';
   				this.setAttribute('scale', '12 12 12');
   				this.setAttribute('color', 'orange');
   			});
   
   			this.el.addEventListener('mouseleave', function () {
   				window.startLeaveTimer = setTimeout(() => {
   					window.CursorFocusEntity = null;
   					this.setAttribute('scale', '10 10 10');
   					this.setAttribute('color', '#bbb');
   				}, 500);
   			});
   		},
   	});
   </script>
   ```

   - 挂载组件到原语

   ```html
   <a-text id=start-text value=Start color=#BBB position="-3 6 -18 " scale="10 10 10" font=mozillavr start-focus></a-text>
   ```

   ![image-20220702153135698](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702153135698.png)

7. 监听光标点击事件

   ```html
   	AFRAME.registerComponent('cursor-listener', {
   		init: function () {
   			// 点击进行攻击
   			this.el.addEventListener('click', function (evt) {
   				console.log('光标点击了')
   			});
   		}
   	});
   
       <a-camera>
           <a-cursor color=#FAFAFA cursor-listener></a-cursor>
   
           <a-gltf-model id=_weapon src=#weapon position="0.5 -0.5 -0.8" scale="1 1 1" rotation="0 180 0">
           </a-gltf-model>
   
       </a-camera>
   ```

8. JavaScript 进行交互

   - 创建实体（子弹球体）

   ```js
   const attackEntity = document.createElement('a-sphere');
   ```

   - 给刚刚创建的球体设置 `radius`、`color`、`position` 和 `animation` 等组件

   ```js
   attackEntity.setAttribute('radius', '0.2');
   attackEntity.setAttribute('color', 'red');
   attackEntity.setAttribute('position', `${newX} ${newY} ${newZ}`);
   attackEntity.setAttribute(
       'animation',
       `property: position; dur: 300; to: ${point.x} ${point.y} ${point.z};`
   );
   ```

   - 计算子弹的初始位置

   ```javascript
   	function getPosition(point) {
   		const { x, y, z } = point;
   		// x, z 平面的角度
   		const tanXZ = x / z;
   		const degXZ =
   			z > 0
   				? 180 + Math.round(Math.atan(tanXZ) / (Math.PI / 180))
   				: Math.round(Math.atan(tanXZ) / (Math.PI / 180));
   		// y, z 平面的角度
   		// @ts-ignore
   		const tanYZ = (y - window.InitHeight) / Math.abs(z);
   		const degYZ = Math.round(Math.atan(tanYZ) / (Math.PI / 180));
   		// 计算新的子弹发射位置
   		const { newX, newY: newZ } = getPointByDegRotate(
   			{ x: 0, y: 0 },
   			{ x1: 0.5, y1: -2 },
   			(-degXZ * Math.PI) / 180
   		);
   		const { newX: newY } = getPointByDegRotate(
   			// @ts-ignore
   			{ x: window.InitHeight, y: 0 },
   			{ x1: 1.3, y1: -2 },
   			(degYZ * Math.PI) / 180
   		);
   		return { newX, newZ, newY };
   	}
   
   	function getPointByDegRotate(basePoint, oldPoint, radian) {
   		const { x, y } = basePoint;
   		const { x1, y1 } = oldPoint;
   		const newX = (x1 - x) * Math.cos(radian) - (y1 - y) * Math.sin(radian) + x;
   		const newY = (y1 - y) * Math.cos(radian) + (x1 - x) * Math.sin(radian) + y;
   		return { newX, newY };
   	}
   ```

   - 获取场景

   ```js
   const scene = document.querySelector('a-scene');
   ```

   - 挂载实体

   ```js
   scene.appendChild(attackEntity);
   ```

   - 销毁实体

   ```js
    const timer = setTimeout(() => {
       scene.removeChild(attackEntity);
       clearTimeout(timer);
     }, 300);
   ```

   ![image-20220702160208387](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702160208387.png)

### Panorama Demo

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>360&deg; Image</title>
    <meta name="description" content="360&deg; Image - A-Frame">
    <script src="https://aframe.io/releases/1.0.3/aframe.min.js"></script>
  </head>
  <body>
    <a-scene>
      <a-sky src="puydesancy.jpg" rotation="0 -130 0"></a-sky>

      <a-text font="kelsonsans" value="LalaX" width="6" position="-2.5 0.25 -1.5"
              rotation="0 15 0"></a-text>
    </a-scene>
  </body>
</html>
```

![image-20220702172521043](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220702172521043.png)

### 调试方法

> 注意：file 形式打开 index.html 因资源跨域问题，不能正常显示渲染纹理

- 通过 localhost 访问 index.html 文件进行调试
  - npm install -g http-server
  - 资源根目录下cmd执行，http-server -p ${your-port} ./
  - 访问  http://localhost:${your-port}
- 局域网内访问，http://10.167.0.86:8000

## 资料库

- [aframe 源码](https://github.com/aframevr/aframe)

- [aframe 官网](https://aframe.io/)

