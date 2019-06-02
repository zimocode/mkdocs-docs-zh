# 设置文档样式

如何设置文档的样式和主题。

---

MkDocs包括一些[内置主题]以及各种[第三方主题]，所有这些都可以使用[额外的CSS或JavaScript][docs_dir]轻松定制，或者覆盖主题的[custom_dir]。 您也可以为你的文档从头开始创建自己的[自定义主题]。

要使用MkDocs中包含的主题，只需将其添加到您的`mkdocs.yml`配置文件中。

    theme: readthedocs

使用下面的[内置主题]中列出的任一一种替换掉[`readthedocs`](#readthedocs)。

要创建新的自定义主题，请参阅[自定义主题][custom theme]页面，或者更多地自定义现有主题，请参阅下面的[自定义主题][customize]部分。

## 内置主题

### mkdocs

MkDocs的默认主题，使用修改过的[Bootstrap]构建而成，支持MkDocs的大部分功能。官方只支持两级导航。参见 #1107）

![mkdocs](../img/mkdocs.png)

除了默认的[主题配置选项]之外，`mkdocs`主题还支持以下选项：

* __`highlightjs`__：JavaScript库在代码块中突出显示源代码。 默认值：`True`。

* __`hljs_style`__：highlight.js库提供79种不同的[样式]（颜色变化），用于突出显示代码块中的源代码。 将其设置为所需样式的名称。 默认值：`github`。

* __`hljs_languages`__：them.默认情况下，highlight.js仅支持23种常用语言。在此列出额外需要支持的语言。

        theme:
            name: mkdocs
            highlightjs: true
            hljs_languages:
                - yaml
                - rust

* __`shortcuts`__：定义键盘快捷键。

        theme:
            name: mkdocs
            shortcuts:
                help: 191    # ?
                next: 78     # n
                previous: 80 # p
                search: 83   # s

    所有值都是数字键代码。 最好使用所有键盘上都有的键。 您可以使用<https://keycode.info/>来确定给定的键的代码。

    * __`help`__：显示一个列出键盘快捷键的帮助模式。默认：`191`（＆quest;）

    * __`next`__：导航到“下一页”。 默认值：`78`（n）

    * __`previous`__：导航到“上一页”。 默认值：`80`（p）

    * __`search`__：显示搜索模式。 默认值：`83`（s）

[styles]: https://highlightjs.org/static/demo/

### readthedocs

[Read the Docs]服务使用的默认主题的克隆，它提供与其父主题相同的受限制功能集。 与其父主题一样，仅支持两个级别的导航。

![ReadTheDocs](../img/readthedocs.png)

除了默认的[主题配置选项]之外，`readthedocs`主题还支持以下选项：

* __`highlightjs`__：使用[highlight.js] JavaScript库在代码块中突出显示源代码。 默认值：`True`。

* __`hljs_languages`__：默认情况下，highlight.js仅支持23种常用语言。在此列出额外需要支持的语言。

        theme:
            name: readthedocs
            highlightjs: true
            hljs_languages:
                - yaml
                - rust

* __`include_homepage_in_sidebar`__：列出侧栏菜单中的主页。由于MkDocs要求在“nav”配置选项中列出主页，因此此设置允许在侧栏中包含或排除主页。请注意，站点名称/Logo始终链接到主页。默认：`True`。

* __`prev_next_buttons_location`__：`bottom`, `top`, `both` 和 `none`中的一个值。相应地显示“下一步”和“上一步”按钮。 默认值：`bottom`。

* __`navigation_depth`__：侧栏中导航树的最大深度。 默认值：`4`。

* __`collapse_navigation`__：仅包含当前页面侧栏中的页面部分标题。 默认值：`True`。

* __`titles_only`__：仅包含侧栏中的页面标题，不包括所有页面的所有节标题。 默认值：`False`。

* __`sticky_navigation`__：如果为True，则在滚动页面时使侧边栏与主页面内容一起滚动。 默认值：`True`。

### 第三方主题

可以在MkDocs[community wiki]中找到第三方主题列表。 如果您已创建自己的，请随时将其添加到列表中。

## 自定义主题

如果您想对现有主题进行一些调整，则无需从头开始创建自己的主题。对于只需要一些CSS和（或）JavaScript的小调整，您可以使用[docs_dir]。但是，对于更复杂的自定义（包括覆盖模板），您需要使用主题的[custom_dir]设置。

### 使用docs_dir

[extra_css]和[extra_javascript]配置选项可用于对现有主题进行调整和自定义。要使用它们，您只需要在[文档目录]中包含CSS或JavaScript文件。

例如，要更改文档中标题的颜色，请创建一个名为`extra.css`的文件，并将其放在文档Markdown旁边。 在该文件中添加以下CSS。

```CSS
h1 {
  color: red;
}
```

!!! note

    如果您使用[ReadTheDocs]部署文档。 您需要明确列出要包含在配置中的CSS和JavaScript文件。为此，请将以下内容添加到mkdocs.yml中。

        extra_css: [extra.css]

在进行这些更改之后，当你运行`mkdocs serve`后将能看到效果。运行该命令后将会，CSS所做的改变将自动更新。

!!! note

    Any extra CSS or JavaScript files will be added to the generated HTML document after the page content. If you desire to include a JavaScript library, you may have better success including the library by using the theme [custom_dir].
    在页面内容之后，任何额外的CSS或JavaScript文件都将添加到生成的HTML文档中。如果您希望包含JavaScript库，则可以通过使用主题的[custom_dir]获得更好支持。

### 使用主题的custom_dir

主题的[custom_dir]配置选项可用于指向覆盖父主题目录中的文件。父主题将是主题中[name]配置选项定义的主题。 `custom_dir`中与父主题中的文件同名的任何文件都将替换父主题中同名文件。`custom_dir`中的任何其他文件都将添加到父主题中。 `custom_dir`的内容应该镜像父主题的目录结构。您可以包含模板，JavaScript文件，CSS文件，图像，字体或主题中包含的任何其他媒体文件。

!!! Note

    为此，主题的`name`设置必须设置为已知的已安装主题。 如果`name`设置被设置为`null`（或未定义[custom theme]。

For example, the [mkdocs] theme ([browse source]), contains the following directory structure (in part):
例如，[mkdocs]主题([查看源代码])包含以下目录结构（部分）：

```nohighlight
- css\
- fonts\
- img\
  - favicon.ico
  - grid.png
- js\
- 404.html
- base.html
- content.html
- nav-sub.html
- nav.html
- toc.html
```

要覆盖该主题中包含的任何文件，请在`docs_dir`旁边创建一个新目录：

```bash
mkdir custom_theme
```

然后将您的`mkdocs.yml`配置文件指向新目录：

```yaml
theme:
    name: mkdocs
    custom_dir: custom_theme/
```

要覆盖404错误页面（“找不到文件”），请将名为`404.html`的新模板文件添加到`custom_theme`目录中。 有关可以包含在模板中的内容的信息，请查看用于构建[自定义主题]的文档。

要覆盖favicon，您可以在`ustom_theme/img/favicon.ico`中添加一个新的图标文件。

要包含JavaScript库，请将库复制到`custom_theme/js/`目录。

您的目录结构现在应如下所示：

```nohighlight
- docs/
  - index.html
- custom_theme/
  - img/
    - favicon.ico
  - js/
    - somelib.js
  - 404.html
- config.yml
```

!!! Note

    父主题中包含的（在`name`中定义）但不包括在`custom_dir`中的任何文件将继续使用。`custom_dir`只会覆盖/替换父主题中的文件。 如果要删除文件或从头开始构建主题，则应查看用于构建[自定义主题]的文档。

#### 覆盖模板块

内置主题在模板块中实现了许多部分，可以在`main.html`模板中单独覆盖。 只需在`custom_dir`中创建一个`main.html`模板文件，并在该文件中定义替换块。 只要确保`main.html`扩展`base.html`。 例如，要更改MkDocs主题的标题，替换的`main.html`模板将包含以下内容：

```django
{% extends "base.html" %}

{% block htmltitle %}
<title>Custom title goes here</title>
{% endblock %}
```

在上面的例子中，将使用自定义`main.html`文件中定义的`htmltitle`块代替父主题中定义的默认`htmltitle`块。 您可以根据需要重新定义任意数量的块，只要这些块在父级中定义即可。 例如，您可以将Google Analytics脚本替换为不同服务的脚本，或者将搜索功能替换为您自己的搜索功能。 您需要查询正在使用的父主题，以确定可以覆盖的块。 MkDocs和ReadTheDocs主题提供以下块：

* `site_meta`：包含文档头中的元标记。
* `htmltitle`：包含文档头中的页面标题。
* `styles`：包含样式表的链接标记。
* `libs`：包含页眉中包含的JavaScript库（jQuery等）。
* `scripts`：包含应在页面加载后执行的JavaScript脚本。
* `analytics`：包含分析脚本。
* `extrahead`：`<head>`中的空块，用于插入自定义标记/脚本/等。
* `site_name`：包含导航栏中的站点名称。
* `site_nav`：包含导航栏中的站点导航。
* `search_box`：包含导航栏中的搜索框。
* `next_prev`：包含导航栏中的下一个和上一个按钮。
* `repo`：包含导航栏中的GitHub存储库链接。
* `content`：包含页面的页面内容和目录。
* `footer`：包含页脚。

您可能需要查看源模板文件，以确保您的修改将适用于站点的结构。 有关可在自定义块中使用的变量列表，请参见[模板变量]。 有关块的更完整说明，请参阅[Jinja文档]。

#### 组合custom_dir和模板块

将JavaScript库添加到`custom_dir`将使其可用，但不会将其包含在MkDocs生成的页面中。因此，需要从HTML添加链接到库。

启动上面的目录结构（截断）：

```nohighlight
- docs/
- custom_theme/
  - js/
    - somelib.js
- config.yml
```

一个指向`custom_theme/js/somelib.js`文件的链接需要添加到模板中。 由于`somelib.js`是一个JavaScript库，它在逻辑上会进入`libs`块。 但是，仅包含新脚本的新`libs`块将替换父模板中定义的块，并且将删除父模板中库的任何链接。 为了避免破坏模板，可以使用[super block]从块中调用`super`：

```django
{% extends "base.html" %}

{% block libs %}
    {{ super() }}
    <script src="{{ base_url }}/js/somelib.js"></script>
{% endblock %}
```

请注意，[base_url]模板变量用于确保链接始终相对于当前页面。

现在生成的页面将包含指向模板提供的库的链接以及`custom_dir`中包含的库。`custom_dir`中包含的任何其他CSS文件也是如此。

[browse source]: https://github.com/mkdocs/mkdocs/tree/master/mkdocs/themes/mkdocs
[内置主题]: #built-in-themes
[Bootstrap]: https://getbootstrap.com/
[theme configuration options]: ./configuration.md#theme
[Read the Docs]: https://readthedocs.org/
[community wiki]: https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes
[自定义主题]: ./custom-themes.md
[customize]: #customizing-a-theme
[docs_dir]: #using-the-docs_dir
[documentation directory]: ./configuration.md#docs_dir
[extra_css]: ./configuration.md#extra_css
[extra_javascript]: ./configuration.md#extra_javascript
[Jinja documentation]: http://jinja.pocoo.org/docs/dev/templates/#template-inheritance
[mkdocs]: #mkdocs
[ReadTheDocs]: ./deploying-your-docs.md#readthedocs
[Template Variables]: ./custom-themes.md#template-variables
[custom_dir]: ./configuration.md#custom_dir
[name]: ./configuration.md#name
[third party themes]: #third-party-themes
[super block]: http://jinja.pocoo.org/docs/dev/templates/#super-blocks
[base_url]: ./custom-themes.md#base_url
[highlight.js]: https://highlightjs.org/
