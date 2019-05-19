# MkDocs中文文档

使用Markdown来构建项目文档

---

## 概述

MkDocs是一个**快速**、**简单**、**华丽**的静态网站生成器，适用于构建项目文档。文档源文件以Markdown编写，并使用一个YAML文件来进行配置。


### 在任何地方部署

MkDocs生成完全静态的HTML网站，您可以将其部署到GitHub pages、Amzzon S3或你自己选择的其它[任意地方][deploy]。

###很棒的主题

MkDocs有一堆很好看的主题。 官方内置了两个主题：[mkdocs]和[readthedocs]，也可以从[MkDocs wiki]中选择第三方主题，或者[自定义主题][build your own]。 

### 实时预览你的网站

当你写作时，内置的开发服务可以帮助你预览显示效果。当文档有改动时，甚至还可以自动载入并刷新你的浏览器。

###易于定制

通过自定义主题，让您的项目文档以您希望的方式呈现。

---

## Installation
## 安装

### Install with a Package Manager
### 使用包管理器安装

If you have and use a package manager (such as [apt-get], [dnf], [homebrew],
[yum], [chocolatey], etc.) to install packages on your system, then you may
want to search for a "MkDocs" package and, if a recent version is available,
install it with your package manager (check your system's documentation for
details). That's it, you're done! Skip down to [Getting Started](#getting-started).
如果您拥有并使用包管理器（例如[apt-get]，[dnf]，[homebrew]，[yum]，[chocolatey]等）来在您的系统上安装包，那么您可能需要搜索 “MkDocs”软件包，如果有最新版本，请与软件包管理器一起安装（有关详细信息，请查看系统文档）。 就是这样，你完成了！ 省略到[入门]（＃getting-started）。

If your package manager does not have a recent "MkDocs" package, you can still
use your package manager to install "Python" and "pip". Then you can use pip to
[install MkDocs](#installing-mkdocs).
如果您的软件包管理器没有最新的“MkDocs”软件包，您仍然可以使用软件包管理器来安装“Python”和“pip”。 然后你可以使用pip来[安装MkDocs]（#instalming-mkdocs）。

[apt-get]: https://help.ubuntu.com/community/AptGet/Howto
[homebrew]: https://brew.sh/
[dnf]: https://dnf.readthedocs.io/en/latest/index.html
[yum]: http://yum.baseurl.org/
[chocolatey]: https://chocolatey.org/

### Manual Installation
### 手动安装

In order to manually install MkDocs you'll need [Python] installed on your
system, as well as the Python package manager, [pip]. You can check if you have
these already installed from the command line:
为了手动安装MkDocs，您需要在系统上安装[Python]，以及Python包管理器[pip]。您可以从命令行检查是否已安装这些：

```bash
$ python --version
Python 2.7.14
$ pip --version
pip 18.1 from /usr/local/lib/python2.7/site-packages/pip (python 2.7)
```

MkDocs supports Python versions 2.7.9+, 3.4, 3.5, 3.6, 3.7, and pypy.
MkDocs支持Python版本2.7.9 +，3.4,3.5,3.6,3.7和pypy。

#### Installing Python
#### 安装Python

Install [Python] by downloading an installer appropriate for your system from
[python.org] and running it.
通过从[python.org]下载适用于您系统的安装程序并运行它来安装[Python]。

!!! Note

    If you are installing Python on Windows, be sure to check the box to have
    Python added to your PATH if the installer offers such an option (it's
    normally off by default).
    如果您在Windows上安装Python，请确保选中此框以在安装程序提供此选项时将Python添加到PATH（默认情况下通常是关闭）。

    ![Add Python to PATH](img/win-py-install.png)

[python.org]: https://www.python.org/downloads/

#### Installing pip
#### 安装pip

If you're using a recent version of Python, the Python package manager, [pip],
is most likely installed by default. However, you may need to upgrade pip to the
lasted version:
如果您使用的是最新版本的Python，则默认情况下很可能会安装Python包管理器[pip]。但是，您可能需要将pip升级到持续版本：

```bash
pip install --upgrade pip
```

If you need to install [pip] for the first time, download [get-pip.py].
Then run the following command to install it:
如果您是第一次安装[pip]，请下载[get-pip.py]。然后运行以下命令进行安装：

```bash
python get-pip.py
```

#### Installing MkDocs
#### 安装MkDocs

Install the `mkdocs` package using pip:
使用pip安装`mkdocs`包：

```bash
pip install mkdocs
```

You should now have the `mkdocs` command installed on your system. Run `mkdocs
--version` to check that everything worked okay.
您现在应该在系统上安装`mkdocs`命令。 运行`mkdocs --version`来检查一切是否正常。

```bash
$ mkdocs --version
mkdocs, version 0.15.3
```

!!! Note
    If you would like manpages installed for MkDocs, the [click-man] tool can
    generate and install them for you. Simply run the following two commands:
    如果您希望为MkDocs安装联机帮助页，则[click-man]工具可以为您生成并安装它们。 只需运行以下两个命令：

        pip install click-man
        click-man --target path/to/man/pages mkdocs

    See the [click-man documentation] for an explanation of why manpages are
    not automatically generated and installed by pip.
    请参阅[click-man文档]，了解为什么pip不会自动生成和安装联机帮助页的原因。

[click-man]: https://github.com/click-contrib/click-man
[click-man documentation]: https://github.com/click-contrib/click-man#automatic-man-page-installation-with-setuptools-and-pip

!!! Note
    If you are using Windows, some of the above commands may not work
    out-of-the-box.
    如果您使用的是Windows，则上述某些命令可能无法实现开箱即用。

    A quick solution may be to preface every Python command with `python -m`
    like this:
    一个快速的解决方案可能是在每个Python命令前加上`python -m`，如下所示：

        python -m pip install mkdocs
        python -m mkdocs

    For a more permanent solution, you may need to edit your `PATH` environment
    variable to include the `Scripts` directory of your Python installation.
    Recent versions of Python include a script to do this for you. Navigate to
    your Python installation directory (for example `C:\Python34\`), open the
    `Tools`, then `Scripts` folder, and run the `win_add2path.py` file by double
    clicking on it. Alternatively, you can [download][a2p] the script and run it
    (`python win_add2path.py`).
    对于更永久的解决方案，您可能需要编辑`PATH`环境变量以包含Python安装的`Scripts`目录。 最近的Python版本包含一个为您执行此操作的脚本。 导航到Python安装目录（例如`C：\ Python34 \`），打开`Tools`，然后打开`Scripts`文件夹，双击运行`win_add2path.py`文件。 或者，您可以[下载] [a2p]脚本并运行它（`python win_add2path.py`）。

[a2p]: https://svn.python.org/projects/python/trunk/Tools/scripts/win_add2path.py

---

## Getting Started
## 入门

Getting started is super easy.
入门非常简单。

```bash
mkdocs new my-project
cd my-project
```

Take a moment to review the initial project that has been created for you.
花一点时间来回顾一下为您创建的初始项目。

![The initial MkDocs layout](img/initial-layout.png)

There's a single configuration file named `mkdocs.yml`, and a folder named
`docs` that will contain your documentation source files. Right now the `docs`
folder just contains a single documentation page, named `index.md`.
有一个名为`mkdocs.yml`的配置文件，以及一个名为`docs`的文件夹，它将包含你的文档源文件。现在`docs`文件夹只包含一个名为`index.md`的文档页面。

MkDocs comes with a built-in dev-server that lets you preview your documentation
as you work on it. Make sure you're in the same directory as the `mkdocs.yml`
configuration file, and then start the server by running the `mkdocs serve`
command:
MkDocs附带一个内置的开发服务器，可以让您在处理文档时预览文档。 确保您与`mkdocs.yml`配置文件位于同一目录中，然后通过运行`mkdocs serve`命令启动服务器：

```bash
$ mkdocs serve
INFO    -  Building documentation...
INFO    -  Cleaning site directory
[I 160402 15:50:43 server:271] Serving on http://127.0.0.1:8000
[I 160402 15:50:43 handlers:58] Start watching changes
[I 160402 15:50:43 handlers:60] Start detecting changes
```

Open up `http://127.0.0.1:8000/` in your browser, and you'll see the default
home page being displayed:
在浏览器中打开`http：//127.0.0.1：8000 /`，您将看到显示的默认主页：

![The MkDocs live server](img/screenshot.png)

The dev-server also supports auto-reloading, and will rebuild your documentation
whenever anything in the configuration file, documentation directory, or theme
directory changes.
dev-server还支持自动重新加载，并且只要配置文件，文档目录或主题目录中的任何内容发生更改，都将重建文档。

Open the `docs/index.md` document in your text editor of choice, change the
initial heading to `MkLorum`, and save your changes. Your browser will
auto-reload and you should see your updated documentation immediately.
在您选择的文本编辑器中打开`docs / index.md`文档，将初始标题更改为“MkLorum”，然后保存更改。 您的浏览器将自动重新加载，您应该立即看到更新的文档。

Now try editing the configuration file: `mkdocs.yml`. Change the
[`site_name`][site_name] setting to `MkLorum` and save the file.
现在尝试编辑配置文件：`mkdocs.yml`。 将[`site_name`] [site_name]设置更改为“MkLorum”并保存文件。

```yaml
site_name: MkLorum
```

Your browser should immediately reload, and you'll see your new site name take
effect.
您的浏览器应立即重新加载，您将看到新的站点名称生效。

![The site_name setting](img/site-name.png)

## Adding pages
## 添加页面

Now add a second page to your documentation:
现在在文档中添加第二页：

```bash
curl 'https://jaspervdj.be/lorem-markdownum/markdown.txt' > docs/about.md
```

As our documentation site will include some navigation headers, you may want to
edit the configuration file and add some information about the order, title, and
nesting of each page in the navigation header by adding a [`nav`][nav]
setting:
由于我们的文档站点将包含一些导航标题，您可能需要编辑配置文件，并通过添加[`nav`] [nav]设置在导航标题中添加有关每个页面的顺序，标题和嵌套的一些信息：

```yaml
site_name: MkLorum
nav:
    - Home: index.md
    - About: about.md
```

Save your changes and you'll now see a navigation bar with `Home` and `About`
items on the left as well as `Search`, `Previous`, and `Next` items on the
right.
保存更改，您现在会看到左侧有“Home”和“About”项目的导航栏，右侧还有“Search”，“Previous”和“Next”项。

![Screenshot](img/multipage.png)

Try the menu items and navigate back and forth between pages. Then click on
`Search`. A search dialog will appear, allowing you to search for any text on
any page. Notice that the search results include every occurrence of the search
term on the site and links directly to the section of the page in which the
search term appears. You get all of that with no effort or configuration on your
part!
尝试菜单项并在页面之间来回导航。 然后单击“搜索”。 将出现一个搜索对话框，允许您搜索任何页面上的任何文本。 请注意，搜索结果包括网站上每次出现的搜索字词，并直接链接到搜索字词所在页面的部分。 您无需付出任何努力或配置即可获得所有这些！

![Screenshot](img/search.png)

## Theming our documentation
## 主题化我们的文档

Now change the configuration file to alter how the documentation is displayed by
changing the theme. Edit the `mkdocs.yml` file and add a [`theme`][theme] setting:
现在更改配置文件以通过更改主题来更改文档的显示方式。 编辑`mkdocs.yml`文件并添加[`theme`] [theme]设置：

```yaml
site_name: MkLorum
nav:
    - Home: index.md
    - About: about.md
theme: readthedocs
```

Save your changes, and you'll see the ReadTheDocs theme being used.
保存更改，您将看到正在使用的ReadTheDocs主题。

![Screenshot](img/readthedocs.png)

## Changing the Favicon Icon
## 更改Favicon图标

By default, MkDocs uses the [MkDocs favicon] icon. To use a different icon, create
an `img` subdirectory in your `docs_dir` and copy your custom `favicon.ico` file
to that directory. MkDocs will automatically detect and use that file as your
favicon icon.
默认情况下，MkDocs使用[MkDocs favicon]图标。 要使用不同的图标，请在`docs_dir`中创建一个`img`子目录，并将自定义`favicon.ico`文件复制到该目录。 MkDocs将自动检测并使用该文件作为您的favicon图标。

[MkDocs favicon]: /img/favicon.ico

## Building the site
## 建立网站

That's looking good. You're ready to deploy the first pass of your `MkLorum`
documentation. First build the documentation:
那看起来不错。 您已准备好部署“MkLorum”文档的第一个传递。 首先构建文档：

```bash
mkdocs build
```

This will create a new directory, named `site`. Take a look inside the
directory:
这将创建一个名为`site`的新目录。 看一下目录：

```bash
$ ls site
about  fonts  index.html  license  search.html
css    img    js          mkdocs   sitemap.xml
```

Notice that your source documentation has been output as two HTML files named
`index.html` and `about/index.html`. You also have various other media that's
been copied into the `site` directory as part of the documentation theme. You
even have a `sitemap.xml` file and `mkdocs/search_index.json`.
请注意，您的源文档已输出为两个名为`index.html`和`about / index.html`的HTML文件。 您还有各种其他媒体已被复制到`site`目录中作为文档主题的一部分。 你甚至有一个`sitemap.xml`文件和`mkdocs / search_index.json`。

If you're using source code control such as `git` you probably don't want to
check your documentation builds into the repository. Add a line containing
`site/` to your `.gitignore` file.
如果您使用的是源代码控制，例如`git`，您可能不希望将文档构建检查到存储库中。 在`.gitignore`文件中添加一行包含`site /`的行。

```bash
echo "site/" >> .gitignore
```

If you're using another source code control tool you'll want to check its
documentation on how to ignore specific directories.
如果您正在使用其他源代码控制工具，则需要检查其文档，了解如何忽略特定目录。

After some time, files may be removed from the documentation but they will still
reside in the `site` directory. To remove those stale files, just run `mkdocs`
with the `--clean` switch.
一段时间后，文件可能会从文档中删除，但它们仍将驻留在`site`目录中。 要删除那些陈旧的文件，只需使用`--clean`开关运行`mkdocs`。

```bash
mkdocs build --clean
```

## Other Commands and Options
##其他命令和选项

There are various other commands and options available. For a complete list of
commands, use the `--help` flag:
还有其他各种命令和选项。 有关命令的完整列表，请使用`--help`标志：

```bash
mkdocs --help
```

To view a list of options available on a given command, use the `--help` flag
with that command. For example, to get a list of all options available for the
`build` command run the following:
要查看给定命令上可用的选项列表，请使用带有该命令的`--help`标志。 例如，要获取`build`命令可用的所有选项的列表，请运行以下命令：

```bash
mkdocs build --help
```

## Deploying
## 部署

The documentation site that you just built only uses static files so you'll be
able to host it from pretty much anywhere. [GitHub project pages] and [Amazon
S3] may be good hosting options, depending upon your needs. Upload the contents
of the entire `site` directory to wherever you're hosting your website from and
you're done. For specific instructions on a number of common hosts, see the
[Deploying your Docs][deploy] page.
您刚刚构建的文档站点仅使用静态文件，因此您几乎可以在任何地方托管它。 [GitHub项目页面]和[Amazon S3]可能是很好的托管选项，具体取决于您的需求。 将整个`site`目录的内容上传到您托管您网站的任何地方，然后您就完成了。 有关许多常见主机的具体说明，请参阅[部署您的文档] [部署]页面。

## Getting help
## 获得帮助

To get help with MkDocs, please use the [discussion group], [GitHub issues] or
the MkDocs IRC channel `#mkdocs` on freenode.
要获得MkDocs的帮助，请使用freenode上的[讨论组]，[GitHub问题]或MkDocs IRC频道`#mkdocs`。

[deploy]: user-guide/deploying-your-docs/
[mkdocs]: user-guide/styling-your-docs/#mkdocs
[readthedocs]: user-guide/styling-your-docs/#readthedocs
[MkDocs wiki]: https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes
[build your own]: user-guide/custom-themes/
[Amazon S3]: https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html
[get-pip.py]: https://bootstrap.pypa.io/get-pip.py
[nav]: user-guide/configuration/#nav
[discussion group]: https://groups.google.com/forum/#!forum/mkdocs
[GitHub issues]: https://github.com/mkdocs/mkdocs/issues
[GitHub project pages]: https://help.github.com/articles/creating-project-pages-manually/
[pip]: https://pip.readthedocs.io/en/stable/installing/
[Python]: https://www.python.org/
[site_name]: user-guide/configuration/#site_name
[theme]: user-guide/configuration/#theme
