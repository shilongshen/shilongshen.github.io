# 学习率的调整




[参考](https://blog.csdn.net/qq_41204464/article/details/83660728)

## 目标函数损失值 曲线

![img](https://img-blog.csdn.net/20180808152358945?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWxpbmE2MDM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1. 曲线 初始时 上扬 [红线]： Solution：初始 学习率过大 导致 振荡，应减小学习率，并 从头 开始训练 。
 2. 曲线 初始时 强势下降 没多久 归于水平 [紫线]：  Solution：后期 学习率过大 导致 无法拟合，应减小学习率，并 重新训练 后几轮 。

  3. 曲线 全程缓慢 [黄线]：  Solution：初始 学习率过小 导致 收敛慢，应增大学习率，并从头 开始训练。
