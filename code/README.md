# 代码管理

---


在现代软件开发项目中，要成为一个有效的软件开发人员，我们必须能够与其他项目贡献者并行进行开发。源代码管理（SCM）系统不是什么新思想。为了编写一些能够更快速、简单地开发以后软件项目的软件，已经进行了很多尝试。最新的源代码解决方案都包含了版本控制系统，它可以对源代码的修改进行回滚，从而将有害的代码剔除出项目之外，或者简单地跟踪哪些人修改了代码的哪些行的内容。版本控制系统试图解决开发人员在试图同时对某个文件进行修改时所出现的冲突问题，可以防止用户覆盖其他人所作的修改。源代码管理使用的很多流行解决方案都试图解决以前 SCM 解决方案中的失效问题。


## 代码管理工具 - Git

    Git 是一个快速、可扩展的分布式版本控制系统，它具有极为丰富的命令集，对内部系统提供了高级操作和完全访问。

## 代码管理流程

代码管理流程大体分为以下几方面：

1. 基于社区稳定版本，一般 Feature 的开发流程
2. 对于社区中 Bugfix / Feature Enhancement 的修复流程
3. 对于私有功能在跨大版本（Mitaka -> Newton）时的代码迁移流程

在代码开发的过程中，使用两个 Git 的 remote 主机：

* `upstream : ` 指向 OpenStack 官方项目
* `$(PROJECT)_patches: ` 指向公司内部项目

以 Swift 为例：

    ➜  swift git:(master) git remote -v
    upstream	git://git.openstack.org/openstack/swift (fetch)
    upstream	git://git.openstack.org/openstack/swift (push)
    swift_patches	git@github.com:changzhi1990/swift.git (fetch)
    swift_patches	git@github.com:changzhi1990/swift.git (push)

对于上面提到的方面 1 和方面 2 ，可以用下图表示代码管理的详细流程：

![git-flow][1]

如上图所示，首先，代码管理以`时间`为基准，这也是和 OpenStack 社区保持一致的，当到达某一时间节点后，必须按照详细的 Release 流程进行操作。在上图中，存在`两个 Git 远程主机，一个是 OpenStack 社区的项目分支（upstream），另一个是公司内部维护的项目分支（$(project)_patches）`，一般的，当公司内部开始对 OpenStack 社区项目进行二次开发时，首先根据 OpenStack 社区官方项目的 `stable/$(version)` 进行 checkout ，随后 push 到公司内部代码仓库，此时公司内部代码仓库和 OpenStack 官方代码仓库保持了一直  （都有功能 1 2 3 4 ），即图中的步骤 1，这一步通常由公司内基础设施团队完成，对于进行二次开发的研发人员来说，也是要基于 OpenStack 社区官方项目的 `stable/$(version)` 进行 checkout，即为图中的步骤 2，随后进行开发新 Feature（图中的5 6 7 ），当开发完成后，根据 Git Review 流程进行代码的提交、测试等，最终合并到公司内部代码仓库，即为图中的步骤 3。

对于上面提到的方面 2，当社区项目中有 Bugfix 或者 Feature Enhancement 时，此时需要以 OpenStack 官方项目中包含此修改的 Commit 或者 Tag 进行 checkout，并且进行 rebase，即为图中的步骤 2A（此时本地 Git 流是 4· 5 6 7），需要注意的是，在 rebase 的过程，无法避免的会遇到冲突，此时需要手动解决冲突。当确定本地代码无误后，这时由于本地工作流和公司内部代码仓库中的工作流基不同（本地是 4· 5 6 7，公司内部仓库是 1 2 3 4 5 6 7，因此无法走 Git review 流程），因此需要 Git push -f 到公司内部代码仓库（即为图中的步骤 2B），因此，公司内部代码仓库经历了步骤 2C。

![git-flow][2]

上图表示了当进行大版本升级时（例如 Mitaka -> Newton）的流程。首先以最新稳定版本的 stable 分支进行 checkout，此时在本地会有两个分支，一个是上一个版本 Feature 分支，一个是和社区同步了的 stable/$(version) 分支，此时由于跨越了大版本，因此 commit 较多，所以我们以 cherry-pick 的形式，将上一版本的 Feature 有选择的（需要注意的是，在这步中如果将所有的功能都迁移到下一稳定版本中，那么每当进行大版本升级时，迁移的工作量都是巨大的，因此我们建议，比如，每个公司内部维护的版本只有 5 个私有 Feature，当进行大版本升级时，由于可能会添加新 Feature，因此建议废除某一在上个版本中的 Feature）迁移到新版本中，最后通过 Git Review 流程，同步到公司内部代码仓库。

通过上述两张流程图，可以：

* 我们开发的 Feature，始终属于 OpenStack 社区官方项目的`尾部`，便于迁移，并且可以随时`砍掉`；
* 灵活处理 OpenStack 社区的 Bugfix / Feature Enhancement；
* 使得公司内部代码变得更加灵活，增强可移植性

缺点：

* 不可避免的，在 Rebase 和 Cherry-pick 时，需要手动解决冲突


需要注意的是，在本代码流程中，要求研发人员对于 `Git` 的使用有较高的要求，需要熟练掌握 `Git` 相关命令的使用，对于 `Git` 的使用可参考相关文档：

* [Git 教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
* [Git 远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)
* [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
* [Git 教程](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)

最后，当不熟悉 Git 某一命令时候，例如不熟悉 `git rebase` 的使用时，通过 `git rebase --help` 查看详细说明也是很好的选择。

[1]: ../images/code/git-flow.png
[2]: ../images/code/git-flow2.png
