# 如何开始搭建自己的Markdown网站？

##### 一、获得网站模板

开始搭建网站，你首先需要先获取一套项目模板，根据你自己的情况，可以有两种方式供你选择：

如果你是一个有前端开发经验，熟悉vue技术和ant-design-vue组件库，并且想自定义一些样式的话，可以通过复制源码，自己编译的方式进行。
<a href="https://github.com/softwaiter/markdown-website" target="_blank">点击打开源码下载页面</a>

如果你并不想自己安装开发环境，自己编译代码，可以直接下载编译好的静态网页压缩包，解压即可。
<a href="https://github.com/softwaiter/markdown-website/releases/download/v1.1.0-preview/v1.1.0-preview.zip">点击下载静态网页压缩包</a>

静态网页目录结构
├── css&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# 网站样式文件
├── js&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# 网站脚本文件
├── static&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# 网站资源文件
├── favicon.ico&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# 网站小图标
└── index.html&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# 网站页面

##### 二、收集网站要展示的Markdown文件
将希望在网站展示的所有Markdown文件收集起来，统一复制到static目录里面。
由于static目录里包含演示的数据文档，在复制之前，请将所有后缀名为.md的文件全部删除。

##### 三、编辑Markdown文件展示目录树
现在要展示的Markdown文件内容有了，接下来，我们来对展示的目录树结构进行编辑。
目录树的内容结构在static目录下的config.json文件内进行设置，使用文本编辑器打开config.json文件，可以看到如下内容：

```json
{
    "title": "markdown-website",
    "topicWidth": 250,
    "openLevel": 1,
    "topics": [
    {
      "id": "01",
      "name": "markdown-website是什么？",
      "src": "static/what.md"
    },
    {
        "id": "02",
        "name": "开始使用",
        "submenus": [
        {
            "id": "0201",
            "name": "搭建markdown网站",
            "src": "static/how.md"
        },
        {
            "id": "0202",
            "name": "更多网站属性配置",
            "src": "static/edit.md"
        },
        {
            "id": "0203",
            "name": "部署发布",
            "src": "static/deploy.md"
        }
      ]
    }
  ]
}
```

其中，topics数组是目录树结构的内容，首先将topics内的演示数据清空，还原成空数组：
```json
{
    "title": "markdown-website",
    "topicWidth": 250,
    "openLevel": 1,
    "topics": []
}
```

每一个目录节点是一个对象，对象内都包含如下4个属性：
<b>id</b>：目录节点的唯一标识，不能重复。
<b>name</b>：目录节点的显示名称。
<b>src</b>：目录节点的关联markdown文件路径，即我们前面复制到static目录的markdown文件。例如：static/xxx.md；也可以根据实际情况设置成外部链接，如：http://www.xxx.com/docs/readme.md。如果是包含`submenus`的父节点，可以不设置。
<b>submenus</b>：子目录树节点，如果没有子节点，可以是空数组[]。

实例，增加一个“我的收藏”的父节点：
```json
{
    "title": "markdown-website",
    "topicWidth": 250,
    "openLevel": 1,
    "topics": [
        {
            "id": "01",
            "name": "我的收藏",
            "submenus": []
        }
    ]
}
```

继续在我的收藏里增加“给长城贴瓷砖方案”和“给珠峰装电梯方案”两个子节点，并关联各自的markdown文件：
```json
{
    "title": "markdown-website",
    "topicWidth": 250,
    "openLevel": 1,
    "topics": [
        {
            "id": "01",
            "name": "我的收藏",
            "submenus": [
                {
                    "id": "0101",
                    "name": "给长城贴瓷砖方案",
                    "src": "static/给长城贴瓷砖方案.md"
                },
                {
                    "id": "0102",
                    "name": "给珠峰装电梯方案",
                    "src": "static/给珠峰装电梯方案.md"
                }
            ]
        }
    ]
}
```

基于以上方式，可以根据自己的实际情况和需要，定义目录树的结构。理论上，支持无限层级，但从实际效果来看，2~3层效果最佳。



<b>注：</b>`src`属性在设置事需要特别注意，如果网站最终部署在根目录，如”www.xxx.com“，这时设置绝对路径"/static/what.md"或者相对路径”static/what.md“都没有问题，能够正常访问；如果网站部署在子目录，如”www.xxx.com/markdown-website“，此时，src属性必须设置成相对路径”static/what.md“，否则，将不能正常访问文档内容。