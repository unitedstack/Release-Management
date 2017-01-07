# 概念和定义

本节从整体来讲述研发的整个流程。

## 总流程

在一个大版本的release management过程中，具体可拆分为开发阶段，预发布阶段，发布阶段。

在整个流程中，需要一套完整可用且好用的CI持续集成系统、打包工具和TripleO自动部署工具。

UnitedStack的CI系统主要由Jenkins，Zuul，Gerrit组成。其中Gerrit是代码review平台。Jenkins负责运行测试任务，且当reviewer在approve开发人员提交的patch之后，由Jenkins重新运行测试，并调用打包工具进行自动打包，调用部署工具将rpm包部署到相应的测试环境。Zuul根据Gerrit事件来触发Jenkins Job。CI系统的详细介绍请参考文末参考文档。

### 一、 开发阶段

此阶段需要开发人员、运维人员的参与，主要是开发人员进行功能开发和bug修复。

**角色和职责：**

- 开发人员：代码开发，包括编写单元测试代码。
- 运维人员：持续维护CI系统，保证CI系统的正常运转。

**CI系统职责：**

- 代码自动化测试及打包部署到dev测试环境。

当开发人员发现CI系统出现问题时，需及时找运维人员进行系统修复。

### 二、 预发布阶段

此阶段需要开发人员（包括项目的PTL或release liaison）、测试人员、运维人员的参与。主要是测试人员进行全面测试。

**角色和职责：**

- 开发人员：若测试出现问题，需重新回到开发阶段，继续解决问题。
- PTL/liaison：对开发到一定阶段的代码打tag。并向releases仓库提交patch，请求release。
- 测试人员：在测试环境进行全面测试。
- 运维人员：持续维护CI系统，保证CI系统的正常运转。

**CI系统职责：**

- 代码自动化测试rpm打包以及部署到预发布环境。

当测试人员在测试过程中发现问题时，需及时通过Jira反馈给开发人员。

其中，tag格式为：X.Y.Z

### 三、 发布阶段

此阶段需要DevOps人员参与。预备进入交付系统。

**角色和职责：**

- 运维人员：持续维护CI系统，保证CI系统的正常运转。

**CI系统职责：**

- 调用Repoforge发布到RPM包管理仓库。

其中，tag格式为：X.Y.Z

下面是总流程的实现图：
![](/images/develop-flow/dev.png)

## 参考文档

1. 理解unitedstack CI系统：https://confluence.ustack.com/pages/viewpage.action?pageId=7373397