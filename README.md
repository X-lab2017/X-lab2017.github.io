<meta name="referrer" content="no-referrer"/>

**<p align="center">X-lab! 👋</p>** 
![xlab大图.png](https://cdn.nlark.com/yuque/0/2024/png/39023202/1717139608189-5890a1b6-9668-4409-bf3a-0465a26cfd7b.png#averageHue=%2342a860&clientId=ua356d4b6-9589-4&from=drop&id=ubdd08c71&originHeight=603&originWidth=1275&originalType=binary&ratio=2&rotation=0&showTitle=false&size=95701&status=done&style=none&taskId=ub93f1a38-d0ef-4f76-bcc1-5ee9fbf1c5d&title=)

---

你好，这里是X-lab官网，X-lab开放实验室定位为一个开源研究与创新的开放群体，是一群由来自国内外著名高校、创业公司、部分互联网与IT企业的专家学者与工程师所构成，聚焦于开源软件产业开放式创新的共同体。
本文旨在帮助你如何共同参与 X-lab 开放实验室网站的建设。如果有好的想法或创意，欢迎提出你的 issue。另外如果想要帮忙改进现有的文档，你可以直接提出 pr 。下面我将介绍参与贡献的具体方法。
# 贡献方式
因为本网站采用的是基于 mkdocs 这一框架，所以网站的网页内容是基于网站建设者提供的 MarkDown 文档来生成的。所以对于网页内容的贡献通常是对其背后 MarkDown 文件的修改。
下面将针对添加内容和修改内容两种情况进行介绍。
## 修改内容
我们建议你将网站仓库 fork 到自己的仓库列表内进行修改。修改方式如下：

1. 选择分支为 main 分支。
2. 进入 docs 文件夹。
3. 根据你想修改的内容在对应文件夹下查找。英文版本的文档在 en 文件夹下，中文版本在 zh 文件夹下。
4. 修改后提交 pr 等待 reviewer 审核通过即可完成修改。
## 增加内容
我们仍然建议你将网站仓库 fork 到自己的仓库列表内进行内容的增添。方式如下：

1. 选择分支为 main 分支。
2. 进入 docs 文件夹。
3. 根据不同的类别选择你想要增加的内容，这里以 Blog 举例。如果你想将你的 Blog 分享在我们的官网上，你可以在 en 与 zh 的 blog 文件夹分别上传你的 blog 的英文版本与中文版本。如果只提供一个版本，我们建议你提供英文版本，因为我们希望全世界的开源同好们能够更广泛的参与我们的活动。需要说明的是一般情况下，en 与 zh 的文件目录结构是一致的。
4. 为了将你上传的文章成功的被生成网页，你需要找到 main 分支根目录下的 mkdocs.yml 文件，将文件以如下方式导入
```python
- "doc_name" : doc Directory
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/39023202/1717141925822-62a67fc2-3729-451a-bad8-2f1e601cb32d.png#averageHue=%23121111&clientId=ua356d4b6-9589-4&from=paste&height=228&id=u3054bb62&originHeight=455&originWidth=2033&originalType=binary&ratio=2&rotation=0&showTitle=false&size=108398&status=done&style=none&taskId=u1e37d1e6-cdcf-455f-befb-f7175611c9b&title=&width=1016.5)
