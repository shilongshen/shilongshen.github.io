# pytorch学习笔记


tensor的一些性质   

~~~python
import torch
x=torch.tensor(2.5)
print(x.dtype)
>>torch.float32#torch默认的数据类型为float32

~~~

数据类型包括：

<img src="https://gitee.com/shilongshen/image-bad/raw/master/20200702083022.png"  />

~~~python
#如何将tensor的数据类型转换？
#在Tensor后加 .long(), .int(), .float(), .double()等即可
x.half()
>>tensor(2.5000, dtype=torch.float16)
~~~

~~~python
#将numpy.ndarray转换为tensor:
    a=numpy.array([1,2,3])
    a.dtype
    >>dtype('int32')
    t=torch.from_numpy(a)
    t
    >>tensor([1, 2, 3], dtype=torch.int32)
~~~

~~~python
#torch.zeros_like(input)->生成一个与input相同size的零矩阵,同理还有torch.ones_like(input);torch.randn_like(input)
input=torch.ones(2,3)
torch.zeros_like(input)
>>tensor([[0., 0., 0.],
        [0., 0., 0.]])
~~~

~~~python
#torch.arange() 和torch.range()的区别
#Note that arange generates values in [start; end), not [start; end].
torch.arange(1,4)
>>tensor([1, 2, 3])
torch.range(1,6)
>>tensor([1., 2., 3., 4., 5., 6.])
~~~

~~~python
#torch.full(size,fill_value)
#Returns a tensor of size size filled with fill_value.常用于GAN训练中的真假标签赋值
torch.full((2,3),3.1415)
>>tensor([[3.1415, 3.1415, 3.1415],
        [3.1415, 3.1415, 3.1415]])
torch.full((2,),3.14)#注意size的表达形式（*，*）或者是（*，*，*）；不能使用（*）
>>tensor([3.1400, 3.1400])
~~~

~~~python
#torch.cat(tensor,dim)
#将输入的张量在指定的维度上进行叠加
x=torch.ones(2,2,)
torch.cat((x,x),0)
>>tensor([[1., 1.],
        [1., 1.],
        [1., 1.],
        [1., 1.]])
torch.cat((x,x),1)
>>tensor([[1., 1., 1., 1.],
        [1., 1., 1., 1.]])

#torch.split(tensor,split_size,dim)
#将输入张量在指定维度上分为特定的份
a=torch.randn(4,4)
a
>>tensor([[-1.0159,  0.2249, -0.0083,  0.4495],
        [-0.5427,  0.0253,  0.3100, -1.8563],
        [ 0.6721, -0.5261, -0.1104,  1.5312],
        [ 0.4096, -1.4238,  0.5939,  0.2700]])
b=torch.split(a,2,0)
b
>>tensor([[-1.0159,  0.2249, -0.0083,  0.4495],
        [-0.5427,  0.0253,  0.3100, -1.8563],
        [ 0.6721, -0.5261, -0.1104,  1.5312],
        [ 0.4096, -1.4238,  0.5939,  0.2700]])


~~~

~~~python
#torch.reshape(input,shape)
#将input重构为shape的形状
a=torch.randn(4,4)
a=torch.reshape(a,(2,8))
a.shape
>>torch.Size([2, 8])

a=torch.reshape(a,(-1,))
#当某一维度为-1时，这一维度的形状不用管，根据input和另一维度计算形状。当另一维度的表达式没有时，就讲input按照“某一维度为-1”的维度排列input
a.shape
>>torch.Size([16])

a=torch.reshape(a,(-1,8))
a.shape
torch.Size([2, 8])
~~~

~~~python
#torch.squeeze(input,dim)
#将输入张量维度为1的size去掉
x=torch.randn(2,1,3,1,4)
x.shape
>>torch.Size([2, 1, 3, 1, 4])
y=torch.squeeze(x)#如果不指定维度就会将所有维度为1的size去掉，如果加了维度，只会在指定维度上
y.shape
>>torch.Size([2, 3, 4])
#torch.unsqueeze(input,dim)
#在指定维度插入size=1
x=torch.randn(4)
x.shape
>>torch.Size([4])
torch.unsqueeze(x,0).shape
>>torch.Size([1, 4])
torch.unsqueeze(x,1).shape
>>torch.Size([4, 1])
~~~



#### pytorch 自动微分

~~~python
#autograd 包是 PyTorch 中所有神经网络的核心.该 autograd 软件包为 Tensors 上的所有操作提供自动微分
#####动态计算图（DCG）->由autograd动态生成。这个图的叶节点是输入张量，根节点是输出张量。梯度是通过跟踪从根到叶的图形，并使用链式法则将每个梯度相乘来计算的。

~~~

~~~python
tensor(data, dtype=None, device=None, requires_grad=False) -> Tensor
 
#参数:
#    data： (array_like): tensor的初始值. 可以是列表，元组，numpy数组，标量等;
#    dtype： tensor元素的数据类型
#    device： 指定CPU或者是GPU设备，默认是None
#    requires_grad：是否可以求导，即求梯度，默认是False，即不可导的
#	注意data必须为浮点数才可导
~~~



![](https://gitee.com/shilongshen/image-bad/raw/master/20200702113424.png)

> data:输入数据
>
> requires_grad：这个成员(如果为true)开始跟踪所有的操作历史，并形成一个用于梯度计算的后向图。
>
> grad: **grad保存梯度值**。如果requires_grad 为False，它将持有一个None值。即使requires_grad 为真，它也将持有一个None值，除非从其他节点调用.backward()函数。例如，如果你对out关于x计算梯度，调用out.backward()，则x.grad的值为∂out/∂x。
>
> grad_fn：这是用来计算梯度的向后函数。
>
> 在调用`backward()`时，只计算`requires_grad`和`is_leaf`同时为真的节点的梯度。

当打开 `requires_grad = True`时，PyTorch将开始跟踪操作，并在每个步骤中存储梯度函数，如下所示：

![](https://gitee.com/shilongshen/image-bad/raw/master/20200702161327.png)

> Backward函数实际上是通过传递参数(默认情况下是1x1单位张量)来计算梯度的，它通过Backward图一直到每个叶节点，每个叶节点都可以从调用的根张量追溯到叶节点。然后将计算出的梯度存储在每个叶节点的.grad中。请记住，在正向传递过程中已经动态生成了后向图。backward函数仅使用已生成的图形计算梯度，并将其存储在叶节点中。

~~~python
#参考：https://blog.csdn.net/qq_27825451/article/details/89393332
###求导的核心函数-torch.autograd.backward
backward(gradient=None, retain_graph=None, create_graph=False)
#在pytorch中，默认是标量对标量求导或者是标量对向量求导
#向量对向量求导->当使用向量对向量求导时需要设置gradient的参数
#一个计算图在进行反向求导之后，为了节省内存，这个计算图就销毁了。 如果你想再次求导，就会报错。那怎么办呢？遇到#这种问题，我们可以通过设置 retain_graph=True 来保留计算图
~~~

~~~python
#标量对标量求导
x=torch.tensor(3.0,requires_grad=True)
y=torch.pow(x,2)
 
#判断x,y是否是可以求导的
print(x.requires_grad)
print(y.requires_grad)
 
#求导，通过backward函数来实现
y.backward()  
 
#查看导数，也即所谓的梯度
print(x.grad)


~~~

~~~python
#标量对向量求导
#在深度学习中，损失函数一般为标量，即将所有项的损失相加。而输入一般为多维张量
#例:1：
#x->(1,3),w->(3,1)
#输出y是一个标量
#创建一个多元函数，即Y=XW+b=Y=x1*w1+x2*w2*x3*w3+b，x不可求导，W,b设置可求导
X=torch.tensor([1.5,2.5,3.5],requires_grad=False)
W=torch.tensor([0.2,0.4,0.6],requires_grad=True)
b=torch.tensor(0.1,requires_grad=True)
Y=torch.add(torch.dot(X,W),b)
 
 
#判断每个tensor是否是可以求导的
print(X.requires_grad)
print(W.requires_grad)
print(b.requires_grad)
print(Y.requires_grad)
 
 
#求导，通过backward函数来实现
Y.backward()  
 
#查看导数，也即所谓的梯度
print(W.grad)
print(b.grad)
 
'''运行结果为：
False
True
True
True
tensor([1.5000, 2.5000, 3.5000])
tensor(1.)
'''
#例子2：
'''
x 是一个（2,3）的矩阵，设置为可导，是叶节点，即leaf variable
y 是中间变量,由于x可导，所以y可导
z 是中间变量,由于x，y可导，所以z可导
f 是一个求和函数，最终得到的是一个标量scaler
'''
 
x = torch.tensor([[1.,2.,3.],[4.,5.,6.]],requires_grad=True)
y = torch.add(x,1)
z = 2*torch.pow(y,2)
f = torch.mean(z)  
~~~

x,y,z的关系如下：

<img src="https://gitee.com/shilongshen/image-bad/raw/master/20200702171745.png" style="zoom: 67%;" />

~~~python
#这样就可以手动求出f对于x11,x12,x13,x21,x22,x23的导数了
#验证：
print(x.requires_grad)
print(y.requires_grad)
print(z.requires_grad)
print(f.requires_grad)
print('===================================')
f.backward()
print(x.grad)
 
'''运行结果为：
True
True
True
True
===================================
tensor([[1.3333, 2.0000, 2.6667],
        [3.3333, 4.0000, 4.6667]])
'''
~~~

~~~python
#向量对向量求导
#例子1：
'''
x 是一个（2,3）的矩阵，设置为可导，是叶节点，即leaf variable
y 也是一个（2,3）的矩阵，即
y=x^2+x (x的平方加x)
实际上，就是要y的各个元素对相对应的x求导
'''
 
x = torch.tensor([[1.,2.,3.],[4.,5.,6.]],requires_grad=True)
y = torch.add(torch.pow(x,2),x)
 
gradient=torch.tensor([[1.0,1.0,1.0],[1.0,1.0,1.0]])#参数都为1
 
y.backward(gradient)
 
print(x.grad)
 
'''运行结果为：
tensor([[ 3.,  5.,  7.],
        [ 9., 11., 13.]])
'''

#例子2：
x = torch.tensor([1.,2.,3.],requires_grad=True)
y = torch.pow(x,2)
 
gradient=torch.tensor([1.0,1.0,1.0])
y.backward(gradient)
print(x.grad)
'''得到的结果：
tensor([2., 4., 6.])   这和我们期望的是一样的
'''  
gradient=torch.tensor([1.0,0.1,0.01])
 
'''输出为：
tensor([2.0000, 0.4000, 0.0600])
'''
#从结果上来看，就是第二个导数缩小了十倍，第三个导数缩小了100倍，这个倍数和gradient里面的数字是息息相关的。
#dy/dx=0.1*dy1/dx+1.0*dy2/dx+0.0001*dy3/dx
#总结：gradient参数的维度与最终的函数y保持一样的形状，每一个元素表示当前这个元素所对应的权

~~~

------



#### 神经网络的构建（1）

~~~python
import torch
import torch.nn as nn#用于定义神经网络模型
import torch.nn.functional as f#用于激活函数的调用
import torch.optim as optim#用于更新梯度
~~~



- [ ] 定义神经网络的模型

  ~~~python
  class Net(nn.Module):
      def __init__(self):
          super(Net,self).__init__()
          self.conv1=nn.Conv2d(1,6,5)
          self.conv2=nn.Conv2d(6,16,5)
          self.fc1=nn.Linear(16*5*5,120)
          self.fc2=nn.Linear(120,84)
          self.fc3=nn.Linear(84,10)
      def forward(self,x):
          x=f.max_pool2d(f.relu(self.conv1(x)),(2,2))
          x=f.max_pool2d(f.relu(self.conv2(x)),2)
          x=x.view(1,-1)#将特征重构
          x=f.relu(self.fc1(x))
          x=f.relu(self.fc2(x))
          x=self.fc3(x)
          return(x)
  ~~~

  ```
  Net(
    (conv1): Conv2d(1, 6, kernel_size=(5, 5), stride=(1, 1))
    (conv2): Conv2d(6, 16, kernel_size=(5, 5), stride=(1, 1))
    (fc1): Linear(in_features=400, out_features=120, bias=True)
    (fc2): Linear(in_features=120, out_features=84, bias=True)
    (fc3): Linear(in_features=84, out_features=10, bias=True)
  )
  ```

  

- [ ] 前向传播

  ~~~python
  net=Net()
  input=torch.rand(1,1,32,32)
  output=net(input)
  ~~~

- [ ] 定义损失函数

  ~~~python
  target=torch.rand(1,10)#label，根据实际定义
  criterion=nn.MSELoss()	#定义损失函数
  ~~~

- [ ] 定义更新方式，梯度清零，计算损失损失函数，计算梯度,梯度更新（反向传播）

  ~~~python
  opt=optim.SGD(net.parameters(),lr=0.01)#定义更新方式
  opt.zero_grad()#梯度清零，清空现存梯度，否则梯度将会叠加
  loss=criterion(output,target)	#计算损失函数
  loss.backward()#计算梯度
  opt.step()#梯度更新
  ~~~



#### 神经网络的构建（2）

~~~python
import torch
import torch.nn
import torch.nn as nn
import torch.optim as optim

class Net(nn.Module):
    def __init__(self,D_in,H,D_out):
        super(Net,self).__init__()
        self.fc1=nn.Linear(D_in,H)
        self.fc2=nn.Linear(H,D_out)

    def forward(self,x):
        h_relu=self.fc1(x)
        y_pred=self.fc2(h_relu)
        return y_pred

N,D_in,H,D_out=64,1000,100,10

x=torch.rand(N,D_in)#定义输入数据
y=torch.rand(N,D_out)#定义labels

#实例化Net
net=Net(D_in,H,D_out)
#定义损失函数
criterion=nn.MSELoss()
#定义更新方式
opt=optim.SGD(net.parameters(),lr=0.001)


#进行前向传播、损失函数计算、梯度清零、梯度计算、梯度更新
for i in range(500):
    y_pred=net(x)#前向传播
    loss=criterion(y_pred,y)#损失函数计算、
    print(i,loss.item())

    opt.zero_grad()#梯度清零
    loss.backward()#梯度计算
    opt.step()#梯度更新





~~~



#### 数据加载和处理1

使用torchvision包括了常用的数据集类（Dataset）和转换（transforms），还有一个常用的数据集类ImageFolder;数据加载器DataLoader

~~~python
import torch
from torchvision import transforms, datasets
#from torch.utils.data.dataloader import DataLoader

#transforms的功能为对输入数据进行预处理，包括了Rescale：缩放图片；RandomCrop:对图片进行随机剪裁；ToTensor:把numpy格式的图片转换为tensor格式的图片
#transforms.Compose表示将这几类转换结合，进行组合转换

#另外transforms.Normalize的工作原理如下例所示：
#    transform.ToTensor(),
#    transform.Normalize((0.5,0.5,0.5),(0.5,0.5,0.5))
#那transform.Normalize()是怎么工作的呢？以上面代码为例，ToTensor()能够把灰度范围从0-255变换到0-1之间，而后面的transform.Normalize()则把0-1变换到#(-1,1).具体地说，对每个通道而言，Normalize执行以下操作：
#image=(image-mean)/std
#其中mean和std分别通过(0.5,0.5,0.5)和(0.5,0.5,0.5)进行指定。原来的0-1最小值0则变成(0-0.5)/0.5=-1，而最大值1则变成(1-0.5)/0.5=1.
#
data_transform = transforms.Compose([
transforms.RandomSizedCrop(224),
transforms.RandomHorizontalFlip(),
transforms.ToTensor(),
transforms.Normalize(mean=[0.485, 0.456, 0.406],
std=[0.229, 0.224, 0.225])
])

#ImageFolder是一个数据集类，它假定数据集是以如下方式构造的：
#root/ants/xxx.png
#root/ants/xxy.jpeg
#root/ants/xxz.png
#.
#.
#.
#root/bees/123.jpg
#root/bees/nsdf3.png
#root/bees/asd932_.png
#

hymenoptera_dataset = datasets.ImageFolder(root='hymenoptera_data/train',
transform=data_transform)

#Dataloader 作为迭代器，每次产生一个 batch 大小的数据
#
dataset_loader = torch.utils.data.DataLoader(hymenoptera_dataset,
batch_size=4, shuffle=True,
num_workers=4)
~~~

~~~python
#Dataset和DataLoader的区别
#
#
#在pytorch中用一个类来抽象的表示数据集，即数据集类，常用的数据集类为Dataset和ImageFloder
#DataLoader做为一个迭代器，每次产生一个batch大小的数据。

#
#
#举例：
from torchvision import transforms, datasets
from torch.utils.data.dataloader import DataLoader
train_loader = DataLoader(
    datasets.MNIST('../data', train=True, download=True,
                   transform=transforms.Compose([
                       transforms.ToTensor(),
                       transforms.Normalize((0.1307,), (0.3081,))
                   ])),
    batch_size=batch_size, shuffle=True)
#datasets.MNIST()是一个torch.utils.data.Datasets对象，batch_size表示我们定义的batch大小（即每轮训练使用的批大小），shuffle表示是否打乱数据顺序（对于整个datasets里包含的所有数据）
#
#一般如何加载DataLoader中的数据进行训练以及测试？
#
epochs=10#定义epochs次数
for epoch in range(epochs)：
	for batch_idx, (data, label) in enumerate(train_loader):
    #batch_idx表示加载的第几批数据（数据以batch的形式进行加载，每一批中有每次产生一个batch大小的数据）、
    #data表示这一批中的训练数据，label表示相应data的标签
        optimizer.zero_grad()
        .
        .
        .
     
        

~~~

#### 数据的加载和处理2 -- 自定义数据集类

假设我们网络有两个输入，这两个输入分别存放在不同的文件夹，如何同时读取两个文件加中的文件进行训练呢？

- 自定义数据集类必须继承torch.utils.data.Dataset，必须覆写__getitem__       __len__
- 并覆写__len__ 实现len(dataset) 返还数据集尺寸
- __getitem__用来获取索引数据
- 在__init__ 中读取文件内容
- 在__getitem__中按照索引读取图片

~~~python
#自定义数据集类
#torch.utils.data.Dataset 是表示数据集的抽象类，因此自定义数据集应该继承Dataset并覆盖以下方法__len__ 实现len(dataset) 返还数据集尺寸。__getitem__用来获取索引数据

from torch.utils.data import Dataset
from pathlib import Path
import os.path

class FlatFolderDataset(Dataset):
    def __init__(self, root, transform):
        super(FlatFolderDataset, self).__init__()
        self.root = root
        self.paths = list(Path(self.root).glob('*'))#将path路径下的文件以列表的形式存放，在__init__ 中读取文件内容,list存放的是文件的路径
        self.transform = transform

    def __getitem__(self, index):#构造加载一个patch的数据集
        path = self.paths[index]#在__getitem__中按照索引读取图片
        img = Image.open(str(path)).convert('RGB')
        img = self.transform(img)#并进行必要的转换
        return img#返回图像，即训练数据

    def __len__(self):
        return len(self.paths)#可以设置返回的训练集大小

    def name(self):
        return 'FlatFolderDataset'

    
    
~~~

假设我们有来两个文件加分别存放两个训练集，路径分别为content_dir、style_dir





总的代码

~~~python

import torch
from torch.utils.data import Dataset, DataLoader
from pathlib import Path
from PIL import Image
from tqdm import tqdm
from torchvision import transforms
import os.path
import matplotlib.pyplot as plt


class FlatFolderDataset(Dataset):
    def __init__(self, style_root, content_root, transform=None):
        super(FlatFolderDataset, self).__init__()
        self.style_root = style_root
        self.style_paths = list(Path(self.style_root).glob('*'))  # 将path路径下的文件路径以列表的形式存放,list存放的是文件的路径
        self.transform = transform

        self.content_root = content_root
        self.content_paths = list(Path(self.content_root).glob('*'))  # 将path路径下的文件路径以列表的形式存放,list存放的是文件的路径

    def __getitem__(self, index):
        style_path = self.style_paths[index]  # 通过索引的方式获取图片路径
        style_img = Image.open(str(style_path)).convert('RGB')  # 获取图片
        style_img = self.transform(style_img)  # 对图像进行必要的处理

        content_path = self.content_paths[index]  # 通过索引的方式获取图片路径
        content_img = Image.open(str(content_path)).convert('RGB')  # 获取图片
        content_img = self.transform(content_img)  # 对图像进行必要的处理

        return {'style_img': style_img, 'content_img': content_img}

    def __len__(self):
        return len(self.style_paths)

    def name(self):
        return 'FlatFolderDataset'


def train_transform():
    transform_list = [
        # transforms.Resize(size=(512, 512)),
        # transforms.RandomCrop(256),
        transforms.ToTensor(),
        transforms.Normalize(mean=[0.5, 0.5, 0.5],
                             std=[0.5, 0.5, 0.5])

    ]
    return transforms.Compose(transform_list)


train_transform = train_transform()

style_dir = "/home/wag/ssl/kongfupainter/gan-getting-started" + "/monet_jpg"
# style_dataset = FlatFolderDataset(style_dir)  # 加载训练数据集
# print(len(style_dataset))
content_dir = "/home/wag/ssl/kongfupainter/gan-getting-started" + "/photo_jpg"
# content_dataset = FlatFolderDataset(content_dir)  # 加载训练数据集
# print(len(content_dataset))

data_set = FlatFolderDataset(style_dir, content_dir, train_transform)
print(len(data_set))

data_loader = DataLoader(data_set, batch_size=6, shuffle=True)

for i, data in enumerate(data_loader):
    style = data['style_img']
    content = data['content_img']
    # print(style.shape)
    # print(content.shape)
    # print(i)

# print(len(content))

~~~





#### Python中parser.add_argument()的用法

**Python解析命令行读取参数 – argparse模块**
 在多个文件或者不同语言协同的项目中，python脚本经常需要**从命令行直接读取参数**。万能的python就自带了argprase包使得这一工作变得简单而规范。

[参考](https://www.cnblogs.com/arkenstone/p/6250782.html)

[参考](https://www.jb51.net/article/179189.htm)

~~~python
import argparse

parser = argparse.ArgumentParser(description="your script description")  # description参数可以用于插入描述脚本用途的信息，可以为空

parser.add_argument('--verbose', required=True, type=int)  #这种模式用于确保某些必需的参数有输入。 required标签就是说--verbose参数是必需的，并且类型为int，输入别的类型会报错。

parser.add_argument('filename', default='text.txt') #一般情况下会设置一些默认参数从而不需要每次输入某些不需要变动的参数，利用default参数即可实现。

~~~

~~~python
#argparse 是 Python 内置的一个用于命令项选项与参数解析的模块，通过在程序中定义好我们需要的参数，argparse 将会从 sys.argv 中解析出这些参数，并自动生成帮助和使用信息。

#简单示例

#我们先来看一个简单示例。主要有三个步骤：

 #   创建 ArgumentParser() 对象
  #  调用 add_argument() 方法添加参数
   # 使用 parse_args() 解析添加的参数



import argparse
 
parser = argparse.ArgumentParser()
parser.add_argument('--sparse', action='store_true', default=False, help='GAT with sparse version or not.')#action - 命令行遇到参数时的动作，默认值是 False。
parser.add_argument('--seed', type=int, default=72, help='Random seed.')
parser.add_argument('--epochs', type=int, default=10000, help='Number of epochs to train.')
 
args = parser.parse_args()
 
print(args.sparse)
print(args.seed)
print(args.epochs)
>>
False
72
10000
~~~

~~~python
parser.add_argument('--sparse',action='store_true',help='GAT')#默认为False
args=parser.parse_args()
print(args.sparse)
>>
False
~~~



#### Pandas中loc和iloc函数用法

~~~python
import numpy as np
import pandas as pd

data = pd.DataFrame(np.arange(16).reshape(4, 4), index=list('abcd'),
                    columns=list('ABCD'))
print(data)
>>
 A   B   C   D
a   0   1   2   3
b   4   5   6   7
c   8   9  10  11
d  12  13  14  15

print(data.loc['a'])
A    0
B    1
C    2
D    3
Name: a, dtype: int64
        
print(data.iloc[1])
A    4
B    5
C    6
D    7
Name: b, dtype: int64
        
#loc函数：通过行索引 “Index” 中的具体值来取行数据（如取"Index"为"A"的行）
#iloc函数：通过行号来取行数据（如取第二行的数据）

#利用loc、iloc提取列数据
print(data.loc[:,['A']]) # #取'A'列所有行，多取几列格式为 data.loc[:,['A','B']]
>>
    A
a   0
b   4
c   8
d  12
print(data.iloc[:,[0]])
>>
  	A
a   0
b   4
c   8
d  12


for i in range(4):
    print(data.iloc[i]['A'],data.iloc[i]['B'])
>>
0 1
4 5
8 9
12 13
~~~

#### __init__()与__getitem__()及__len__()的使用

[参考](https://zhuanlan.zhihu.com/p/87786297)



**自定义类，实现三个魔法方法并分析其作用**

首先给出类的结构，定义一个Fun类，并实现上述的三个魔法方法如下：

```python
class Fun:
    def __init__(self, x_list):
        pass
    
    def __getitem__(self, idx):
        pass
    
    def __len__(self):
        pass
```



**init()函数实现**

__init__（）方法负责初始化，根据自己的经验，必须要对初始化函数的参数进行类型检查，避免不必要的麻烦。self表示这个方法是属于类实例的，而不是类的。（个人认为，python不像是java或者C++那样，它崇尚“万物皆引用”，如果不小心就会搞错对象类型），初始化函数如下：

~~~python
def __init__(self, x_list):
        """ initialize the class instance
        Args:
            x_list: data with list type
        Returns:
            None
        """
        if not isinstance(x_list, list):
            raise ValueError("input x_list is not a list type")
        self.data = x_list
        print("intialize success")

#函数中的self.data是非常重要的实例属性，后面的两个方法的意义实际上就是来获取这个实例属性的信息。

#那么__init__函数是什么时候被调用的呢？其实它是在对象创建时被解释器自动调用的，现在创建一个实例试一试：

fun = Fun(x_list=[1, 2, 3, 4, 5])

#终端会输出下面的字符串，说明__init__()函数被调用了：

>>intialize success




~~~

__init__相当于java中的构造方法：

~~~java
class Student{
    private String name;//field
    private int age;//field,相当于python中的self.data
    
    public Student(String name,int age){//java中的构造方法用于实例初始化
        this.name=name;
        this.age=age;
    }
       
}

public class Main{
	public static void main(String[] args){
        Student s1=new Student("xiaohong",18);//构建实例时用于初始化
    }
}

~~~

---

__getitem__()函数的实现

对于value[0]这个使用方框索引，来获取序列中的指定位置的值的方法，相信大家一定很熟悉。但是如果自己定义一个类，能否使用索引的方式获取这个类实例的属性值？答案就是使用__getitem__()方法（这个方法的作用不止于此，以后笔记中关于迭代器和for语法中还要用到它）。

- **使用索引的方式来获取类实例的属性值**

```python
def __getitem__(self, idx):
        print("__getitem__ is called")
        return self.data[idx]
```

__getitem__()方法接收一个idx参数，这个参数就是fun[2]中的2，也就是自己给的索引值。当**fun[2]**这个语句出现时，就会触发__getitem__(self,idx)，这个方法就会返回self.data[2]。一个很自然的问题就是，self.data[2]这里为什么使用了[2]这种索引方式？这里应该由于self.data是list类型，[2]触发的是list的__getitem__方法。测试一下：

```python
print(fun[2])
```

终端会输出下面两条，说明__getitem__被调用了:

```python
__getitem__ is called
3
```



__getitem__相当于java中的javabean的读方法（getter）

~~~java
class Student{
    private String name;//field
    private int age;//field,相当于python中的self.data
    
    public Student(String name,int age){//java中的构造方法用于实例初始化
        this.name=name;
        this.age=age;
    }
    
    public String getName(){//读取实例field
        return this.name;
    }
       
}

public class Main{
	public static void main(String[] args){
        Student s1=new Student("xiaohong",18);//构建实例时用于初始化
        System.out.print(s1.getName);
    } 
}

~~~

---

__len__函数实现

python包含一个内置方法len（），使用它可以测量list、tuple等序列对象的长度，如下：

```python
name = ["Li Hua", "Daming", "Honghong"]
print(len(name))
```

终端将会输出：3

- **测量实例属性的长度**

那么如果自定义一个类，是不是也能使用len（）函数测量某个实例属性的长度呢？只要实现__len__（）就能实现这个功能，如下：

```python
def __len__(self):
        print("__len__ is called")
        return len(self.data)
```

这时就能使用len()函数测量self.data的长度了，试一下：

```python
print(len(fun))
```

终端输出下面两条，说明__len__()被自动调用了：

```python
__len__ is called
5
```

完整代码：

~~~python
class Fun():
    def __init__(self, x_list):
        if not isinstance(x_list, list):
            raise ValueError("input error")
        self.data = x_list
        print("init success")

    def __getitem__(self, item):
        print("__getitem__ is called")
        return self.data[item]

    def __len__(self):
        print("__len__ is called")
        return len(self.data)


fun = Fun([1, 2, 3])
print(fun[2])
print(len(fun))

#输出为：
#init success
#__getitem__ is called
#3
#__len__ is called
#3

~~~





#### pytorch Module里的children()与modules()的区别

[参考](https://blog.csdn.net/LXX516/article/details/79016980?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

~~~python
m = nn.Sequential(nn.Linear(2,2), 
                  nn.ReLU(),
                 nn.Sequential(nn.Sigmoid(), nn.ReLU()))

m.modules()
>>
[Sequential(
   (0): Linear(in_features=2, out_features=2)
   (1): ReLU()
   (2): Sequential(
     (0): Sigmoid()
     (1): ReLU()
   )
 ), Linear(in_features=2, out_features=2), ReLU(), Sequential(
   (0): Sigmoid()
   (1): ReLU()
 ), Sigmoid(), ReLU()]


#用list举例就是：
a=[1,2,[3,4]]
#modules返回
[1,2,[3,4]], 1, 2, [3,4], 3, 4
~~~

#### Numpy中matrix和ndarray的区别

[参考](https://blog.csdn.net/wzyaiwl/article/details/90552935)



#### pytorch并行处理方法

[参考](https://pytorch.org/tutorials/beginner/blitz/data_parallel_tutorial.html)

[参考](https://oldpan.me/archives/pytorch-to-use-multiple-gpus)

[参考](https://pytorch.org/docs/stable/generated/torch.nn.DataParallel.html#torch.nn.DataParallel)

[参考](https://zhuanlan.zhihu.com/p/102697821)

pytorch默认只使用一个GPU进行训练，如果想要使用多个GPU运行，需要使用`nn.DataParallel`

> 首先在前向过程中，你的输入数据会被划分成多个子部分（以下称为副本）送到不同的device中进行计算，而你的模型module是在每个device上进行复制一份，也就是说，输入的batch是会被平均分到每个device中去，但是你的模型module是要拷贝到每个devide中去的，每个模型module只需要处理每个副本即可，当然你要保证你的batch size大于你的gpu个数。然后在反向传播过程中，每个副本的梯度被累加到原始模块中。概括来说就是：DataParallel 会自动帮我们将数据切分 load 到相应 GPU，将模型复制到相应 GPU，进行正向传播计算梯度并汇总。
>
> 注意还有一句话，官网中是这样描述的：
>
> The parallelized `module` must have its parameters and buffers on `device_ids[0]` before running this `DataParallel` module.
>
> 意思就是：在运行此DataParallel模块之前，并行化模块必须在device_ids [0]上具有其参数和缓冲区。在执行DataParallel之前，会首先把其模型的参数放在device_ids[0]上，一看好像也没有什么毛病，其实有个小坑。我举个例子，服务器是八卡的服务器，刚好前面序号是0的卡被别人占用着，于是你只能用其他的卡来，比如你用2和3号卡，如果你直接指定device_ids=[2, 3]的话会出现模型初始化错误，类似于module没有复制到在device_ids[0]上去，这就需要CUDA_VISIBLE_DEVICES设置对程序可见的device

```python
device = torch.device("cuda:0")
model.to(device)#默认只使用一个GPU进行训练
```



~~~python
# CUDA_VISIBLE_DEVICES设置说明，设置device对程序可见

#CUDA_VISIBLE_DEVICES=1 // 仅使用device1 (即卡一)

#CUDA_VISIBLE_DEVICES=0,1 // 仅使用device 0和 device1

#CUDA_VISIBLE_DEVICES="0,1" // 同上, 仅使用device 0和 device1

#CUDA_VISIBLE_DEVICES=0,2,3 // 仅使用device 0, device2和device3

#CUDA_VISIBLE_DEVICES=2,0,3 // 仅使用device0, device2和device3


#设置临时的环境变量
Linux： export CUDA_VISIBLE_DEVICES=2,3
windows:  set CUDA_VISIBLE_DEVICES=2,3
#或者使用
os.environ["CUDA_VISIBLE_DEVICES"] = "2, 3"
#设置上面两行代码后，那么对这个程序而言可见的只有2和3号卡，和其他的卡没有关系，这是物理上的号卡，逻辑上来说其实是对应0和1号卡，即device_ids[0]对应的就是第2号卡，device_ids[1]对应的就是第3号卡。（当然你要保证上面这两行代码需要定义在以下代码之前）
device_ids = [0, 1]
net = torch.nn.DataParallel(net, device_ids=device_ids)


#device为程序可见的0号开始可用的GPU编号
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

#Model为用户的网络模型
model = Model()
if torch.cuda.device_count() > 1:
  model = nn.DataParallel(model)#使用多个GPU并行处理       在全部GPU上训练
  #model = nn.DataParallel(model,device_ids=[0,1,2,3])  可以指定在哪个GPU上训练，这里指定在0,1,2,3号GPU上训练（注意是以list的方式传入的）,这个的作用与设置CUDA_VISIBLE_DEVICES=0,1,2,3是一样的

model.to(device)
#model.cuda()与model.to(device)的效果是一样的
#


input = data.to(device)#输入数据也需要放入device中进行处理


官方解释：
# DataParallel splits your data automatically and sends job orders to multiple
# models on several GPUs. After each model finishes their job, DataParallel
# collects and merges the results before returning it to you.




#在跑程序的时候出现了一个bug提示：AttributeError: 'DataParallel' object has no attribute '**'

#这次参考了：https://github.com/jytime/Mask_RCNN_Pytorch/issues/2

#其中提到，因为使用torch.nn.DataParallel而造成了AttributeError: 'DataParallel' object has no attribute '**'，‘**’为用户自定义的模型中的函数，此时因为使用了DataParallel函数对原本用户定义的model进行了包装，原model变为了DataParallel的一个子模块。

#因此想要再次调用原model中的函数，需要将原本的model.load()函数变为model.module.load()形式。其中load()为我在model中自定义的函数，综上，在model与函数中间加入.module即可。

~~~

小结：

```python
#并行训练
os.environ["CUDA_VISIBLE_DEVICES"] = "2, 3"#指定程序可见GPU
device_ids = [0, 1]
model = torch.nn.DataParallel(m, device_ids=device_ids)#将数据切分到device上
model.to(device)#或者是model.cuda()
```





#### matplotlib 画动态图以及plt.ion()和plt.ioff()的使用

[参考](https://blog.csdn.net/zbrwhut/article/details/80625702)

在使用matplotlib的过程中，常常会需要画很多图，但是好像并不能同时展示许多图。这是因为python可视化库matplotlib的显示模式默认为阻塞（block）模式。什么是阻塞模式那？我的理解就是在plt.show()之后，程序会暂停到那儿，并不会继续执行下去。如果需要继续执行程序，就要关闭图片。那如何展示动态图或多个窗口呢？这就要使用plt.ion()这个函数，使matplotlib的显示模式转换为交互（interactive）模式。即使在脚本中遇到plt.show()，代码还是会继续执行。下面这段代码是展示两个不同的窗口：

~~~python
    import matplotlib.pyplot as plt
    plt.ion()    # 打开交互模式
    # 同时打开两个窗口显示图片
    plt.figure()  #图片一
    plt.imshow(i1)
    plt.figure()    #图片二
    plt.imshow(i2)
    # 显示前关掉交互模式
    plt.ioff()
    plt.show()
~~~

在交互模式下：

1、plt.plot(x)或plt.imshow(x)是直接出图像，不需要plt.show()

2、如果在脚本中使用ion()命令开启了交互模式，没有使用ioff()关闭的话，则图像会一闪而过，并不会常留。要想防止这种情况，需要在plt.show()之前加上ioff()命令。

在阻塞模式下：

1、打开一个窗口以后必须关掉才能打开下一个新的窗口。这种情况下，默认是不能像Matlab一样同时开很多窗口进行对比的。

2、plt.plot(x)或plt.imshow(x)是直接出图像，需要plt.show()后才能显示图像

#### 链表推导式

~~~python
x=[1,2,3]
b=[x+1 for x in x]
print(b)
>>[2, 3, 4]
~~~

#### iter()函数和next()函数

list、tuple等都是可迭代对象，我们可以通过iter()函数获取这些可迭代对象的迭代器。然后我们可以对获取到的迭代器不断使⽤next()函数来获取下⼀条数据。

~~~python
x=[1,2,3]
b=iter(x)
next(b)
>>
1

#判断一个对象是否为可迭代对象
from collections import Iterable
isinstance('abc',Iterable)#str是否可迭代
>>True
~~~

#### 深拷贝和浅拷贝的区别

[参考](https://blog.csdn.net/u010712012/article/details/79754132)

~~~python
import copy

a=[1,2,[3,4]]
copy1=copy.copy(a)
copy2=copy.deepcopy(a)
copy1==copy2
>>Ture
copy1 is copy2
>>False

#当改变a的值时
a[2][0]=9
a
>>[1, 2, [9, 4]]
copy1
>>[1, 2, [9, 4]]
copy2
>>[1, 2, [3, 4]]

a[1]=5
>>[1, 5, [9, 4]]
copy1
>>[1, 2, [9, 4]]
copy2
>>[1, 2, [3, 4]]
~~~

#### 学习率调整

[参考](https://blog.csdn.net/qyhaill/article/details/103043637)

[参考](https://pytorch.org/docs/stable/optim.html#how-to-adjust-learning-rate)

[参考](https://blog.csdn.net/shanglianlm/article/details/85143614)

~~~python
import torch
from torch.optim import lr_scheduler

#设置优化器
optimizer_ft = optim.SGD(model_ft.parameters(), lr=0.001, momentum=0.9)

# 学习率衰减
exp_lr_scheduler = lr_scheduler.StepLR(optimizer_ft, step_size=7, gamma=0.1)

optimizer_ft.step()#梯度更新
exp_lr_scheduler.step()  # 学习率调整
~~~

**学习率调整应放在梯度更新之后**

~~~python
import torch
import torch.nn as nn
from torch.optim.lr_scheduler import LambdaLR

initial_lr = 0.1

class model(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(in_channels=3, out_channels=3, kernel_size=3)

    def forward(self, x):
        pass

net_1 = model()

optimizer_1 = torch.optim.Adam(net_1.parameters(), lr = initial_lr)
scheduler_1 = LambdaLR(optimizer_1, lr_lambda=lambda epoch: 1/(epoch+1))

print("初始化的学习率：", optimizer_1.defaults['lr'])

for epoch in range(1, 11):
    # train

    optimizer_1.zero_grad()
    optimizer_1.step()
    print("第%d个epoch的学习率：%f" % (epoch, optimizer_1.param_groups[0]['lr']))
    scheduler_1.step()

~~~



#### pytorch的初始化 (torch.nn.init)

[参考](https://pytorch.org/docs/1.5.1/nn.init.html)

[参考](https://blog.csdn.net/dss_dssssd/article/details/83959474)

1. Xavier 初始化

   ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200730100636.png)

为什么需要Xavier 初始化？

1. 简答的说就是：

- 如果初始化值很小，那么随着层数的传递，方差就会趋于0，此时输入值 也变得越来越小，在sigmoid上就是在0附近，接近于线性，**失去了非线性**
- 如果初始值很大，那么随着层数的传递，方差会迅速增加，此时输入值变得很大，而sigmoid在大输入值写倒数趋近于0，反向传播时会遇到**梯度消失**的问题

其他的激活函数同样存在相同的问题。

所以论文提出，**在每一层网络保证输入和输出的方差相同**。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200730100816.png)

2. He initialization

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200730101102.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200730101219.png)

#### Pytorch使用tensorboard进行可视化

[参考](https://blog.csdn.net/MiaoB226/article/details/104213709?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.channel_param)

[参考](https://github.com/Sun-DongYang/Pytorch)

#### Pytorch使用visdom进行可视化

[参考](https://blog.csdn.net/wen_fei/article/details/82979497)

visdom支持多种数据格式的可视化，包括数值、图像、文本以及视频等，支持Pytorch、Torch和Numpy。用户可以通过编程的方式组织可视化空间或者通过用户接口为数据打造仪表板，检查实验结果和调试代码。

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20200730150438.png"  />

激活visdom服务：	

​								或后台运行 nohup python -m visdom.server & 

本地打开浏览器，输入http://localhost:8097，Environment选择test1,即可查看图像

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200730150726.png)



`在本地浏览器打开服务器端口：`服务器IP+端口号

① visdom.image()显示的图像数组的格式是 通道数 x 高 x 宽，而像PIL.Image读取的图像是高 x 宽 x 通道数，因此需要对其numpy数组进行转置一下。

~~~python
from PIL import Image
import numpy as np
import visdom

vis = visdom.Visdom()

img = Image.open('xxx.jpg')
img = np.array(img).transpose([2, 0, 1])

vis.image(img)
~~~

**利用visdom画损失函数**

**visdom.line的使用方法：**

画一条曲线时(Y只有一个方括号)

~~~python
viz.line([loss.item()], [global_step], win='train_loss', update='append' if global_step > 0 else None,opts=dict(title='train loss'))#第一个参数表示Y，第二个参数表示X
~~~

画多条曲线时（Y有两个方括号）

~~~python
viz.line([[test_loss, correct / len(test_loader.dataset)]],
			 [global_step], win='test', update='append' if global_step > 0 else None,opts=dict(title='test loss&acc.',
												   legend=['loss', 'acc.']))
~~~

注意此处的更新方式（很重要），`update='append' if global_step > 0 else None` 当`global_step=0` 不更新，此时传入的初值为第一个损失函数值，**这一步相当于创建了一个新的新的曲线**，当`global_step>0` 时曲线才以`append ` 的方式进行更新。



**visdom.images的使用方法：**

注意visdom.image：显示单张图像;visdom.images：可以显示多张图像。visdom.images显示的图像格式为C*H*W，输入可以为tensor或numpy(注意numpy图像的格式为H*W*C，使用时需要进行转换，例如：image_numpy.transpose([2, 0, 1]))

\1.  vis.image：显示一张图片

```
viz.image(
    np.random.rand(3, 512, 256),
    opts=dict(title='Random!', caption='How random.'),
)
```

- `opts.jpgquality`：`JPG`质量（`number0-100`;默认= `100`）
- `opts.caption`：图像的标题

 

\2.  vis.images：显示一组图片

```
viz.images(
    np.random.randn(20, 3, 64, 64),
    opts=dict(title='Random images', caption='How random.')
)
```

`输入类型：B x C x H x W`张量或`list of images`全部相同的大小。它使大小的图像（`B / Nrow，Nrow`）的网格。 可选参数opts：

- `nrow`：连续的图像数量
- `padding`：在图像周围填充，四边均匀填充
- `opts.jpgquality`：`JPG`质量（`number0-100;`默认= `100`）
- `opts.caption`：图像的标题

**注意**：进行操作：

```
transforms.Normalize(mean=[0.5, 0.5, 0.5],
                     std=[0.5, 0.5, 0.5])
```

得到的张量数值为-1～1,此时直接进行显示可能会出现图像失真的情况，要进行处理为0～1

```python
Photo_show=(Photo + 1) / 2.0 #0~1，Photo为输入的张量
Monet_show=(Monet + 1) / 2.0 #0~1
fake_photo_show=(fake_photo + 1) / 2.0 #0~1
fake_monet_show=(fake_monet + 1) / 2.0 #0~1
cycle_photo_show=(cycle_photo + 1) / 2.0 #0~1
cycle_monet_show=(cycle_monet + 1) / 2.0 #0~1
# if i==0:
#     print('after',photo_show)
# photo_show=self.tensor2im(Photo)
viz.images(Photo_show, win='Photo', opts=dict(title='Photo'))
viz.images(Monet_show, win='Monet', opts=dict(title='Monet'))
viz.images(fake_photo_show, win='fake_photo', opts=dict(title='fake_photo'))
viz.images(fake_monet_show, win='fake_monet', opts=dict(title='fake_monet(target_image)'))
viz.images(cycle_photo_show, win='cycle_photo', opts=dict(title='cycle_photo'))
viz.images(cycle_monet_show, win='cycle_monet', opts=dict(title='cycle_monet'))
```

[参考](https://blog.csdn.net/SHU15121856/article/details/88818539?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~first_rank_v2~rank_v25-1-88818539.nonecase&utm_term=visdom%E6%80%8E%E4%B9%88%E6%B7%BB%E5%8A%A0%E5%88%B0%E5%AE%9E%E6%97%B6%E8%AE%AD%E7%BB%83)

~~~python
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchvision import datasets, transforms

from visdom import Visdom

batch_size = 200
learning_rate = 0.01
epochs = 10

train_loader = torch.utils.data.DataLoader(
	datasets.MNIST('../data', train=True, download=True,
				   transform=transforms.Compose([
					   transforms.ToTensor(),
					   # transforms.Normalize((0.1307,), (0.3081,))
				   ])),
	batch_size=batch_size, shuffle=True)
test_loader = torch.utils.data.DataLoader(
	datasets.MNIST('../data', train=False, transform=transforms.Compose([
		transforms.ToTensor(),
		# transforms.Normalize((0.1307,), (0.3081,))
	])),
	batch_size=batch_size, shuffle=True)


class MLP(nn.Module):

	def __init__(self):
		super(MLP, self).__init__()

		self.model = nn.Sequential(
			nn.Linear(784, 200),
			nn.LeakyReLU(inplace=True),
			nn.Linear(200, 200),
			nn.LeakyReLU(inplace=True),
			nn.Linear(200, 10),
			nn.LeakyReLU(inplace=True),
		)

	def forward(self, x):
		x = self.model(x)
		return x


device = torch.device('cuda:0')
net = MLP().to(device)
optimizer = optim.SGD(net.parameters(), lr=learning_rate)
criteon = nn.CrossEntropyLoss().to(device)

viz = Visdom()
#viz.line([0.], [0.], win='train_loss', opts=dict(title='train loss'))
#viz.line([[0.0, 0.0]], [0.], win='test', opts=dict(title='test loss&acc.',
#												   legend=['loss', 'acc.']))
global_step = 0

for epoch in range(epochs):

	for batch_idx, (data, target) in enumerate(train_loader):
		data = data.view(-1, 28 * 28)
		data, target = data.to(device), target.cuda()

		logits = net(data)
		loss = criteon(logits, target)

		optimizer.zero_grad()
		loss.backward()
		# print(w1.grad.norm(), w2.grad.norm())
		optimizer.step()

		global_step += 1
		viz.line([loss.item()], [global_step], win='train_loss', update='append' if global_step > 0 else None,opts=dict(title='train loss'))

		if batch_idx % 100 == 0:
			print('Train Epoch: {} [{}/{} ({:.0f}%)]\tLoss: {:.6f}'.format(
				epoch, batch_idx * len(data), len(train_loader.dataset),
					   100. * batch_idx / len(train_loader), loss.item()))

	test_loss = 0
	correct = 0
	for data, target in test_loader:
		data = data.view(-1, 28 * 28)
		data, target = data.to(device), target.cuda()
		logits = net(data)
		test_loss += criteon(logits, target).item()

		pred = logits.argmax(dim=1)
		correct += pred.eq(target).float().sum().item()

	viz.line([[test_loss, correct / len(test_loader.dataset)]],
			 [global_step], win='test', update='append' if global_step > 0 else None,opts=dict(title='test loss&acc.',
												   legend=['loss', 'acc.']))
	viz.images(data.view(-1, 1, 28, 28), win='x')
	viz.text(str(pred.detach().cpu().numpy()), win='pred',
			 opts=dict(title='pred'))

	test_loss /= len(test_loader.dataset)
	print('\nTest set: Average loss: {:.4f}, Accuracy: {}/{} ({:.0f}%)\n'.format(
		test_loss, correct, len(test_loader.dataset),
		100. * correct / len(test_loader.dataset)))

~~~





#### GAN中detach()的作用

[参考](https://blog.csdn.net/hungryof/article/details/78035332)

简单来说detach就是**截断反向传播的梯度流**。

~~~python
#GAN的pytorch实现
    def forward(self):
        self.real_A = Variable(self.input_A)
        self.fake_B = self.netG.forward(self.real_A)
        self.real_B = Variable(self.input_B)
        
    def backward_D(self):
        # Fake
        # stop backprop to the generator by detaching fake_B
        fake_AB = self.fake_B
        # fake_AB = self.fake_AB_pool.query(torch.cat((self.real_A, self.fake_B), 1))
        self.pred_fake = self.netD.forward(fake_AB.detach())
        self.loss_D_fake = self.criterionGAN(self.pred_fake, False)

        # Real
        real_AB = self.real_B # GroundTruth
        # real_AB = torch.cat((self.real_A, self.real_B), 1)
        self.pred_real = self.netD.forward(real_AB)
        self.loss_D_real = self.criterionGAN(self.pred_real, True)

        # Combined loss
        self.loss_D = (self.loss_D_fake + self.loss_D_real) * 0.5

        self.loss_D.backward()

    def backward_G(self):
        # First, G(A) should fake the discriminator
        fake_AB = self.fake_B
        pred_fake = self.netD.forward(fake_AB)
        self.loss_G_GAN = self.criterionGAN(pred_fake, True)

        # Second, G(A) = B
        self.loss_G_L1 = self.criterionL1(self.fake_B, self.real_B) * self.opt.lambda_A

        self.loss_G = self.loss_G_GAN + self.loss_G_L1

        self.loss_G.backward()

    # 先调用 forward, 再 D backward， 更新D之后； 再G backward， 再更新G
    def optimize_parameters(self):
        self.forward()

        self.optimizer_D.zero_grad()
        self.backward_D()
        self.optimizer_D.step()

        self.optimizer_G.zero_grad()
        self.backward_G()
        self.optimizer_G.step()
~~~

**解释`backward_D`:**

对于D，我们值需要，如果输入是真实图，那么产生loss，输入生成图，也产生loss。 
 这两个梯度进行更新D。如果是真实图(real_B)，由于real_B是初始结点，所以没什么可担心的。**但是对于生成图fake_B**，**由于  fake_B是由 netG.forward(real_A)产生的。我们`只希望该loss更新D不要影响到 G`.  因此这里需要“截断反传的梯度流”，用 fake_AB = fake_B,** **fake_AB.detach()从而让梯度不要通过 fake_AB反传到netG中！**

**解释`backward_G`:**

由于在调用 backward_G已经调用了zero_grad，所以没什么好担心的。 
 更新G时，来自D的GAN损失是， netD.forward(fake_AB)，得到 pred_fake，然后得到损失，反传播即可。 
 注意，这里反向传播时，会先将梯度传到 fake_AB结点，然而我们知道 fake_AB即 fake_B结点，而fake_B正是由netG(real_A)产生的，所以还会顺着继续往前传播，从而得到G的对应的梯度。

#### pytorch 中.data和.detach的区别和联系

[参考](https://blog.csdn.net/qq_27825451/article/details/96837905?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param)

~~~python
#.data的使用
import torch
 
a = torch.tensor([1,2,3.], requires_grad = True)
out = a.sigmoid()
c = out.data  # 需要注意的是，通过.data “分离”得到的的变量会和原来的变量共用同样的数据，而且新分离得到的张量是不可求导的，c发生了变化，原来的张量也会发生变化
c.zero_()     # 改变c的值，原来的out也会改变
print(c.requires_grad)
print(c)
print(out.requires_grad)
print(out)
print("----------------------------------------------")
 
out.sum().backward() # 对原来的out求导，
print(a.grad)  # 不会报错，但是结果却并不正确
'''运行结果为：
False
tensor([0., 0., 0.])
True
tensor([0., 0., 0.], grad_fn=<SigmoidBackward>)
----------------------------------------------
tensor([0., 0., 0.])
'''
~~~

**tensor.data的两点总结：**

**（1）tensor .data** 返回和 x 的相同数据 tensor,而且这个新的tensor和原来的tensor是共用数据的，一者改变，另一者也会跟着改变，而且新分离得到的tensor的require s_grad = False, 即不可求导的。（这一点其实detach是一样的）

**（2）使用tensor.data的局限性。**文档中说使用tensor.data是不安全的, 因为 x.data 不能被 autograd 追踪求微分  。什么意思呢？从上面的例子可以看出，由于我更改分离之后的变量值c,导致原来的张量out的值也跟着改变了，但是这种改变对于autograd是没有察觉的，它依然按照求导规则来求导，导致得出完全错误的导数值却浑然不知。它的风险性就是如果我再任意一个地方更改了某一个张量，求导的时候也没有通知我已经在某处更改了，导致得出的导数值完全不正确，故而风险大。

~~~python
#detach()的使用
import torch
 
a = torch.tensor([1,2,3.], requires_grad = True)
out = a.sigmoid()
c = out.detach()  # 需要走注意的是，通过.detach() “分离”得到的的变量会和原来的变量共用同样的数据，而且新分离得到的张量是不可求导的，c发生了变化，原来的张量也会发生变化
c.zero_()     # 改变c的值，原来的out也会改变
print(c.requires_grad)
print(c)
print(out.requires_grad)
print(out)
print("----------------------------------------------")
 
out.sum().backward() # 对原来的out求导，
print(a.grad)  # 此时会报错，错误结果参考下面,显示梯度计算所需要的张量已经被“原位操作inplace”所更改了。
'''
False
tensor([0., 0., 0.])
True
tensor([0., 0., 0.], grad_fn=<SigmoidBackward>)
----------------------------------------------
RuntimeError: one of the variables needed for gradient computation has been modified by an inplace operation
'''
~~~

**tensor.detach()的两点总结：**

**（1）tensor .detach()** 返回和 x 的相同数据 tensor,而且这个新的tensor和原来的tensor是共用数据的，一者改变，另一者也会跟着改变，而且新分离得到的tensor的require s_grad = False, 即不可求导的。（这一点其实 .data是一样的）

**（2）使用tensor.detach()的优点。**从上面的例子可以看出，由于我更改分离之后的变量值c,导致原来的张量out的值也跟着改变了，这个时候如果依然按照求导规则来求导，由于out已经更改了，所以不会再继续求导了，而是报错，这样就避免了得出完全牛头不对马嘴的求导结果。

`总结`：

**相同点：**tensor.data和tensor.detach() 都是变量从图中分离，而且这都是“原位操作 inplace operation”。

**不同点：**

（1）.data 是一个属性，二.detach()是一个方法；

（2）.data 是不安全的，.detach()是安全的。



#### pytorch保存模型等相关参数，利用torch.save()，以及读取保存之后的文件

[参考](https://www.cnblogs.com/qinduanyinghua/p/9311410.html)

[参考](https://github.com/soravux/pytorch_tutorial/blob/master/example3.py)

[参考](https://pytorch.org/tutorials/beginner/saving_loading_models.html)

第一部分讲如何保存模型参数，优化器参数等等，第二部分则讲如何读取。

假设网络为model = Net(), optimizer = optim.Adam(model.parameters(), lr=args.lr), 假设在某个epoch，我们要保存模型参数，优化器参数以及epoch

一、

\1. 先建立一个字典，保存三个参数：

state = {‘net':model.state_dict(), 'optimizer':optimizer.state_dict(), 'epoch':epoch}

2.调用torch.save():

torch.save(state, dir)

其中dir表示保存文件的绝对路径+保存文件名，如'/home/qinying/Desktop/modelpara.pth'

二、

当你想恢复某一阶段的训练（或者进行测试）时，那么就可以读取之前保存的网络模型参数等。

checkpoint = torch.load(dir)

model.load_state_dict(checkpoint['net'])

optimizer.load_state_dict(checkpoint['optimizer'])

start_epoch = checkpoint['epoch'] + 1

~~~python
model.state_dict()#读取模型的参数
torch.save(model.state_dict(),'dir')#将模型的参数保存，路径加文件名（以.pth结尾）

torch.load('dir')#将模型的参数进行加载
model.load_state_dict(torch.load('dir'))#将模型的参数赋予当前的网络

~~~

#### pytorch中contiguous()

[参考](https://blog.csdn.net/appleml/article/details/80143212)

contiguous：view只能用在contiguous的variable上。如果在view之前用了transpose, permute等，需要用contiguous()来返回一个contiguous copy。 
 一种可能的解释是： 
 有些tensor并不是占用一整块内存，而是由不同的数据块组成，而tensor的`view()`操作依赖于内存是整块的，这时只需要执行`contiguous()`这个函数，把tensor变成在内存中连续分布的形式。 
 判断是否contiguous用`torch.Tensor.is_contiguous()`函数。

~~~python
import torch
x = torch.ones(10, 10)
x.is_contiguous()  # True
x.transpose(0, 1).is_contiguous()  # False
x.transpose(0, 1).contiguous().is_contiguous()  # True
~~~

#### pytorch中的.t()

**意义就是将Tensor进行转置**

~~~python
    def t(self, input): # real signature unknown; restored from __doc__
        """
        t(input) -> Tensor
        
        Expects :attr:`input` to be a matrix (2-D tensor) and transposes dimensions 0
        and 1.
        
        Can be seen as a short-hand function for :meth:`transpose(input, 0, 1)`
        
        Args:
            input (Tensor): the input tensor
        
        Example::
        
            >>> x = torch.randn(2, 3)
            >>> x
            tensor([[ 0.4875,  0.9158, -0.5872],
                    [ 0.3938, -0.6929,  0.6932]])
            >>> torch.t(x)
            tensor([[ 0.4875,  0.3938],
                    [ 0.9158, -0.6929],
                    [-0.5872,  0.6932]])
        """
        pass
~~~

#### 卷积中的填充方法

[参考](https://zhuanlan.zhihu.com/p/95368411)

***1、零填充ZeroPad2d\***

***2、常数填充ConstantPad2d\***

***3、镜像填充ReflectionPad2d\***

使用输入边界的反射来填充输入tensor

***4、重复填充ReplicationPad2d\***

#### 棋盘效应

[参考](https://www.cnblogs.com/hellcat/p/9707204.html)

通过神经网络生成的图片，放大了看会有棋盘格的现象

#### torch.mean()和torch.mean(dim=0).mean(dim=1)

以二维为例：torch.mean(）返回的是一个标量，而torch.mean(dim=0).mean(dim=1)返回的是一个1行1列的张量，虽然数值相同

~~~python
x=torch.arange(12).view(4,3)
x=x.float()
x_mean=torch.mean(x)
print(x_mean)
y= x.mean(dim=0, keepdim=True).mean(dim=1, keepdim=True)
print(y)
~~~

~~~python
tensor(5.5000)
tensor([[5.5000]])
~~~

~~~python
    Args:
        input (Tensor): the input tensor.
        dim (int or tuple of ints): the dimension or dimensions to reduce.
        keepdim (bool): whether the output tensor has :attr:`dim` retained or not.
        out (Tensor, optional): the output tensor.
    
    Example::
    
        >>> a = torch.randn(4, 4)
        >>> a
        tensor([[-0.3841,  0.6320,  0.4254, -0.7384],
                [-0.9644,  1.0131, -0.6549, -1.4279],
                [-0.2951, -1.3350, -0.7694,  0.5600],
                [ 1.0842, -0.9580,  0.3623,  0.2343]])
        >>> torch.mean(a, 1)#按行求均值
 tensor([-0.0163, -0.5085, -0.4599,  0.1807])#此时的shape为torch.Size([4])
        >>> torch.mean(a, 1, True)#此时的shape为torch.Size([4, 1])
        tensor([[-0.0163],
                [-0.5085],
                [-0.4599],
                [ 0.1807]])

~~~

#### 矩阵乘法

[参考](https://zhuanlan.zhihu.com/p/100069938)

#### pytorch 模型训练时多卡负载不均衡（GPU的0卡显存过高）解决办法



> 出现0卡显存更高的原因：网络在反向传播的时候，计算loss的梯度默认都在0卡上计算。因此会比其他显卡多用一些显存，具体多用多少，主要还要看网络的结构。

[参考](https://blog.csdn.net/weixin_43922901/article/details/106117774)

[参考](https://zhuanlan.zhihu.com/p/86441879)

重写DataParallel

```python
from torch.nn.parallel import DataParallel
import torch
from torch.nn.parallel._functions import Scatter
from torch.nn.parallel.parallel_apply import parallel_apply

def scatter(inputs, target_gpus, chunk_sizes, dim=0):
    r"""
    Slices tensors into approximately equal chunks and
    distributes them across given GPUs. Duplicates
    references to objects that are not tensors.
    """
    def scatter_map(obj):
        if isinstance(obj, torch.Tensor):
            try:
                return Scatter.apply(target_gpus, chunk_sizes, dim, obj)
            except:
                print('obj', obj.size())
                print('dim', dim)
                print('chunk_sizes', chunk_sizes)
                quit()
        if isinstance(obj, tuple) and len(obj) > 0:
            return list(zip(*map(scatter_map, obj)))
        if isinstance(obj, list) and len(obj) > 0:
            return list(map(list, zip(*map(scatter_map, obj))))
        if isinstance(obj, dict) and len(obj) > 0:
            return list(map(type(obj), zip(*map(scatter_map, obj.items()))))
        return [obj for targets in target_gpus]

    # After scatter_map is called, a scatter_map cell will exist. This cell
    # has a reference to the actual function scatter_map, which has references
    # to a closure that has a reference to the scatter_map cell (because the
    # fn is recursive). To avoid this reference cycle, we set the function to
    # None, clearing the cell
    try:
        return scatter_map(inputs)
    finally:
        scatter_map = None

def scatter_kwargs(inputs, kwargs, target_gpus, chunk_sizes, dim=0):
    r"""Scatter with support for kwargs dictionary"""
    inputs = scatter(inputs, target_gpus, chunk_sizes, dim) if inputs else []
    kwargs = scatter(kwargs, target_gpus, chunk_sizes, dim) if kwargs else []
    if len(inputs) < len(kwargs):
        inputs.extend([() for _ in range(len(kwargs) - len(inputs))])
    elif len(kwargs) < len(inputs):
        kwargs.extend([{} for _ in range(len(inputs) - len(kwargs))])
    inputs = tuple(inputs)
    kwargs = tuple(kwargs)
    return inputs, kwargs

class BalancedDataParallel(DataParallel):
    def __init__(self,  *args, **kwargs):
        self.gpu0_bsz = 2
        super().__init__(*args, **kwargs)

    def forward(self, *inputs, **kwargs):
        if not self.device_ids:
            return self.module(*inputs, **kwargs)
        if self.gpu0_bsz == 0:
            device_ids = self.device_ids[1:]
        else:
            device_ids = self.device_ids
        inputs, kwargs = self.scatter(inputs, kwargs, device_ids)
        if len(self.device_ids) == 1:
            return self.module(*inputs[0], **kwargs[0])
        replicas = self.replicate(self.module, self.device_ids)
        if self.gpu0_bsz == 0:
            replicas = replicas[1:]
        outputs = self.parallel_apply(replicas, device_ids, inputs, kwargs)
        return self.gather(outputs, self.output_device)

    def parallel_apply(self, replicas, device_ids, inputs, kwargs):
        return parallel_apply(replicas, inputs, kwargs, device_ids)

    def scatter(self, inputs, kwargs, device_ids):
        bsz = inputs[0].size(self.dim)
        num_dev = len(self.device_ids)
        gpu0_bsz = self.gpu0_bsz
        bsz_unit = (bsz - gpu0_bsz) // (num_dev - 1)
        if gpu0_bsz < bsz_unit:
            chunk_sizes = [gpu0_bsz] + [bsz_unit] * (num_dev - 1)
            delta = bsz - sum(chunk_sizes)
            for i in range(delta):
                chunk_sizes[i + 1] += 1
            if gpu0_bsz == 0:
                chunk_sizes = chunk_sizes[1:]
        else:
            return super().scatter(inputs, kwargs, device_ids)
        return scatter_kwargs(inputs, kwargs, device_ids, chunk_sizes, dim=self.dim)
```

然后

~~~python
    if len(gpu_ids) > 1:
        # netG = torch.nn.DataParallel(netG, device_ids=gpu_ids)
        netG = BalancedDataParallel(netG, dim=0).to('cuda')
        # netG.cuda()
~~~

#### Pytorch张量数据索引切片与维度变换操作

[参考](http://www.mamicode.com/info-detail-2783943.html)

(1-1)pytorch张量数据的索引与切片操作
1、对于张量数据的**索引操作**主要有以下几种方式：

~~~python
a=torch.rand(4,3,28,28):DIM=4的张量数据a

(1)a[:2]:**取第一个维度的前2个维度数据(不包括2)；      
(2)a[:2,:1,:,:]：**取第一个维度的前两个数据，取第2个维度的前1个数据，后两个维度全都取到；
(3)a[:2,1:,:,:]：**取第一个维度的前两个数据，取第2个维度的第1个索引到最后索引的数据(包含1)，后两个维度全都取到；
(4)a[:2,-3:]：**负号表示第2个维度上从倒数第3个数据取到最后倒数第一个数据-1(包含-3)；
(5)a[:,:,0:28:2,0:28:2]：**两个冒号表示隔行取数据，一定的间隔；
(6)a[:,:,::2,::3]：**两个冒号直接写表示从所有的数据中隔行取数据。
~~~





2、对于tensor数据的**切片与其中某些维度数据的提取方法**:
**a.index_select(x,torch.tensor([m,n])):**表示提取tensor数据a的第x个维度上的索引为m和n的数据



3、**torch.masked_select(x,mask):**该函数主要用来选取x数据中的mask性质的数据，比如mask=x.ge(0.5)表示选出大于0.5的所有数据，并且输出时将其转换为了dim=1的打平tensor数据。



4、**#take函数的应用**:先将张量数据打平为一个dim=1的张量数据（依次排序下来成为一个数据列），然后按照索引进行取数据
a=torch.tensor([[1,2,3],[4,5,6]])
**torch.take(a,torch.tensor([1,2,5]))：**表示提取a这个tensor数据打平以后的索引为1/2/5的数据元素
(1-2)tensor数据的维度变换
1、对于tensor数据的**维度变换主要有四大API函数：**
**(1)view/reshape**：主要是在保证tensor数据大小不变的情况下对tensor数据进行形状的重新定义与转换
**(2)Squeeze/unsqueeze:**删减维度或者增加维度操作
**(3)transpose/t/permute**：类似矩阵的转置操作，对于多维的数据具有多次或者单次的转换操作
(4)Expand/repeat：维度的扩展，将低维数据转换为高维的数据
2、**view(reshape)**维度转换操作时需要保证数据的大小numl保持不变，即数据变换前后的prod是相同的：
prod(a.size)=prod(b.size)
另外，对于view操作有一个致命的缺陷就是在数据进行维度转换之后数据之前的存储与维度顺序信息会丢失掉，不能够复原，而这对于训练的数据来说非常重要。
3、**squeeze/unsqueeze挤压和增加维度操作的函数**

~~~python
a=torch.rand(4,3,28,28)
a.unsqueeze(1):在a原来维度索引1之间增加一个维度
a.unsqueeze(-1):在a原来维度索引-1之后增加维度
#例如：
a=torch.tensor([1.2,1.3])  #[2]
print(a.unsqueeze(0))      #[1,2]
print(a.unsqueeze(-1))     #[2,1]
a=torch.rand(4,32,28,28)
b=torch.rand(32)   #如果要实现a和数据b的叠加，则需要对于数据b进行维度扩张
print(b.unsqueeze(1).unsqueeze(2).unsqueeze(0).shape)


~~~



4、维度删减squeeze()
对于维度的挤压squeeze，主要是挤压掉tensor数据中维度特征数为1的维度，如果不是1的话就不可以挤压

~~~python
b=torch.rand(32)
b=b.unsqueeze(1).unsqueeze(2).unsqueeze(0)
print(b.squeeze().shape)
print(b.squeeze(0).shape)
print(b.squeeze(1).shape)
print(b.squeeze(-1).shape)
~~~





5、**维度的扩展：expand(绝对扩展)/repeat(相对扩展**)
\#维度的扩张expand（绝对值）/repeat,repeat扩展实质是重复拷贝的次数-相对值，并且由于拷贝操作，原来的数据不能再用，已经改变，而expand是绝对扩展，其实现只能从1扩张到n，不能从M扩张到N，另外-1表示对该维度保持不变的操作。

~~~python
a=torch.rand(4,32,14,14)
b=torch.rand(32)
b=b.unsqueeze(1).unsqueeze(2).unsqueeze(0)
print(a.shape,b.shape)
print(b.expand(4,32,14,14).shape)
print(b.expand(-1,32,-1,-1).shape) #-1表示对维度保持不变
print(b.repeat(4,32,1,1).shape)
print(b.repeat(4,1,14,14).shape)
~~~





**6、维度交换操作：**
**(1).t()操作**：只可以对DIM=2的矩阵进行转置操作
**(2)transpose**操作：对不同的DIM均可以进行维度交换

~~~python
a=torch.rand(4,3,32,32)
a1=a.transpose(1,3).contiguous().view(4,32*32*3).view(4,32,32,3).transpose(1,3)
print(a1.shape)
print(torch.all(torch.eq(a,a1)))
#整体的变换顺序为a[b,c,h,w]->[b,w,h,c]->[b,w*h*c]->[b,w,h,c]->[b,c,h,w]
~~~



7、permute操作
相比于transpose只可以进行两个维度之间的一次交换操作，permute维度交换操作可以一步实现多个维度之间的交换(相当于transpose操作的多步操作)
\#.t()和transpose/permute维度交换操作，需要考虑数据的信息保存，不能出现数据的污染和混乱.contiguous()操作保持存储顺序不变

~~~python
c=torch.rand(3,4)
print(c)
print(c.t())
a=torch.rand(4,3,32,32)
a1=a.transpose(1,3).contiguous().view(4,32*32*3).view(4,32,32,3).transpose(1,3)
print(a1.shape)
print(torch.all(torch.eq(a,a1)))
a=torch.rand(4,3,28,32)
a1=a.permute(0,2,3,1)
print(a1.shape)
a2=a.contiguous().permute(0,2,3,1)
print(torch.all(torch.eq(a1,a2)))
~~~



对于以上的数据维度变换和索引切片训练代码如下所示：


~~~python

import torch
a=torch.rand(4,3,28,28)
print(a)
print(a.shape)
print(a.dim())#4
#索引与切片操作**
print(a[0].shape)#torch.Size([3, 28, 28])
print(a[0,0,1,2])
print(a[:2].shape)#torch.Size([2, 3, 28, 28]) -> 第一个维度取0～1
print(a[:2,:1,:,:].shape)#torch.Size([2, 1, 28, 28]) -> 第一个维度取0～1,第二个维度取0，第三，四个维度全取
print(a[:2,1:,:,:].shape)# torch.Size([2, 2, 28, 28]) -> 第一个维度取0～1,第二个维度取1到最后一个（包括1），第三，四个维度全取
print(a[:2,-3:].shape)#torch.Size([2, 3, 28, 28]) -> 第一个维度取0～1,第二个维度取倒数第三到倒数第一（包括倒数第三），第三，四个维度全取
print(a[:,:,0:28:2,0:28:2].shape)#torch.Size([4, 3, 14, 14]) -> 第一个维度全取,第二个维度全取，第三维度取0～27,间隔为2,第四维度取0～27,间隔为2
print(a[:,:,::2,::3].shape)#torch.Size([4, 3, 14, 10]) -> 第一个维度全取,第二个维度全取，第三维度取0～27,间隔为2,第四维度取0～27,间隔为3

#########################################################################################

#选择其中某维度的某些索引数据**
b=torch.rand(5,3,3)
print(b)
print(b.index_select(0,torch.tensor([1,2,4])))
print(b.index_select(2,torch.arange(2)).shape)

#########################################################################################

#...操作表示自动判断其中得到维度区间**
a=torch.rand(4,3,28,28)
print(a[...,2].shape)
print(a[0,...,::2].shape)
print(a[...].shape)

#########################################################################################

#msaked_select**
x=torch.randn(3,4)
print(x)
mask=x.ge(0.5)          #选出所有元素中大于0.5的数据
print(mask)
print(torch.masked_select(x,mask))  #选出所有元素中大于0.5的数据，并且输出时将其转换为了dim=1的打平tensor数据

#########################################################################################

**#take函数的应用**:先将张量数据打平为一个dim=1的张量数据（依次排序下来成为一个数据列），然后按照索引进行取数据
a=torch.tensor([[1,2,3],[4,5,6]])
print(a)
print(a.shape)
print(torch.take(a,torch.tensor([1,2,5])))


#########################################################################################

**#tensor数据的维度变换**
**#view/reshape操作**：不进行额外的记住和存贮就会丢失掉原来的数据的数据和维度顺序信息，而这是非常重要的
a=torch.rand(4,1,28,28)
print(a.view(4,28*28))
b=a.view(4,28*28)
print(b.shape)


#########################################################################################

**#squeeze/unsqueeze挤压和增加维度的操作**
a=torch.rand(4,3,28,28)
print(a)
print(a.unsqueeze(0).shape)
print(a.unsqueeze(-1).shape)
print(a.unsqueeze(-4).shape)
a=torch.tensor([1.2,1.3])      #[2]
print(a.unsqueeze(0))          #[1,2]
print(a.unsqueeze(-1))         #[2,1]
a=torch.rand(4,32,28,28)
b=torch.rand(32)   #如果要实现a和数据b的叠加，则需要对于数据b进行维度扩张
print(b.unsqueeze(1).unsqueeze(2).unsqueeze(0).shape)
b=b.unsqueeze(1).unsqueeze(2).unsqueeze(0)
print(b.shape)
print(b.squeeze().shape)
print(b.squeeze(0).shape)
print(b.squeeze(1).shape)
print(b.squeeze(-1).shape)


#########################################################################################

**#维度的扩张expand（绝对值）/repeat**（重复拷贝的次数-相对值，并且由于拷贝操作，原来的数据不能再用，已经改变），只能从1扩张到n，不能从M扩张到N，另外***-1表示对该维度保持不变的操作***
a=torch.rand(4,32,14,14)
b=torch.rand(32)
b=b.unsqueeze(1).unsqueeze(2).unsqueeze(0)
print(a.shape,b.shape)
print(b.expand(4,32,14,14).shape)
print(b.expand(-1,32,-1,-1).shape) #-1表示对维度保持不变
print(b.repeat(4,32,1,1).shape)
print(b.repeat(4,1,14,14).shape)

#########################################################################################

**#.t()和transpose/permute*****维度交换操作，需要考虑数据的信息保存，不能出现数据的污染和混乱.contiguous()操作保持存储顺序不变***
c=torch.rand(3,4)
print(c)
print(c.t())
a=torch.rand(4,3,32,32)
a1=a.transpose(1,3).contiguous().view(4,32*32*3).view(4,32,32,3).transpose(1,3)
print(a1.shape)
print(torch.all(torch.eq(a,a1)))
a=torch.rand(4,3,28,32)
a1=a.permute(0,2,3,1)
print(a1.shape)
a2=a.contiguous().permute(0,2,3,1)
print(torch.all(torch.eq(a1,a2)))
~~~

#### Python hasattr() 函数

​    **hasattr()** 函数用于判断对象是否包含对应的属性。

```
hasattr(object, name)
```

- object -- 对象。

- name -- 字符串，属性名

- 如果对象有该属性返回 True，否则返回 False。        

~~~python

#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
class Coordinate:
    x = 10
    y = -5
    z = 0
 
point1 = Coordinate() 
print(hasattr(point1, 'x'))
print(hasattr(point1, 'y'))
print(hasattr(point1, 'z'))
print(hasattr(point1, 'no'))  # 没有该属性

~~~

~~~python
True
True
True
False
~~~

#### Python getattr() 函数

**getattr()** 函数用于返回一个对象属性值。

```
getattr(object, name[, default])
```

- object -- 对象。
- name -- 字符串，对象属性。
- default -- 默认返回值，如果不提供该参数，在没有对应属性时，将触发 AttributeError。
- 返回对象属性值。

~~~python
>>>class A(object):
...     bar = 1
... 
>>> a = A()
>>> getattr(a, 'bar')        # 获取属性 bar 值
1
>>> getattr(a, 'bar2')       # 属性 bar2 不存在，触发异常
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'A' object has no attribute 'bar2'
>>> getattr(a, 'bar2', 3)    # 属性 bar2 不存在，但设置了默认值
3
~~~

#### Python setattr() 函数

**setattr()** 函数对应函数 [getattr()](https://www.runoob.com/python/python-func-getattr.html)，用于设置属性值，该属性不一定是存在的。

```
setattr(object, name, value)
```

- object -- 对象。
- name -- 字符串，对象属性。
- value -- 属性值。

- 无返回值

对已存在的属性进行赋值：

~~~python

>>>class A(object):
...     bar = 1
... 
>>> a = A()
>>> getattr(a, 'bar')          # 获取属性 bar 值
1
>>> setattr(a, 'bar', 5)       # 设置属性 bar 值
>>> a.bar
5

~~~

如果属性不存在会创建一个新的对象属性，并对属性赋值

~~~python
>>>class A():
...     name = "runoob"
... 
>>> a = A()
>>> setattr(a, "age", 28)
>>> print(a.age)
28
>>>
~~~

#### python lambda函数

~~~python
lambda x ， y ： x * y
~~~

等价为：

~~~python
def add(x, y):
	return x * y
~~~

#### Python  filter() 函数

**filter()** 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。

该接收两个参数，**第一个为函数，第二个为序列**，<u>序列的每个元素作为参数传递给函数进行判断，然后返回 True 或 False，最后将返回 True 的元素放到新列表中。</u>

```
filter(function, iterable)
```

- function --  判断函数。
- iterable --  可迭代对象。

过滤出列表中的所有奇数：

~~~python

#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
def is_odd(n):
    return n % 2 == 1
 
newlist = filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
print(newlist)

#[1, 3, 5, 7, 9]
~~~

或者

~~~python
newlist = filter(lambda n : n%2==1 , [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
~~~

#### [pytorch] TypeError cannot assign torch.FloatTensor as parameter weight

[参考](https://blog.csdn.net/weixin_35499984/article/details/106416197)

TypeError: cannot assign ‘torch.FloatTensor’ as parameter ‘weight’ (torch.nn.Parameter or None expected)

在尝试赋值线性层权重时候出现的错误

```python
def __init__(self, input_dim, atten_dim, attribute, classifier):
    super(MODEL,self).__init__()
    
	nclass, smt_dim = attribute.size()
    self.fc_smt = nn.Linear(smt_dim, nclass, False)
    self.fc_smt.weight = attribute#出错
    for para in self.fc_smt.parameters():
        para.requires_grad = False
```
将tensor赋值给了线性层的权重，应该是parameter才能赋值，或者将tensor赋值给weight.data

解决方法：

~~~python
from torch.nn.parameter import Parameter

        self.fc_smt.weight = Parameter(attribute)

~~~

#### Pytorch 中的 Tensor , Variable & Parameter

[参考](https://www.jianshu.com/p/cb739922ce88)

#### pytorch自定义网络结构不进行参数初始化会怎样？

pytorch自己默认有初始化

[参考](https://blog.csdn.net/u011668104/article/details/81670544)





#### pytorch中的item()用法

pytorch中，.item()方法 是得到一个元素张量里面的元素值
 具体就是 用于**将一个零维张量转换成浮点数**，比如计算loss，accuracy的值

~~~python
G_losses = []
...
errG = criterion(output, label)
...
G_losses.append(errG.item())
~~~

#### Pytorch中Tensor与各种图像格式的相互转化

[参考](https://blog.csdn.net/qq_36955294/article/details/82888443)

[参考](https://stackoverflow.com/questions/54862480/topilimage-object-has-no-attribute-show)

PIL转tensor

~~~python
import torch
import torchvision
form PIL import Image

loader = transforms.Compose([
    transforms.ToTensor()])  
# 输入图片地址
# 返回tensor变量
def image_loader(image_name):
    image = Image.open(image_name).convert('RGB')
    image = loader(image).unsqueeze(0)
    return image.to(device, torch.float)

# 输入PIL格式图片
# 返回tensor变量
def PIL_to_tensor(image):
    image = loader(image).unsqueeze(0)
    return image.to(device, torch.float)

~~~



tensor转PIL

~~~python
unloader = transforms.ToPILImage()
# 输入tensor变量
# 输出PIL格式图片
def tensor_to_PIL(tensor):
    image = tensor.cpu().clone()
    image = image.squeeze(0)
    image = transforms.ToPILImage()(image)
    return image
~~~

实际应用中：

~~~python
    def Test(self):
        # TODO :修改代码
        # pred_monet_list = []
        model.eval()
        # trans = transforms.ToPILImage()
        print('saving test images...\n')
        t = tqdm(test_data_loader, leave=False, total=test_data_loader.__len__())
        for i, photo in enumerate(t):
            with torch.no_grad():
                pred_monet = self.P2M(photo.to(device)).cpu().detach()


            pred_monet=pred_monet.cpu().clone()
            pred_monet=pred_monet.squeeze(0)
            pred_monet=transforms.ToPILImage()(pred_monet)
            pred_monet.save("/home/wag/ssl/kongfupainter/result/" + str(i + 1) + ".jpg")

        print('saving test images end.')

~~~



numpy转tensor

注意tensor的维度是`C*H*W` ， numpy的维度的`H*W*C`

~~~python
import cv2
import torch
import matplotlib.pyplot as plt

def toTensor(img):
    assert type(img) == np.ndarray,'the img type is {}, but ndarry expected'.format(type(img))
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    img = torch.from_numpy(img.transpose((2, 0, 1)))
    return img.float().div(255).unsqueeze(0)  # 255也可以改为256
~~~



tensor转numpy

~~~python
def tensor_to_np(tensor):
    img = tensor.mul(255).byte()
    img = img.cpu().numpy().squeeze(0).transpose((1, 2, 0))
    return img
~~~

#### InstanceNorm2d Load Error: Unexpected running stats buffer(s) “model.model.1.model.2.running_mean“

[参考](https://blog.csdn.net/qazwsxrx/article/details/107094002)

方法2的思路是：

1. 遍历保存的state_dict的所有keys；
2. 使用字符串匹配的方法判断每一个key是否包括running_mean或running_var；
3. 如果包括则从state_dict中删除掉该key；否则继续遍历。

其中，判断字符串是否含有特定字符使用Python自带的.find()方法；删除key则使用del[].

代码如下：

```python
def load_network(self, network, network_label, epoch_label):
    # pdb.set_trace()
    save_filename = '%s_net_%s.pth' % (epoch_label, network_label)  # 'latest_net_G.pth'
    save_path = os.path.join(self.save_dir, save_filename)  # './checkpoints/net_G.pth'
    try:    # 先尝试直接load
        network.load_state_dict(torch.load(save_path))
    except:     # 如果报错，则删除running_mean和running var
        state_dict = torch.load(save_path)
        for k in list(state_dict.keys()):
            if (k.find('running_mean')>0) or (k.find('running_var')>0):
                del state_dict[k]
                print('\n'.join(map(str,sorted(state_dict.keys()))))
            
         network.load_state_dict(state_dict)
```

#### pytorch中model.modules()和model.children()的区别

model.modules()和model.children()均为迭代器，model.modules()会遍历model中所有的子层，而model.children()仅会遍历当前层。

使用：

```python
for key in model.modules()：



    print(key)
# model.modules()类似于 [[1, 2], 3],其遍历结果为：



[[1, 2], 3], [1, 2], 1, 2, 3



 



# model.children()类似于 [[1, 2], 3],其遍历结果为：



[1, 2], 3
```

也就是说，用model.children()进行初始化参数时，可能会漏掉部分，用model.modules()会遍历所有层

reference: https://discuss.pytorch.org/t/module-children-vs-module-modules/4551

#### 如何使用预训练的VGG模型

~~~python
import torch
import torch.nn as nn
import torchvision.models.vgg as models


class Pre_VGG(nn.Module):
    def __init__(self):  # perceptual_layers=3
        super(Pre_VGG, self).__init__()


        # vgg = models.vgg19(pretrained=True).features

        vgg19 = models.vgg19(pretrained=False)  # 加载VGG19模型
        vgg19.load_state_dict(torch.load('/home/wag/ssl/deepfashion/vgg19-dcbb9e9d.pth'))  # 赋予参数
        self.vgg = vgg19.features

        for param in self.vgg.parameters():
            param.requires_grad_(False)  # 不需要训练

    def get_features(self, image, model, layers=None):  # 这是提取VGG19对应特征层的特征
        if layers is None:
            layers = {'4': 'pool_1', '9': 'pool_2', '18': 'pool_3'}
        features = {}
        x = image
        # model._modules is a dictionary holding each module in the model
        for name, layer in model._modules.items():#遍历VGG中的所有层（所有可能组合的层数）
            x = layer(x)
            if name in layers:
                features[layers[name]] = x
        return features

    def forward(self, input):
        sty_fea = self.get_features(input, self.vgg)
        target_feature64 = sty_fea['pool_1']
        target_feature128 = sty_fea['pool_2']
        target_feature256 = sty_fea['pool_3']
        return target_feature64, target_feature128, target_feature256


net=Pre_VGG()
input=torch.randn(1,3,256,176)
out1,out2,out3=net(input)
print(out1.shape)#torch.Size([1, 64, 128, 88])
print(out2.shape)#torch.Size([1, 128, 64, 44])
print(out3.shape)#torch.Size([1, 256, 32, 22])


#VGG19的层数有0,1,2,3,4,5,6,7,8,9...
#即name可能等于0,1,2,3,4,5,6,7,8,9...
#一旦name与layers中的“key”部分相等，则将VGG19该子层的输出赋予对应的“value”
#"value"用一个列表来存放

~~~

#### mask的使用

~~~python 
mask[mask < 0.9999] = 0
mask[mask > 0] = 1
~~~

#### importlib.import_module()的使用

Python中动态导入对象importlib.import_module()的使用,[参考](https://blog.csdn.net/edward_zcl/article/details/88809212)

背景

一个函数运行需要根据不同项目的配置，动态导入对应的配置文件运行。

解决

**文件结构**

```
a #文件夹
	│a.py
	│__init__.py
b #文件夹
	│b.py
	│__init__.py
	├─c#文件夹
		│c.py
		│__init__.py

# c.py 中内容
args = {'a':1}

class C:
    
    def c(self):
        pass
1234567891011121314151617
```

**目的**
 [向a模块中导入c.py](http://xn--ac-qy2cu0kw8dyrg3vjup3a.py) 中的对象

**解决方案**

[a.py](http://a.py)

```
import importlib

params = importlib.import_module('b.c.c') #绝对导入
params_ = importlib.import_module('.c.c',package='b') #相对导入

# 对象中取出需要的对象
params.args #取出变量
params.C  #取出class C
params.C.c  #取出class C 中的c 方法
123456789
```

以上就是动态函数import_module的使用方法

#### __dict__的使用

```python
class A(object):
    """
    Class A.
    """

    a = 0
    b = 1

    def __init__(self):
        self.a = 2
        self.b = 3

    def test(self):
        pass

    @staticmethod
    def static_test(self):
        pass

    @classmethod
    def class_test(self):
        pass
```

```python
A.__dict__

mappingproxy({'__module__': '__main__',
              '__doc__': '\n    Class A.\n    ',
              'a': 0,
              'b': 1,
              '__init__': <function __main__.A.__init__(self)>,
              'test': <function __main__.A.test(self)>,
              'static_test': <staticmethod at 0x7ff8c81a7bd0>,
              'class_test': <classmethod at 0x7ff8c81a7c10>,
              '__dict__': <attribute '__dict__' of 'A' objects>,
              '__weakref__': <attribute '__weakref__' of 'A' objects>})

obj=A()
obj.__dict__

{'a': 2, 'b': 3}


```

由此可见， 类的静态函数、类函数、普通函数、全局变量以及一些内置的属性都是放在**类__dict__**里的。**对象的__dict__**中存储了一些self.xxx的一些东西。

以字典的形式返回

在 [Python](http://c.biancheng.net/python/) 类的内部，无论是类属性还是实例属性，都是以**字典**的形式进行存储的，其中属性名作为键，而值作为该键对应的值。

 为了方便用户查看类中包含哪些属性，Python 类提供了 __dict__ 属性。需要注意的一点是，该属性可以用类名或者类的实例对象来调用，用类名直接调用 __dict__，会输出该由类中所有类属性组成的字典；而使用类的实例对象调用 __dict__，会输出由类中所有实例属性组成的字典。

#### Python中items()的用法

items() 方法的遍历：items() 方法把字典中每对 key 和 value 组成一个元组，并把这些元组放在列表中返回。

```python
d = {'one': 1, 'two': 2, 'three': 3}
>>> d.items()
dict_items([('one', 1), ('two', 2), ('three', 3)])

>>> type(d.items())
<class 'dict_items'>
 
>>> for key,value in d.items():#当两个参数时
    print(key + ':' + str(value))
one:1
two:2
three:3
 
>>> for i in d.items():#当参数只有一个时
    print(i)
('one', 1)
('two', 2)
('three', 3)
```

#### @staticmethod和@classmethod的用法

[参考](https://blog.csdn.net/polyhedronx/article/details/81911548)

**一般来说，要使用某个类的方法，需要先实例化一个对象再调用方法。**  **而使用@staticmethod或@classmethod，就可以不需要实例化，直接类名.方法名()来调用。**

既然@staticmethod和@classmethod都可以直接类名.方法名()来调用，那他们有什么区别呢 
 从它们的使用上来看,

- @staticmethod不需要表示自身对象的self和自身类的cls参数，就跟使用函数一样。

- @classmethod也不需要self参数，但第一个参数需要是表示自身类的cls参数。

  如果在@staticmethod中要调用到这个类的一些属性方法，只能直接类名.属性名或类名.方法名。 
   而@classmethod因为持有cls参数，可以来调用类的属性，类的方法，实例化对象等，避免硬编码。 
   下面上代码。

```python
class A(object):  
    bar = 1  
    def foo(self):  
        print 'foo'  

    @staticmethod  
    def static_foo():  
        print 'static_foo'  
        print A.bar  

    @classmethod  
    def class_foo(cls):  
        print 'class_foo'  
        print cls.bar  
        cls().foo()  
###执行  
A.static_foo()  #不需要实例化，直接通过类名调用静态方法
A.class_foo()  
```

#### torch.nn.Module和torch.autograd.Function	

[参考](https://blog.csdn.net/qq_27825451/article/details/95189376?ops_request_misc=%25257B%252522request%25255Fid%252522%25253A%252522161053798116780277045932%252522%25252C%252522scm%252522%25253A%25252220140713.130102334.pc%25255Fblog.%252522%25257D&request_id=161053798116780277045932&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v1~rank_blog_v1-2-95189376.pc_v1_rank_blog_v1&utm_term=pytorch%E7%9A%84%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8B%93%E5%B1%95%E4%B9%8B%EF%BC%88%E4%B8%80%EF%BC%89%E2%80%94%E2%80%94torch.nn.Module%E5%92%8Ctorch.autograd.Function)

通过继承torch.nn.Function类来实现拓展

> - 在有些操作通过组合pytorch中已有的层或者是已有的方法实现不了的时候，比如你要实现一个新的方法，这个新的方法需要forward和backward一起写，然后自己写对中间变量的操作。
> - 需要重新实现__init__和forward函数，以及backward函数，需要自己定义求导规则；
> - 不可以保存参数和状态信息

**总结：** 当不使用自动求导机制，需要自定义求导规则的时候，就应该拓展torch.autograd.Function类。 否则就是用torch.nn.Module类，后者更简单更常用。

**autograd.Function类的定义**

```python
class Function(with_metaclass(FunctionMeta, _C._FunctionBase, _ContextMethodMixin, _HookMixin)):
 
    __call__ = _C._FunctionBase._do_forward
    is_traceable = False
 
    @staticmethod
    def forward(ctx, *args, **kwargs):
 
    @staticmethod
    def backward(ctx, *grad_outputs):
```
其实就是实现前向传播和反向传播两个函数。注意这里和Module类最明显的区别是它多了一个backward方法，这也是他俩**最本质的区别：**

**（1）**torch.autograd.Function类实际上是某一个操作函数的父类，一个操作函数必须具备两个基本的过程，即<u>前向</u>的运算过程和<u>反向</u>的求导过程，

**（2）**torch.nn.Module类实际上是对torch.xxxx以及torch.nn.functional.xxxx这些函数的包装组合，而torch.xxxx和torch.nn.functional.xxxx都是实现了autograd.Function类的两个基本功能（前向运算和反向传播），如果是我们需要的某一个功能torch.xxxx和torch.nn.functional里面都没有，也不能通过组合得到，这就需要<u>定义新的操作函数，这个函数就需要继承自autograd.Function类，重写前向运算和反向传播</u>。（**注意体会这段话**）

**（3）**很显然，nn.Module更加高层，而autograd.Function更加底层，其实从名字中也能看出二者的区别，Module是针对模块的，即神经网络中的层、激活层、损失函数、网络模型等等，而Function是针对函数的，针对的是一些需要自己定义的函数而言的。如果某一个函数my_function继承自Function类，实现了这个类的forward和backward方法，那么我依然可以用nn.Module对这个自定义的的函数my_function进行包装组合，因为此时my_function跟torch.xxxx和torch.nn.functional.xxxx里面的函数已经具备了等同的地位了。（**注意体会这段话**），可以这么说，Module不仅包括了Function，还包括了对应的参数，以及其他函数与变量，这是Function所不具备的。

```python
class My_Function(Function):
 def forward(self, inputs, parameters):
        self.saved_for_backward = [inputs, parameters]
        # output = [对输入和参数进行的操作，其实就是前向运算的函数表达式]
        return output
 
 def backward(self, grad_output):
        inputs, parameters = self.saved_tensors # 或者是self.saved_variables
        # grad_inputs = [求函数forward(input)关于 parameters 的导数，其实就是反向运算的导数表达式] * grad_output
        return grad_input
```

例

```python
import torch
from torch.autograd import Function
 
# 类需要继承Function类，此处forward和backward都是静态方法
class MultiplyAdd(Function):  
                                                             
    @staticmethod                                  
    def forward(ctx, w, x, b):                 
        ctx.save_for_backward(w,x)    #保存参数,这跟前一篇的self.save_for_backward()是一样的
        output = w * x + b
        return output                        
         
    @staticmethod                                 
    def backward(ctx, grad_output):    #获取保存的参数,这跟前一篇的self.saved_variables()是一样的
        w,x = ctx.saved_variables  
        print("=======================================")             
        grad_w = grad_output * x
        grad_x = grad_output * w
        grad_b = grad_output * 1
        return grad_w, grad_x, grad_b  # backward输入参数和forward输出参数必须一一对应
 
x = torch.ones(1,requires_grad=True)  # x 是1，所以grad_w=1
w = torch.rand(1,requires_grad=True)  # w 是随机的，所以grad_x=随机的一个数
b = torch.rand(1,requires_grad=True)  # grad_b 恒等于1
 
print('开始前向传播')
z=MultiplyAdd.apply(w, x, b)   # forward,这里的前向传播是不一样的，这里没有使用函数去包装自定义的类，而是直接使用apply方法
print('开始反向传播')
z.backward()                   # backward
 
print(x.grad, w.grad, b.grad)
'''运行结果为：
开始前向传播
开始反向传播
=======================================
tensor([0.1784]) tensor([1.]) tensor([1.])
'''
```






























