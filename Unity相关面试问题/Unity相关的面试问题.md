# Unity相关的面试问题（取自牛客网面经）

## 1.骨骼动画原理、IK原理

骨骼动画是进一步的动画类型，原理构成极其简单，但是解决问题极其有优势。将模型分为骨骼Bone和蒙皮Mesh两个部分，其基本的原理可以阐述为：模型的骨骼可分为基本多层父子骨骼，在动画关键帧数据的驱动下，计算出各个父子骨骼的位置，基于骨骼的控制通过顶点混合动态计算出蒙皮网格的顶点。在骨骼动画中，通常包含的是骨骼层次数据，网格Mesh数据， 网格蒙皮数据Skin Info和骨骼的动画关键帧数据。



“**IK是Inverse Kinematic的缩写，也就是反向动力学。是根据骨骼的终节点来推算其他父节点的位置的一种方法。比如通过手的位置推算手腕、胳膊肘的骨骼的位置。**”
 “没错，那你能想到一些适用的场景么？”
 “嗯。。。我想想：**比如角色需要拿各种不同的东西，让角色的手能符合各种不同的东西的握持位置，这样就不用针对每种不同的东西单独制作动画了！**”
 “非常棒！这是IK最常见的一种用途。**其他的用途其实还有比如：角色的头的旋转，这样可以和你视角的方向一致。角色的脚的位置，这样可以让角色踩在地面跟贴合。**”
 “对对对，我只想到手了。那还有其他的么？”
 “**Unity中IK能设置的部位就是5个，分别是：头、左右手、左右脚。**所以没有其他部位的IK了，我们常见的其实也都是这些"。

## 2.Unity里的动作是如何做平滑混合的

首先准备好美术资源 

1角色模型fbx，不带动作信息，只有模型和蒙皮和骨骼信息，max导出时要导出avatar

2单独的动作文件 run.anim和attack.anim

制作animator

1创建animator controller动画控制器，挂到角色的animator组件里

2双击animator controller打开animator面板，新建uplayer和downlayer两个层，用来做上下半身的混合

3选中uplayer，把attack.anim拖进去，选中downlayer，把run.anim拖进去

4新建up和down两个avatar mask，用来标识关连的骨骼

5up.mask的transform mask里 use skeleton from 选中角色的avatar，在import skeleton下选中角色骨骼中的上半身所有骨骼，(选中上半身根节点，子节点会自动选中)

6down.mask的transform mask里 use skeleton from 选中角色的avatar，在import skeleton下选中角色骨骼中的下半身所有骨骼，(选中上半身根节点，子节点会自动选中)

7把up.mask和down.mask分别给uplayer和downlayer

这样当uplayer和downlayer的weight为1时，角色就会上半身播放攻击动作，下半身播放run动作

其它说明

1base layer里可以建立一些基本动作，用来正常动作之间的切换，通过设置parameters来控制动作切换,

2下层的layer用做动作混合，建立不同的avator mask，关联不同的骨骼，给不同的layer，不同的layer放置不同的动——通过设置layer的weight来控制各个层的混合或停用

layer属性功能

weight  动作的控制权，越下层的layer的weight高于上层layer的weight的控制权

mask 使用的avatar mask

blending override 覆盖上层  additive叠加上层

svnc 勾选后，可以选择同步到其它layer的，把同步的layer全复制过来后在可以添加自己的