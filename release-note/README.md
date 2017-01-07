# Release Note 管理

---

## 简介

OpenStack 所有官方项目的 Release Note 均记录在 [这里](https://releases.openstack.org/)。

![release][1]

如上所示，所有 Releases 版本以大版本为分类标准，每个大版本 Releases 中都有各个项目详细的 Release Notes 信息，比如，Newton 版 Neutron Releases Notes 记录在[这里](http://docs.openstack.org/releasenotes/neutron/newton.html)。

## Release Notes

OpenStack 通过 [Reno](http://docs.openstack.org/developer/reno/)管理 Release Notes，下面介绍 Reno 的使用。

### 安装 Reno

通过 `pip` 安装即可：

   ➜  ~ pip install reno

### 创建 Release Note

以 Neutron 为例，创建一个 Release Note：

    ➜  neutron git:(master) reno
    usage: reno [-h] [-v] [-q] [--rel-notes-dir RELNOTESDIR]
                {report,new,cache,list} ...
    reno: error: too few arguments
    ➜  neutron git:(master) reno new new-feature
    Created new notes file in releasenotes/notes/new-feature-9410ed5595d36e38.yaml

Release Note 是一个 `yaml` 文件，内容格式为 [reStructuredText](http://www.sphinx-doc.org/en/stable/rest.html)，内容如下：

    ---
    prelude: >
        Replace this text with content to appear at the top of the section for this
        release. All of the prelude content is merged together and then rendered
        separately from the items listed in other parts of the file, so the text
        needs to be worded so that both the prelude and the other items make sense
        when read independently. This may mean repeating some details. Not every
        release note requires a prelude. Usually only notes describing major
        features or adding release theme details should have a prelude.
    features:
      - |
        List new features here, or remove this section.  All of the list items in
        this section are combined when the release notes are rendered, so the text
        needs to be worded so that it does not depend on any information only
        available in another section, such as the prelude. This may mean repeating
        some details.
    issues:
      - |
        List known issues here, or remove this section.  All of the list items in
        this section are combined when the release notes are rendered, so the text
        needs to be worded so that it does not depend on any information only
        available in another section, such as the prelude. This may mean repeating
        some details.
    upgrade:
      - |
        List upgrade notes here, or remove this section.  All of the list items in
        this section are combined when the release notes are rendered, so the text
        needs to be worded so that it does not depend on any information only
        available in another section, such as the prelude. This may mean repeating
        some details.
    deprecations:
      - |
        List deprecations notes here, or remove this section.  All of the list
        items in this section are combined when the release notes are rendered, so
        the text needs to be worded so that it does not depend on any information
        only available in another section, such as the prelude. This may mean
        repeating some details.
    critical:
      - |
        Add critical notes here, or remove this section.  All of the list items in
        this section are combined when the release notes are rendered, so the text
        needs to be worded so that it does not depend on any information only
        available in another section, such as the prelude. This may mean repeating
        some details.
    security:
      - |
        Add security notes here, or remove this section.  All of the list items in
        this section are combined when the release notes are rendered, so the text
        needs to be worded so that it does not depend on any information only
        available in another section, such as the prelude. This may mean repeating
        some details.
    fixes:
      - |
        Add normal bug fixes here, or remove this section.  All of the list items
        in this section are combined when the release notes are rendered, so the
        text needs to be worded so that it does not depend on any information only
        available in another section, such as the prelude. This may mean repeating
        some details.
    other:
      - |
        Add other notes here, or remove this section.  All of the list items in
        this section are combined when the release notes are rendered, so the text
        needs to be worded so that it does not depend on any information only
        available in another section, such as the prelude. This may mean repeating
        some details.

### 生成 Release Notes

通过命令 `reno report <path-to-git-repository>` 生成某一项目的 Releases Notes，例如：

    ➜  community reno report neutron

所有的信息以屏幕输出的形式展现，也可生成特定分支的 Release Notes，例如：

    ➜  community reno report neutron --branch origin/stable/liberty

## 关于 Release Notes

很多情况下，当研发开发完成新的功能开发、Bug 修复、或者性能提升后，前端人员（销售人员、售前人员）没有固定的渠道获得这些信息。通过 Release Notes 可以很好的解决这一问题，当研发人员完成相关工作后，由项目相关人员完成相关 Release Notes 的书写，并提交到代码仓库，最终统一显示在固定前端页面（目前尚未有此页面），以便于信息的及时传递。

[1]: ../images/release_notes/release.png
