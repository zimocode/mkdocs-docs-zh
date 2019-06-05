# 部署文档

将文档部署到各种托管服务提供商的基本指南

---

## GitHub Pages

如果您在[GitHub]上托管项目的源代码，则可以轻松使用[GitHub Pages]来托管项目的文档。GitHub页面有两种基本类型：[Project Pages]站点和[User and Organization Pages]站点。它们几乎完全相同但有一些重要的区别，在部署时需要不同的工作流程。

### Project Pages

项目页面站点更简单，因为站点文件被部署到项目存储库中的分支（默认情况下为“gh-pages”）。 在“checkout”git存储库的主要工作分支（通常是`master`）之后，您可以在其中维护项目的源文档，运行以下命令：

```sh
mkdocs gh-deploy
```

就是这么简单就完成了部署！ MkDocs将构建您的文档并使用[ghp-import]工具将它们提交到`gh-pages`分支，然后将`gh-pages`分支推送到GitHub。

使用`mkdocs gh-deploy --help`获取可用于`gh-deploy`命令的完整选项列表。

请注意，在将其推送到GitHub之前，您将无法查看构建的站点。 因此，您可能希望通过使用`build`或`serve`命令并在本地查看构建的文件来验证您对文档所做的任何更改。

!!! warning "警告："

    如果使用`gh-deploy`命令，则不应手动编辑页面存储库中的文件，因为手动所作的更改在下次运行该命令的时候将丢失。 

### Organization and User Pages

User and Organization Pages站点不依赖于特定项目，站点文件部署到以“GitHub”帐户名命名的专用存储库中的“master”分支。 因此，您需要在本地系统上使用两个存储库的工作副本。 例如，请考虑以下文件结构：

```no-highlight
my-project/
    mkdocs.yml
    docs/
orgname.github.io/
```

在制作并验证项目更新后，您需要将目录更改为`orgname.github.io`存储库并从那里调用`mkdocs gh-deploy`命令：

```sh
cd ../orgname.github.io/
mkdocs gh-deploy --config-file ../my-project/mkdocs.yml --remote-branch master
```

需要注意的是，您需要显式指向`mkdocs.yml`配置文件，因为它不再位于当前工作目录中。 您还需要通知部署脚本提交到`master`分支。 您可以使用[remote_branch]配置设置覆盖默认值，但是如果您在运行部署脚本之前忘记更改目录，它将提交到可能不是你期望的项目的`master`分支。

请注意，在将其推送到GitHub之前，您将无法查看构建的站点。 因此，您可能希望通过使用`build`或`serve`命令并在本地查看构建的文件来验证您对文档所做的任何更改。

!!! warning "警告："

    如果使用`gh-deploy`命令，则不应手动编辑页面存储库中的文件，因为手动所作的更改在下次运行该命令的时候将丢失。 

### 自定义域名

GitHub Pages支持为您的站点使用[自定义域名]。除了GitHub记录的步骤之外，您还需要执行一个额外的步骤，以便MkDocs可以与您的自定义域一起使用。您需要将“CNAME”文件添加到[docs_dir]的根目录中。该文件必须在一行中包含一个裸域或子域（请参阅MkDocs自己的[CNAME文件]作为示例）。您可以手动创建文件，或使用GitHub的Web界面设置自定义域（在Settings/Custom Domain下）。如果您使用Web界面，GitHub将为您创建`CNAME`文件并将其保存到“pages”分支的根目录。因此，下次部署时不会删除该文件，您需要将文件复制到`docs_dir`。将文件正确地包含在`docs_dir`中，MkDocs将在您构建的站点中包含该文件，并在每次运行`gh-deploy`命令时将其推送到“pages”分支。

如果您在使自定义域名时遇到问题，请参阅[Troubleshooting custom domains]的GitHub文档。

[GitHub]: https://github.com/
[GitHub Pages]: https://pages.github.com/
[Project Pages]: https://help.github.com/articles/user-organization-and-project-pages/#project-pages-sites
[User and Organization Pages]: https://help.github.com/articles/user-organization-and-project-pages/#user-and-organization-pages-sites
[ghp-import]: https://github.com/davisp/ghp-import
[remote_branch]: ./configuration.md#remote_branch
[Custom Domain]: https://help.github.com/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site
[docs_dir]: ./configuration.md#docs_dir
[CNAME file]: https://github.com/mkdocs/mkdocs/blob/master/docs/CNAME
[Troubleshooting custom domains]: https://help.github.com/articles/troubleshooting-custom-domains/

## Read the Docs

[Read the Docs][rtd]提供免费文档托管。您可以使用任何主要版本控制系统导入文档，包括Mercurial，Git，Subversion和Bazaar。Read the Docs支持MkDocs开箱即用。 按照其网站上的[说明]正确排列存储库中的文件，创建一个帐户并将其指向您公开托管的存储库。如果配置正确，每次将提交推送到公共存储库时，文档都会更新。

!!! note "注意："

    要从Read the Docs提供的所有[功能]中受益，您需要使用MkDocs附带的[Read the Docs主题][theme]。 Read the Docs文档中提到的各种主题是Sphinx特定主题，不适用于MkDocs。

[rtd]: https://readthedocs.org/
[instructions]: https://read-the-docs.readthedocs.io/en/latest/getting_started.html#in-markdown
[功能]: https://read-the-docs.readthedocs.io/en/latest/features.html
[theme]: ./styling-your-docs.md#readthedocs

## 其他提供商

任何可以提供静态文件的托管服务的提供商都可以用来部署MkDocs生成的文档。虽然不能一一提供每个服务商的部署方式，但是以下指南可以提供一些一般性的帮助。 

当您构建站点时（使用`mkdocs build`命令），所有文件都将写入您的`mkdocs.yaml`配置文件中给[site_dir]选项所设置的目录中（默认为“site”）。 通常，您只需将该目录的内容复制到托管服务提供商服务器的根目录。 根据您的托管服务提供商的设置，您可能需要使用图形或命令行[ftp]，[ssh]或[scp]客户端来传输文件。

例如，命令行中的一组典型命令可能如下所示：

```sh
mkdocs build
scp -r ./site user@host:/path/to/server/root
```

当然，您需要将“user”替换为您的托管服务提供商的用户名，“host”替换为你自己的域名。 此外，您需要调整路径`/path/to/server/root`以匹配主机文件系统的配置。

[ftp]: https://en.wikipedia.org/wiki/File_Transfer_Protocol
[ssh]: https://en.wikipedia.org/wiki/Secure_Shell
[scp]: https://en.wikipedia.org/wiki/Secure_copy

有关详细信息，请参阅相应服务商的说明文档。 您可能希望在其文档中搜索“ftp”或“上传站点(uploading site)”。

## 404页面

当MkDocs构建文档时，它将在[构建目录][site_dir]中包含一个404.html文件。 部署到[GitHub](#github-pages)时，将自动使用此文件，但仅限于自定义域名。其他Web服务器可以配置使用它，但该功能并不总是可用。 有关详细信息，请参阅所选Web服务器的文档。

[site_dir]: ./configuration.md#site_dir
