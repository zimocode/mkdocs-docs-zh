# 为MkDocs做贡献

为MkDocs项目做出贡献的介绍。

MkDocs项目欢迎并依赖于开源社区中开发人员和用户的贡献。 贡献可以通过多种方式进行，例如：

- 提交代码补丁
- 改进文档
- 错误报告和补丁评论

## 行为准则

在MkDocs项目的代码库，问题跟踪器，聊天室和邮件列表中进行交互的每个人都应遵循[PyPA行为准则]。

## 报告问题

请尽可能详细地提供详细信息。 让我们知道您的平台和MkDocs版本。如果问题是可视的（例如主题或设计问题），请添加屏幕截图，如果出现错误，请包含完整错误和追溯。

## 测试开发版本

如果您只想安装并试用MkDocs的最新开发版本，可以使用以下命令执行此操作。如果您想为新功能提供反馈或想要确认您遇到的错误是否已在git主服务器中修复，则此功能非常有用。 强烈建议您在[virtualenv]中执行此操作。

```bash
pip install https://github.com/mkdocs/mkdocs/archive/master.tar.gz
```

## 安装开发

首先，您需要fork并克隆存储库。 获得本地副本后，请运行以下命令。 强烈建议您在[virtualenv]中执行此操作。

```bash
pip install --editable .
```

这将在开发模式下安装MkDocs，它将`mkdocs`命令绑定到git存储库。

## 运行测试

要运行测试，建议您使用[tox]。

通过运行命令`pip install tox`使用[pip]安装Tox。 然后，可以通过在MkDocs存储库的根目录中运行命令`tox`来为MkDocs运行测试套件。

它将尝试针对我们支持的所有Python版本运行测试。 所以不要担心，如果你错过了一些，他们会失败。 当您提交pull request时，[Travis]将验证其余部分。

## 提交Pull Requests

一旦您对更改感到满意，或者您已准备好提供反馈，请将其推送到您的分支并发送拉取请求。要接受更改，如果它是新功能，则很可能需要测试和文档。

[virtualenv]: https://virtualenv.pypa.io/en/latest/userguide.html
[pip]: https://pip.pypa.io/en/stable/
[tox]: https://tox.readthedocs.io/en/latest/
[travis]: https://travis-ci.org/repositories
[PyPA Code of Conduct]: https://www.pypa.io/en/latest/code-of-conduct/
