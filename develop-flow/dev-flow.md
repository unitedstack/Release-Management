# 研发流程

本节将从分支开发流程，提交代码流程两个方面来讲述研发流程，形成流程规范。


## 一、 分支开发流程

开发阶段，对于UnitedStack开发人员，我们使用gitlab实现代码的版本控制，使用Jenkins实现构建，单元测试和功能测试。在构建过程中，由Jenkins自动调用Packforge生成rpm包，自动调用Puppet部署到公共dev。若部署后出现问题，将重新回到开发阶段，直到CI系统集成通过。
当一切测试环境可用，测试系统正常运行时，此阶段中最难的一点在于版本和分支的管理。

本小节将在后文代码管理中详细讲解，请读者仔细阅读。

## 二、 提交代码流程

这里可以参考社区的一张图：
![](/images/develop-flow/code_review.png)

这里我们将公司git.ustack.com中的代码作为Upstream Code。
下面以nova为例，具体操作如下：
1.申请通道机用户名和权限，登录通道机ssh.ustack.com.

2.克隆代码。

    $ git clone http://git.ustack.com/united_openstack/nova.git

3.切换到**分支。

    $ git checkout **

4.本地修改代码。

5.添加代码到仓库。

	$ git add --all

6.提交commit，这里必须按照正确的格式添加commit message，否则CI测试会失败。

    $ git commit

这里还需要注意，若是第一次使用，需设置git的username和password。并将自己的ssh key（cat /.ssh/isa_pub）加入到review.ustack.com和git.ustack.com个人设置的SSH KEY中。

    $ git config --global user.name "Your Name"
    $ git config --global user.email "you_email@example.com"

7.安装git review。

    # Ubuntu
    $ apt-get install git-review
    
    # CentOS/Red Hat
    $ yum install git-review
    
    # Mac OS/Unix-like system
    $ pip install git-review

8.使用git review推送patch到远程仓库。

	$ git review
 
若是第一次使用，会提示如下信息：

    Could not connect to gerrit.
    Enter your gerrit username: 

没有关系，输入用户名即可。
然后这里会获得一个url，可登陆review.ustack.com查看状态。

9.等待CI测试结果，可上Jenkins查看测试过程。

10.测试通过后，代码提交完成，等待reviewer查阅。

11.若出现问题，继续重复以上步骤（配置步骤除外），且在修改代码后，执行下面命令重新提交commit.

    $ git commit --amend

## 参考文档

1. Devloper's Guide：http://docs.openstack.org/infra/manual/developers.html
2. 深入理解Zuul：https://confluence.ustack.com/pages/viewpage.action?pageId=7373593