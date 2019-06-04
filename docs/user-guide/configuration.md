# 配置文件

所有可用配置选项的指南。

---

## 简介

始终使用名为`mkdocs.yml`的位于项目目录中的YAML文件来进行设置。

此配置文件必须包含`site_name`设置。 所有其他设置都是可选项。

## 项目信息

### site_name

这是一个**必需的设置**，应该是一个字符串，用作项目文档的主标题。 例如：

```yaml
site_name: Marshmallow Generator
```

渲染主题时，此设置将作为`site_name`上下文变量传递。

### site_url

设置站点的规范URL。 这将添加带有规范URL的链接标记到生成的HTML标头。

**默认值**： `null`

### repo_url

置后，在每个页面上提供指向您的存储库（GitHub，Bitbucket，GitLab，...）的链接。

```yaml
repo_url: https://github.com/example/repository/
```

**默认值**： `null`

### repo_name

设置后，在每个页面上提供指向存储库的链接的名称。

**默认值**： `'GitHub'`, `'Bitbucket'` or `'GitLab'`， 如果`repo_url`不能匹配这些域名，则使用来自`repo_url`的域名。

### edit_uri

Path from the base `repo_url` to the docs directory when directly viewing a page, accounting for specifics of the repository host (e.g. GitHub, Bitbucket, etc), the branch, and the docs directory itself. MkDocs concatenates `repo_url` and `edit_uri`, and appends the input path of the page.
直接查看页面时从基础`repo_url`到docs目录的路径，考虑存储库主机（例如GitHub，Bitbucket等），分支和docs目录本身的细节。 MkDocs连接`repo_url`和`edit_uri`，并附加页面的输入路径。

设置后，如果您的主题支持它，则直接提供源存储库中页面的链接。 这样可以更轻松地查找和编辑页面的源。 如果未设置`repo_url`，则忽略此选项。 在某些主题上，设置此选项可能会导致使用编辑链接代替存储库链接。 其他主题可能会显示这两个链接。

`edit_uri`支持查询（'？'）和片段（'＃'）字符。 对于使用查询或片段访问文件的存储库主机，可以按如下方式设置`edit_uri`。 （注意URI中的`？`和`#` ...）

```yaml
# Query string example
edit_uri: '?query=root/path/docs/'
```

```yaml
# Hash fragment example
edit_uri: '#root/path/docs/'
```

对于其他存储库主机，只需指定docs目录的相对路径即可。

```yaml
# Query string example
edit_uri: root/path/docs/
```

!!! note "注意："
    在一些已知的主机（特别是GitHub，Bitbucket和GitLab）上，`edit_uri`是从'repo_url'派生出来的，不需要手动设置。 只需定义一个`repo_url`就会自动填充`edit_uri`配置设置。

    例如，对于GitHub或GitLab托管的存储库，`edit_uri`将自动设置为`edit/master/docs/`（注意`edit`路径和`master`分支）。

    对于Bitbucket托管的存储库，等效的`edit_uri`将自动设置为`src/default/docs/`（注意`src`路径和`default`分支）。

    要使用与默认值不同的URI（例如不同的分支），只需将`edit_uri`设置为所需的字符串即可。如果您不希望页面上显示任何“编辑URL链接”，请将`edit_uri`设置为空字符串以禁用自动设置。

!!! warning "警告："
    在GitHub和GitLab上，默认的“编辑”路径（`edit/master/docs/`）在在线编辑器中打开页面。此功能要求用户拥有并已登录GitHub/GitLab帐户。 否则，用户将被重定向到登录/注册页面。 或者，使用“blob”路径（`blob/master/docs/`）打开只读视图，该视图支持匿名访问。

**默认值**：`edit/master/docs/`用于GitHub和GitLab存储库，`src/default/docs/`用于Bitbucket存储库，如果`repo_url`匹配那些域，否则为`null`

### site_description

设置站点描述。 这将为生成的HTML标头添加元标记。

**默认值**： `null`

### site_author

设置作者的姓名。 这将为生成的HTML标头添加元标记。

**默认值**： `null`

### copyright

设置版权信息以包含在主题的文档中。

**默认值**： `null`

### google_analytics

设置Google Analytics跟踪配置。

```yaml
google_analytics: ['UA-36723568-3', 'mkdocs.org']
```

**默认值**： `null`

### remote_branch

当使用`gh-deploy`来部署到Github Pages时，设置欲远程提交到的分支。可以通过`gh-deploy`中的命令行选项覆盖此选项。

**默认值**： `gh-pages`

### remote_name

当使用`gh-deploy`部署到Github Pages时，设置要推送到的远程名称。 可以通过`gh-deploy`中的命令行选项覆盖此选项。

**默认值**： `origin`

## 文档布局

### nav

此设置用于确定站点的全局导航的格式和布局。 例如，以下内容将创建“Introduction”，“User Guide”和“About”导航项。

```yaml
nav:
    - 'Introduction': 'index.md'
    - 'User Guide': 'user-guide.md'
    - 'About': 'about.md'
```

所有路径必须相对于`mkdocs.yml`配置文件。 有关更详细的细分，请参阅[配置页面和导航]部分，包括如何创建子导航。

导航项目还可能包括指向外部站点的链接。 虽然标题对于内部链接是可选的，但外部链接需要它们。 外部链接可以是完整URL或相对URL。 假定在文件中找不到的任何路径都是外部链接。

```yaml
nav:
    - Home: index.md
    - User Guide: user-guide.md
    - Bug Tracker: https://example.com/
```

在上面的示例中，前两个项指向本地文件，而第三个指向外部站点。

但是，有时MkDocs站点托管在项目站点的子目录中，您可能希望链接到同一站点的其他部分而不包括完整域。 在这种情况下，您可以使用和适当的相对URL。

```yaml
site_url: https://example.com/foo/

nav:
    - Home: ../
    - User Guide: user-guide.md
    - Bug Tracker: /bugs/
```

在上面的示例中，使用了两种不同风格的外部链接。 首先请注意，`site_url`表示MkDocs站点托管在域的`/foo/`子目录中。 因此，“Home”导航项是一个相对链接，它将一个级别提升到服务器根目录并有效地指向“https:// example.com/”。 “Bug Tracker”项使用来自服务器根目录的绝对路径，并有效地指向“https://example.com/bugs/”。 当然，“User Guide”指向本地MkDocs页面。

**默认值**：默认情况下，`nav`将包含在`docs_dir`及其子目录中找到的所有Markdown文件的字母数字排序的嵌套列表。如果没有找到它将是`[]`（空列表）。

## 构建目录

### theme

设置文档站点的主题和主题特定配置。 可以是字符串或一组键/值对。

如果是字符串，则必须是已知安装主题的字符串名称。 有关可用主题的列表，请访问[样式化您的文档]。

一组示例键/值对可能如下所示：

```yaml
theme:
    name: mkdocs
    custom_dir: my_theme_customizations/
    static_templates:
        - sitemap.html
    include_sidebar: false
```

如果是一组键/值对，则可以定义以下嵌套键：

!!! block ""

    #### name:

    已知安装主题的字符串名称。 有关可用主题的列表，请访问[样式化您的文档]。

    #### custom_dir:

    包含自定义主题的目录。 这可以是相对目录，在这种情况下，它相对于包含配置文件的目录进行解析，或者它可以是来自本地文件系统根目录的绝对目录路径。

    如果您想要调整现有主题，请参阅[样式化您的文档][theme_dir]了解详细信息。

    如果您想从头开始构建自己的主题，请参阅[自定义主题]。

    #### static_templates:

    要呈现为静态页面的模板列表。 模板必须位于主题的模板目录中或主题配置中定义的`custom_dir`中。

    #### 主题特定关键字

    还可以定义主题支持的任何其他关键字。 有关详细信息，请参阅您正在使用的主题的文档。

**默认值**： `'mkdocs'`

### docs_dir

源文档markdown文件的目录。 这可以是相对目录，在这种情况下，它相对于包含配置文件的目录进行解析，或者它可以是来自本地文件系统根目录的绝对目录路径。

**默认值**： `'docs'`

### site_dir

为输出HTML和其他文件而创建的目录。 这可以是相对目录，在这种情况下，它相对于包含配置文件的目录进行解析，或者它可以是来自本地文件系统根目录的绝对目录路径。

**默认值**： `'site'`

!!! note "注意："
    如果您使用的是源代码控制，通常需要确保*输出*文件未提交到存储库中，并且只保留*源文件*在版本控制下。 例如，如果使用`git`，您可以将以下行添加到`.gitignore`文件中：

        site/

    如果您正在使用其他源代码控制工具，则需要查看有关如何忽略特定目录的文档。

### extra_css

设置主题中包含到`docs_dir`中的CSS文件列表。 例如，以下示例将在[docs_dir]（＃docs_dir）的css子目录中包含extra.css文件。

```yaml
extra_css:
    - css/extra.css
    - css/second_extra.css
```

**默认值**： `[]` (an empty list).

### extra_javascript

设置要包含到主题中的`docs_dir`中的JavaScript文件列表。请参阅[extra_css]中的示例以了解用法。

**默认值**： `[]` (an empty list).

### extra_templates

设置要由MkDocs构建的`docs_dir`中的模板列表。 要了解有关为MkDocs编写模板的更多信息，请阅读有关[自定义主题]的文档，特别是有关模板的[可用变量]的部分。 有关用法，请参阅[extra_css]中的示例。

**默认值**： `[]` (an empty list).

### extra

一组键值对，其中值可以是任何有效的YAML构造，将传递给模板。 这在创建自定义主题时具有很大的灵活性。

例如，如果您使用的主题支持显示项目版本，则可以将其传递给主题，如下所示：

```yaml
extra:
    version: 1.0
```

**默认值**： 默认情况下，“extra”将是一个空键值映射。

## 预览控制

### use_directory_urls

此设置控制用于链接的样式。

下表演示了将“use_directory_urls”设置为“true”或“false”时网站上显示的URL的不同之处。

源文件            | use_directory_urls: true  | use_directory_urls: false
---------------- | ------------------------- | -------------------------
index.md         | /                         | /index.html
api-guide.md     | /api-guide/               | /api-guide.html
about/license.md | /about/license/           | /about/license.html

`use_directory_urls：true`的默认样式创建了更多用户友好的URL，通常是您想要使用的。

如果您希望在直接从文件系统打开页面时保持文档正确链接，则备用样式有时会很有用，因为它会创建直接指向目标*文件*而非目标*目录*的链接。

**默认值**： `true`

### strict

确定警告的处理方式。 设置为“true”以在发出警告时停止处理。 设置为“false”以打印警告并继续处理。

**默认值**： `false`

### dev_addr

确定运行`mkdocs serve`时使用的地址。 必须是“IP:PORT”格式。

允许设置自定义默认值，而无需在每次调用`mkdocs serve`命令时通过`--dev-addr`选项传递它。

**默认值**： `'127.0.0.1:8000'`

## 格式化选项

### markdown_extensions

MkDocs使用[Python Markdown][pymkd]库将Markdown文件翻译成HTML。 Python Markdown支持各种[扩展][pymdk-extensions]，可以自定义页面的格式。 此设置允许您启用超出MkDocs默认使用的扩展名列表（`meta`，`toc`，`tables`和`fenced_code`）。

例如，要启用[SmartyPants排版扩展][smarty]，请使用：

```yaml
markdown_extensions:
    - smarty
```

某些扩展提供了自己的配置选项。 如果要设置任何配置选项，则可以嵌套给定扩展支持的任何选项的键/值映射（`option_name:option value`）。请参阅所用扩展的文档以确定它们所提供的扩展选项。

例如，要在（内置的）`toc`扩展中启用永久链接，请使用：

```yaml
markdown_extensions:
    - toc:
        permalink: True
```

请注意冒号（`：`）必须跟随扩展名（`toc`），然后在新起的一行上，选项名称和值必须缩进并用冒号分隔。如果要为单个扩展定义多个选项，则必须在单独的一行上分别定义每个选项：

```yaml
markdown_extensions:
    - toc:
        permalink: True
        separator: "_"
```

为每个扩展名添加一个附加项目到列表中。 如果没有为特定扩展设置的配置选项，则只需省略该扩展的选项：

```yaml
markdown_extensions:
    - smarty
    - toc:
        permalink: True
    - sane_lists
```

!!! note "延申阅读："
    Python-Markdown文档提供了[扩展名列表][exts]，它们是开箱即用的。有关给定扩展可用的配置选项列表，请参阅该扩展的文档。

    您也可以安装和使用各种[第三方扩展][3rd]。 有关安装说明和可用配置选项，请参阅这些扩展提供的文档。

**默认值**： `[]` (an empty list).

### plugins

构建站点时要使用的插件列表（带有可选配置设置）。 有关完整详细信息，请参阅[插件]文档。

如果`plugins`配置设置在`mkdocs.yml`配置文件中定义，则忽略任何默认值（例如`search`），如果要继续使用它们，则需要显式重新启用默认值：

```yaml
plugins:
    - search
    - your_other_plugin
```

要完全禁用所有插件，包括任何默认值，请将`plugins`设置为空列表：

```yaml
plugins: []
```

**默认值**： `['search']` ，MkDocs内置了该'search'插件（“搜索”）.

#### Search

默认情况下，MkDocs提供了一个搜索插件，它使用[lunr.js]作为搜索引擎。以下配置选项可用于更改搜索插件的行为：

##### **separator**

一个正则表达式，它与构建索引时用作单词分隔符的字符相匹配。 默认情况下使用空格和连字符（`-`）。 要将点（`.`）添加为单词分隔符，您可以执行以下操作：

```yaml
plugins:
    - search:
        separator: '[\s\-\.]+'
```

  **默认值**： `'[\s\-]+'`

##### **lang**

构建由[ISO 639-1]语言代码标识的搜索索引时使用的语言列表。 使用[Lunr Languages]，支持以下语言：

* `da`: Danish
* `du`: Dutch
* `en`: English
* `fi`: Finnish
* `fr`: French
* `de`: German
* `hu`: Hungarian
* `it`: Italian
* `jp`: Japanese
* `no`: Norwegian
* `pt`: Portuguese
* `ro`: Romanian
* `ru`: Russian
* `es`: Spanish
* `sv`: Swedish
* `th`: Thai
* `tr`: Turkish

您也可以[贡献其他语言]。

!!! Warning "警告："

    虽然搜索支持同时使用多种语言，但最好不要添加其他语言，除非您确实需要它们。 每种附加语言都会增加大量带宽需求并使用更多浏览器资源。一般来说，最好将MkDoc的每个实例保持为单一语言。

!!! Note "注意："

    Lunr Languages目前不包括对中文或其他亚洲语言的支持。 但是，一些用户使用日语报告了不错的结果。

**默认值**： `['en']`

##### **prebuild_index**

（可选）生成所有页面的预构建索引，从而为较大的站点提供一些性能改进。在启用之前，请检查您使用的主题是否明确支持使用预建索引（内置主题可以）。

预建索引有两种选择：

使用[Node.js]将`prebuild_index`设置为`True`或`node`。 此选项要求安装Node.js并且命令`node`在系统路径上。 如果启用此功能并因任何原因失败，则会发出警告。 在构建时可以使用`--strict`标志来导致这样的失败而不是引发错误。

使用[Lunr.py]设置`prebuild_index`到`python`。Lunr.py作为mkdocs的一部分安装，并保证与Lunr.js的兼容性，即使在英语以外的语言上也是如此。 如果您发现实质性的不一致或问题，请在[Lunr.py's issues]上报告，然后再回到Node.js版本。

!!! Note "注意："

    在较小的站点上，不建议使用预先构建的索引，因为它会造成带宽显著增加，而对用户几乎没有明显改善。但是，对于较大的站点（数百页），带宽增加相对较小，您的用户会注意到搜索性能的显着提高。

**默认值**： `False`

[custom themes]: custom-themes.md
[variables that are available]: custom-themes.md#template-variables
[pymdk-extensions]: https://python-markdown.github.io/extensions/
[pymkd]: https://python-markdown.github.io/
[smarty]: https://python-markdown.github.io/extensions/smarty/
[exts]: https://python-markdown.github.io/extensions/
[3rd]: https://github.com/Python-Markdown/markdown/wiki/Third-Party-Extensions
[configuring pages and navigation]: writing-your-docs.md#configure-pages-and-navigation
[theme_dir]: styling-your-docs.md#using-the-theme_dir
[styling your docs]: styling-your-docs.md
[extra_css]: #extra_css
[Plugins]: plugins.md
[lunr.js]: https://lunrjs.com/
[ISO 639-1]: https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
[Lunr Languages]: https://github.com/MihaiValentin/lunr-languages#lunr-languages-----
[contribute additional languages]: https://github.com/MihaiValentin/lunr-languages/blob/master/CONTRIBUTING.md
[Node.js]: https://nodejs.org/
[Lunr.py]: http://lunr.readthedocs.io/
[Lunr.py's issues]: https://github.com/yeraydiazdiaz/lunr.py/issues
