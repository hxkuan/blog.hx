# 基本信息

## 建站
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```shell
$ hexo init <folder>
$ cd <folder>
$ npm install
```
新建完成后，指定文件夹的目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

- _config.yml 网站的配置](https://hexo.io/zh-cn/docs/configuration.html)信息
- scaffolds [模版](https://hexo.io/zh-cn/docs/writing.html) 文件夹。当您新建文章(hexo new ..)时，Hexo 会根据 scaffold 来建立文件。
- source 资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。
- themes [主题](https://hexo.io/zh-cn/docs/themes.html)文件夹