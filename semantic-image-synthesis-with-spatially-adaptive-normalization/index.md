# Semantic Image Synthesis with Spatially-Adaptive Normalization




**SPADE 为一个新颖的网络归一化层。**



![](https://gitee.com/shilongshen/image-bad/raw/master/20200728084859.png)

SPADE和AdaIN十分的相似，不同的是利用了batch norm进行归一化，类似的使用segmentation mask来提取调剂参数。这能够更好的保存segmentation mask的语义信息。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200728085306.png)

其中$\gamma_{c,y,x}^i(m)$和$\beta_{c,y,x}^i(m)$是归一化层的调剂参数，它与输入语义图有关并会随着位置$(y,x)$变化



#### SPADE生成器网络结构

![](https://gitee.com/shilongshen/image-bad/raw/master/20200728085611.png)



### why does the SPADE work better?

传统的生成器以segmentation mask（有不同的标签）直接作为网络的输入，经过卷积层和归一化层处理。无论输入的segmentation mask的标签的怎样的，当卷积层的输出经过归一化层处理后，其均值为0，因此其语义信息就会丢失。

而在SPADE中，语义图经过卷积操作映射成为调剂参数，而没有经过归一化层处理。仅仅只有上一层的激活值被归一化了。因此SPADE能够更好的保存语义信息。


