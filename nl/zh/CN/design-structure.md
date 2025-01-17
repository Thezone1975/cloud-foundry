---

copyright:

  years: 2018
lastupdated: "2018-10-26"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 设计 IBM Cloud Foundry Enterprise Environment 的结构
{: #bpimplementation}

您可以实现开发者和测试者可与其他团队成员协作的环境，而不采用严格定义的传统的开发、测试和生产方法。如果设计了要用于开发和交付应用程序的方式，请创建空间来实现该方法。请考虑从空间级别向上设计 {{site.data.keyword.cfee_full}} (CFEE)，而不是从组织级别向下设计环境。

请考虑计划开发和部署的应用程序的规模和作用域。Cloud Foundry 空间可以用作紧密连接或定义的一个或多个应用程序的开发环境。除了开发空间外，还可以创建多个空间用于其他用途，例如，用于单元测试、性能测试和集成测试。还可以定义空间以用于构建、编译打包和生产。创建的每个空间都可以与同一组织内的不同团队成员共享。

如果有人员在不同的业务领域工作，并且他们的活动互不重叠，请创建单独的 Cloud Foundry 组织。如果有两个独立的组，那么为每个组创建一个组织，就能为团队成员和资源的交付及管理定义明确的边界。您可以定义一个 API 以用于在组织之间进行通信。

可以创建 Cloud Foundry 组织，以匹配您希望的工作方式，而不是公司内的结构。通常，公司组织可能会变动，但应用程序的开发和维护仍会继续进行。
因此，请针对应用程序的生命周期而不是公司组织结构来设计 {{site.data.keyword.cfee_full}} 实例。

迭代开发和部署可能会导致应用程序迅速扩大。交付过程设计必须能够快速、轻松地扩展。您会希望以快速部署速度进行持续开发。通过将开发和生产空间置于同一 CFEE 组织中，可提供对相同资源的访问权。在单个组织内管理不同空间可减少管理工作量。如果开发、测试和运营人员是在同一 CFEE 组织中工作，那么他们可以轻松进行协作。

实施命名标准，以用于明确地标识组织和空间用途。例如，您可以包含云的类型、地理区域、用途类型（如开发、测试或生产）、应用程序名称以及版本号或修订版号。然后，就可以轻松标识组织和空间以用于管理和访问目的。  

空间的数量可能会由于迭代开发而迅速激增。您可以在一个组织内根据需要定义任意多个空间。如果计划定义大量空间，您可能需要创建一个应用程序来帮助管理这些空间。当空间数超过 60 个时，可能需要考虑定义另一个组织。

安排一人负责创建和管理组织，定义空间，以及向团队成员授予访问权。当组织管理者无法正常工作时，可向另一人授予相同的访问权来维护环境。  

确定需要访问每个空间和组织的所有人员。确定他们的角色。团队成员的工作角色将决定他们的权限。例如，高级开发者需要查看和更新整个 {{site.data.keyword.cfee_full}} 开发环境的权限。然而，初级开发者在可以查看和更新的内容方面会受到限制。
