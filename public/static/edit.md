# 更多网站属性配置

为了保证网站最终的显示效果以及初次打开时的状态，项目提供了一些配置属性供用户使用，这些属性都可以在static目录下的config.json文件里进行修改，目前提供了3个属性：
<b>title</b>：网站的显示标题，默认是markdown-website。
<b>topicWidth</b>：左侧目录树侧边栏的宽度，默认是250像素。
<b>openLevel</b>：网站初次打开时，目录树展开的层级，默认1级。

```json
{
    "title": "markdown-website",
    "topicWidth": 250,
    "openLevel": 1,
    "topics": []
}
```
