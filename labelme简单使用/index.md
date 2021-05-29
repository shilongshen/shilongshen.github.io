# LabelMe简单使用


# LabelMe安装

[参考](https://blog.csdn.net/u014061630/article/details/88756644)

- Ubuntu

```shell
# Ubuntu 14.04 / Ubuntu 16.04
# Python2
# sudo apt-get install python-qt4  # PyQt4
sudo apt-get install python-pyqt5  # PyQt5
sudo pip install labelme
# Python3
sudo apt-get install python3-pyqt5  # PyQt5
sudo pip3 install labelme

```

# 基本操作

```shell
labelme  # 打开labelme软件

labelme apc2016_obj3.jpg  # 指定图像文件
labelme apc2016_obj3.jpg -O apc2016_obj3.json  # 保存后关闭labelme
labelme apc2016_obj3.jpg --nodata  # JSON文件不包含图像数据，而包含图像的相对路径
labelme apc2016_obj3.jpg \
  --labels highland_6539_self_stick_notes,mead_index_cards,kong_air_dog_squeakair_tennis_ball  # 指定 label list

labelme data_annotated/  # 指定图像文件夹
labelme data_annotated/ --labels labels.txt  # 使用文件指定 label list
```

- 将输出的json文件转换为图像

```shell
labelme_draw_label_png <文件名>.json
```

## 图像分割标注工具labelme改变标注颜色

[参考](https://blog.csdn.net/qq_41776136/article/details/115568341?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-4&spm=1001.2101.3001.4242)

将conda虚拟环境位置下的`.../site-packages/imgviz/label.py`中

```python
 r = np.bitwise_or(r, (bitget(id, 0) << 7 - j))
 g = np.bitwise_or(g, (bitget(id, 1) << 7 - j))
 b = np.bitwise_or(b, (bitget(id, 2) << 7 - j))
```

改为

```python
if i == 1:
   r = 255
   g = 255
   b = 255
else:
   r = np.bitwise_or(r, (bitget(id, 0) << 7-j))
   g = np.bitwise_or(g, (bitget(id, 1) << 7-j))
   b = np.bitwise_or(b, (bitget(id, 2) << 7-j))
```


