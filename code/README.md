# 代码管理

---


在现代软件开发项目中，要成为一个有效的软件开发人员，我们必须能够与其他项目贡献者并行进行开发。源代码管理（SCM）系统不是什么新思想。为了编写一些能够更快速、简单地开发以后软件项目的软件，已经进行了很多尝试。最新的源代码解决方案都包含了版本控制系统，它可以对源代码的修改进行回滚，从而将有害的代码剔除出项目之外，或者简单地跟踪哪些人修改了代码的哪些行的内容。版本控制系统试图解决开发人员在试图同时对某个文件进行修改时所出现的冲突问题，可以防止用户覆盖其他人所作的修改。源代码管理使用的很多流行解决方案都试图解决以前 SCM 解决方案中的失效问题。


## 代码管理工具 - Git

    Git 是一个快速、可扩展的分布式版本控制系统，它具有极为丰富的命令集，对内部系统提供了高级操作和完全访问。

## 代码管理流程

代码管理流程大体分为以下几方面：

* 基于社区稳定版本，一般 Feature 的开发流程
* 对于社区中 Bugfix / Feature Enhancement 的修复流程
* 对于私有功能在跨大版本（Mitaka -> Newton）时的代码迁移流程

在代码开发的过程中，使用两个 Git 的 remote 主机：

* `upstream : ` 指向 OpenStack 官方项目
* `$(PROJECT)_patches: ` 指向公司内部项目

以 Swift 为例：

    ➜  swift git:(master) git remote -v
    upstream	git://git.openstack.org/openstack/swift (fetch)
    upstream	git://git.openstack.org/openstack/swift (push)
    swift_patches	git@github.com:changzhi1990/swift.git (fetch)
    swift_patches	git@github.com:changzhi1990/swift.git (push)

![git-flow][1]

![git-flow][2]


[1]: ../../images/code/git-flow.png
[2]: ../../images/code/git-flow2.png
