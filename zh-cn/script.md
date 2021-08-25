# 脚本

脚本是BanGround的一大特色。您可以通过控制脚本来制作很多炫酷的特效，增强谱面的表现力。

脚本基于**lua语言**，如果您不知道什么是**lua语言**，建议您先学习相关的基本知识。

## 开始

每个难度都对应一份不同的脚本文件。您可以在编辑器内点击**Script Editor**创建或编辑该难度的脚本。

如果您想要在外部编辑器编辑脚本，请到BanGround的数据存储目录，在**filesystem**中找到您谱面对应的kpak文件进行编辑。

## 类

### ScriptCamera

脚本控制的相机。

#### 设置位置

````c#
void SetPosition(float x, float y, float z)
````

控制镜头的位置。

<div style="text-align: center;">
  <div><img src="https://s3.ax1x.com/2020/12/29/rHeFqx.jpg" /></div>
  <div style="font-size: 0.75em; color: #999999">
    <font color=Red>X</font>-<font color=Green>Y</font>-<font color=Blue>Z</font>坐标轴示意图
  </div>
</div>

| 参数名 | 数据类型 | 说明                                                         |
| ------ | -------- | ------------------------------------------------------------ |
| x      | float    | x轴偏移。x轴指镜头正对轨道时，与屏幕的长边（显示器底边）平行的轴。 |
| y      | float    | y轴偏移。y轴指镜头正对轨道时，与屏幕的宽边（显示器侧边）平行的轴。 |
| z      | float    | z轴偏移。z轴指镜头正对轨道时，与屏幕所在平面垂直的轴。       |

**示例**

```lua
-- 获取相机对象
local camera = BanGround: GetCamera();
-- 设置相机位置
camera: SetPosition(0, 6, 0);
```



#### 设置旋转

````c#
void SetRotation(float pitch, float yaw, float roll)
````

控制镜头的旋转。

<div style="text-align: center;">
  <div><img src="https://s3.ax1x.com/2020/12/29/rHeAZ6.jpg" style="max-width: 280px" /></div>
  <div style="font-size: 0.75em; color: #999999">三维空间的右手笛卡尔坐标示意图</div>
</div>

| 参数名 | 数据类型 | 说明                        |
| ------ | -------- | --------------------------- |
| pitch  | float    | 俯仰角。围绕x轴旋转的角度。 |
| yaw    | float    | 偏航角。围绕y轴旋转的角度。 |
| roll   | float    | 翻滚角。围绕z轴旋转的角度。 |

**示例**

```lua
local camera = BanGround: GetCamera();
-- 设置相机旋转角度
camera: SetRotation(3, 0, 0);
```



### ScriptSoundEffect

脚本控制的音效。

#### 播放一次

```c#
void PlayOneShot();
```

立即播放一次当前音效。

**示例**

```lua
-- 创建音效对象
local se_normal = BanGround: PrecacheSound("se_normal.wav");
function OnJudge(result)
    -- 每当发生Perfect判定时，播放音效
    if result.JudgeResult == 0 then se_normal: PlayOneShot() end
end
```

> 注：这个音效的音量大小由**人声音量**选项控制，而非**击打音效音量**。



### ScriptSprite

脚本控制的精灵。

#### 设置位置

````c#
void SetPosition(float x, float y, float z)
````

控制精灵的位置。

| 参数名 | 数据类型 | 说明                                                         |
| ------ | -------- | ------------------------------------------------------------ |
| x      | float    | x轴偏移。x轴指镜头正对轨道时，与屏幕的长边（显示器底边）平行的轴。 |
| y      | float    | y轴偏移。y轴指镜头正对轨道时，与屏幕的宽边（显示器侧边）平行的轴。 |
| z      | float    | z轴偏移。z轴指镜头正对轨道时，与屏幕垂直的轴。               |

**示例**

```lua
-- 读取材质
local texture_neko = BanGround: LoadTexture("banground_neko.png");
-- 创建精灵
local sprite_neko = BanGround: CreateSprite(texture_neko);
-- 设置精灵的位置
sprite_neko: SetPosition(6, 2.5, 2);
```



#### 设置旋转

````c#
void SetRotation(float pitch, float yaw, float roll)
````

控制精灵的旋转。

| 参数名 | 数据类型 | 说明                        |
| ------ | -------- | --------------------------- |
| pitch  | float    | 俯仰角。围绕x轴旋转的角度。 |
| yaw    | float    | 偏航角。围绕y轴旋转的角度。 |
| roll   | float    | 翻滚角。围绕z轴旋转的角度。 |

**示例**

```lua
local texture_neko = BanGround: LoadTexture("banground_neko.png");
local sprite_neko = BanGround: CreateSprite(texture_neko);
-- 把精灵移动到镜头能看到的位置
sprite_neko: SetPosition(0, 0, 2);
-- 设置精灵的旋转角度
sprite_neko: SetRotation(5, 1.5, 3);
```



#### 设置颜色

```c#
void SetColor(float r, float g, float b, float a)
```

控制精灵的颜色。其实质是通过正片叠底的方式叠加在材质上的图层。

| 参数名 | 数据类型 | 说明     |
| ------ | -------- | -------- |
| r      | float    | 红。     |
| g      | float    | 绿。     |
| b      | float    | 蓝。     |
| a      | float    | 透明度。 |

**示例**

```lua
local texture_neko = BanGround: LoadTexture("banground_neko.png");
local sprite_neko = BanGround: CreateSprite(texture_neko);
-- 把精灵移动到镜头能看到的位置
sprite_neko: SetPosition(0, 0, 2);
-- 设定一个变量，记录精灵是否隐藏
local sprite_display = false;
-- 通过设置alpha通道值（透明度）为0来隐藏精灵
sprite_neko: SetColor(0, 0, 0, 0);
function OnUpdate(audioTime)
    -- 播放到10秒以后，如果精灵没有显示，则设置精灵显示出来
    if audioTime >= 10000 and sprite_display == false then
        sprite_display = true;
        sprite_neko: SetColor(255, 255, 255, 255);
    end
end
```



#### 替换材质

```c#
void OverrideTexture(int texId);
```

替换精灵的材质。

| 参数名 | 数据类型 | 说明                                    |
| ------ | -------- | --------------------------------------- |
| texId  | int      | 使用[加载材质](#加载材质)读入的材质ID。 |

**示例**

```lua
local texture_flower = BanGround: LoadTexture("flower.png");
local texture_flower_dark = BanGround: LoadTexture("flower_dark.png");
local sprite_flower = BanGround: CreateSprite(texture_flower);
-- 把精灵移动到镜头能看到的位置
sprite_neko: SetPosition(0, 0, 2);
-- 记录当前材质的名字
local sprite_flower_texture = "flower";
function OnUpdate(audioTime)
    -- 播放到30秒以后，如果精灵的材质不是“flower_dark”，就把材质替换成“flower_dark”
    if audioTime >= 30000 and sprite_flower_texture != "flower_dark" then
        sprite_flower_texture = "flower_dark";
        sprite_flower: OverrideTexture(texture_flower_dark);
    end
end
```



#### 设置缩放

```c#
void SetScale(float x, float y, float z)
```

设置精灵在各方向上的缩放。

| 参数名 | 数据类型 | 说明                                                         |
| ------ | -------- | ------------------------------------------------------------ |
| x      | float    | 在x轴上的缩放程度。                                          |
| y      | float    | 在y轴上的缩放程度。                                          |
| z      | float    | <del>在z轴上的缩放程度。</del>此设置无效，调用时提供为1即可。 |

**示例**

```lua
local texture_neko = BanGround: LoadTexture("banground_neko.png");
local sprite_neko = BanGround: CreateSprite(texture_neko);
-- 把精灵移动到镜头能看到的位置
sprite_neko: SetPosition(0, 0, 2);
-- 等比例放大为原来的1.2倍
sprite_neko: SetScale(1.2, 1.2, 1);
```





## 全局事件

### 音频时间刷新

```lua
function OnUpdate(audioTime)
```

根据屏幕刷新率，每隔一段时间触发一次，携带播放位置参数。

| 参数名    | 数据类型 | 说明                                           |
| --------- | -------- | ---------------------------------------------- |
| audioTime | float    | 事件发生时，音频的播放位置。单位为毫秒（ms）。 |

**示例**

```lua
local camera = BanGround: GetCamera();
function OnUpdate(audioTime)
    camera: SetPosition(math.sin(math.rad(audioTime)), 0, 0);
end
```



### 节拍刷新

```lua
function OnBeat(beat)
```

根据屏幕刷新率，每隔一段时间触发一次，携带节拍参数。

| 参数名 | 数据类型 | 说明                                                 |
| ------ | -------- | ---------------------------------------------------- |
| beat   | float    | 事件发生时，当前的节拍位置。与编辑器内节拍位置对应。 |

**示例**

```lua
local camera = BanGround: GetCamera();
function OnBeat(beat)
    if beat % 2 < 1 then
        camera: SetPosition(0, 0, beat % 2);
    else
        camera: SetPosition(0, 0, 0);
    end
end
```



### 判定发生

```lua
function OnJudge(result)
```

发生判定时触发，携带判定结果参数。

| 参数名 | 数据类型 | 说明                                   |
| ------ | -------- | -------------------------------------- |
| result | JudgeResult      | 事件发生时，触发事件的音符的判定结果。 |

#### JudgeResult 类型的属性:

| 属性名 | 属性类型 | 说明 |
| ----- | ------- | --- |
| Lane | int | 被判定 Note 的轨道 (0-6) |
| Type | int | 被判定 Note 的类型 (见下<sup>1</sup>) |
| Time | int | 被判定 Note 的时间 (毫秒) |
| Beat | float | 被判定 Note 的节拍 |
| JudgeResult | int | 判定结果 (见下<sup>2</sup>) |
| JudgeTime | int | 判定发生的时间 (毫秒) |
| JudgeOffset | int | 判定发生的时间距离被判定 Note 的时间的差 |

##### 注：
Note 的类型<sup>1</sup>

| 类型 | 值 |
| --- | -- |
| Single | 0 |
| Flick | 1 |
| Slide Start | 2 |
| Slide Tick | 3 |
| Slide End | 4 |
| Slide End (Flick) | 5 |

判定结果<sup>2</sup>
| 结果 | 值 |
| --- | -- |
| Perfect | 0 |
| Great | 1 |
| Good | 2 |
| Bad | 3 |
| Miss | 4 |

**示例**

```lua
local texture_don_normal = BanGround: LoadTexture("don_normal.png");
local sprite_don = CreateSprite(texture_don_normal);
sprite_don: SetPosition(6, 2, 2);
function OnJudge(result)
    -- Perfect判定时给精灵染红色，Miss时染灰色，其它不染色
    if (result.JudgeResult == 0) then
        sprite_don: SetColor(255, 0, 0, 255);
    elseif (result.JudgeResult == 4) then
        sprite_don: SetColor(30, 30, 30, 255);
    else
        sprite_don: SetColor(0, 0, 0, 255);
    end
end
```



## BanGround API

### 控制台输出

```c#
void Msg(string str)
```

向控制台输出一行文本。

| 参数名 | 数据类型 | 说明               |
| ------ | -------- | ------------------ |
| str    | string   | 要输出的文本内容。 |

**示例**

```lua
BanGround: Msg("Hello World!");
```



### 获取摄像机

```c#
ScriptCamera GetCamera()
```

获取摄像机对象以进行操作。摄像机的默认位置为(0, 0, 0)。

**示例**

```lua
local camera = BanGround: GetCamera();
```



### 读取材质

```c#
int LoadTexture(string tex)
```

读取一个材质到内存中。

| 参数名 | 数据类型 | 说明                                 |
| ------ | -------- | ------------------------------------ |
| tex    | string   | 材质文件名，路径与谱面所在目录相同。 |

**示例**

*见[创建精灵](#创建精灵)。*



### 创建精灵

```c#
ScriptSprite CreateSprite(int textureId)
```

使用一个材质创建一个精灵对象。精灵的默认位置为(0, 0, 0)。

| 参数名    | 数据类型 | 说明     |
| --------- | -------- | -------- |
| textureId | int      | 材质ID。 |

**示例**

```lua
-- 读取材质
local tex = BanGround: LoadTexture("tex.png");
-- 创建精灵
local spr = BanGround: CreateSprite(tex);
```



### 设置背景

```c#
void SetBackground(int texId)
```

将背景替换为指定材质。

| 参数名 | 数据类型 | 说明     |
| ------ | -------- | -------- |
| texId  | int      | 材质ID。 |

**示例**

```lua
-- 读取材质
local bg = BanGround: LoadTexture("bg.png");
-- 替换背景
BanGround: SetBackground(bg);
```



### 设置轨道材质

```c#
void SetLaneBackground(int texId)
```

将轨道替换为指定材质。

| 参数名 | 数据类型 | 说明     |
| ------ | -------- | -------- |
| texId  | int      | 材质ID。 |

**示例**

```lua
-- 读取材质
local lane = BanGround: LoadTexture("lane.png");
-- 替换轨道材质
BanGround: SetLaneBackground(lane);
```



### 设置判定线颜色

```c#
void SetJudgeLineColor(float r, float g, float b)
```

设置判定线的颜色。

| 参数名 | 数据类型 | 说明 |
| ------ | -------- | ---- |
| r      | float    | 红。 |
| g      | float    | 绿。 |
| b      | float    | 蓝。 |

**示例**

```lua
-- 判定线变为红色
BanGround: SetJudgeLineColor(255, 0, 0);
```



### 指定时间设置关键帧

```c#
void UntilTime(float time, Action<float> callback)
```

在指定的时间设置一个关键帧，之后在每次帧刷新时调用一次回调函数。

| 参数名   | 数据类型 | 说明                          |
| -------- | -------- | ----------------------------- |
| time     | float    | 关键帧的时间，单位为秒（s）。 |
| callback | function | 帧刷新时的回调函数。          |

**callback**格式：

```lua
function callback(progress)
```

帧刷新的回调。

| 参数名   | 数据类型 | 说明                    |
| -------- | -------- | ----------------------- |
| progress | float    | 关键帧进度，范围为0~1。 |

**示例**

```lua
-- 效果：0-2秒和4-6秒，转一圈缓停，再转回来缓停
local camera = BanGround: GetCamera();
local spin = function (progress)
    camera: SetRotation(0, math.sin(math.rad(progress * 180)) * 360, 0);
end
BanGround: UntilTime(2.0, spin);
BanGround: UntilTime(4.0, function (progress) end);
BanGround: UntilTime(6.0, spin);
```



### 指定节拍设置关键帧

```c#
void UntilBeat(float beat, Action<float> callback)
```

在指定的时间设置一个关键帧，之后在每次帧刷新时调用一次回调函数。

| 参数名   | 数据类型 | 说明                 |
| -------- | -------- | -------------------- |
| beat     | float    | 关键帧的节拍。       |
| callback | function | 帧刷新时的回调函数。 |

**callback**格式：

```lua
function callback(progress)
```

帧刷新的回调。

| 参数名   | 数据类型 | 说明                    |
| -------- | -------- | ----------------------- |
| progress | float    | 关键帧进度，范围为0~1。 |

**示例**

```lua
-- 效果：0-4拍和8-12拍，转一圈缓停，再转回来缓停
local camera = BanGround: GetCamera();
local spin = function (progress)
    camera: SetRotation(0, math.sin(math.rad(progress * 180)) * 360, 0);
end
BanGround: UntilBeat(4, spin);
BanGround: UntilBeat(8, function (progress) end);
BanGround: UntilBeat(12, spin);
```



### 获取生命值

```c#
float GetHealth()
```

获取生命值。

**示例**

```lua
local tex = BanGround: LoadTexture("tex.png");
local spr = BanGround: CreateSprite(tex);
spr: SetPosition(0, 0, 2);
function OnJudge(result)
    local health = BanGround: GetHealth();
    spr: SetScale(health / 1000, health / 1000, 1);
end
```



### 读取音效

```c#
ScriptSoundEffect PrecacheSound(string snd)
```

读取音效。

| 参数名 | 数据类型 | 说明                                 |
| ------ | -------- | ------------------------------------ |
| snd    | string   | 音效文件名，路径与谱面所在目录相同。 |

*注：音效仅支持wav、mp3和ogg格式。*

**示例**

```lua
local se = BanGround: PrecacheSound("se.wav");
se.PlayOneShot();
```


## 示例

以下是一些效果的示例，它们是完全可用的——也就是您甚至可以直接复制粘贴后使用。

### 变为mania模式

通过控制镜头，您可以让游戏变得看起来像一个mania游戏——即您的视野平面和音符所在的平面是平行的。

```lua
-- 定义一个布尔型变量，可以通过控制该变量开启或关闭mania模式
local mania = true;
local camera = BanGround: GetCamera();
if mania == true then
    camera: SetRotation(58.739, 0, 0);
    camera: SetPosition(0, 18.08, 17.26);
end
```

### 添加一个入场动画

您可以让镜头逐渐下降，实现一个简单的入场动画效果。

```
local camera = BanGround: GetCamera();
function OnUpdate(audioTime)
    if audioTime < -100 then
        camera: SetPosition(0, audioTime / -900， 0);
    end
end
```
