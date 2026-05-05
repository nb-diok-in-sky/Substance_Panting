<img width="2553" height="1599" alt="Transform_2D" src="https://github.com/user-attachments/assets/c609161a-1dfc-473a-b411-e2428a5ab93b" /># Substance_Panting
这个是讲解部分pbr贴图绘制的一些常用知识点和常用节点 以Substance_Designer为核心的一些贴图绘制方法
作为DCC工具全流程开发的一个了解
我想应该不会像隔壁Shader编写写那么多东西 原理和我所理解的shader原理开发其实都差不多 
都是通过数学在图上绘制某些形状来达成效果

更新了半天还是觉得有些水 索性把原理简要说明一变

节点式编辑图像工具本质是通过灰度图RGB颜色图来结合绘制图案
他和UV很像 都是一个点对着一个点 你督读到这个点的时候 就可以读取这个点里面存储的一个灰度图片    好吧我感觉就是一个东西
主要依赖灰度图的形变来达成目的
灰度图分黑白灰 值越接近0或者干脆小于0的时候越黑   值越接近1 甚至大于1的时候越白
通过黑色和白色的区分来进行绘制图片
比如我想画一个圆 那么我就需要对应的运算公式来找到一个圆
或者直接通过 Shape节点来获取一个圆 
你想要很多圆以以及可能需要对圆进行某一些外形的处理 同时你的场景里面有一些杂乱的元素 你就可以把你的圆绘制成白色区域 然后黑色区域留空变成0以下
拿去乘以你提前预设好的形状 此时黑色部分会乘以0甚至0以下的东西，然后直接消失
白色部分的图案会被拿去乘以你预设好的图案与圆在图案上的一个重合点 他们每个点都会去相乘


复杂的图形也是通过简单的图形叠加而成的
而那些看似复杂的图案 比如说生锈效果 可以基于Noise贴图 和颜色值 进行绘制
本质上都是在通过数学的方式来控制灰度图
然后来获取你想要的图形





关于想要的节点的话 直接使用空格来进行一个搜索就行了 

<img width="2559" height="1599" alt="N" src="https://github.com/user-attachments/assets/1efb59bc-6458-42d3-bac9-ef1618da8755" />
一般新建PBR贴图都是用这个 Metallic Roughness

建好之后里面是这样的 
<img width="2559" height="1599" alt="New_Window" src="https://github.com/user-attachments/assets/d1f5cb0b-fc7b-4f73-88aa-6a282cbd1244" />
我们分板块来 看这些部分是用来干嘛的

首先从下面这张图导入你搓好的模型——右击那个“未完成的包”  ——“链接” ————=“3D场景” 找到你的obj模型
<img width="594" height="789" alt="WareHouse" src="https://github.com/user-attachments/assets/7cdb1c96-da03-4cb3-8fb0-363db47f2ad8" />

双击刚刚的模型你就可以在这里发现你的模型了  之后你1的材质贴图的预览就在这里  这里也叫3D预览
<img width="722" height="768" alt="Model" src="https://github.com/user-attachments/assets/cc32bb4b-72c0-4286-b514-b29f1aee0f91" />

然后这里是导入你展好的uv的东西 这里需要点击那个————有两个方块一上一下斜着相交的图标来导入你的V
<img width="695" height="669" alt="UV_input" src="https://github.com/user-attachments/assets/dbf760af-04f7-4f52-abdb-aea08e8bb2a3" />


然后是节点链接的主要场所  这里有这么多选项 也不是所有的选项都需要选 一般就是使用法线以及粗糙度、金属度、高度、基础颜色之类的 
需要注意的是这里你想要连接的最终的贴图需要右键 然后选择 3Ds视图预览 并且给予正确类型的值、并且你还要将你搓好的东西他的团放置在你的合适的UV上 这样你才能在你的3D预览图标看见你的团对于模型造成的影响
<img width="1251" height="747" alt="Node" src="https://github.com/user-attachments/assets/4fb39da9-cc8a-41b1-8f4d-8d65463f162f" />



# 常用节点
由于这个DS的一些原理和Shader连连看的原理基本上都是一致的所以讲解不会很详细 主要以 记录节点信息为和用法为主

# 混合节点 Blend
非常万恶之源的一个东西 类比加减乘除 相当重要
混合模式 常用正片叠底（乘法）添加（线性减淡）以及减去 或者是最大值、最小值 
可以通过你的透明度来调节你传入进来的值的强度 就是相当于你第一个接口插进来的一个强度权重
可以通过修改裁剪区域的值来裁剪你第一个接口传入的图片的大小
<img width="2025" height="1569" alt="Blend" src="https://github.com/user-attachments/assets/eca41bee-f403-485f-b614-538c3155d5ee" />

# Shape 形状（生成形状） 节点
好用啊 好用节点
这个是万物之源
Shape节点可以通过调整 *样式* 来绘制不同的图形 里面形状老多了 常用的就就是半圆 半钟 方形 胶囊等等等
可以通过scale来改变图形的大小 通过Size来调整图形的尺寸 
可以通过Tilling(平铺/重复)来调节生成物体的一个数量 可以自己研究一下 Tilling越大 同一个形状同屏的数量就越多 
<img width="2535" height="1509" alt="Shape" src="https://github.com/user-attachments/assets/b28021fb-5c31-4226-acf9-88a071dc218a" />
  
# Transform 2D 2D变形节点  
依旧万恶之源
我这里掏了一个简单的的螺丝钉制作成品 不想一个个截图了
使用变形节点可以很明显看见 我把一个Shape节点的图片从竖着的放平了 这样就 没错他就是干这个的
你还可以通过在2D预览图里面进一步调整 可以给他放大缩小
<img width="2553" height="1599" alt="Transform_2D" src="https://github.com/user-attachments/assets/9c5d40dd-789e-4acb-ad64-f54423b37ae3" />
比较需要注意的一点是这里的编辑模式
你可以通过点击编辑模式右边的蓝色的图形 把他调整成一个叫 绝对 的模式
这样你就可以调整他的一个重复的属性 很多时候你会可能把这个图案拖来拖去 但是边界此时又冒出来一个相同的图形 通过把这个编辑模式修改成 无 这样他就不有重复了 有点像那个OpenGL里面的纹理的感觉
<img width="744" height="243" alt="bianji_Model" src="https://github.com/user-attachments/assets/07f91bad-8c84-4b61-a17a-be690ddd9691" />

# Bevel 斜坡/倒角
这个节点功能是让你的图片边缘进行一个阶梯化与周围像素进行插值 
得出的结果是 你的图片模糊了 并且边界的值逐渐递减
在视觉上会形成一个斜坡 或者说一个倒角
实际反馈在3D视图里面会更加明显 
 这个可以通过调整 *距离* 来调整你边缘的模糊程度 
 这里面的“Coner type” 可以用于调整边缘拐角的形状 分别是尖锐的三角形 和普通的圆形   
 Bevel 的Smooth参数的作用是在按照Distance 进行插值后对边缘进行一定的模糊处理 他只能让边缘的过渡更加柔和一些
他和Bevel Smooth 不一样 这是两个完全不同的概念
<img width="2238" height="1599" alt="Bevel" src="https://github.com/user-attachments/assets/6f1086fe-dc7d-4a3f-b586-05eec1630bad" />

# Level 色阶
可以用来调整黑白灰的权重比例 以及黑白值的上限和下限 
上面用来调整黑白灰值在画面中所占的比例
下面用来调整黑白值的上下限
适用于需要对物体的边界进行一定的限定的情景 用的也是蛮多的
<img width="1959" height="1560" alt="Color_Levels" src="https://github.com/user-attachments/assets/c8a5cf0b-83a2-46e1-9b49-0b4c82a71419" />


# Curve 曲线
常常用于调整某一区域的值
可以通过曲线来选中某一种值的范围 对他进行提升或者降低
也就是会使得灰度变白or黑 
反映在法线或者高度上面就是升高或者降低
适合需要比较精确控制物体的某一部分的值的地方
<img width="1959" height="1560" alt="Curve" src="https://github.com/user-attachments/assets/bfb914e4-eb78-4d1a-ac20-4ed8dc87b52c" />


# dot 点节点
这个节点可以参考 Unity 里面的 Register节点和 UE里面的Name节点
都是可以起到一个一个节点复用的功能
可以给点节点命名 
左边的节点是传入变量值，然后命名完后就可以拿去用了 
在输入门户可以选择你接入的点节点名称 然后用右边的接口输出
<img width="2157" height="1299" alt="Dot" src="https://github.com/user-attachments/assets/714a7d41-c665-4c54-8bb4-f5d20f23ef16" />



# Tile sampler grayscale 瓷砖节点
这个是比较常用的节点 可以生成很多重复的图形 与Shape相比 它可以接入不同的Pattern 甚至是直接用Shape接入，然后将你传入的Shape作为Pattern 来绘制图形

主要操控实例参数里面的X\Y amount Pattern Size Position Rotation 以及Color参数进行对应目标的绘制
<img width="1962" height="1281" alt="Sampler_grayscale" src="https://github.com/user-attachments/assets/87aa3a0f-a4b9-425e-be34-6e3507052c8a" />

在Pattern中可以调整样式（你绘制大量重复图形的形状）
选择 Pattern in 你就可以不断重复绘制你传入的图形 画啥都可以  通过调整Pattern input Number 可以传入多种不同的图形
他会把你传入的两个图形随机分布在绘制区域内部 这一点可以通过只传入一种参数来看见 
如图所示：
<img width="2091" height="1440" alt="Pattern_Input0" src="https://github.com/user-attachments/assets/46caa125-822c-4721-b0de-d1c68a5cfba8" />
<img width="2046" height="1491" alt="Pattrtn_Input" src="https://github.com/user-attachments/assets/be8bd879-1ab3-444c-a24e-1c93b4bdc1d1" />

在Size里面可以调控每个图案的大小，他们是可以超过边界的 调太大了可能会崩坏
可以通过Size Random 来使得每个图案有随机的大小
其他选项用的不是很多

然后是 *Position*
这些参数也差不多和Size一样
可以通过位移来调整图案的整体偏移

*Rotation*
这个可以用来调整图案整体的旋转角度
用Random 的话就是随机旋转图案的角度

*Color*
用来调整颜色的大小的 
在灰度图里面给上随机就会使值发生0-1的变化
调整全局不透明度也就是字面意思 所有的图案一起进行调整
还有一个背景颜色调整，这个是用来调整那些除了绘制的图案以外的区域的颜色


# Height Blend
这个也蛮重要的
常常用于在做好的Shape图片上进行进一步的配置图案
比如说在画好了一些板子 你要往上面加螺丝钉 就可以用Height Blend
通过调整offset的值来增加图形的高度 增大两者的对比 让下面的灰度值减小 让在上面的灰度值增大 
通过Contrast值可以将上面的物体的边缘进行一个收缩 调整物体边缘对比度
<img width="2043" height="1305" alt="Height_Blend" src="https://github.com/user-attachments/assets/c0987d29-cd48-406a-8e38-2295d8dbe48f" />


# Normal 法线
字面意思 把灰度值转化成法线形式
在3D图中预览法线效果的时候需要使用这个节点把对应的灰度图转化成法线
比较需要注意的点是需要选择Direct X或者OpenGL的法线贴图格式以及法线强度
<img width="1878" height="1214" alt="Normal" src="https://github.com/user-attachments/assets/7e2da68f-b7a0-491d-a594-9fa048849f78" />


# Normal Combine 法线融合
字面意思 可以理解为为法线之间的加法 
需要处理多个法线贴图的融合就用这个
可以调整合成的质量  没啥好说的
<img width="2016" height="1506" alt="Normal_Combine" src="https://github.com/user-attachments/assets/71ce4eb6-87d6-4c6b-add6-496f6778f494" />



# Normal_Intensity 法线强度
觉得目前的法线的强度（突出度）不够的话可以用这个
没啥好说的
<img width="2073" height="1545" alt="Normal_Intensity" src="https://github.com/user-attachments/assets/a5396307-55a5-430d-93ae-f4cbb3041d38" />

# Invert grayscale 反转灰度
字面意思 把你的灰度值反转 
可以在Invert里面勾选是否反转（True or False）
<img width="1908" height="1599" alt="Invert" src="https://github.com/user-attachments/assets/aedcab08-1044-45cd-a985-28eb79b66b65" />


# 统一颜色 
没啥好说的 就是一个色块 
它支持用灰度或者纯粹的egb色彩来进行颜色调整
常常用于搭配其他的节点 比如遮罩节点之类的一块使用
<img width="2160" height="1599" alt="Color" src="https://github.com/user-attachments/assets/0fda5ffc-c43a-4e07-8bbe-1b1006a56d82" />


常用的节点差不多就这些以后进一步发展再继续更新

  







