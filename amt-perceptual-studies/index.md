# AMT perceptual study


[参考文献](https://openaccess.thecvf.com/content_cvpr_2017/papers/Isola_Image-To-Image_Translation_With_CVPR_2017_paper.pdf)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201031103257.png)



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201031104557.png)



注意到，这里评判的指标是志愿者**将生成图像标记为真实图像的概率**，也就是生成图像“欺骗”的概率（G2R）

---

[参考文献](https://openaccess.thecvf.com/content_ICCV_2017/papers/Lassner_A_Generative_Model_ICCV_2017_paper.pdf)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201031103346.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201031105301.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201031105359.png)



---



要点：

- 给出一组真实图像和一组生成图像（以不同的次序），让志愿者为每一张图像进行标记（该张图像是真实图像还是生成图像）
- 每一张图像只出现1秒
- 前10张图像由于warming up，会告知志愿者正确的结果
- 若有多个算法进行比较时，一组实验只进行测试一种算法

注意到评价的指标通常为R2G、G2R，即将真实图像标记为真实图像的概率以及将生成图像标记为生成图像的概率，也就是“fool rate”。


