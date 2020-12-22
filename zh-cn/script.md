# 脚本

您通过可以编写一份脚本文件实现控制镜头的目的。

以下是脚本的相关说明。

## 事件

### 帧刷新

```function OnUpdate(audioTime)```

帧刷新时调用此函数。

| 参数名    | 数据类型 | 说明                                           |
| --------- | -------- | ---------------------------------------------- |
| audioTime | number   | 事件发生时，音频的播放位置。单位为毫秒（ms）。 |

## 方法

### 获取摄像机

```CS.ChartCamera()```

获取摄像机对象以进行操作。摄像机的默认位置为(0, 0, 0)。

**示例**

```lua
local camera = CS.ChartCamera();
```

### 设置相机旋转

```var:SetRotation(pitch, yaw, roll)```

设置相机旋转的角度。

![Rotation](/img/rotation.jpg "三维空间的右手笛卡尔坐标示意图")

<font color=#999999>三维空间的右手笛卡尔坐标示意图</font>

| 参数名 | 数据类型 | 说明                        |
| ------ | -------- | --------------------------- |
| pitch  | number   | 俯仰角。围绕x轴旋转的角度。 |
| yaw    | number   | 偏航角。围绕y轴旋转的角度。 |
| roll   | number   | 翻滚角。围绕z轴旋转的角度。 |

**示例**

```lua
camera:SetRotation(0, 0, audioTime / 1000)
```

### 设置相机位置

```var:SetPosition(x, y, z)```

设置相机旋转的角度。

![Position](/img/position.jpg "X-Y-Z坐标轴示意图")

<font color=Red>X</font><font color=#999999>-</font><font color=Green>Y</font><font color=#999999>-</font><font color=Blue>Z</font><font color=#999999>坐标轴示意图</font>

| 参数名 | 数据类型 | 说明      |
| ------ | -------- | --------- |
| x      | number   | x轴偏移。 |
| y      | number   | y轴偏移。 |
| z      | number   | z轴偏移。 |

**示例**

```lua
camera:SetPosition(0, audioTime / 10000, 0)
```

### 载入音频文件

```CS.ScriptSoundEffect(fileName)```

载入一个音频文件用于播放。

| 参数名   | 数据类型 | 说明                                                 |
| -------- | -------- | ---------------------------------------------------- |
| fileName | string   | 要载入的音频文件的文件名。文件名相对于脚本所在目录。 |

**示例**

```lua
local se1 = CS.ScriptSoundEffect("click_drum.wav");
```

### 播放音频文件

```var:PlayOneShot()```

播放一个音频。需要注意的是，在脚本中使用音频时，音频与全局无关——也就是说，可能会出现以下的情况：

* 用户调节了偏移，导致音频和背景音乐无法对齐；
* 用户点击了暂停，但音频还在继续播放。

**示例**

```
se1:PlayOneShot();
```

## 示例

以下是一些效果的示例，它们是完全可用的——也就是您甚至可以直接复制粘贴后使用。

### 变为mania模式

通过控制镜头，您可以让游戏变得看起来像一个mania游戏——即您的视野平面和音符所在的平面是平行的。

```lua
-- 定义一个布尔型变量，可以通过控制该变量开启或关闭mania模式
local mania = true
local camera = CS.ChartCamera()
if mania == true then
    camera:SetRotation(58.739, 0, 0)
    camera:SetPosition(0, 18.08, 17.26)
end
```

### 添加一个入场动画

您可以让镜头逐渐下降，实现一个简单的入场动画效果。

```
local camera = CS.ChartCamera()
function OnUpdate(audioTime)
    if audioTime < -100 then
        camera:SetPosition(0, audioTime / -900， 0)
    end
end
```