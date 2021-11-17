## 配置golang开发环境

- 2021-11-17
- VisualStudioCode-1.62

建议vscode的配置按工作区去配置，这样不同项目互不影响。

### 扩展

vscode的扩展主要就三个。

- Go
- CodeRunner
- Chinese

其他的工具是go提供的，vscode会在界面右下角用提示框提示安装。

### 工作区设置（settings.json）

菜单栏=>文件=>首选项=>设置。

在设置中点击`在settings.json中编辑`，就会自动在`.vscode`目录，创建settings.json设置文件。

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

普通的修改设置不会出现上面的`"[go]": {}`这个参数。

在设置中搜索formatOnSave，然后点击`[go]`标签下面的`在settings.json中编辑`，然后`"[go]": {}`参数就会出现。