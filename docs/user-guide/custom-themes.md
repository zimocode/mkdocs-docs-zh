# 自定义主题

创建和分发自定义主题的指南。

---

!!! Note "注意："

    如果你只是寻找第三方主题，请转到MkDocs在GitHub上的[wiki页面](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes)。如果你想共享自己的创作的主题，也可以在该页面上提交。

创建新主题时，您可以按照本指南中的步骤从头开始创建一个，也可以下载`mkdocs-basic-theme`，在此基础开始创作。该主题是一个基本的，但是包含了一个主题所必须的所有文件。 **您可以在[GitHub](https://github.com/mkdocs/mkdocs-basic-theme)**上找到此基本主题。它包含代码中的详细注释，用于描述不同的功能及其用法。

## 创建自定义主题

自定义主题至少包含一个`main.html`[Jinja2 template]文件，要求该文件存放于一个*不是*[docs_dir]子目录的目录中。 在`mkdocs.yml`中，将[theme.custom_dir]选项设置为包含`main.html`的目录的路径。 路径应该相对于配置文件。 例如，给定此示例项目布局：

```no-highlight
mkdocs.yml
docs/
    index.md
    about.md
custom_theme/
    main.html
    ...
```

您将在`mkdocs.yml`中包含以下设置以使用自定义主题目录：

```yaml
theme:
    name: null
    custom_dir: 'custom_theme/'
```

!!! Note "注意："

    通常，在构建自己的自定义主题时，[theme.name]配置设置将设置为`null`。 但是，如果theme.[custom_dir]配置值与现有主题结合使用，则theme.[custom_dir]可用于仅替换内置主题的特定部分。 例如，使用上面的布局，如果你设置`name："mkdocs"`那么主题中的`main.html`文件。[custom_dir]将替换`mkdocs`主题中的同名文件，否则 `mkdocs`主题将保持不变。如果要对现有主题进行小幅调整，这将非常有用。

    有关更具体的信息，请参阅[样式化您的文档]。

[样式化您的文档]: ./styling-your-docs.md#using-the-theme-custom_dir
[theme.custom_dir]: ./configuration.md#custom_dir
[theme.name]: ./configuration.md#name
[docs_dir]:./configuration.md#docs_dir

## 基本主题

最简单的`main.html`文件如下：

```django
<!DOCTYPE html>
<html>
  <head>
    <title>{% if page.title %}{{ page.title }} - {% endif %}{{ config.site_name }}</title>
  </head>
  <body>
    {{ page.content }}
  </body>
</html>
```

使用`{{page.content}}`标签指代每个页面的正文内容。 样式表和脚本可以像普通HTML文件一样进入此主题。 导航栏和目录也可以分别通过`nav`和`toc`对象自动生成和包含。 如果您希望编写自己的主题，建议您从[内置主题]之一开始，并相应地进行修改。

!!! Note "注意："

    由于MkDocs使用[Jinja]作为其模板引擎，因此您可以访问Jinja的所有功能，包括[模板继承]。您可能会注意到MkDocs中包含的主题广泛使用模板继承和块，允许用户轻松地从主题[custom_dir]覆盖模板的一小部分。 因此，内置主题在`base.html`文件中实现，`main.html`扩展。虽然不是必需的，但鼓励第三方模板作者遵循类似的模式，并且可能希望定义与内置主题中使用的相同[块]以保持一致性。

[Jinja]: http://jinja.pocoo.org/
[模板继承]: http://jinja.pocoo.org/docs/dev/templates/#template-inheritance
[theme_dir]: ./styling-your-docs.md#using-the-theme_dir
[blocks]: ./styling-your-docs.md#overriding-template-blocks

## 模板变量

主题中的每个模板都使用模板上下文构建。 这些是主题可用的变量。 上下文取决于正在构建的模板。目前，模板是使用全局上下文或特定于页面的上下文构建的。全局上下文用于不代表单个Markdown文档的HTML页面，例如404.html页面或search.html。

### 全局上下文

以下变量可在任何模板上全局使用。

#### config

`config`变量是从`mkdocs.yml`配置文件生成的MkDocs配置对象的实例。 虽然您可以使用任何配置选项，但一些常用选项包括：

* [config.site_name](./configuration.md#site_name)
* [config.site_url](./configuration.md#site_url)
* [config.site_author](./configuration.md#site_author)
* [config.site_description](./configuration.md#site_description)
* [config.extra_javascript](./configuration.md#extra_javascript)
* [config.extra_css](./configuration.md#extra_css)
* [config.repo_url](./configuration.md#repo_url)
* [config.repo_name](./configuration.md#repo_name)
* [config.copyright](./configuration.md#copyright)
* [config.google_analytics](./configuration.md#google_analytics)

#### nav

`nav`变量用于创建文档的导航。 `nav`对象是由[nav]配置设置定义的[导航对象](#navigation-objects)的可迭代对象。

[nav]: configuration.md#nav

除了[navigation objects](#navigation-objects)的可迭代之外，`nav`对象还包含以下属性：

##### nav.homepage

网站主页的[页面](#page)对象。

##### nav.pages

导航中包含的所有[页面](#page)对象的平面列表。此列表不一定是所有网站页面的完整列表，因为它不包含导航中未包含的页面。此列表与用于所有“下一页”和“上一页”链接的页面列表和顺序相匹配。有关所有页面的列表，请使用[pages](#page)模板变量。

##### Nav变量示例

以下是将第一级和第二级导航输出为嵌套列表的基本用法示例。

```django
{% if nav|length>1 %}
    <ul>
    {% for nav_item in nav %}
        {% if nav_item.children %}
            <li>{{ nav_item.title }}
                <ul>
                {% for nav_item in nav_item.children %}
                    <li class="{% if nav_item.active%}current{% endif %}">
                        <a href="{{ nav_item.url|url }}">{{ nav_item.title }}</a>
                    </li>
                {% endfor %}
                </ul>
            </li>
        {% else %}
            <li class="{% if nav_item.active%}current{% endif %}">
                <a href="{{ nav_item.url|url }}">{{ nav_item.title }}</a>
            </li>
        {% endif %}
    {% endfor %}
    </ul>
{% endif %}
```

#### base_url

`base_url`提供了MkDocs项目根目录的相对路径。虽然可以通过将其预先添加到本地相对URL来直接使用它，但最好使用[url](#url)模板过滤器，它更灵活地应用`base_url`。

#### mkdocs_version

包含当前的MkDocs版本。

#### build_date_utc

一个Python日期时间对象，表示以UTC格式构建文档的日期和时间。 这对于显示文档最近的更新非常有用。

#### pages

包含项目中*所有*页面的[page](#page)对象列表。该列表是一个平面列表，所有页面按字母顺序按目录和文件名排序。请注意，索引页面排在目录中的顶部。 此列表可以包含未包含在全局[导航](#nav)中的页面，并且可能与该导航中的页面顺序不匹配。

#### page

在未从Markdown源文件呈现的模板中，`page`变量为`None`。 在从Markdown源文件呈现的模板中，`page`变量包含`page`对象。 相同的`page`对象在全局[navigation](#nav)和[pages](#pages)模板变量中用作`page` [导航对象](＃navigation-objects)。

所有`page`对象都包含以下属性：

##### page.title

包含当前页面的标题。

##### page.content

呈现Markdown为HTML，这是文档的内容。

##### page.toc

一个可迭代的对象，表示页面的目录。 `toc`中的每个项目都是`AnchorLink`，它包含以下属性：

* `AnchorLink.title`: 该项目的文本。
* `AnchorLink.url`: 指向该项的URL中的以#开头的片段。
* `AnchorLink.level`: 项目的从零开始的级别。
* `AnchorLink.children`: 任何子项的可迭代。

以下示例将显示页面的目录的前两个级别。

```django
<ul>
{% for toc_item in page.toc %}
    <li><a href="{{ toc_item.url }}">{{ toc_item.title }}</a></li>
    {% for toc_item in toc_item.children %}
        <li><a href="{{ toc_item.url }}">{{ toc_item.title }}</a></li>
    {% endfor %}
{% endfor %}
</ul>
```

##### page.meta

包含在markdown页面顶部的元数据的映射。 在这个例子中，我们在页面标题上面定义了一个`source`属性。

```no-highlight
source: generics.py
        mixins.py

# Page title

Content...
```

模板可以使用`meta.source`变量访问页面的元数据。 然后，可以使用它链接到与文档页面相关的源文件。

```django
{% for filename in page.meta.source %}
  <a class="github" href="https://github.com/.../{{ filename }}">
    <span class="label label-info">{{ filename }}</span>
  </a>
{% endfor %}
```

##### page.url

页面的URL相对于MkDocs`site_dir`。 预计这将与[url](#url)过滤器一起使用，以确保URL相对于当前页面。

```django
<a href="{{ page.url|url }}">{{ page.title }}</a>
```

[base_url]: #base_url

##### page.abs_url

来自服务器根目录的页面的绝对URL，由分配给[site_url]配置设置的值确定。 该值包括`site_url`中包含的任何子目录，但不包括域。 [base_url]不应与此变量一起使用。

例如，如果`site_url：https：//example.com/`，那么页面`foo.md`的`page.abs_url`的值将是`/foo/`。但是，如果`site_url：https：//example.com/bar/`，那么页面`foo.md`的`page.abs_url`的值将是`/bar/foo/`。

[site_url]: ./configuration.md#site_url

##### page.canonical_url

通过分配给[site_url]配置设置的值确定的当前页面的完整，规范URL。 该值包括域和“site_url”中包含的任何子目录。 [base_url]不应与此变量一起使用。

##### page.edit_url

源存储库中源页面的完整URL。 通常用于提供编辑源页面的链接。 [base_url]不应与此变量一起使用。

##### page.is_homepage

对网站的主页评估为`True`，对所有其他页面评估为`False`。 这可以与`page`对象的其他属性一起使用来改变行为。 例如，要在主页上显示不同的标题：

```django
{% if not page.is_homepage %}{{ page.title }} - {% endif %}{{ site_name }}
```

##### page.previous_page

上一页的页面对象或“无”。如果当前页面是站点导航中的第一项，或者当前页面根本不包含在导航中，则该值将为“无”。 当值是页面对象时，用法与“page”的用法相同。

##### page.next_page

下一页的页面对象或“无”。如果当前页面是站点导航中的最后一项，或者当前页面根本不包含在导航中，则该值将为“无”。 当值是页面对象时，用法与“page”的用法相同。

##### page.parent

[网站导航](#nav)中页面的直接父级。 如果页面位于顶层，则为`None`。

##### page.children

页面不包含子项，属性始终为`None`。

##### page.active

当`True`时，表示该页面是当前查看的页面。 默认为`False`。

##### page.is_section

表示导航对象是“section”对象。 页面对象始终为`False`。

##### page.is_page

表示导航对象是“页面”对象。 页面对象始终为`True`。

##### page.is_link

表示导航对象是“链接”对象。 页面对象始终为`False`。

### 导航对象

[nav](#nav)模板变量中包含的导航对象可以是[section](#section)对象，[page](#page)对象和[link](#link)对象之一。 虽然节点对象可能包含嵌套的导航对象，但页面和链接却不包含。

页面对象是用于当前[页面]（＃页面）的完整页面对象，具有所有可用的相同属性。Section和Link对象包含以下定义的那些属性的子集：

#### Section

`section`导航对象在导航中定义命名部分，并包含子导航对象的列表。请注意，这些部分不包含URL，也不是任何类型的链接。但是，默认情况下，如果主题选择这样做，MkDocs会将索引页面排序到顶部，并且第一个子节点可能会用作节点的URL。

`section`对象提供以下属性：

##### section.title

节点的标题。

##### section.parent

如果该节点位于顶层，则该节点的直接父级或`None`。

##### section.children

所有子导航对象的可迭代。 子级可能包括嵌套的部分，页面和链接。

##### section.active

当`True`时，表示此节点的子页面是当前页面，可用于将该部分突出显示为当前查看的节点。 默认为`False`。

##### section.is_section

表示导航对象是"节点"对象。 节点对象始终为`True`。

##### section.is_page

表示导航对象是“页面”对象。 对于节点对象，始终为`False`。

##### section.is_link

表示导航对象是“链接”对象。 对于节点对象，始终为`False`。

#### Link

`link`导航对象包含一个不指向内部MkDocs页面的链接。 `link`对象提供以下属性：

##### link.title

链接的标题。 这通常用作链接的标签。

##### link.url

链接指向的URL。 URL应该始终是绝对URL，并且不需要预先设置`base_url`。

##### link.parent

链接的直接父级。 如果链接位于顶层，则为`None`。

##### link.children

链接不包含子项，属性始终为`None`。

##### link.active

外部链接不能“活动”，属性始终为`False`。

##### link.is_section

表示导航对象是“section”对象。 链接对象始终为`False`。

##### link.is_page

表示导航对象是“页面”对象。 链接对象始终为`False`。

##### link.is_link

表示导航对象是“链接”对象。 链接对象始终为`True`。

### 额外的上下文

可以使用[`extra`](/user-guide/configuration.md＃extra)配置选项将其他变量传递给模板。 这是一组键值对，可以使自定义模板更加灵活。

例如，这可以用于包括所有页面的项目版本以及与项目相关的链接列表。 这可以通过以下`extra`配置来实现：

```yaml
extra:
    version: 0.13.0
    links:
        - https://github.com/mkdocs
        - https://docs.readthedocs.org/en/latest/builds.html#mkdocs
        - https://www.mkdocs.org/
```

然后在自定义主题中显示此HTML。

```django
{{ config.extra.version }}

{% if config.extra.links %}
  <ul>
  {% for link in config.extra.links %}
      <li>{{ link }}</li>
  {% endfor %}
  </ul>
{% endif %}
```

## 模板过滤器

除了Jinja的默认过滤器之外，还可以在MkDocs模板中使用以下自定义过滤器：

### url

规范化URL。 绝对URL通过不变的方式传递。如果URL是相对的并且模板上下文包括页面对象，则相对于页面对象返回URL。否则，返回的URL前缀为[base_url](#base_url)。

```django
<a href="{{ page.url|url }}">{{ page.title }}</a>
```

### tojson

Safety convert a Python object to a value in a JavaScript script.
用于JavaScript中，安全地转换python对象的值。

```django
<script>
    var mkdocs_page_name = {{ page.title|tojson|safe }};
</script>
```

## 主题中的搜索

截至MkDocs版本* 0.17 *客户端搜索支持已通过`search`插件添加到MkDocs。主题需要为插件提供一些与主题一起使用的东西。

虽然默认情况下会激活`search`插件，但用户可以禁用该插件，主题应该考虑到这一点。建议主题模板使用对插件的检查来包装搜索特定标记：

```django
{% if 'search' in config['plugins'] %}
    search stuff here...
{% endif %}
```

在其最基本的功能上，搜索插件将只提供一个索引文件，该文件不过是包含所有页面内容的JSON文件。 主题需要在客户端实现自己的搜索功能。 但是，通过一些设置和必要的模板，插件可以提供基于[lunr.js]的完整功能的客户端搜索工具。

需要将以下HTML添加到主题中，以便提供的JavaScript能够正确加载搜索脚本并从当前页面进行搜索结果的相对链接。

```django
<script>var base_url = '{{ base_url }}';</script>
```

使用正确配置的设置，模板中的以下HTML将为您的主题添加完整的搜索实现。

```django
<h1 id="search">Search Results</h1>

<form action="search.html">
  <input name="q" id="mkdocs-search-query" type="text" >
</form>

<div id="mkdocs-search-results">
  Sorry, page not found.
</div>
```

插件中的JavaScript通过查找上述HTML中使用的特定ID来工作。 用户键入搜索查询的表单输入必须使用`id =“mkdocs-search-query”来标识，并且必须使用`id =“mkdocs-search-results”来识别将放置结果的div。

该插件支持在[theme的配置文件]`mkdocs_theme.yml`中设置以下选项：

### include_search_page

确定搜索插件是否期望主题通过位于`search / search.html`的模板提供专用搜索页面。

当`include_search_page`设置为`true`时，搜索模板将在`search / search.html`上构建并可用。 这个方法被`readthedocs`主题使用。

当`include_search_page`设置为`false`或未定义时，预期主题提供一些其他显示搜索结果的机制。 例如，`mkdocs`主题通过模态在任何页面上显示结果。

### search_index_only

确定搜索插件是仅生成搜索索引还是生成完整的搜索解决方案。

当`search_index_only`设置为`false`时，搜索插件通过添加自己的`templates`目录（优先级低于主题）来修改Jinja环境，并将其脚本添加到`extra_javascript`配置设置中。

当`search_index_only`设置为`true`或未定义时，搜索插件不会对Jinja环境进行任何修改。使用提供的索引文件的完整解决方案是主题的责任。

搜索索引将写入[site_dir]中的`search / search_index.json`JSON文件。该文件中包含的JSON对象最多可包含三个对象。

```json
{
    config: {...},
    data: [...],
    index: {...}
}
```

如果存在，`config`对象包含在`plugings.search`下用户的`mkdocs.yml`配置文件中为插件定义的配置选项的键/值对。 `config`对象是MkDocs版本* 1.0 *中的新对象。

`data`对象包含文档对象列表。 每个文档对象由“位置”（URL），“标题”和“文本”组成，可用于创建搜索索引和/或显示搜索结果。

如果存在，`index`对象包含一个预构建的索引，它为较大的站点提供性能改进。 请注意，仅当用户明确启用[prebuild_index]配置选项时，才会创建预构建索引。 主题应该期望索引不存在，但可以选择在可用时使用索引。 `index`对象是MkDocs版本* 1.0 *中的新对象。

[Jinja2 template]: http://jinja.pocoo.org/docs/dev/
[built-in themes]: https://github.com/mkdocs/mkdocs/tree/master/mkdocs/themes
[theme's configuration file]: #theme-configuration
[lunr.js]: https://lunrjs.com/
[site_dir]: configuration.md#site_dir
[prebuild_index]: configuration.md#prebuild_index

## 打包主题

MkDocs利用[Python packaging]来分发主题。 这有一些要求。

要查看包含一个主题的包的示例，请参阅[MkDocs Bootstrap主题];查看包含许多主题的包，请参阅[MkDocs Bootswatch主题]。

!!! Note "注意："

    打包主题并不是绝对必要的，因为整个主题可以包含在`custom_dir`中。如果你创造了一个“一次性主题”，那就足够了。 但是，如果您打算将主题分发给其他人使用，则打包主题具有一些优势。通过打包您的主题，您的用户可以更轻松地安装它，然后他们可以利用[custom_dir]对您的主题进行调整，以更好地满足他们的需求。

[Python packaging]: https://packaging.python.org/en/latest/
[MkDocs Bootstrap主题]: https://mkdocs.github.io/mkdocs-bootstrap/
[MkDocs Bootswatch主题]: https://mkdocs.github.io/mkdocs-bootswatch/

### 包布局

建议主题使用以下布局。 顶级目录中的两个文件名为`MANIFEST.in`和主题目录旁边的`setup.py`，其中包含一个空的`__init __。py`文件，一个主题配置文件（`mkdocs-theme.yml`），以及模板和媒体文件。

```no-highlight
.
|-- MANIFEST.in
|-- theme_name
|   |-- __init__.py
|   |-- mkdocs-theme.yml
|   |-- main.html
|   |-- styles.css
`-- setup.py
```

`MANIFEST.in`文件应包含以下内容，但更新了theme_name，并在include中添加了任何额外的文件扩展名。

```no-highlight
recursive-include theme_name *.ico *.js *.css *.png *.html *.eot *.svg *.ttf *.woff
recursive-exclude * __pycache__
recursive-exclude * *.py[co]
```

`setup.py`应该包含以下文本以及下面描述的修改。

```python
from setuptools import setup, find_packages

VERSION = '0.0.1'


setup(
    name="mkdocs-themename",
    version=VERSION,
    url='',
    license='',
    description='',
    author='',
    author_email='',
    packages=find_packages(),
    include_package_data=True,
    entry_points={
        'mkdocs.themes': [
            'themename = theme_name',
        ]
    },
    zip_safe=False
)
```

填写URL，许可证，说明，作者和作者电子邮件地址。

该名称应遵循惯例`mkdocs-themename`（如`mkdocs-bootstrap`和`mkdocs-bootswatch`），从MkDocs开始，使用连字符分隔单词并包括主题名称。

该文件的大部分内容可以未经编辑。 我们需要更改的最后一部分是entry_points。 这就是MkDocs如何找到您在包中包含的主题。左侧的名称是用户将在其mkdocs.yml中使用的名称，右侧的名称是包含主题文件的目录。

您在本节开头使用main.html文件创建的目录应包含所有其他主题文件。 最低要求是它包含主题的“main.html”。 它**必须**还包括一个应该为空的`__init __。py`文件，这个文件告诉Python该目录是一个包。

### 主题配置

打包的主题需要包含名为`mkdocs_theme.yml`的配置文件，该文件放在模板文件的根目录中。该文件应包含主题的默认配置选项。 但是，如果主题不提供配置选项，则仍需要该文件，并且可以留空。

主题作者可以自由定义任何认为必要的任意选项，并且这些选项将在模板中提供以控制行为。例如，主题可能希望侧边栏可选，并在`mkdocs_theme.yml`文件中包含以下内容：

```yaml
show_sidebar: true
```

然后在模板中，可以引用该配置选项：

```django
{% if config.theme.show_sidebar %}
<div id="sidebar">...</div>
{% endif %}
```

用户可以覆盖项目的`mkdocs.yml`配置文件中的默认值：

```yaml
theme:
    name: themename
    show_sidebar: false
```

除了主题定义的任意选项外，MkDoc还定义了一些改变其行为的特殊选项：

!!! block ""

    #### static_templates

    此选项镜像同名的[theme]配置选项，并允许主题设置一些默认值。 请注意，虽然用户可以向此列表添加模板，但用户无法删除主题配置中包含的模板。

    #### extends

    定义此主题继承的父主题。 该值应该是父主题的字符串名称。 正常的Jinja继承规则适用。

插件还可以定义一些选项，允许主题通知插件它期望的插件选项集。 请参阅文档，了解您可能希望在主题中支持的任何插件。

### 分发主题

通过上述更改，您的主题现在应该可以安装了。 如果你仍然和setup.py在同一目录下，可以使用pip install。来完成这个。

大多数Python包，包括MkDocs，都是在PyPI上发布的。 为此，您应该运行以下命令。

```no-highlight
python setup.py register
```

如果您没有设置帐户，系统会提示您创建帐户。

有关更详细的指南，请参阅[打包和分发项目]的官方Python打包文档。

[Packaging and Distributing Projects]: https://packaging.python.org/en/latest/distributing/
[theme]: ./configuration.md#theme
