# MkDocs插件

安装，使用和创建MkDocs插件的指南

---

## 安装插件

在使用插件之前，必须将其安装在系统上。 如果您使用的是MkDocs附带的插件，则在安装MkDocs时会安装它。 但是，要安装第三方插件，您需要确定相应的软件包名称并使用`pip`进行安装：

```bash
pip install mkdocs-foo-plugin
```

插件成功安装后即可使用。 它只需要在配置文件中[启用](＃using-plugins)。

## 使用插件

[`plugins`] [config]配置选项应包含构建站点时要使用的插件列表。 每个“插件”必须是分配给插件的字符串名称（请参阅给定插件的文档以确定其“名称”）。 此处列出的插件必须已经[安装](#instalting-plugins)。

```yaml
plugins:
    - search
```

某些插件可能提供自己的配置选项。 如果要设置任何配置选项，则可以嵌套给定插件支持的任何选项的键/值映射（`option_name：option value`）。请注意冒号（`：`）必须遵循插件名称，然后在新行上，选项名称和值必须缩进并用冒号分隔。如果要为单个插件定义多个选项，则必须在单独的行上定义每个选项。

```yaml
plugins:
    - search:
        lang: en
        foo: bar
```

有关给定插件可用配置选项的信息，请参阅该插件的文档。

有关默认插件的列表以及如何覆盖它们，请参阅[配置文件][config]文档。

## 开发插件

像MkDocs一样，插件必须用Python编写。 通常期望每个插件将作为单独的Python模块分发，尽管可以在同一模块中定义多个插件。 至少，MkDocs插件必须由[BasePlugin]子类和指向它的[entry point]组成。

### BasePlugin

`mkdocs.plugins.BasePlugin`的子类应该定义插件的行为。该类通常包括对构建过程中的特定事件执行的操作以及插件的配置方案。

所有`BasePlugin`子类都包含以下属性：

#### config_scheme

:   配置验证实例的元组。 每个项目必须包含一个两项元组，其中第一项是配置选项的字符串名称，第二项是`mkdocs.config.config_options.BaseConfigOption`或其任何子类的实例。

    例如，以下`config_scheme`定义了三个配置选项：`foo`，它接受一个字符串; `bar`，接受一个整数; 和`baz`，它接受一个布尔值。

        class MyPlugin(mkdocs.plugins.BasePlugin):
            config_scheme = (
                ('foo', mkdocs.config.config_options.Type(mkdocs.utils.string_types, default='a default value')),
                ('bar', mkdocs.config.config_options.Type(int, default=0)),
                ('baz', mkdocs.config.config_options.Type(bool, default=True))
            )

    加载用户配置后，将使用上述方案验证配置并填写用户未提供的设置的任何默认值。验证类可以是`mkdocs.config.config_options`中提供的任何类，也可以是插件中定义的第三方子类。

    用户提供的任何验证失败或未在`config_scheme`中定义的设置都会引发`mkdocs.config.base.ValidationError`。

#### config

:   插件的配置选项字典，在配置验证完成后由`load_config`方法填充。 使用此属性可以访问用户提供的选项。

        def on_pre_build(self, config):
            if self.config['bool_option']:
                # implement "bool_option" functionality here...

所有`BasePlugin`子类都包含以下方法：

#### load_config(options)

:   从选项字典加载配置。 返回`（错误，警告）`的元组。MkDocs在配置验证期间调用此方法，并且不需要由插件调用。

#### on_&lt;event_name&gt;()

:   定义特定[事件]行为的可选方法。 插件应该在这些方法中定义它的行为。 将`<event_name>`替换为事件的实际名称。 例如，`pre_build`事件将在`on_pre_build`方法中定义。

    大多数事件接受一个位置参数和各种关键字参数。 通常期望插件修改（或替换）位置参数并返回。 如果没有返回任何内容（该方法返回“None”），则使用原始未修改的对象。简单地提供关键字参数以提供可用于确定如何修改位置参数的上下文和/或供应数据。 接受关键字参数为“** kwargs”是一种好习惯。如果在未来版本的MkDocs中为事件提供了其他关键字，则无需更改您的插件。

    例如，以下事件会向主题配置添加额外的static_template：

        class MyPlugin(BasePlugin):
            def on_config(self, config, **kwargs):
                config['theme'].static_templates.add('my_template.html')
                return config

### 事件

有三种事件：[全局事件]，[页面事件]和[模板事件]。

#### 全局事件

在构建过程的开始或结束时，每个构建都会调用一次全局事件。在这些事件中所做的任何更改都将对整个站点产生全局影响。

##### on_serve

:   只有在开发过程中使用`serve`命令时才会调用`serve`事件。 它传递给`Server`实例，可以在激活之前进行修改。 例如，可以将附加文件或目录添加到“监视”文件列表中以进行自动重新加载。

    Parameters:
    : __server:__ `livereload.Server`的实例
    : __config:__ 全局配置对象

    Returns:
    : `livereload.Server`的实例

##### on_config

:   `config`事件是第一个在构建时调用的事件，在加载和验证用户配置后立即运行。 应在此处对配置进行任何更改。

    Parameters:
    : __config:__ 全局配置对象

    Returns:
    : 全局配置对象

##### on_pre_build

:   `pre_build`事件不会改变任何变量。 使用此事件可以调用预构建脚本。

    Parameters:
    : __config:__ 全局配置对象

##### on_files

:   从`docs_dir`填充文件集合后调用`files`事件。 使用此事件可以添加，删除或更改集合中的文件。 请注意，Page对象尚未与集合中的文件对象关联。 使用[页面事件]来处理页面特定数据。

    Parameters:
    : __files:__ 全局配置对象
    : __config:__ 全局配置对象

    Returns:
    : 全局文件集合

##### on_nav

:   创建站点导航后会调用`nav`事件，并可用于更改站点导航。

    Parameters:
    : __nav:__ 全局导航对象
    : __config:__ 全局配置对象
    : __files:__ 全局文件集合

    Returns:
    : 全局导航对象

##### on_env

:   在创建Jinja模板环境之后调用`env`事件，并且可以使用它来改变Jinja环境。

    Parameters:
    : __env:__ 全局Jinja环境
    : __config:__ 全局配置对象
    : __files:__ 全局文件集合

    Returns:
    : 全局Jinja环境

##### on_post_build

:   `post_build`事件不会改变任何变量。 使用此事件来调用构建后脚本。

    Parameters:
    : __config:__ 全局配置对象

#### 模板事件

为每个非页面模板调用一次模板事件。 将为[extra_templates]配置设置中定义的每个模板以及主题中定义的任何[static_templates]调用每个模板事件。 所有模板事件在[env]事件之后和任何[页面事件]之前调用。

##### on_pre_template

:   加载主题模板后立即调用`pre_template`事件，可用于更改模板的内容。

    Parameters:
    : __template__: 模板内容为字符串
    : __template_name__: 模板的字符串文件名
    : __config:__ 全局配置对象

    Returns:
    : 模板内容为字符串

##### on_template_context

:   在为主题模板创建上下文之后立即调用`template_context`事件，并且可以仅使用该事件来更改该特定模板的上下文。

    Parameters:
    : __context__: 模板上下文变量的字典
    : __template_name__: 模板的字符串文件名
    : __config:__ 全局配置对象

    Returns:
    : 模板上下文变量的字典

##### on_post_template

:   `post_template`事件在渲染模板后调用，但在写入光盘之前调用，可用于更改模板的输出。如果返回空字符串，则跳过该模板，并且没有任何内容写入光盘。

    Parameters:
    : __output_content__: 将呈现的模板输出为字符串
    : __template_name__: 模板的字符串文件名
    : __config:__ 全局配置对象

    Returns:
    : 将呈现的模板输出为字符串

#### 页面事件

对于网站中包含的每个Markdown页面，都会调用一次页面事件。 所有页面事件在[post_template]事件之后和[post_build]事件之前调用。

##### on_pre_page

:   在对主题页面执行任何操作之前调用`pre_page`事件，并且可以使用它来更改`Page`实例。

    Parameters:
    : __page:__ `mkdocs.nav.Page` 的实例
    : __config:__ 全局配置对象
    : __files:__ 全局文件集合

    Returns:
    : `mkdocs.nav.Page` 实例

##### on_page_read_source

:   `on_page_read_source`事件可以替换默认机制，以从文件系统中读取页面源的内容。

    Parameters:
    : __page:__ `mkdocs.nav.Page`实列
    : __config:__ 全局配置对象

    Returns:
    : 页面的原始源作为unicode字符串。 如果返回“None”，则将执行文件的默认加载。

##### on_page_markdown

:   在从文件加载页面标记后，调用`page_markdown`事件，可用于更改Markdown源文本。元数据已被剥离，此时可以作为“page.meta”使用。

    Parameters:
    : __markdown:__ Markdown源文本作为字符串
    : __page:__ `mkdocs.nav.Page`实例
    : __config:__ 全局配置对象
    : __files:__ 全局文件集合

    Returns:
    : Markdown源文本作为字符串

##### on_page_content

:   在将Markdown文本呈现为HTML（但在传递给模板之前）之后调用`page_content`事件，并且可以使用该事件来更改页面的HTML主体。

    Parameters:
    : __html:__ 从Markdown源呈现的HTML作为字符串
    : __page:__ `mkdocs.nav.Page`实例
    : __config:__ 全局配置对象
    : __files:__ 全局文件集合

    Returns:
    : 从Markdown源呈现的HTML作为字符串

##### on_page_context

:   在创建页面的上下文之后调用`page_context`事件，并且该事件可用于仅更改该特定页面的上下文。

    Parameters:
    : __context__: 模板上下文变量的字典
    : __page:__ `mkdocs.nav.Page`实例
    : __config:__ 全局配置对象
    : __nav:__ 全局导航对象

    Returns:
    : 模板上下文变量的字典

##### on_post_page

:   在呈现模板之后，但在将其写入光盘之前调用`post_page`事件，并且可以使用该事件来改变页面的输出。 如果返回空字符串，则跳过该页面并且不会将任何内容写入光盘。

    Parameters:
    : __output:__ 将呈现的模板输出为字符串
    : __page:__ `mkdocs.nav.Page`实例
    : __config:__ 全局配置对象

    Returns:
    : 将呈现的模板输出为字符串

### Entry Point

插件需要打包为Python库（分布在PyPI上，与MkDocs分开），每个插件必须通过setuptools entry_point注册为插件。 将以下内容添加到`setup.py`脚本中：

```python
entry_points={
    'mkdocs.plugins': [
        'pluginname = path.to.some_plugin:SomePluginClass',
    ]
}
```

`pluginname`将是用户使用的名称（在配置文件中）；`path.to.some_plugin：SomePluginClass`将是可导入的插件本身（`from path.to.some_plugin import SomePluginClass`）其中`SomePluginClass`是 [BasePlugin]的子类，定义插件行为。 当然，多个插件类可以存在于同一个模块中。 只需将每个定义为单独的entry_point即可。

```python
entry_points={
    'mkdocs.plugins': [
        'featureA = path.to.my_plugins:PluginA',
        'featureB = path.to.my_plugins:PluginB'
    ]
}
```

请注意，注册插件不会激活它。 如果通过配置，用户仍然需要告诉MkDocs使用。

[BasePlugin]:#baseplugin
[config]: configuration.md#plugins
[entry point]: #entry-point
[env]: #on_env
[events]: #events
[extra_templates]: configuration.md#extra_templates
[Global Events]: #global-events
[Page Events]: #page-events
[post_build]: #on_post_build
[post_template]: #on_post_template
[static_templates]: configuration.md#static_templates
[Template Events]: #template-events
