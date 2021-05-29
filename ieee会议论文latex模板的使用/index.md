# IEEE会议论文Latex模板的使用


注意，此处是会议论文模板的使用（期刊模板类似）；解决的问题为**外部bib文件的引用**

- 模板下载地址：[链接](https://journals.ieeeauthorcenter.ieee.org/create-your-ieee-journal-article/authoring-tools-and-templates/tools-for-ieee-authors/ieee-article-templates/)或[链接](https://www.ieee.org/conferences/publishing/templates.html)

注意此时参考文献只能够在`tex`文件内部编辑，比较麻烦。如果我们已经有了外部的`bib`文件，例如

```tex
@inproceedings{goodfellow2014generative,
	title={Generative Adversarial Nets},
	author={Goodfellow, Ian and Pouget-Abadie, Jean and Mirza, Mehdi and Xu, Bing and Warde-Farley, David and Ozair, Sherjil and Courville, Aaron and Bengio, Yoshua},
	booktitle={Advances in neural information processing systems},
	
	pages={2672--2680},
	year={2014},
	
	
}


@inproceedings{men2020controllable,
	title={Controllable Person Image Synthesis with Attribute-Decomposed GAN},
	author={Men, Yifang and Mao, Yiming and Jiang, Yuning and Ma, Wei-Ying and Lian, Zhouhui},
	booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
	
	pages={5084--5093},
	year={2020}
}
```

我们可以直接通过外部bib文件的引用的方式导入已有的参考文献。首先下载[链接](https://ctan.org/tex-archive/macros/latex/contrib/IEEEtran)，打开`bibtex文件夹`，这里面包含了`ieee`的参考文献格式定义。将`IEEEtran.bst`复制到原文件夹中，并将外部bib文件复制到原文件夹中。此时只需要使用

```tex
%外部bib文件的引用
\bibliographystyle{IEEEtran}
\bibliography{references}
```

即可实现**外部bib文件的引用**。
