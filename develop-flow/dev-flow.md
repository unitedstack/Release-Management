# 研发流程管理

## 一、目的

本章节主要描述公司产品研发的管理流程，主要需遵循的群体是公司内的开发人员。目的是希望通过本流程的实施，让公司内的项目研发，交付，运维等环节能自行运转得尽可能顺利，有规律。以免混乱无章，让相关人员处于混沌状态。

## 二、流程定义

### 2.1 总流程

在一个大版本的release management过程中，具体可拆分为开发阶段，预发布阶段，发布阶段。

在整个流程中，需要一套完整可用且好用的CI持续集成系统、打包工具和Puppet自动部署工具。

UnitedStack的CI系统主要由Genkins，Zuul，Gerrit组成。其中Gerrit是代码review平台。Genkins负责运行测试任务，且当reviewer在approve开发人员提交的patch之后，由Genkins重新运行测试，并调用打包工具进行自动打包，调用部署工具将rpm包部署到相应的测试环境。Zuul根据Gerrit事件来触发Jenkins Job。

#### 2.1.1 开发阶段

此阶段需要开发人员、运维人员的参与，主要是开发人员进行功能开发或bug修复。

**角色和职责：**
- 开发人员：代码开发，包括编写单元测试代码。
- 运维人员：持续维护CI系统，保证CI系统的正常运转。

**CI系统职责：**
代码自动化测试及打包部署到dev测试环境。
当开发人员发现CI系统出现问题时，需及时找运维人员进行系统修复。

#### 2.1.2 预发布阶段

此阶段需要开发人员、测试人员、运维人员的参与。主要是测试人员进行全面测试。

**角色和职责：**
- 开发人员：若测试出现问题，需随时候命解决问题。
- 测试人员：在测试环境进行全面测试。
- 运维人员：对开发阶段结束之后的代码打tag。持续维护CI系统，保证CI系统的正常运转。

**CI系统职责：**
rpm打包以及部署到demo环境。

当测试人员在测试过程中发现问题时，需及时通过Jira反馈给开发人员。

其中，tag格式为：XXXX-XX-XXrc (daterc)

#### 2.1.3 发布阶段

需要运维人员参与。

**角色和职责：**
- 运维人员：对预发布阶段结束之后的代码打tag。持续维护CI系统，保证CI系统的正常运转。

**CI系统职责：**
1. 调用Packforge打包到预发布环境；
2. 调用puppet部署到预发布环境；
3. 调用Repoforge发布到products仓库。

其中，tag格式为：XXXX-XX-XX

下面是总流程的实现图：
![](/images/develop-flow/dev.png)

### 2.2 分支开发流程

开发阶段，对于UnitedStack开发人员，我们使用gitlab实现代码的版本控制，使用Jenkins实现构建，单元测试和功能测试。在构建过程中，由Jenkins自动调用Packforge生成rpm包，自动调用Puppet部署到公共dev。若部署后出现问题，将重新回到开发阶段，直到CI系统集成通过。
当一切测试环境可用，测试系统正常运行时，此阶段中最难的一点在于版本和分支的管理。

以下将详细描述开发人员在使用gitlab进行版本控制时需要遵循的分支管理规范：

XXXXXX

### 2.3 提交代码流程

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

11.若出现问题，继续重复以上步骤（配置步骤除外），且在修改代码后，执行下面命令修改commit message.

    $ git commit --amend

## 参考文档

1. 理解unitedstack CI系统：https://confluence.ustack.com/pages/viewpage.action?pageId=7373397
2. Devloper's Guide：http://docs.openstack.org/infra/manual/developers.html
3. 深入理解Zuul：https://confluence.ustack.com/pages/viewpage.action?pageId=7373593