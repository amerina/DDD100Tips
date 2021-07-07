



[TOC]

**Implementing Domain Driven Design**

Apractical guide for implementing the Domain Driven Designwith the ABP Framework

## 引言

**介绍**

这是实现领域驱动的**实用指南**设计（DDD）。虽然实现细节依赖于ABP 框架基础设施,但是核心概念、原则和模式适用于任何类型的解决方案，即使它不是.NET 解决方案。

### **目标**

本书的目标是:

●**介绍和解释**DDD 架构、概念、原则、模式和构建块。

●解释ABP框架提供的**框架结构**和解决方案结构

●引入**显式规则**来实现 DDD 模式和通过**具体示例的**给出最佳实践

●展示**ABP 框架为**您**提供**以适当的方式实施 DDD 的基础设施

●最后，基于**软件开发经验提供最佳实践建议**并创建可**维护的代码库**

### **简单的代码**

**踢足球**很**简单**，但**踢简单的足球**是**最困难的事情**。—*约翰克鲁伊夫*

如果我们将这句名言用于编程，我们可以说：**写代码**很**简单**，但**写简单的代码**才是最重要的**最难的事情**。

在本文档中，我们将介绍一些**简单的规则**，它们是**易于实施的。**一旦您的**应用程序增长**，将**很难遵循**这些规则。有时你会发现**打破规则**会短期内节省你的时间。但是，短期内节省的时间将带来**更多的**中长期**时间损失**。你的代码库变得**复杂**且难以维护。大多数业务应用程序被**重写**只是因为你**不能****再维护**它。如果您**遵循规则和最佳实践**，您的代码库将更简单，更容易维护。您的应用程序更快应对变化。

## **什么是领域驱动设计？**

领域驱动设计 (DDD) 是一种软件方法通过连接**复杂的**需求来开发实施到一个**不断发展的**模型；

DDD适用于**复杂领域**和**大规模**应用程序而不是简单的 CRUD 应用程序。它专注于在**核心领域逻辑**，而不是基础架构细节。它有助于构建**灵活**、模块化和可**维护的**代码。

### **面向对象和SOLID**

实现 DDD 高度依赖面向对象编程 (OOP) 和[SOLID](https://en.wikipedia.org/wiki/SOLID)原则。实际上，它**实现和**扩展**这些原则。所以，对**OOP 和 SOLID 的**理解**对你有很大帮助实施 DDD。

### **DDD分层和清洁架构**

领域驱动设计有四个基本层:

![image-20210706084137733](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210706084137733.png)



**业务逻辑**分为两层，*领域层*和*应用层*，而它们包含不同种类的业务逻辑;

●**领域层**实现核心用例域/系统的独立业务逻辑。

●**应用层**实现的用例基于领域。一个用例可以是被认为是用户界面上的用户交互（用户界面）。

●**展示层**包含 UI 元素（页面、组件）的应用程序。

●**基础设施层**支持其他层通过实现抽象或集成第三方库和系统。

相同的分层可以如下图所示被称为**清洁架构，**或者有时是**洋葱结构**

![image-20210706084629315](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210706084629315.png)



在 Clean Architecture 中，每一层只**依赖于在它里面的层**。最独立的层显示在最内圈，它就是领域层。

### **核心构建块**

DDD 主要**关注领域层和应用层**而忽略表示层和基础设施层。他们被视为**实现细节**而业务层不应该依赖于它们。这并不意味着表示层和基础设施层不重要。它们非常重要。UI框架和数据库提供商有自己的规则和最佳实践，您需要了解并应用他们,然而这些都与DDD的话题无关。

本节介绍基本构建块:领域层和应用层。

**领域层构建块**

●**实体Entity：**一个[实体](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Entities)是具有其自身属性的对象（状态、数据）和实现业务的方法在这些属性上执行的逻辑。一个实体是由其唯一标识符 (Id) 标识。两个实体对象具有不同 ID 的被视为不同的实体。

●**值对象Value Object：**一个[值对象](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Value-Objects)是另一种领域对象由其属性而不是一个唯一Id标识身份。这意味着具有相同属性的两个值对象被视为同一个对象。值对象通常被实现为不可变的,大多数值对象都比实体简单得多。

**●聚合及聚合根Aggregate & Aggregate Root：**一个[聚合](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Entities)是一群绑定在一起的对象（实体和值对象）。一个聚合根是一个特定类型的实体以及一些额外的功能。

●**存储库Repository**(接口)：一个[存储库](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Repositories)是领域层和应用层使用的一个类似集合的接口访问数据持久性系统的层(数据库）。它隐藏了 DBMS 业务代码的复杂性。领域层包含仓储接口。

●**领域服务Domain Service：**一个[领域服务](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Domain-Services)是无状态的服务实现领域的核心业务规则。用于实现依赖于多个聚合（实体）类型或一些外部服务的领域逻辑。

●**规约模式Specification：**[规约](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Specifications)用来定义命名的、可重用的、可组合的业务对象过滤器

●**领域事件Domain Event：**[领域事件](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Event-Bus)是以一种松散耦合的方式当领域特定事件发生时通知其他服务的方式

**应用层构建块**

●**应用服务Application Service：**一个[应用服务](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Application-Services)是无状态的实现应用用例的服务。一个应用程序服务通常获取和返回 DTO,被展示层调用。它调用相关领域对象来实现用例。一个用例通常被视为一个工作单元。

●**数据传输对象Data Transfer Object(DTO)：**[DTO](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Data-Transfer-Objects)是一个在应用层和表示层(展示层)之间用于传输状态(数据)的没有任何业务逻辑的简单对象。

●**工作单元Unit of Work(UOW)：**[工作单元](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Unit-Of-Work)是一个原子工作应该作为一个事务来完成。所有UOW 内的操作应该在成功时提交或在失败时回滚。

## **实现：概览**

### **.NET 解决方案的分层**

下图显示了使用ABP的[应用启动模板](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Startup-Templates/Application)创建的 Visual Studio 解决方案：

![image-20210706092658116](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210706092658116.png)

解决方案名称是IssueTracking，它由多个项目组成。解决方案考虑**DDD 原则**以及**开发**和**部署**实践分层。

以下部分解释了解决方案中的项目:

| 您的解决方案结构可能略有不同，如果您选择不同的 UI 或数据库提供程序。然而领域层和应用层是相同的，这是DDD实践的要点。如果你想了解有关解决方案结构的更多信息参考[应用程序启动模板](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Startup-Templates/Application)文档 |
| ------------------------------------------------------------ |

**领域层Domain Layer** 分为两个项目:

**●IssueTracking.Domain**是**必不可少的领域层**包含之前介绍过的所有**构建块**(实体、值对象、领域服务、规约、存储库接口等)。

**●IssueTracking.Domain.Shared**是很薄的一层包含一些属于领域层的类型，但与所有其他层共享。例如它可能包含一些相关的常量和枚举对象但需要**被其他层重用**。

**应用层Application Layer**也分为两个项目

**●IssueTracking.Application.Contracts包含应用服务接口和这些接口使用的DTO**。此项目可由客户应用程序(包括 UI)共享。

**●IssueTracking.Application是必不可少的应用层**,该层实现Application.Contracts层定义的接口。

**展示层Presentation Layer**

**●IssueTracking.Web**是一个 ASP.NET Core MVC/Razor Pages示例应用程序。这是唯一的可执行文件为应用层和 API 提供服务的应用程序。

| ABP 框架还支持不同类型的 UI 框架包括[Angular](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/UI/Angular/Quick-Start)和[Blazor](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/UI/Blazor/Overall). 在这些情况下，解决方案中不存在IssueTracking.Web 。相反，一个IssueTracking.HttpApi.Host应用程序将在解决方案中将 HTTP API 作为要使用的独立端点提供服务由 UI 应用程序通过 HTTP API 调用。 |
| ------------------------------------------------------------ |

**远程服务层Remote Service Layer**

**●IssueTracking.HttpApi**项目包含通过解决方案定义的 HTTP API。它通常包含 MVC Controller和相关模型(如果有)。因此，您在这个项目中写 HTTP API。

| 大多数时候，API 控制器只是封装应用层服务并将它们公开给远程客户端。通过ABP 框架[自动API控制器系统](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/API/Auto-API-Controllers)**自动配置和公开您的应用层作为 API 控制器的服务**，您通常不需要创建本项目中的控制器。但是，示例启动解决方案包含适用于您需要手动创建 API 控制器的情况。 |
| ------------------------------------------------------------ |

**●IssueTracking.HttpApi.Client**项目在您需要使用 HTTP 的 C# 应用时特别有效。一旦客户端应用程序引用了这个项目，它可以直接[注入](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Dependency-Injection)和使用应用服务。这是通过 ABP 框架 [动态 C#](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/API/Dynamic-CSharp-API-Clients)[客户端 API 代理系统](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/API/Dynamic-CSharp-API-Clients)的帮助

| 解决方案的测试文件夹中有一个控制台应用程序，名为IssueTracking.HttpApi.Client.ConsoleTestApp。它使用IssueTracking.HttpApi.Client项目来使用应用程序公开的 API。这只是一个演示应用程序您可以安全地删除它。你甚至可以删除IssueTracking.HttpApi.Client项目，如果你认为不需要他们 |
| ------------------------------------------------------------ |

**基础设施层Infrastructure Layer**

在 DDD 实现中，您可能只有一个基础设施项目来实现所有的抽象和集成，或者对于每个依赖项，您可能有不同的项目。

我们建议采取一种平衡的方法:为主要基础设施依赖项(如 Entity Framework Core)创建单独的项目和其他基础设施的通用基础设施项目。

ABP的启动方案对于Entity有两个项目框架核心集成:

●IssueTracking.EntityFrameworkCore是必不可少的EF Core 的集成包。应用程序的**DbContext、数据库映射、存储库实现和其他 EF Core 相关内容**位于这里。

●IssueTracking.EntityFrameworkCore.DbMigrations是一个管理 Code First 数据库迁移的特殊项目。这个项目中有一个单独的DbContext跟踪迁移。你通常不会总是接触这个项目，除了你需要创建一个新的数据库迁移或添加具有某些功能的[应用程序模块](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Modules/Index)数据库表，自然需要创建一个新的数据库迁移。

| 您可能想知道为什么 EF Core 有两个项目。主要由于[模块化](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Module-Development-Basics)。每个模块都有自己的独立的DbContext并且您的应用程序也有一个数据库上下文。DbMigrations项目包含跟踪和应用单个迁移路径的模块。尽管大多数时候你不需要知道它，你可以查看 [EF Core migrations](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Entity-Framework-Core-Migrations)文档以获取更多信息。 |
| ------------------------------------------------------------ |

**其他的项目Other Projects**

还有一个项目IssueTracking.DbMigrator，这是一个**迁移**数据库架构的简单控制台应用程序并在您执行它时[播种](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Data-Seeding)**初始**数据。这是一个有用的**实用程序应用程序**，您可以在开发以及在生产环境中使用它。

### **解决方案中项目的依赖关系**

Dependencies of the Projects in the Solution

下图显示了解决方案中项目的基本的依赖关系:

![image-20210706101826285](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210706101826285.png)

这些项目之前已经解释过。现在，我们可以解释依赖的原因：

**●Domain.Shared**是所有其他项目的直接或间接依赖。所以，这里的所有类型适用于所有项目。(Domain.Shared在最供其他层调用，定义枚举、自定义属性、异常和公共模型)

**●Domain**只依赖于Domain.Shared因为它已经是领域的（共享）一部分。例如，一个在Domain.Shared定义的IssueType enum类型可以在Domain Issue实体中引用。

**●Application.Contracts**依赖于Domain.Shared 。这样，您就可以在 DTO 中重用这些类型。例如，Domain.Shared中相同的IssueType enum类型可以被CreateIssueDto用作属性。(Application.Contracts调用Domain.Shared层，定义DTO)

**●应用层Application**依赖于Application.Contracts因为它实现应用服务接口并使用Application.Contracts里面的 DTO。Application也依赖Domain层,因为应用层使用Domain层中定义的领域对象。

**●EntityFrameworkCore**依赖于领域层Domain，因为它将领域对象（实体和值类型）映射到数据库表(因为它是一个 ORM)并实现Domain层中定义的存储库接口。

**●HttpApi**依赖于Application.Contracts，因为其中的控制器注入并使用如前所述应用层服务接口。

**●HttpApi.Client**依赖于Application.Contracts因为之前解释过它可以应用应用层服务。

**●Web**依赖于HttpApi，因为它应用HttpApi中定义的 API。而且，通过这种方式，它间接地消费Application.Contracts项目页面/组件中的应用层服务。

**虚线依赖Dashed Dependencies**

当您查看解决方案时，您将看到另外两个上图中虚线所示的依赖关系。Web项目依赖于应用层Application和EntityFrameworkCore项目，理论上不应该那样，但实际上是这样。

这是因为Web是运行和托管应用程序的最终项目,应用程序需要在运行时调用**应用服务和存储库**的实现。

**此设计决策可能允许您使用实体和表示层中的EF Core对象但是应该严格避免这种情况。然而，我们发现替代设计比较复杂(However, we find the alternative designs over complicated.)**。如果要删除依赖，有两种选择；

**●将Web项目转换为 razor 类库并创建一个新项目，如Web.Host，依赖于Web、Application和EntityFrameworkCore项目托管应用程序。你不在这里写任何 UI 代码，而仅用于托管**。

**●移除Application和EntityFrameworkCore依赖项并在应用程序初始化程序集时加载它们。您可以使用 ABP[插件模块 ](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/PlugIn-Modules)实现这个目标**。

### **基于 DDD 的应用程序的执行流程**

Execution Flow of a DDD Based Application

下图显示了一个典型的基于 DDD 模式开发的应用程序 Web 请求流。

![image-20210706104604666](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210706104604666.png)



●该请求通常始于用户在浏览器上的一个用例操作引发对服务器的 HTTP 请求。

●MVC 控制器或 Razor 页面处理程序表示层(或在分布式服务层)处理请求并执行一些横切这个阶段的过滤([授权](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Authorization),[验证](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Validation),[异常处理](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Exception-Handling)等)。控制器/页面注入相关的应用服务接口并通过发送和接收DTO调用方法。

●应用服务Application Service使用领域对象(实体、存储库接口、领域服务等)实现用例。应用层Application Layer实现一些横切关注点(授权、验证、等等)。应用服务方法应该是[Unit of Work](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Unit-Of-Work)。这意味着它应该是原子的。

大多数横切关注点是通常由 ABP 框架自动实现**，您通常不需要为它们编写代码。**

### **共同原则**

Common Principles

在详细介绍之前，让我们先看看一些总体的 DDD 原则:

**数据库提供者/ORM 独立性Database Provider / ORM Independence**

领域层和应用层应该是 ORM /数据库提供者不可知的。他们应该只依赖于Repository 接口,Repository 接口不使用任何 ORM 特定对象。

这个原则的主要原因：

1.使您的领域层/应用层**基础架构**独立，**因为基础设施可能会在将来或者您可能需要之后支持第二种数据库类型。**

**2.使您的领域层/应用层**专注于业务**通过将基础结构细节隐藏在存储库代码背后**。

3.使您的**自动化测试**更容易，因为您可以在这种情况下模拟存储库。

| 出于对这一原则的尊重，解决方案中的任何项目都没有引用EntityFrameworkCore项目，除了启动应用程序。 |
| ------------------------------------------------------------ |

**关于数据库独立性的讨论原则Discussion About the Database Independence Principle**

特别地**原因 1**深深影响了您的领域**对象设计**（尤其是实体关系）和**应用程序代码**。假设您正在使用 [Entity Framework Core](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Entity-Framework-Core)与关系型数据库。如果您希望以后可以切换到[MongoDB](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/MongoDB)，你不能用一些非常**有用的EF 核心功能**

例如:

●您不能使用[更改跟踪](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.microsoft.com/en-us/ef/core/querying/tracking)因为MongoDB提供程序做不到。所以，你总是需要明确地更新更改的实体。

●在您的实体中你不能用 [导航属性](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.microsoft.com/en-us/ef/core/modeling/relationships%3Ftabs%3Dfluent-api%2Cfluent-api-simple-key%2Csimple-key)(或集合)引用到其他聚合，因为这不能用于文档数据库。参见“规则：仅使用Id引用其他聚合”部分了解更多信息。

如果您认为这些功能对您很**重要**并且您永远不会偏离**EF Core，我们相信它是值得的**扩展这个原则。我们仍然建议使用存储库模式来隐藏基础设施细节。但你可以假设您在设计实体关系时正在使用 EF Core并编写您的应用程序代码。你甚至可以在应用层引用EF Core NuGet 包直接使用异步 LINQ 扩展方法，比如ToListAsync() （参见*IQueryable & Async Operations*部分有关更多信息，请参阅[Repositories](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Repositories)文档）。

**演示技术不可知论Presentation Technology Agnostic**

呈现技术（UI Framework）是最重要的技术之一，更改了现实世界应用程序的部分内容。将**领域层和应用层**设计成完全**不知道**演示技术/框架非常重要。这个原则比较容易实现而ABP的启动模板使它变得更加容易。

在某些情况下，你可能需要在**应用层**和**表示层**有**重复的逻辑**。例如，您可能需要在两者中重复**验证**和**授权**检查。UI层的检查主要是为了**用户体验**而检查应用层和领域层是为了**安全性和数据完整性**。这是完全正常并且必要的。

**关注状态变化，而不是报表Focus on the State Changes, Not Reporting**

DDD 关注领域对象如何**变化和交互**；如何创建实体并更改其属性通过保持数据**完整性/有效性**并实施**业务规则**。

DDD**忽略报表**和批量查询。这并不意味着它们并不重要。如果您的应用程序没有花哨的仪表板和报表，谁会使用它？然而，报表是另一个话题。您通常希望充分利用SQL Server 甚至使用单独的数据源（如ElasticSearch) 用于报表目的。你会写优化查询、创建索引甚至编写存储过程。**只要这些实现没有污染业务逻辑，就可以自由地做所有这些事情**。

## **实现：构建块**

**Implementation: The Building Blocks**

这是本指南的重要部分。我们将介绍和用示例解释一些**明确的规则**。你可以在你的领域驱动设计解决方案中遵循这些规则。

### **领域示例**

**The Example Domain**

这些示例将使用 GitHub 使用的一些你已经很熟悉了的概念，像Issue、Repository、Label和User。下图显示了一些聚合aggregates，聚合根aggregate roots、实体entities,、值对象value object和它们之间关系：

![image-20210706130634616](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210706130634616.png)

问题聚合(Issue Aggregate)由一个问题聚合根(Issue Aggregate Root)组成:包含Comment和IssueLabel集合。其他聚合显示地比较简单，因为我们将关注问题聚合(Issue Aggregate)![image-20210706130852428](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210706130852428.png)

### **聚合Aggregates**

如前所述，[聚合](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Entities)是一组对象（实体和值对象）由聚合根对象(Aggregate Root object)绑定在一起。本节将介绍聚合相关的原则和规则。

------

我们将术语聚合根和子集合实体称为实体，除非我们特别指明编写聚合根或子集合实体。

We refer the term Entity both for Aggregate Root and sub-collection entities unless we explicitly write Aggregate Root or sub-collection entity.

------

**聚合/聚合根原则Aggregate / Aggregate Root Principles**

**业务规则Business Rules**

实体(Entities)负责执行自己的属性相关的业务规则。聚合根实体(Aggregate Root Entities)还负责其子集合实体业务规则实现。

聚合应该通过以下方式保持其自我完整性和有效性:实现领域逻辑和约束。这意味着，与 DTO 不同，实体通过**方法来实现一些业务逻辑**。实际上我们应该尽可能在实体中实现业务逻辑。

**单元Single Unit**

一个聚合作为**查询并保存的最小单元**,包含所有子集合和属性。

**例如**，如果你想添加评论Comment到一个问题Issue，你需要:

●从数据库中获取问题Issue，包括所有子集合(Comments和IssueLabels)

●使用Issue类上的方法添加新评论，像Issue.AddComment(...)

●使用单个数据库操作(更新)将问题Issue(包括所有子集合)保存到数据库中。

对于过去使用**EF Core和关系数据库** 的开发人员来说，这似乎很奇怪。获取问题Issue与所有细节似乎没有必要而且效率低下。我们为什么不可以在不查询任何数据的情况下对数据库执行 SQL Insert命令？

**答案是我们应该执行业务规则并保持代码中的数据一致性和完整性**。如果我们有一个业务规则，比如“*用户不能评论锁定问题*”，我们如何在不检查问题的锁定状态的情况下从数据库中检索它？所以，我们可以执行业务逻辑仅当相关对象在应用程序代码中的时候。

另一方面，**MongoDB**开发人员会发现这条规则非常自然。在 MongoDB 中，一个聚合对象（带有子集合）保存在数据库中的**单个集合**中（虽然它在一个关系数据库中分布几个表中）。所以，当你得到一个聚合时，所有的子集合已经作为查询的一部分被检索，无需任何额外配置。

**ABP 框架有助于在您的应用程序中实现这条原则。**

**示例：向问题添加评论Example: Add a comment to an issue**

```c#
public class IssueAppService : ApplicationService，IIssueAppService
{
	private readonly IRepository<Issue,Guid> _issueRepository;
	public IssueAppService(TRepository<Issue,Guid> issueRepository)
    {
        _issueRepository = issueRepository;
    }
	[Authorize]
	public async Task CreateCommentAsync(CreateCommentDto input)
    {
		var issue = await _issueRepository.GetAsync(input.IssueId);
        issue.AddComment(CurrentUser.GetId(),input.Text);
		await _issueRepository.UpdateAsync(issue);
	}
}
```

_issueRepository.GetAsync方法在默认情况下检索问题Issue与所有详细信息(子集合)作为一个单元。虽然这为 MongoDB 开箱即用，但是对于EF Core您需要配置您的聚合详细信息。但是一旦你配置了，存储库就会自动处理它。_issueRepository.GetAsync方法获取一个可选参数includeDetails，您可以可以在需要时传递false以禁用此行为。

| 请参阅 [EF Core](https://docs.abp.io/en/abp/latest/Entity-Framework-Core) 的加载相关实体部分配置和替代方案的文档 |
| ------------------------------------------------------------ |

Issue.AddComment得到一个userId和comment text，实施必要的业务规则并添加到Issue的 Comments collection。

最后，我们使用__issueRepository.UpdateAsync来保存更改到数据库。_

| EF Core 具有**更改跟踪(change tracking)**功能。所以，你实际上不需要调用_issueRepository.UpdateAsync。这将由 ABP 的工作单元系统在方法结束时自动调用DbContext.SaveChanges()自动保存。但是，对于 MongoDB，您需要显式更新改变的实体。所以，如果你想写你的代码 Database Provider独立，你应该总是调用UpdateAsync方法 |
| ------------------------------------------------------------ |

**事务边界Transaction Boundary**

一个聚合通常被认为是一个事务边界。如果用例使用单个聚合，则读取并将其保存为一个单元，对聚合对象作为原子操作一起保存并且您不需要显式的数据库事务。

然而，在现实生活中，在单个用例中你可能需要改变**不止一个聚合实例**，您需要使用数据库事务以确保**原子更新**和**数据一致性**。因此，ABP 框架使用显式用例的数据库事务(应用程序服务方法边界an application service method boundary)。请参阅[工作单元](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Unit-Of-Work)文档以了解更多信息。

**可串行化Serializability**

一个聚合(带有根实体和子集合)应该可以作为一个单元在网络上串行化和传输。例如，MongoDB 将聚合序列化为 JSON 文档在保存到数据库并从 JSON 反序列化时从数据库中读取。当您使用关系数据库和 ORM时，此要求不是必需的。然而，这是一个重要的领域驱动设计实践。

以下规则已经带来了可序列化性。

**聚合/聚合根规则和最佳实践Aggregate / Aggregate Root Rules & Best Practices**

以下规则确保实现以上介绍的原则:

1. **仅通过 ID 引用其他聚合Reference Other Aggregates Only by ID**

   第一条规则说一个聚合应该仅使用ID引用其他聚合。这意味着你不能添加导航属性到其他聚合。

   ●这条规则使得实现可序列化成为可能。

   ●它还可以防止不同的聚合间相互操作或聚合间业务逻辑泄漏。

   您在下面的例子会看到两个聚合根，GitRepository和Issue:

   ```C#
   public class GitRepository : AggregateRoot<Guid>
   {
   	public string Name { get; set; }
       public int starcount { get; set; }
       //错误做法
   	public collection<Issue> Issues{get; set;}
   )
       
   public class Issue : AggregateRoot<Guid>
   {
   	public string Text { get; set; }
       //错误做法
   	public GitRepository Repository { get; set;} 
       //正确做法
       public Guid RepositoryId { get; set; }
   }
   ```

   ●GitRepository不应包含Issue的集合因为它们是不同的聚合。

   ●Issue不应具有GitRepository相关的导航属性，因为它是一个不同的聚合。

   ●Issue可以有RepositoryId (作为Guid)。

   因此，当您遇到Issue并需要与此问题相关的GitRepository 时，您需要通过RepositoryId从数据库查询。

2. **对于 EF Core 和关系数据库For EF Core & Relational Databases**

   在MongoDB中，自然不适合有这样的导航属性/集合。如果你这样做，你会找到一份数据库集合中的目标聚合对象源聚合，因为它在保存时被序列化为 JSON。

   但是，EF Core 和关系数据库开发人员可能会发现这个限制性规则是不必要的，因为 EF Core在数据库读写时可以处理它。我们认为这是一个重要的规则有助于**降低**领域**的复杂性**以及潜在的问题，我们强烈建议实施此规则。但是，如果您认为忽略此规则是可行的，请参阅以上部分*关于数据库独立原则*的*讨论*。

3. **保持小聚合Keep Aggregates Small**

   一个好的做法是保持聚合simple和small。这是因为聚合将被加载并保存为单个单元而读/写大对象有性能问题。

   请参阅下面的示例：

   ```c#
   public class Role : AggregateRoot<Guid>
   {
   	public string Name { get; set; }
       //错误的做法
   	public collection<UserRole> users { get; set;}
   }
   public class UserRole : valueobject
   {
   	public Guid UserId { get; set;}
   	public Guid RoleId { get; set;}
   }
   
   
   public class User : AggregateRoot<Guid>
   {
       public string Name { get; set; }
       //正确的做法
       public collection<UserRole> Roles { get; set;}
   }
   ```

   角色聚合Role aggregate有一组UserRole值对象跟踪为此角色分配的用户。注意UserRole不是另一个聚合，它不是*仅按 Id 引用其他聚合*规则的问题。然而，它是一个实际中的问题。在现实生活场景中一个角色可能被分配给数千个(甚至数百万)用户，每当您从数据库中查询角色将加载数千个Item这是一个重要的性能问题（记住：聚合由它们的子集合作为一个单元被加载)。

   另一方面，用户User可能有这样一个角色集合(Roles collection)，因为用户实际上没有太多角色，当你在使用用户聚合时有一个角色列表它可能很有用。

   如果仔细想想，在使用非关系数据库如**MongoDB**时如果 Role和User都有对方相关的集合。在这种情况下，相同的信息是在不同的集合中重复，将很难保持数据一致性(每当您向User.Roles添加角色,您也需要将其添加到Role.Users 中)。

   因此，根据以下情况考虑聚合的边界和大小:

   - 一起使用的对象。
   - 查询(加载/保存)性能和内存消费
   - 数据完整性、有效性和一致性。

   在实践中；

   - 大多数聚合根**不会有子集合not have sub-collections**
   - 在大多数情况下一个子集合里面的**项目**不应超过**100-150**。如果你认为一个集合可能有更多的项目，不要定义集合作为聚合的一部分并将实体内集合提取为另一个聚合根。

4. **聚合根/实体上的主键Primary Keys on the Aggregate Roots / Entities**

   - 聚合根通常具有单个Id属性，用于它的标识符（Primark Key：PK）。我们更喜欢Guid作为聚合根实体的 PK（参见 [指南](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Guid-Generation)[生成文档](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Guid-Generation)以了解原因）。

   - 一个聚合内实体（不是聚合根）可以使用复合主键。

     例如，请参阅下面的聚合根和实体：

     

   ```c#
   //Aggregate Root
   //Define a single Primary Key (ld)
   public class organization
   {
       public Guid Id { get; set; }
       public string Name { get; set; }
       //
   }
   //Entity
   //Can define a composite Primary Key
   public class OrganizationUser
   {
       //OrganizationId and UserId as Primary Key
       public Guid OrganizationId { get; set; }
   	public Guid UserId { get; set; }
       
       public bool Isowner { get; set; }
   }
   ```

   - organization有一个Guid标识符 ( Id )
   - OrganizationUser是的Organizationd的子集有一个由OrganizationId和UserId组成的复合主键 
   - 这并不意味着子集合实体应该始终具有复合 PK。当需要时，它们可能具有单个Id属性。
   - 复合PK实际上是关系数据库的一个概念因为子集合实体有自己的表需要PK。另一方面，例如，在 MongoDB 中，您根本不需要为子集合实体定义 PK，因为它们作为聚合根的一部分存储。

5. **聚合根/实体的构造函数Constructors of the Aggregate Roots / Entities**

   构造函数位于实体生命周期所在的位置开始。一个设计良好的构造函数：

   - 获取**所需的实体属性**作为参数**创建一个有效的实体**。应该强制传递必需的参数，非必需的属性作为可选参数。
   - **检查**参数的**有效性**。
   - 初始化**子集合**

   **示例问题（聚合根）构造函数Example Issue (Aggregate Root) constructor**

   ```c#
   using System;
   using System.Collections. Generic;
   using System.Collections.ObjectModel;using Volo.Abp;
   using Volo.Abp. Domain.Entities;
   namespace IssueTracking.Issues
   {
   	public class Issue : AggregateRoot<Guid>
       {
   		public Guid RepositoryId { get;set;}
           public string Title { get; set;}
   		public string Text { get;set;}
   		public Guid? AssignedUserId { get; set;}
           public bool IsClosed { get; set;}
   		public IssueCloseReason? CloseReason { get;set;} //enum
   		public ICollection<IssueLabel> Labels { get; set;}
   		public Issue(Guid id,Guid repositoryId,string title,string text = null,Guid? assignedUserId = null): base(id)
           {
               Repositoryld = repositoryId;
               Title = Check.NotNul10rWhiteSpace(title，nameof(title));
               Text = text;
               AssignedUserId = assignedUserId;
               Labels = new Collection<IssueLabel>();
           }
           private Issue(){/* for deserialization & ORMs*/}
   	}	
   }
   ```

   - Issue通过构造函数传入最少所需必须属性创建一个有效的实体
   - 构造函数验证输入(Check.NotNullOrWhiteSpace(...)如果给定值为空，则抛出ArgumentException ）
   - **初始化子集合**，所以创建Issue后你尝试使用Labels时不会得到一个空值的引用异常。
   - 构造函数也**接受**id并传递给基类。我们不会在构造函数中生成Guid而把这个责任委托给另一个服务([Guid Generation](https://translate.google.com/translate?hl=zh-CN&prev=_t&sl=auto&tl=zh-CN&u=https://docs.abp.io/en/abp/latest/Guid-Generation))
   - ORM 需要私有的**空构造**函数。我们将其设为私有以防止在我们自己意外使用它代码。
   - 见 [实体](https://docs.abp.io/en/abp/latest/Entities)文档以了解有关创建实体的更多信息

6. **实体属性访问器和方法Entity Property Accessors & Methods**

   你可能觉得上面的例子很奇怪！例如，我们强制在构造函数中传递一个非空的Title。然而，开发人员可以将Title属性设置为null，而无需任何控制。这是因为上面的示例代码只关注构造函数。如果我们使用公共 setter 声明所有属性（例如例如上面的Issue类），我们不能在实体生命周期中强制有效性和完整性。

   所以

   - 当你需要在设置该属性时执行任何逻辑那么需要把为属性Setter设置为**私有 **
   - 定义公共方法来操作这些属性

   **示例：在受控对象中更改属性的方法Example: Methods to change the properties in a controlled way**

   ```c#
   using System;
   using Volo.Abp;
   using Volo.Abp.Domain.Entities;
   namespace IssueTracking. Issues
   {
   	public class Issue : AggregateRoot<Guid>
       {
   		public Guid RepositoryId { get; private set;}//Never changes
           public string Title { get; private set;}//Needs validation
           public string Text { get; set; }//No validation
   		public Guid? AssignedUserId { get; set;}//No validation
   		public bool IsClosed { get; private set; }//Should be changed with CloseReason
   		public IssueCloseReason? CloseReason { get; private set; }//Should be changed with IsC
   
   		public void SetTitle(string title)
   		{
   			Title = Check.NotNul10rWhiteSpace(title，nameof(title));
           }
   		public void Close(IssueCloseReason reason)
           {
               IsClosed = true;
               CloseReason = reason;
           }
           public void ReOpen()
           {
           	IsClosed = false;
               CloseReason = null;
           }
   	}
   }
   ```

   - RepositoryId setter 设为私有，Issue创建后没有办法更改它，这是我们想要的：一个问题不能转移到另一个仓储。
   - Title setter已设为私有且如果您想稍后以受控方式更改它，则创建SetTitle方法。
   - Text和AssignedUserId有public setters，因为有对他们没有限制。它们可以为 null 或任何其他值。我们认为没有必要单独定义方法来设置它们。如果我们以后需要，我们可以添加方法并将 setter 设为私有。**领域层重大变化没有问题，因为领域层是一个内部项目，它不暴露给客户。**
   - IsClosed和IssueCloseReason是成对属性。定义Close和ReOpen方法来一起改变它们。通过这种方式，我们可以防止issue在没有任何原因的情况下被关闭。

7. **实体中的业务逻辑和异常Business Logic & Exceptions in the Entities**

   当您在实体中实现验证和业务逻辑时，您经常需要处理特殊情况。在这些情况下:

   - 创建特定于领域的异常
   - 必要时从实体方法中抛出这些异常

   例如：

   ```c#
   public class Issue : AggregateRoot<Guid>
   {
       //...
       public bool IsLocked { get; private set;}
       public bool IsClosed { get; private set;}
       public IssueCloseReason? CloseReason { get;private set;}
       public void Close(IssueCloseReason reason)
       {
           IsClosed = true;
           CloseReason = reason;
       }
       public void Re0pen()
       {
       	if (IsLocked)
           {
              throw new IssueStateException("Can not open a locked issue! Unlock it first.");
           }
       	IsClosed = false;CloseReason = null;
       }
       public void Lock()
       {
   		if(!IsClosed)
           {
              throw new IssueStateException("Can not open a locked issue! Unlock it first.");
           }
           IsLocked = true;
   	}
   	public void Unlock()
       {
   		IsLocked = false;
   	}
   }
   ```

   这里有两个业务规则:

   - 无法重新打开锁定的问题。
   - 您无法锁定未解决的问题。

   在这些情况下，问题类会抛出一个IssueStateException强制业务规则

   ```C#
   using System;
   namespace IssueTracking.Issues
   {
   	public class IssueStateException : Exception
       {
   		public IssueStateException(string message): base(message)
           {
           }
   	}
   }
   ```

   抛出这样的异常有两个潜在的问题:

   1. 如果出现此类异常，**最终用户**是否应该看到异常（错误）消息？如果是这样，你怎么**定位**的异常消息？你不能使用[本地化](https://docs.abp.io/en/abp/latest/Localization)系统，因为你不能注入和使用IStringLocalizer在实体中。
   2. 对于 Web 应用程序或 HTTP API，应该返回给客户端什么**HTTP 状态码**？

   ABP 的[异常处理](https://docs.abp.io/en/abp/latest/Exception-Handling)系统解决了这些以及类似的问题。

   **示例：使用代码抛出业务异常Throwing a business exception with code**

   ```c#
   using Volo.Abp;
   namespace IssueTracking.Issues
   {
   	public class IssueStateException : BusinessException
       {
   		public IssueStateException(string code): base(code)
           {
           }
   	}
   }
   ```

   - IssueStateException类继承了BusinessException类。从BusinessException派生的异常ABP 默认返回 403（禁止）HTTP 状态代码(而不是 500 - 内部服务器错误)
   - 该Code被用作在本地化资源文件中找到本地化的消息的Key

   现在，我们可以更改ReOpen方法，如下所示：

   ```c#
   public void ReOpen()
   {
       if (IsLocked)
       {
       	throw new IssueStateException("IssueTracking: CanNotOpenLockedIssue");
       }
   	IsClosed=false;
       CloseReason = null;
   }
   
   //使用常量而不是魔术字符串。
   
   //向本地化资源添加一个条目，如下所示：
   "IssueTracking:CanNotOpenLockedIssue":"Can not open a locked issue! Unlock it first."
   ```

   - 当你抛出异常时，ABP 自动使用本地化消息(基于当前语言)显示给最终用户。
   - 异常代码（这里是IssueTracking:CanNotOpenLockedIssue）也发送给客户端，所以它可以以编程方式处理错误情况。
   - 对于这个例子，你可以直接抛出BusinessException而不是定义一个专门的问题状态异常。结果将是相同的。见[异常处理文档](https://docs.abp.io/en/abp/latest/Exception-Handling)的所有细节

   

8. **需要外部服务的实体中的业务逻辑Business Logic in Entities Requiring External Services**

   **当业务逻辑仅使用该实体的属性时在实体方法中实现业务逻辑很简单。**如果业务逻辑需要查询数据库**或者**使用任何需要**[依赖注入](https://docs.abp.io/en/abp/latest/Dependency-Injection)系统解决的外部服务。记住实体不能注入服务！**

   有两种常见的方式来实现这样的业务逻辑：

   - 在实体方法上**把外部依赖作为参数**的方法实现业务逻辑
   - 创建**领域服务。**

   领域服务将在后面解释。但是，现在让我们看看它如何在实体类中实现。

   **示例：业务规则：不能同时分配超过 3 个open issues to a user**

   ```c#
   public class Issue : AggregateRoot<Guid>
   {
       public Guid? AssignedUserId { get;private set; }
   	public async Task AssignToAsync(AppUser user，IUserIssueService userIssueService)
       {
           var openIssueCount = await userlssueService.Get0penIssueCountAsync(user.Id);
   		if (openIssueCount >= 3)
           {
           	throw new BusinessException("IssueTracking:ConcurrentOpenIssueLimit");
           }
           AssignedUserId = user.Id;
       }
       public void CleanAssignment()
       {
       	AssignedUserId = null;
       }
   }
   ```

   - AssignedUserId属性设置器设为私有。所以，唯一更改它的方式是使用AssignToAsync 的方法和CleanAssignment方法
   - AssignToAsync获取一个AppUser实体。实际上只使用 user.Id，因此您可以获得Guid值，例如userId。然而，**这种方式可以确保的Guid值是现有用户标识的而不是随机的Guid值**
   - IUserIssueService是一个任意服务，用于获取用户的未解决问题总数。通过方法AssignToAsync传递IUserIssueService
   - 如果业务规则不匹配， AssignToAsync抛出异常
   - 最后，如果一切正确，设置AssignedUserId属性

   这个方法完美的保证了当您想将问题分配给用户时业务逻辑的应用。然而，它有一些问题:

   - 它使实体类**依赖于外部服务**这使得实体变得**复杂**
   - 它使得实体很难使用。调用实体代码现在需要注入IUserIssueService服务并传递给AssignToAsync方法

   实现此业务逻辑的另一种方法是引入一个**Domain Service领域服务**，后面会解释。

### **仓储模式Repositories**

一个[仓储](https://docs.abp.io/en/abp/latest/Repositories)是一个类似集合的接口被领域层和应用层用来访问持久化系统(例如数据库)读取和写入业务对象,通常是聚合。

常见的存储库原则是：

- **在领域层**定义存储库**接口**(因为它用于领域和应用层),**在基础设施层实现**(启动模板中的*EntityFrameworkCore*项目)。
- **不要**在存储库中**包含业务逻辑**。
- 存储库接口应该是**数据库提供者/ORM**独立的。**例如，不从存储库方法返回DbSet。DbSet是 EF  Core提供的对象。**
- **为聚合根创建存储库**，而不是所有实体。因为，子集合实体(对一个聚合来说)应该通过聚合根访问。

**不要在存储库中包含域逻辑**

虽然这条规则一开始看起来很明显，但很容易将业务逻辑泄露到存储库中。

**示例：从存储库中获取非活动问题Get inactive issues from a repository**

```C#
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Volo.Abp.Domain.Repositories;
namespace IssueTracking.Issues
{
	public interface IIssueRepository : TRepository<Issue, Guid>
    {
		Task<List<Issue>>GetInActiveIssuesAsync();
    }
}
```

IIssueRepository通过添加GetInActiveIssuesAsync方法扩展了标准的IRepository<...>接口。这个存储库使用以下Issue类：

```C#
public class Issue : AggregateRoot<Guid>,IHasCreationTime
{
	public bool IsClosed { get; private set;}
	public Guid? AssignedUserId{ get; private set;}
    public DateTime CreationTime { get;private set;}
    public DateTime? LastCommentTime { get; private set;}
    //...代码只显示了我们在这个例子中需要的属性
}
```

规则说**存储库不应该知道业务规则**。这里的问题是“**什么是不活跃InActive的问题？**它是一项业务规则吗？"

让我们看一下实现来理解它：

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using IssueTracking.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Volo.Abp.Domain.Repositories.EntityFrameworkCore;
using Volo.Abp.EntityFrameworkCore;
namespace IssueTracking.Issues
{
	public class EfCoreIssueRepository :EfCoreRepository<IssueTrackingDbContext，Issue，Guid>,IIssueRepository
	{
		public EfCoreIssueRepository(IDbContextProvider<IssueTrackingDbContext> dbContextProvider): base(dbContextProvider)
        {
        }
		public async Task<List<Issue>> GetInActiveIssuesAsync()
        {
            var daysAgo30 = DateTime.Now.Subtract(TimeSpan.FromDays(30));
			var dbSet = await GetDbSetAsync();
            return await dbSet.Where(i=>
            //0pen
            !i.IsClosed&&
                                     
            //Assigned to nobody
            i.AssignedUserId == null &&
                                     
            //Created 30+ days ago
            i.CreationTime< daysAgo30 &&
                                     
            //No comment or the last comment was30+days ago
            (i.LastCommentTime == null || i.LastCommentTime < daysAgo30)
            ).ToListAsync();

        }
    }
}
```

（使用 EF Core 进行实现。请参阅[EF Core integration document](https://docs.abp.io/en/abp/latest/Entity-Framework-Core)以了解如何通过 EF Core创建自定义存储库）

当我们检查GetInActiveIssuesAsync实现时，我们看到定义了一个in-active issue业务规则：The issue应该是open, assigned to nobody, created 30+ days
ago**并且**在过去 30 天内没有评论。

这是隐式定义在存储库方法中隐藏的业务规则。当我们需要重用这个业务逻辑的时候问题就出现了。

例如，假设我们要在实体上添加一个bool IsInActive()方法。这样，当我们有一个issue实体时我们可以检查issue活跃度。

让我们看看实现：

```c#
public class Issue : AggregateRoot<Guid>，IHasCreationTime
{
	public bool IsClosed { get; private set;}
	public Guid? AssignedUserId { get; private set;}
    public DateTime CreationTime { get;private set;}
    public DateTime? LastCommentTime { get; private set;}
    //...
    
	public bool IsInActive()
    {
		var daysAgo30 = DateTime.Now.Subtract(TimeSpan. FromDays(30));
        return
                //Open
                !IsClosed &&
            
                //Assigned to nobody
                AssignedUserId==null&&
            
                //Created 30+ days ago
                CreationTime < daysAgo30 &&
            
                //No comment or the last comment was 30+ days ago
                (LastCommentTime ==null || LastCommentTime < daysAgo30);
	}
}
```

我们不得不复制/粘贴/修改代码。如果定义活跃度规则发生变化？我们不应该忘记更新两个地方。这是重复的业务逻辑，这是很危险的。

这个问题的一个很好的解决方案是*规约模式*！

### **规约模式Specifications**

一个[规约](https://docs.abp.io/en/abp/latest/Specifications)是一个**命名的，可重复使用，可组合**和**可测试**的类以根据业务规则过滤领域对象。

ABP 框架提供了必要的基础设施来轻松地创建规约类并在您的应用程序内部使用它们。让我们将in-active issue过滤器实现为规约模式：

```c#
using System;
using System.Linq.Expressions;
using Volo.Abp.Specifications;
namespace IssueTracking.Issues
{
	public class InActiveIssueSpecification : Specification<Issue>
    {
		public override Expression<Func<Issue,bool>> ToExpression()
        {
			var daysAgo30 = DateTime.Now.Subtract(TimeSpan.FromDays(30));
            return i=>
                    //Open
                    !i.IsClosed &&
                
                    //Assigned to nobody
                    i.AssignedUserId == null &&
                    
                    //Created 30+ days ago
                    i.CreationTime< daysAgo30 &&
                    
                	//No comment or the last comment was 30+ days ago
                    (i.LastCommentTime == null || i.LastCommentTime < daysAgo30);
		}
	}
}
```

Specification<T>基类简化了通过定义表达式来创建规约类。把仓储部分的表达式移动到规约类。现在，我们可以在Issue实体和EfCoreIssueRepository类中重用InActiveIssueSpecification

**在实体内使用Using within the Entity**

规约类提供了一个IsSatisfiedBy方法返回真如果给定对象（实体）满足规约。我们可以将Issue.IsInActive方法重写为如下图：

```C#
public class Issue : AggregateRoot<Guid>，IHasCreationTime
{
	public bool IsClosed { get; private set;}
	public Guid? AssignedUserId { get; private set;}
    public DateTime CreationTime { get;private set;}
    public DateTime? LastCommentTime { get; private set;}
    //...
    
	public bool IsInActive()
    {
        return new InActiveIssueSpecification().IsSatisfiedBy(this);
	}
}
```

**与存储库一起使用Using with the Repositories**

首先，从存储库接口开始：

```c#
public interface IIssueRepository : IRepository<Issue, Guid>
{
	Task<List<Issue>> GetIssuesAsync(ISpecification<Issue> spec);
}
```

重命名GetInActiveIssuesAsync为GetIssuesAsync通过传入一个规约对象。由于自**规格(过滤器)已移出存储库**，我们不再需要创建不同的方法来解决不同条件下的问题（如GetAssignedIssues(...)、GetLockedIssues(...)等）

更新后的存储库的实现可能是这样的：

```c#
public class EfCoreIssueRepository : EfCoreRepository<IssueTrackingDbContext,Issue，Guid>,IIssueRepository
{
    public EfCoreIssueRepository(IDbContextProvider<IssueTrackingDbContext> dbContextProvider): base(dbContextProvider)
    {
        
    }
	public async Task<List<Issue>> GetIssuesAsync(ISpecification<Issue> spec)
    {
		var dbSet = await GetDbSetAsync();
        return await dbSet.Where(spec.ToExpression>.ToListAsync();
	}
}
```

由于ToExpression()方法返回一个表达式，它可以直接传递给Where方法来过滤实体。最后，我们可以将任何 Specification 实例传递给GetIssuesAsync方法：

```c#
public class IssueAppService : ApplicationService,IIssueAppService
{
	private readonly IIssueRepository _issueRepository;
	public IssueAppService(IIssueRepository issueRepository)
    {
		_issueRepository = issueRepository;
	}
	public async Task DoItAsync()
    {
		var issues= await _issueRepository.GetIssuesAsync(new InActiveIssueSpecification());
		//...
	}
}
```

**使用默认存储库With Default Repositories**

实际上，您不必创建自定义存储库即可使用规约模式。标准的IRepository已经在IQueryable扩展，所以你可以使用标准的LINQ扩展方法：

```C#
public class IssueAppService : ApplicationService,IIssueAppService
{
	private readonly IRepository<Issue,Guid> _issueRepository;
	public IssueAppService(IIssueRepository issueRepository)
    {
		_issueRepository = issueRepository;
	}
	public async Task DoItAsync()
    {
        var queryable=await _issueRepository.GetQueryableAsync();
		var issues= AsyncExecuter.ToListAsync(queryable.Where(new InActiveIssueSpecification()));
		//...
	}
}
```

AsyncExecuter是 ABP 框架提供的一个实用程序，用于使用异步 LINQ 扩展方法（如此处ToListAsync）而不依赖于 EF Core NuGet 包。查看 [存储库文档](https://docs.abp.io/en/abp/latest/Repositories)以获取更多信息。

**组合规约Combining the Specifications**

规约的一个强大方面是它们是可组合的。假设我们有另一个返回true 的规约仅当Issue在Milestone时：

```c#
public class MilestoneSpecification : Specification<Issue>
{
	public Guid MilestoneId { get;}
	public MilestoneSpecification(Guid milestoneId)
    {
		MilestoneId = milestoneId;
    }
	public override Expression<Func<Issue, bool>> ToExpression()
    {
		return i => i.MilestoneId == MilestoneId;
	}
}
```

与InActiveIssueSpecification不同，该规约是*参数化*的。我们可以结合两种规格获取特定milestone中的inactive issues列表：

```c#
public class IssueAppService : ApplicationService,IIssueAppService
{
	private readonly IRepository<Issue，Guid> _issueRepository;
    public IssueAppService(IRepository<Issue,Guid issueRepository)
    {
		_issueRepository = issueRepository;
	}
	public async Task DoItAsync(Guid milestoneId)
    {
		var queryable = await _issueRepository.GetQueryableAsyncO;
        var issues = AsyncExecuter.ToListAsync(
					queryable
						.Where(new InActiveIssueSpecification
                               .And(new MilestoneSpecification(milestoneId))
                               .ToExpression()
                               )
            		);	

	}
}
```

上面的例子使用And扩展方法来组合规约。还有更多的组合方法可用，例如Or(...)和AndNot(...)。有关更多详细信息查看ABP 提供的规范基础设施框架[规约文档](https://docs.abp.io/en/abp/latest/Specifications)。

### **领域服务Domain Services**

领域服务实现领域逻辑，其中:

- 依赖**服务和存储库**
- 逻辑实现需要使用**多个聚合**，所以不适合放到任何聚合内部。

领域服务与领域对象一起工作。他们的方法可以**获取和返回实体、值对象、原始类型...**等。但是，**他们不会获取/返回 DTOs**。DTO 是应用层的一部分。

**示例：将问题分配给用户Assigning an issue to a user**

记住Issue实体是如何实现问题分配的：

```c#
public class Issue : AggregateRoot<Guid>
{
	//...
	public Guid? AssignedUserld { get; private set;}
	public async Task AssignToAsync(AppUser user,TUserIssueService userIssueService)
    {
		var openIssueCount = await userIssueService.Get0penissueCountAsync(user.Id);
        if CopenIssueCount >- 3)
        {
       		 throw new BusinessException("IssueTracking:ConcurrentOpenIssueLimit");
        }
    	AssignedUserId - user.Id;
    }
	public void CleanAssignment()
    {
        AssignedUserId = null;
    }
}
```

在这里，我们将把这个逻辑移到领域服务中。

首先，更改Issue类：

```c#
public class Issue : AggregateRoot<Guid>
{
    //...
    public Guid? AssignedUserId { get; internal set;}
}
```

- 删除了与分配相关的方法
- 将AssignedUserId属性的 setter 从私有更改为internal ，允许从领域服务设置它。

下一步是创建一个领域服务，命名为IssueManager ，具有AssignToAsync来分配给定的问题给给定的用户。

```c#
public class IssueManager : DomainService
{
	private readonly IRepository<Issue,Guid>_issueRepository;
    public IssueManager(IRepository<Issue，Guid> issueRepository)
    {
		_issueRepository = issueRepository;
    }
	public asyne Task AssignToAsync(Issue issue,AppUser user)
    {
        var openIssueCount = await _issueRepository.CountAsync(
				i =>i.AssignedUserId == user.Id && !i.IsClosed);
        if (openIssueCount >= 3)
        {
        	throw new BusinessException("IssueTracking:ConcurrentOpenIssueLimit");
        }
		issue.AssignedUserId = user.Id;
    }
}
```

IssueManager可以注入任何服务依赖项并用于查询用户的未解决问题计数(open issue count)

**我们更喜欢并建议对领域使用Manager后缀服务。**

这种设计的唯一问题是Issue.AssignedUserId对类外修改开放的。但是，它不是public的，是internal的只能在同一个内部程序集修改它，如此示例解决方案的IssueTracking.Domain项目。我们认为这是合理的:

- 领域层开发人员已经意识到领域规则，他们使用IssueManager
- 应用层开发人员已经被迫使用IssueManager因为他们不直接设置它

虽然两种方法之间存在权衡，但当业务逻辑需要外部服务的时候我们更喜欢创建领域服务。

**如果你没有充分的理由，我们认为没有必要为领域服务创建接口（为IssueManager创建IIssueManager接口）**

### **应用服务Application Services**

[应用服务](https://docs.abp.io/en/abp/latest/Application-Services)是一个无状态的实现应用程序**用例**的服务。应用服务通常**获取并返回 DTO** 。它由展示层使用。它**使用和协调领域对象**（实体、存储库等）来实现用例。

应用服务的共同原则是：

- 实现特定于当前用例的**应用程序逻辑**。不实现核心域应用服务内部的逻辑。我们会回到应用程序领域逻辑之间的差异。
- **永远不要**为应用服务方法**获取或返回实体**。这打破了领域层的封装。始终获取和返回 DTO。

**示例：将问题分配给用户Assigning an issue to a user**

```C#
using System;
using System.Threading.Tasks;
using IssueTracking.Users;
using Microsoft.AspNetCore.Authorization;
using Volo.Abp.Application.Services;
using Volo.Abp.Domain.Repositories;
namespace IssueTracking.Issues
{
	public class IssueAppService : ApplicationService,IIssueAppService
    {
        private readonly IssueManager _issueManager;
        private readonly IRepository<Issue, Guid> _issueRepository;
        private readonly IRepository<AppUser,Guid> _userRepository;
        
        public IssueAppService(IssueManager issueManager,IRepository<Issue,Guid>issueRepository,
                               IRepository<AppUser,Guid> userRepository)
        {
            _issueManager = issueManager;
            _issueRepository = issueRepository;
            _userRepository = userRepository;
        }
        [Authorize]
		public async Task AssignAsync (IssueAssignDto input)
        {
			var issue = await _issueRepository.GetAsync(input. IssueId);
            var user = await__userRepository.GetAsync(input.UserId);
            await _issueManager.AssignToAsync(issue，user);
			await _issueRepository.UpdateAsync(issue);
		}
    }
}
```

一个应用服务方法通常像这里实现的那样包含三个步骤：

1. 从数据库中获取相关的领域对象来实现用例。
2. 使用领域对象（领域服务、实体等）来执行实际操作。
3. 更新已更改的实体到数据库中。

本例中的IssueAssignDto是一个简单的 DTO 类：

```c#
using System;
namespace IssueTracking.Issues
{
	public class IssueAssignDto
    {
		public Guid IssueId { get;set;}
    	public Guid UserId { get;set;}
	}
}
```

| 如果你使用的是 EF Core，则不需要最后一次更新(await _issueRepository.UpdateAsync(issue))因为它有一个变更跟踪系统。如果你想使用EF Core 变更跟踪功能的优势，请参阅上面*关于数据库独立原则*部分的讨论。 |
| ------------------------------------------------------------ |

### **数据传输对象Data transfer Objects**

[DTO](https://docs.abp.io/en/abp/latest/Data-Transfer-Objects)是应用层和表现层之间用于传送状态的简单对象（数据）。所以，应用服务Application Service方法获取和返回 DTO。

**通用 DTO 原则和最佳实践**

- 就其性质而言，DTO**应该是可序列化的**。因为，大多数时候它是通过网络传输的。所以应该有一个**无参数（空）构造函数**
- 不应包含任何**业务逻辑**
- **永远不要**继承或引用**实体。**

**输入 DTO(Input DTOs)** （那些被传递到应用服务方法）与**输出 DTO(Output DTOs)**（那些从应用程序服务方法返回）是不同的。所以，他们应该被区别对待。

**输入 DTO 最佳实践**

- **不要为输入 DTO 定义未使用的属性**

仅**定义用例**所需的属性！否则，客户在使用该应用服务时会感到困惑。您当然可以定义**可选属性，**但是当客户端提供它们时它们应该影响用例的工作方式。

首先这条规则似乎没有必要。谁会为一个方法定义未使用的参数（输入 DTO 属性）？但它会发生，尤其是当您尝试重用输入 DTO 时。

- **不要重复使用输入 DTO**

**为每个用例**定义一个**专门的输入 DTO**（应用服务方法）。否则，在某些情况下某些属性没有被使用，这违反了上面定义的规则：*不要为输入 DTO 定义未使用的属性*。

有时，两个用例重用相同的 DTO 类似乎很有吸引力，因为它们几乎相同。即使现在他们是一样的，可能会随着时间变得不同你会遇到同样的问题。**代码重复是一种比耦合用例更好的实践。**重用输入的DTO的另一种方式是DTO彼此**继承**。虽然这在极少数情况下很有用，但大多数情况下它把你带到同样的问题。

**示例：用户应用服务User Application Service**

```c#
public interface IUserAppService : IApplicationService
{
	Task CreateAsync(UserDto input);
    Task UpdateAsync(UserDto input);
	Task ChangePasswordAsync(UserDto input);
}
```

IUserAppService在所有方法中都使用UserDto作为输入 DTO（用例）。UserDto定义如下：

```c#
public class UserDto
{
	public Guid Id { get; set; }
	public string UserName { get;set;}
    public string Email { get;set;}
    public string Password { get; set;}
	public DateTime CreationTime { get; set;}
}
```

对于这个例子：

- Create 方法中不使用ID属性由服务器确定
- Update不使用Password属性，因为我们有另一个方法
- CreationTime从未被使用，因为我们不能让客户发送创建时间。它应该在服务器中设置。

一个真正的实现可以是这样的：

```c#
public interface IUserAppService : IApplicationService
{
	Task CreateAsync(UserCreationDto input);
    Task UpdateAsync(UserUpdateDto input);
	Task ChangePasswordAsync(UserChangePasswordDto input);
}
```

使用给定的输入 DTO 类：

```c#
public class UserCreationDto
{
	public string UserName { get;set;]
    public string Email { get; set;}
    public string Password { get;set;}
}
public class UserUpdateDto
{
	public Guid Id { get; set;}
	public string UserName { get;set;}
    public string Email { get; set;}
}
public class UserChangePasswordDto
{
	public Guid Id { get; set;}
	public string Password { get; set;}
}

```

虽然写了更多的代码但是这是更易于维护的方法。

**例外情况：**此规则可能有一些例外：如果你总是想**并行**开发两种方法，它们可能共享相同的输入 DTO（通过继承或直接重用）。例如，如果您的报表页面包含一些过滤器而你有多种应用服务方法（比如 screen报表、excel报表和csv报表方法）使用相同过滤但返回不同的结果，您可能需要重用相同的过滤器输入 DTO 来**耦合这些用例。**因为，在这个例子，每当你改变一个过滤器时，你必须使对所有方法进行必要的更改以保持报表系统的一致。(Notes:就是出现了这么一种情况:输入参数总是相同的只是输出不同格式的数据Excel、CSV等这时候可以考虑使用相同的DTO)

**输入 DTO 验证逻辑**

- 仅在 DTO 内实施格式**验证**。用数据注释验证(Data Annotation Validation)属性或实现IValidatableObject用于格式验证。
- **不要执行领域验证**。例如，不要尝试检查 DTO 中的唯一用户名约束

**示例：使用数据注释属性Using Data Annotation Attributes**

```c#
using System.ComponentModel.DataAnnotations;
namespace IssueTracking.Users
{
    public class UserCreationDto
    {
        [Required]
        [StringLength(UserConsts.MaxUserNameLength)]
        public string UserName { get; set; }
        
        [Required]
        [EmailAddress]
        [StringLength(UserConsts.MaxEmailLength]
        public string Email { get; set;}
                      
        [Required]
        [StringLength(UserConsts.MaxEmailLength,MinimumLength = UserConsts. MinPasswordLength)]
        public string Password { get; set;}
    }
}
```

ABP 框架自动验证输入 DTO，抛出AbpValidationException并将 HTTP 状态400返回给客户端指示输入无效。

| 一些开发人员认为最好将验证规则和 DTO 类分开。我们认为声明式 (DataAnnotation) 方法实用且有用，不会导致任何设计问题。但是，ABP 也支持[FluentValidation ](https://docs.abp.io/en/abp/latest/FluentValidation)如果您更喜欢其他方法，参考[验证文件](https://docs.abp.io/en/abp/latest/Validation)包含的所有验证选项。 |
| ------------------------------------------------------------ |

**输出 DTO 最佳实践**

- 保持输出**DTO总数最少**。尽可能重复使用（例外：不要重用输入 DTO 作为输出 DTO）
- DTO的输出可以包含比用于在客户端代码**更多的属性**
- 从**Create**和**Update**方法返回实体 DTO 

这些建议的主要目标是:

- 使客户端代码易于开发和扩展
  - 处理**相似但不相同的**DTO客户端有问题。
  - 未来 UI/客户端**需要其他属性**是很常见的。返回所有属性（通过考虑实体的安全性和特权）使客户端代码易于改进，无需接触到后端代码。
  - 如果你开放API给**第三方客户端**，你不知道每个客户的需求
- 使服务端代码易于开发和扩展
  - 你可以**了解和维护**更少的类
  - 你可以重用 Entity->DTO**对象映射**代码
  - 从不同的方法返回相同的类型使创建**新方法**变得简单明了

**示例：从不同的方法返回不同的 DTO(Returning Different DTOs from different methods)**

```c#
public interface IUserAppService : IApplicationService
{
	UserDto Get(Guid id);
	List<UserNameAndEmailDto> GetUserNameAndEmail(Guid id);
    List<string>GetRoles(Guid id);
	List<UserListDto> GetList();
	UserCreateResultDto Create(UserCreationDto input);
    UserUpdateResultDto Update(UserUpdateDto input);
}
```

(为了使示例更清晰我们没有使用异步方法，*但在你的真实世界应用程序中请使用异步！)*

上面的示例代码为每个方法返回不同的 DTO 类型。你可以想象到，会有很多重复的代码用于查询数据，将实体映射到 DTO。



上面的IUserAppService服务可以简化：

```c#
public interface TUserAppService : IApplicationService
{
	UserDto Get(Guid id);
    List<UserDto> GetList();
	UserDto Create(UserCreationDto input);
    UserDto Update(UserUpdateDto input);
}
```

使用单输出 DTO：

```C#
public class UserDto
{
	public Guid Id { get;set; }
	public string UserName { get;set;}
    public string Email { get; set;}
	public DateTime CreationTime { get; set;}
    public List<string> Roles { get;set;}
}
```

- 移除了GetUserNameAndEmail和GetRoles方法因为Get方法已经返回了必要的信息
- GetList现在返回与Get相同的DTO
- Create和Update也返回相同的UserDto

如上所述，使用相同的 DTO 有很多优点。例如，考虑在UI上显示用户网格的场景。更新用户后，您可以获得返回值并**在 UI 上更新它**。所以，你不需要再调用GetList。这就是为什么我们建议返回实体 DTO（此处为UserDto）作为Create和Update 的返回值操作。

**讨论Discussion**

一些输出 DTO 的建议可能不适合所有场景。这些建议可能忽略了**性能**，尤其是当**大数据集**返回或当您为你自己的 UI 创建服务，你有**太多并发请求。**

在这些情况下，您可能需要创建**专门的输出信息最少的 DTO。**以上建议是对应用**维护代码库**比**可忽略的性能损失**更重要的情况**。**

**对象到对象映射Object to Object Mapping**

当两个对象具有相同或相似的属性时，自动[对象到对象映射](https://docs.abp.io/en/abp/latest/Object-To-Object-Mapping)是一种有用的方法，将值从一个对象复制到另一个对象。

DTO 和实体Entity类通常具有相同/相似的属性并且您通常需要从实体创建 DTO 对象。ABP 的[对象到对象映射系统](https://docs.abp.io/en/abp/latest/Object-To-Object-Mapping)集成了[AutoMapper](http://automapper.org/)，使这些操作相对于手动映射来说更容易。

- 仅对**实体**到输出 DTO使用**自动对象映射**
- **不要使用DTO 输入到实体**自动对象映射

**不应该使用**输入 DTO到实体自动映射的一些原因：

1. 实体类通常有一个**构造函数**，它接受参数并确保有效的对象创建。自动对象映射操作一般需要一个空的构造函数
2. 大多数实体属性具有**私有 setter**您应该使用受控的方式方法来更改这些属性
3. 您通常需要**仔细验证和处理**的用户/客户端输入而不是盲目地映射到实体属性

虽然其中一些问题可以通过映射解决配置（例如，AutoMapper 允许定义自定义映射规则），它使您的业务代码**隐式/隐藏**并与基础设施**紧密耦合**。我们认为业务代码应该明确、清晰且易于理解。

请参阅下面的*实体创建*部分有关示例落实本节提出的建议。

## **用例示例**

**Example Use Cases**

本节将演示一些示例用例和讨论替代方案。

### **实体创建**

从实体/聚合根类创建对象是实体生命周期的第一步。*聚合/聚合根规则和最佳实践*部分建议为 Entity 类**创建一个主构造函数**保证**创建一个有效的实体**。所以，每当我们需要创建该实体的实例，我们都应该始终**使用它的构造函数**。

请参阅下面的Issue聚合根(Aggregate Root)类：

```C#
public class Issue : AggregateRoot<Guid>
{
	public Guid RepositoryId { get;private set;}
    public string Title { get;private set;}
	public string Text { get;set;}
	public Guid? AssignedUserId { get;internal set; }
    
	public Issue(Guid id,Guid repositoryId,string title,string text = null）: base(id)
    {
        RepositoryId = repositoryId;
        Title = Check.NotNu110rWhiteSpace(title,nameof(title));
        Text = text;//Allow empty/null
    }
    private Issue()
    {
        /* Empty constructor is for ORMS */
    }
    public void SetTitle(string title)
    {
        Title = Check.NotNu110rWhiteSpace(title,nameof(title));
    }
    //...
}
```

- 此类保证通过实体构造函数创建有效实体
- 如果稍后需要更改Title，则需要使用SetTitle方法继续保持Title有效状态
- 如果要将此issue分配给用户，则需要使用IssueManager(它之前实现了一些业务规则分配 - 请参阅上面的*领域服务*部分)
- 该Text属性有一个public setter，因为它也接受空值并且没有任何验证规则。对于这个例子，它在构造函数中也是可选的。


让我们看一个用于创建issue的 Application Service 方法

```c#
public class IssueAppService : ApplicationService,IIssueAppService
{
    private readonly IssueManager _issueManager;
	private readonly IRepository<Issue, Guid> _issueRepository;
    private readonly IRepository<AppUser,Guid> _userRepository;
    public IssueAppService(IssueManager issueManager,
                           IRepository<Issue,Guid> issueRepository,
                           IRepository<AppUser,Guid>userRepository)
    {
        _issueManager = issueManager;
		_issueRepository = issueRepository;
        _userRepository = userRepository;
    }

	public async Task<IssueDto> CreateAsync(IssueCreationDto input)
    {
		//Create a valid entity
        var issue = new Issue(GuidGenerator.Create()，input.RepositoryId,input.Title,input.Text);
        
		//Apply additional domain actions
        if (input.AssignedUserId.HasValue)
        {
			var user= await __userRepository.GetAsync(input.AssignedUserId.Value);
            await _issueManager.AssignToAsync(issue,user);
        }
		// Save
		await _issueRepository.InsertAsync(issue);
        //Return a DT0 represents the new Issue
		return 0bjectMapper.Map<Issue,IssueDto>(issue);
    }
}
```

CreateAsync方法:

- 使用Issue**构造函数**来创建一个有效的问题。它使用[IGuidGenerator](https://docs.abp.io/en/abp/latest/Guid-Generation)服务创建Id。在这里它没有使用自动对象映射
- 如果客户端想在对象创建时**将此问题issue分配给用户user**，它使用IssueManager允许在assignment之前执行必要的检查
- **将**实体**保存**到数据库
- 最后使用IObjectMapper自动从新Issue实体创建IssueDto返回

**在实体创建中应用领域规则Applying Domain Rules on Entity Creation**

示例Issue实体在实体创建时没有业务规则，除了构造函数中的一些格式验证。但是，实体创建时可能存在一些额外的业务规则需要检查。

例如，假设如果已经存在具有**完全相同**Title的issue您**不允许**再次创建。那么问题来了，在哪里执行这个规则？在**应用服务中**实现这个规则是**不正确的**，因为它是一个应始终检查的**核心业务（领域）规则**。

这个规则应该在**领域服务中实现**，当前示例就是**IssueManager**。所以，我们需要强制应用程序层总是使用IssueManager创建一个新的Issue。

首先，我们可以将Issue构造函数设置为internal，而不是public：

```c#
public class Issue : AggregateRoot<Guid>
{
	internal Issue(Guid id,Guid repositoryId,string title,string text = null）: base(id)
    {
        RepositoryId = repositoryId;
        Title = Check.NotNu110rWhiteSpace(title,nameof(title));
        Text = text;//Allow empty/null
    }

    //...
}
```

这可以防止应用服务直接使用构造函数，因此他们将使用IssueManager。接下来我们可以将CreateAsync方法添加到IssueManager：

```c#
using System;
using System.Threading.Tasks;
using Volo.Abp;
using Volo.Abp.Domain.Repositories;
using Volo.Abp. Domain.Services;
namespace IssueTracking.Issues
{
	public class IssueManager : DomainService
    {
		private readonly IRepository<Issue,Guid> _issueRepository;
        
		public IssueManager(IRepository<Issue,Guid issueRepository)
        {
			_issueRepository = issueRepository;
        }
		public async Task<Issues> CreateAsync(Guid repositoryId,string title,string text = null)
		{
			if (await_issueRepository.AnyAsync(i => i.Title == title)
            {
				throw new BusinessException("IssueTracking:IssueWithSameTitleExists");
			}
			return new Issue(GuidGenerator.Create(),repositoryld,title,text);
		}
	}
}
```

- CreateAsync方法检查是否已经存在具有相同的标题的问题并抛出business exception
- 如果没有重复，它会创建并返回一个新的Issue

IssueAppService做出如下改变以使用IssueManager的CreateAsync方法：

```C#
public class IssueAppService : ApplicationService,IIssueAppService
{
	private readonly IssueManager _issueManager;
	private readonly IRepository<Issue,Guid> _issueRepository;
    private readonly IRepository<AppUser,Guid> _userRepository;
    
	public IssueAppService(IssueManager issueManager,
                           IRepository<Issue,Guid> issueRepository,
                           IRepository<AppUser,Guid> userRepository)
    {
        _issueManager=issueManager;
        _issueRepository = issueRepository;
        _userRepository - userRepository;
    }
    
	public async Task<IssueDto> CreateAsync(IssueCreationDto input)
    {
		//Create a valid entity using the IssueManager
        var issue = await _issueManager.CreateAsync(input.RepositoryId,input.Title,input.Text);

        //Apply additional domain actions
        if (input.AssignedUserId.HasValue)
        {
           	var user = await _userRepository.GetAsync(input.AssignedUserId.value);
            await _issueManager.AssignToAsync(issue,user);
        }
		//Save
		await _issueRepository.InsertAsync(issue);

        //Return a DTO represents the new issue
		return 0bjectMapper.Map<Issue,IssueDto>(issue);
    }
}
//IssueCreationDto class
public class IssueCreationDto
{
	public Guid RepositoryId { get; set;}
    [Required]
	public string Title { get;set;}
	public Guid? AssignedUserId { get; set;}
    public string Text { get; set;}
}
```

**讨论：为什么Issue没有在IssueManager 中保存到数据库？**

**您可能会问“为什么IssueManager没有将Issue保存到数据库？”**。我们认为这是应用服务的责任。

因为，应用服务可能需要在保存之前对Issue对象进行额外的更改/操作。如果Domain Service 保存它，那么保存操作就重复了；

- 由于数据库往返翻倍导致性能下降
- 它需要涵盖两者操作的显式数据库事务
- 如果其他操作因为业务规则取消实体创建，数据库事务应该回滚

当你仔细查看IssueAppService代码 ，将看到IssueManager.CreateAsync**不将**Issue**保存**到数据库中的优点。否则，我们需要执行一次*插入*（在IssueManager 中）和一次*更新*（在Assignment之后）。

**讨论：为什么重复标题检查不在应用服务中实现？**

我们可以简单地说“因为它是一个**核心领域逻辑**，应该在领域层中实现”。但是，它带来了一个新问题“**你是怎么决定**它是一个核心的领域逻辑，而不是应用程序逻辑？”（我们将稍后讨论更多细节的区别）。

对于这个例子，一个简单的问题可以帮助我们做出决定：“如果我们有另一种方式（用例）来创建问题，我们还应该应用同样的规则吗？这条规则应该*总是*被被应用”。你可能会想“为什么我们有第二个创建问题的方式？”。然而，在现实生活中，你有：

- 应用程序的**最终用户**可能会在您的应用程序的标准 UI上创建issues
- 您可能有第二个**后台**应用程序由您自己的员工使用，您可能想要提供一种创建issues的方法（在这种情况下可能具有不同的授权规则）
- 您可能有一个对**第三方**开放的 HTTP API**客户**，他们会创建issues
- 您可能有一个**后台服务**如果它检测到一些问题，就会创建issues。这样，它将创建一个没有任何用户交互的issues(可能没有任何标准授权检查)
- 您可能在 UI 上有一个按钮可以**转换**某事(例如，讨论)到一个issue

我们可以举出更多的例子。所有这些都应该由**不同的应用服务方法实现**（见下面的*多应用层*部分），但它们**始终**遵循规则：新issue的标题不能与任何现有问题相同！这就是为什么这个逻辑是一个**核心领域**逻辑**，应该位于领域层，**不应该**在所有这些应用服务方法中**重复。

### **更新/操作实体**

创建实体后，它会被用户更新/操作直到从系统中删除。可能有不同用例直接或间接地改变了一个实体。

在本节中，我们将讨论一个典型的更新操作更改Issue 的多个属性。

这一次，从UpdateDTO开始：

```c#
public class UpdateIssueDto
{
	[Required]
	public string Title { get; set;}
    public string Text { get;set; }
	public Guid? AssignedUserId { get; set;}
}
```

通过与IssueCreationDto进行比较，您看不到RepositoryId。因为，我们的系统不允许转移问题（考虑下 GitHub 仓储）。只有标题Title是必须的其他属性是可选的。

让我们看看IssueAppService 中的*Update*实现：

```c#
public class IssueAppService : ApplicationService，IIssueAppService
{
	private readonly IssueManager _issueManager;
	private readonly IRepository<Issue,Guid> _issueRepository;
    private readonly IRepository<AppUser，Guid> _userRepository;
	public IssueAppService(IssueManager issueManager,
                           IRepository<Issue, Guid> issueRepository,
                           IRepository<AppUser，Guid> userRepository)
    {
        _issueManager = issueManager;
    	_issueRepository = issueRepository;
        _userRepository = userRepository;
    }
    
	public async Task<IssueDto> UpdateAsync(Guid id, UpdateIssueDto input)
    {
		//Get entity from database
		var issue = await _issueRepository. GetAsync(id);
        
		//Change Title
		await _issueManager.ChangeTitleAsync(issue，input.Title);
        
        // Change Assigned User
		if (input.AssignedUserId.HasValue)
        {
			var user = await _userRepository.GetAsync(input.AssignedUserId.Value);
            await _issueManager.AssignToAsync(issue, user);
		}
		//Change Text (no business rule,all values accepted)
        issue.Text = input.Text;
		// Update entity in the database
		await issueRepository.UpdateAsync(issue);
		//Return a DTo represents the new Issue
		return 0bjectMapper.Map<Issue, IssueDto>(issue);
    }
}
```

- UpdateAsync方法把id作为单独的参数获取。它不包含在UpdateIssueDto 中。这个设计有助于 ABP 正确定义 HTTP 路由的决策当您将此服务[自动公开](https://docs.abp.io/en/abp/latest/API/Auto-API-Controllers)为 HTTP API 端点时。所以，这与 DDD 无关。
- 它首先从数据库中**获得**了Issue实体
- 使用IssueManager的ChangeTitleAsync而不是直接调用Issue.SetTitle(...)。因为我们需要实现刚刚在*实体创建中*完成的**重复Title检查**。这需要Issue类和IssueManager类做些修改（将在下面解释）
- 使用IssueManager的AssignToAsync方法，如果此请求更改assigned  user
- 直接设置Issue.Text因为设置Text有**没有业务规则**。如果以后需要，我们可以随时重构
- **保存更改**到数据库。再次，保存改变实体协调业务对象和事务是应用服务的责任。如果IssueManager已在ChangeTitleAsync和AssignToAsync方法内部保存，那么将是双数据库操作(见以上讨论：**为什么没有在IssueManager中保存Issue到数据库？**)
- 最后使用IObjectMapper从updated Issue实体自动创建IssueDto返回

如前所述，我们需要对Issue类和IssueManager类 进行一些更改

首先，在Issue类中将 SetTitle 设为internal：

```c#
internal void SetTitle(string title)
{
	Title = Check.NotNul10rWhiteSpace(title,nameof(title);
}
```

然后向IssueManager添加一个新方法来更改Title：

```c#
public async Task ChangeTitleAsync(Issue issue，string title)
{
    if(issue.Title== title)
    {
		return;
    }
	if(await _issueRepository.AnyAsync(i =>i.Title==title))
    {
		throw new BusinessException("IssueTracking: IssueWithSameTitleExists");
    }
	issue.SetTitle(title);
}
```

## **领域逻辑与应用逻辑**

如前所述，领域驱动中的*业务逻辑*设计分为两部分（层）：*领域逻辑*和*应用逻辑*：

![image-20210707141321950](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210707141321950.png)

**领域逻辑由系统的*核心领域规则*组成而 Application Logic 实现了特定于应用程序的*用户用例*。**

虽然定义很明确，但实现起来可能并不简单。您可能不确定哪些代码应该放在应用层，哪些代码应该在领域层。本节试图解释这些差异。

### **多个应用层**

当您的系统很大时，DDD 有助于**处理复杂性**。特别是，如果有**多个应用程序**正在在**单个领域中**开发，那么**领域逻辑 vs 应用逻辑分离**变得更加重要。

假设您正在构建一个具有多个应用程序的系统:

- 一个**公共网站应用程序(Public Web Site Application)**，用ASP.NET Core MVC构建，向用户展示您的产品。这样的网站无需身份验证即可查看产品。只有当他们需要执行一些操作（例如将产品添加到购物车）才需要用户登录
- 一个**后台管理程序(Back Office Application)**，使用Angular UI构建(后台使用 REST API)。公司工作人员使用的此应用程序管理系统（如编辑产品说明）
- 一个**移动应用(Mobile Application)**与公共网站相比具有更简单的用户界面。它可以通过 REST API 或其他技术（如 TCP sockets）与后台服务交互

![image-20210707142411850](C:\Users\160588\AppData\Roaming\Typora\typora-user-images\image-20210707142411850.png)



每个应用程序都会有不同的**要求**，不同的用户用例**（应用服务方法），不同的**DTO，不同的**验证**和**授权**规则...等

将所有这些逻辑混合到一个应用服务层中，服务包含太多if条件**复杂**的业务逻辑**使您的代码更难开发、维护并测试**并导致潜在的错误。

如果您在一个领域中有多个应用程序:

- 为每个应用程序/客户端类型创建**单独的应用层**并在这些独立层中实现应用程序特定的业务逻辑
- 使用**单个领域层**来共享核心领域逻辑

这样的设计使得区分领域逻辑和应用逻辑变得更加重要

为了更清楚地区分实现，您可以为每个应用程序类型创建的不同项目 (. csproj )

例如：

- 后台（管理）应用程序=>IssueTracker.Admin.Application & IssueTracker.Admin.Application.Contracts
- 公共网站应用程序=>IssueTracker.Public.Application &IssueTracker.Public.Application.Contracts
- 移动应用=>IssueTracker.Mobile.Application &IssueTracker.Mobile.Application.Contracts



### **示例**

本节包含一些应用服务和领域服务实例讨论如何决定放置这些服务内部的业务逻辑。

**示例：在领域服务中创建新组织Creating a new Organization in a Domain Service**

```C#
public class OrganizationManager : DomainService
{
	private readonly IRepository<Organization> _organizationRepository;
    private readonly ICurrentUser _currentUser;
	private readonly IAuthorizationService _authorizationService;
    private readonly IEmailSender _emailSender;
	public OrganizationManager(IRepository<Organization> organizationRepository,
                               ICurrentUser currentUser,
							   IAuthorizationService authorizationService,
                               IEmailSender emailSender)
    {
        _organizationRepository = organizationRepository;
        _currentUser = currentUser;
		_authorizationService= authorizationService;
        _emailSender = emailSender;
    }
	public async Task<Organization> CreateAsync(string name)
    {
        if(await _organizationRepository.AnyAsync(x => x.Name==name))
        {
            throw new BusinessException("IssueTracking: DuplicateOrganizationName");
        }

        await _authorizationService.CheckAsync("OrganizationCreationPermission");
        
        Logger.LogDebug($"Creating organization {name} by {_currentUser.UserName}");
        
        var organization = new Organization();
        
		await _emailSender.SendAsync("systemadmineissuetracking.com","New Organization",
                                     "A new organization created with name: " +name);
		return organization;
    }
}
```

让我们一步一步看CreateAsync方法来讨论哪些代码应该或不应该在领域服务中

- **正确CORRECT：**它首先检查**组织名称是否重复**在这种情况下，**name**重复会抛出异常。这是与核心领域规则相关的东西，我们从不允许重名
- **错误WRONG：**领域服务不应该检查**授权**。[授权](https://docs.abp.io/en/abp/latest/Authorization)应该在应用层
- **错误WRONG：**它记录一条消息，包括[当前用户](https://docs.abp.io/en/abp/latest/CurrentUser)的用户名。领域服务不应依赖在当前用户上。领域服务应该可用即使系统中没有用户。当前用户(Session) 应该是一个表示/应用(Presentation/Application)层相关概念
- **错误WRONG：**它发送了一封关于这个新组织创建的[电子邮件](https://docs.abp.io/en/abp/latest/Emailing)。我们认为这也是一个特定于用例的业务逻辑。您可能希望在不同的用例下发送不同的电子邮件或在某些情况下不需要发送电子邮件

**示例：在应用服务中创建新组织Creating a new Organization in an Application Service**

```c#
public class OrganizationAppService : ApplicationService
{
	private readonly OrganizationManager _organizationManager;
    private readonly IPaymentService _paymentService;
	private readonly IEmailSender _emailSender;
	public OrganizationAppService(OrganizationManager organizationManager,
                                  IPaymentService paymentService,
                                  IEmailSender emailSender)
    {
		_organizationManager = organizationManager;
        _paymentService = paymentService;
        _emailSender = emailSender;
    }
    [UnitOfWork]
	[Authorize("OrganizationCreationPermission")]
	public async Task<Organization> CreateAsync(CreateOrganizationDto input)
    {
		await paymentService.ChargeAsync(CurrentUser.Id,GetOrganizationPrice());
		
        var organization = await_organizationManager.CreateAsync(input.Name);
        
		awit _organizationManager.InsertAsync(organization);
        
		await emailsender.SendAsync("systemadmin@issuetracking.com",
                                    "New Organization",
                                    "A new organization created with name: " + input.Name);
        
		return organization;
    }

    private double GetOrganizationPrice()  
    {
        return 42;//Gets from somewhere else... 
    }
}


```

让我们一步一步看CreateAsync方法来讨论哪些代码应该或不应该在应用服务中

- **正确CORRECT：**应用程序服务方法应该是原子工作unit of work (transactional)。ABP[工作单元](https://docs.abp.io/en/abp/latest/Unit-Of-Work)系统自动实现（甚至不需要给应用服务添加[UnitOfWork]属性)
- **正确CORRECT：**[授权](https://docs.abp.io/en/abp/latest/Authorization)应该在应用层。在这里它使用[Authorize]]属性
- **正确CORRECT：**Payment（一项基础设施服务）被调用到为此操作收费（创建一个组织是我们业务中的一项付费服务）
- **正确CORRECT：**应用程序服务方法负责保存对数据库的更改
- **正确CORRECT：**我们可以发送[电子邮件](https://docs.abp.io/en/abp/latest/Emailing)通知系统管理员
- **错误WRONG：**不要从应用服务返回实体，而是返回 DTO

**讨论：我们为什么不把支付逻辑移到领域服务？**

您可能想知道为什么付款代码不在OrganizationManager。这是一件**重要的事情**，我们从不想**错过付款。**

但是，**重要并不足以**将代码作为核心业务逻辑。我们可能还有**其他用例**，其中我们创建一个新组织不收取任何费用。

例如:

- 管理员用户可以使用后台应用程序来创建新组织无需支付任何费用
- 一个后台工作数据导入/集成/同步系统可能需要创建组织没有任何支付操作

如您所见，**支付不是创建一个有效的组织的必要操作。**它是一个特定于用例的应用程序逻辑。

**示例：CRUD 操作**

```C#
public class IssueAppService
{
	private readonly IssueManager _issueManager;
	public IssueAppService(IssueManager issueManager)
    {
		_issueManager=issueManager;
    }
    
	public async Task<IssueDto> GetAsync(Guid id)
    {
		return await _issueManager.GetAsync(id);
	}
    
	public async Task CreateAsync(IssueCreationDto input)
    {
		await _issueManager.CreateAsync(input);
	}
    
	public async Task UpdateAsync(UpdateIssueDto input)
    {
		await _issueManager.UpdateAsync(input);
	}
    
	public async Task DeleteAsync(Guid id)
    {
		await _issueManager.DeleteAsync(id);
    }
}
```

此应用程序服务本身什么都不做，而是委托所有工作交给*领域服务*。它甚至将 DTO 传递给IssueManager

- **不要为没有任何域逻辑的简单CRUD操作创建领域服务方法**
- **永远不要将DTO传递给领域服务或从领域服务返回DTO**

应用服务可以直接与存储库查询、创建、更新或删除数据一起工作，除非在这些操作期间应该执行一些领域逻辑。在这种情况下，创建领域服务方法，**除非真的有必要否则不要创建领域方法**。

| 不要认为将来可能需要它们就创建这样的 CRUD 领域服务方法（[雅格尼](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)）！当你需要重构现有代码时由于应用层优雅地抽象了领域层，重构过程不会影响UI层和其他客户端。 |
| ------------------------------------------------------------ |



## **相关书籍**

如果您对领域驱动设计和构建大型企业系统有兴趣，以下书籍推荐作为参考书:

- Eric Evans 的“*领域驱动设计Domain Driven Design*”
- Vaughn Vernon的“*实现领域驱动设计Implementing Domain Driven Design*“
- Robert C. Martin的“*Clean Architecture*”

## 番外:ABP引用

### [模块化](https://docs.abp.io/zh-Hans/abp/latest/PlugIn-Modules)

### 领域驱动设计

### 多租户

### 微服务架构
