## 用 Visual Studio Code 写 Go

- 文档更新时间：2021-08-05
- Visual Studio Code-1.58.2

建议vscode的配置按工作区去配置，这样不同项目互不影响。

### 工作区设置(settings.json)

在设置中点击，在settings.json中编辑，就会自动在.vscode目录，创建settings.json设置文件

```json
{
  "editor.fontSize": 16,
  "debug.console.fontSize": 16,
  "terminal.integrated.fontSize": 16,
  "go.formatTool": "goformat",
  "[go]": {
    "editor.renderWhitespace": "boundary", //显示空格和制表符
    "editor.detectIndentation": false, //不强制检测和转换
    "editor.tabSize": 2, //制表符2个字符长度
    "editor.insertSpaces": true, //使用空格替代制表符
    "editor.formatOnSave": true, //保存时格式化代码
    "editor.codeActionsOnSave": {
      "source.organizeImports": true
    },
    "editor.suggest.snippetsPreventQuickSuggestions": false
  }
}
```

普通的修改不会出现`"[go]": {}`这样的参数。这个参数，是在设置中，搜索了formatOnSave，然后点了，[go]标签下面的，在settings.json中编辑后出现的。