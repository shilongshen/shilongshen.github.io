# Fashion Editing with Adversarial Parsing Learning




该文为人物的时尚编辑

网络包含三个部分：

-  Free-form Parsing Network

-  Parsing-aware Inpainting Network

- Attention Normalization Layers

  ---

  # Free-form Parsing Network

  - 给定不完整的人体语义分析图以及任意的草图和笔画，能够合成完整的人体语义分析图。

  网络结构 ： U-net 

  ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201004091025.png)

  输入：

  1. an incomplete parsing map,

        			2.   a binary sketch that describes the structure of the removed region
        			3.   a noise sampled from the Gaussian distribution,
        			4.   sparse color strokes 
        			5.   a mask.

  注意：相同的incomplete parsing map和不同的sketch和strokes能够合成不同的parsing map,这意味这parsing generation model是可控的。

#  Parsing-aware Inpainting Network

网络结构：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201004091111.png)



输入：

1. Incomplete image + composed mask  ->  partial conv encoder
2. Synthesized parsing  -> standard conv encoder 



# Attention Normalization Layers

网络结构：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201004093209.png)



数学表达式：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201004093521.png)



其中

​		$\alpha$：leaned attention map

​		$\beta$ : bias 

Sketch+Color+Noise  ->Conv->Conv ->Sigmoid->  leaned attention map

Sketch+Color+Noise  ->Conv->Conv-> bias



Attention normalization layers有效性的解释：

`The effectiveness of ANLs is due to their inherent characteristics. Similar to SPADE, ANLs also can avoid washing away semantic information in activations, since the attention map and bias are spatially-varying. Moreover, the multi-scale ANLs can not only adapt the various scales of activations during upsampling but also extract coarse-to-fine semantic information from external data, which guide the fashion editing more precisely.`

- avoid washing away semantic information in activations  - > attention map and bias are spatially-varying
- can not only adapt the various scales of activations during upsampling but also extract coarse-to-fine semantic information from external data   ->多尺寸的激活值，以及从“粗到细”的语义信息



与SPADE的比较：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201004095746.png)






