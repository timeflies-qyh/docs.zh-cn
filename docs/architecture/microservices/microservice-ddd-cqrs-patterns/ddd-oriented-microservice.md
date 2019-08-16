---
title: 设计面向 DDD 的微服务
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解面向 DDD 的订购微服务及其应用层的设计。
ms.date: 10/08/2018
ms.openlocfilehash: 303f8909d12dddef93b20604a00b9ea8e8493ee5
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68674344"
---
# <a name="design-a-ddd-oriented-microservice"></a><span data-ttu-id="25bc9-103">设计面向 DDD 的微服务</span><span class="sxs-lookup"><span data-stu-id="25bc9-103">Design a DDD-oriented microservice</span></span>

<span data-ttu-id="25bc9-104">域驱动设计 (DDD) 提倡基于与用例相关的真实业务来构建模型。</span><span class="sxs-lookup"><span data-stu-id="25bc9-104">Domain-driven design (DDD) advocates modeling based on the reality of business as relevant to your use cases.</span></span> <span data-ttu-id="25bc9-105">在构建应用程序的上下文中，DDD 用域来描述问题。</span><span class="sxs-lookup"><span data-stu-id="25bc9-105">In the context of building applications, DDD talks about problems as domains.</span></span> <span data-ttu-id="25bc9-106">它将独立的问题区域描述为界定的上下文（每个界定的上下文关联一个微服务），并强调使用一种通用的语言来讨论这些问题。</span><span class="sxs-lookup"><span data-stu-id="25bc9-106">It describes independent problem areas as Bounded Contexts (each Bounded Context correlates to a microservice), and emphasizes a common language to talk about these problems.</span></span> <span data-ttu-id="25bc9-107">它还提出许多技术概念和模式，如具有充血模型的域实体（无[贫血模型](https://martinfowler.com/bliki/AnemicDomainModel.html)）、值对象、聚合和聚合根（或根实体）规则，用于支持内部实现。</span><span class="sxs-lookup"><span data-stu-id="25bc9-107">It also suggests many technical concepts and patterns, like domain entities with rich models (no [anemic-domain model](https://martinfowler.com/bliki/AnemicDomainModel.html)), value objects, aggregates and aggregate root (or root entity) rules to support the internal implementation.</span></span> <span data-ttu-id="25bc9-108">本部分介绍这些内部模式的设计和实现。</span><span class="sxs-lookup"><span data-stu-id="25bc9-108">This section introduces the design and implementation of those internal patterns.</span></span>

<span data-ttu-id="25bc9-109">有时这些 DDD 技术规则和模式被认为是障碍，因为会导致实施 DDD 方法时的学习曲线较陡峭。</span><span class="sxs-lookup"><span data-stu-id="25bc9-109">Sometimes these DDD technical rules and patterns are perceived as obstacles that have a steep learning curve for implementing DDD approaches.</span></span> <span data-ttu-id="25bc9-110">但重要的并非模式本身，而是如何根据业务问题组织代码，并使用相同的业务术语（通用语言）。</span><span class="sxs-lookup"><span data-stu-id="25bc9-110">But the important part is not the patterns themselves, but organizing the code so it is aligned to the business problems, and using the same business terms (ubiquitous language).</span></span> <span data-ttu-id="25bc9-111">此外，只有在需要实现采用重要业务规则的复杂微服务时，才应使用 DDD 方法。</span><span class="sxs-lookup"><span data-stu-id="25bc9-111">In addition, DDD approaches should be applied only if you are implementing complex microservices with significant business rules.</span></span> <span data-ttu-id="25bc9-112">较为简单的功能，如 CRUD 服务，可使用较为简单的方法来管理。</span><span class="sxs-lookup"><span data-stu-id="25bc9-112">Simpler responsibilities, like a CRUD service, can be managed with simpler approaches.</span></span>

<span data-ttu-id="25bc9-113">设计和定义微服务时，最关键的任务是界定边界。</span><span class="sxs-lookup"><span data-stu-id="25bc9-113">Where to draw the boundaries is the key task when designing and defining a microservice.</span></span> <span data-ttu-id="25bc9-114">借助 DDD 模式，可了解域中的复杂性。</span><span class="sxs-lookup"><span data-stu-id="25bc9-114">DDD patterns help you understand the complexity in the domain.</span></span> <span data-ttu-id="25bc9-115">对于每个界定的上下文的域模型，需确定和定义为域建模时所需的实体、值对象和聚合。</span><span class="sxs-lookup"><span data-stu-id="25bc9-115">For the domain model for each Bounded Context, you identify and define the entities, value objects, and aggregates that model your domain.</span></span> <span data-ttu-id="25bc9-116">生成和优化限定在某个边界内的域模型，该边界用于定义上下文。</span><span class="sxs-lookup"><span data-stu-id="25bc9-116">You build and refine a domain model that is contained within a boundary that defines your context.</span></span> <span data-ttu-id="25bc9-117">如果是微服务的形式，这会十分明晰。</span><span class="sxs-lookup"><span data-stu-id="25bc9-117">And that is very explicit in the form of a microservice.</span></span> <span data-ttu-id="25bc9-118">这些边界内的组件最终会成为微服务，但在某些情况下，BC 或业务微服务可由多个物理服务组成。</span><span class="sxs-lookup"><span data-stu-id="25bc9-118">The components within those boundaries end up being your microservices, although in some cases a BC or business microservices can be composed of several physical services.</span></span> <span data-ttu-id="25bc9-119">DDD 与边界相关，微服务也是如此。</span><span class="sxs-lookup"><span data-stu-id="25bc9-119">DDD is about boundaries and so are microservices.</span></span>

## <a name="keep-the-microservice-context-boundaries-relatively-small"></a><span data-ttu-id="25bc9-120">保持相对较小的微服务上下文边界</span><span class="sxs-lookup"><span data-stu-id="25bc9-120">Keep the microservice context boundaries relatively small</span></span>

<span data-ttu-id="25bc9-121">确定界定的上下文之间的边界需要在两个相互冲突的目标之间进行权衡。</span><span class="sxs-lookup"><span data-stu-id="25bc9-121">Determining where to place boundaries between Bounded Contexts balances two competing goals.</span></span> <span data-ttu-id="25bc9-122">一方面，最初需要创建尽可能小的微服务，但这不应是主要的驱动因素；应在需要内聚的内容周围创建边界。</span><span class="sxs-lookup"><span data-stu-id="25bc9-122">First, you want to initially create the smallest possible microservices, although that should not be the main driver; you should create a boundary around things that need cohesion.</span></span> <span data-ttu-id="25bc9-123">另一方面，要避免微服务之间的非正式通信。</span><span class="sxs-lookup"><span data-stu-id="25bc9-123">Second, you want to avoid chatty communications between microservices.</span></span> <span data-ttu-id="25bc9-124">这些目标可能相互冲突。</span><span class="sxs-lookup"><span data-stu-id="25bc9-124">These goals can contradict one another.</span></span> <span data-ttu-id="25bc9-125">要平衡这些目标，应将系统分解为尽可能多的小型微服务，直至通信边界数量迅速增加，同时不断试图划分新的界定的上下文。</span><span class="sxs-lookup"><span data-stu-id="25bc9-125">You should balance them by decomposing the system into as many small microservices as you can until you see communication boundaries growing quickly with each additional attempt to separate a new Bounded Context.</span></span> <span data-ttu-id="25bc9-126">在单个界定的上下文中，内聚很重要。</span><span class="sxs-lookup"><span data-stu-id="25bc9-126">Cohesion is key within a single bounded context.</span></span>

<span data-ttu-id="25bc9-127">它类似于实现类时的[不适当亲密关系代码异味](https://sourcemaking.com/refactoring/smells/inappropriate-intimacy)。</span><span class="sxs-lookup"><span data-stu-id="25bc9-127">It is similar to the [Inappropriate Intimacy code smell](https://sourcemaking.com/refactoring/smells/inappropriate-intimacy) when implementing classes.</span></span> <span data-ttu-id="25bc9-128">如果两个微服务之间需要大量相互配合，它们极可能是相同的微服务。</span><span class="sxs-lookup"><span data-stu-id="25bc9-128">If two microservices need to collaborate a lot with each other, they should probably be the same microservice.</span></span>

<span data-ttu-id="25bc9-129">另一种考量方法是观察自治性。</span><span class="sxs-lookup"><span data-stu-id="25bc9-129">Another way to look at this is autonomy.</span></span> <span data-ttu-id="25bc9-130">如果一个微服务必须依赖另一个服务才能处理请求，那它就不是真正自治。</span><span class="sxs-lookup"><span data-stu-id="25bc9-130">If a microservice must rely on another service to directly service a request, it is not truly autonomous.</span></span>

## <a name="layers-in-ddd-microservices"></a><span data-ttu-id="25bc9-131">DDD 微服务中的层</span><span class="sxs-lookup"><span data-stu-id="25bc9-131">Layers in DDD microservices</span></span>

<span data-ttu-id="25bc9-132">在业务和技术层面较为复杂的大多数企业应用程序都由多个层定义。</span><span class="sxs-lookup"><span data-stu-id="25bc9-132">Most enterprise applications with significant business and technical complexity are defined by multiple layers.</span></span> <span data-ttu-id="25bc9-133">层是一种逻辑项目，与服务部署无关。</span><span class="sxs-lookup"><span data-stu-id="25bc9-133">The layers are a logical artifact, and are not related to the deployment of the service.</span></span> <span data-ttu-id="25bc9-134">它们的存在是为了帮助开发人员管理代码中的复杂性。</span><span class="sxs-lookup"><span data-stu-id="25bc9-134">They exist to help developers manage the complexity in the code.</span></span> <span data-ttu-id="25bc9-135">不同的层（如域模型层与表示层等）可能具有不同的类型，并为这些类型之间的转换提供授权。</span><span class="sxs-lookup"><span data-stu-id="25bc9-135">Different layers (like the domain model layer versus the presentation layer, etc.) might have different types, which mandates translations between those types.</span></span>

<span data-ttu-id="25bc9-136">例如，实体可从数据库进行加载。</span><span class="sxs-lookup"><span data-stu-id="25bc9-136">For example, an entity could be loaded from the database.</span></span> <span data-ttu-id="25bc9-137">相关信息的一部分或全部信息，包括来自其他实体的其他数据，可通过 REST Web API 发送到客户端 UI。</span><span class="sxs-lookup"><span data-stu-id="25bc9-137">Then part of that information, or an aggregation of information including additional data from other entities, can be sent to the client UI through a REST Web API.</span></span> <span data-ttu-id="25bc9-138">此处的重点是域实体限定在域模型层内，不应将其传播到其不属于的区域（如表示层）。</span><span class="sxs-lookup"><span data-stu-id="25bc9-138">The point here is that the domain entity is contained within the domain model layer and should not be propagated to other areas that it does not belong to, like to the presentation layer.</span></span>

<span data-ttu-id="25bc9-139">此外，需要通过聚合根（根实体）控制始终有效的实体（请参阅[设计域模型层中的验证](domain-model-layer-validations.md)部分）。</span><span class="sxs-lookup"><span data-stu-id="25bc9-139">Additionally, you need to have always-valid entities (see the [Designing validations in the domain model layer](domain-model-layer-validations.md) section) controlled by aggregate roots (root entities).</span></span> <span data-ttu-id="25bc9-140">因此，不应将实体绑定到客户端视图，因为在 UI 级别，某些数据可能仍未进行验证。</span><span class="sxs-lookup"><span data-stu-id="25bc9-140">Therefore, entities should not be bound to client views, because at the UI level some data might still not be validated.</span></span> <span data-ttu-id="25bc9-141">这正是 ViewModel（视图模式）的功能所在。</span><span class="sxs-lookup"><span data-stu-id="25bc9-141">This is what the ViewModel is for.</span></span> <span data-ttu-id="25bc9-142">ViewModel 是专为解决表示层需要而创建的数据模型。</span><span class="sxs-lookup"><span data-stu-id="25bc9-142">The ViewModel is a data model exclusively for presentation layer needs.</span></span> <span data-ttu-id="25bc9-143">域实体并不直接属于 ViewModel。</span><span class="sxs-lookup"><span data-stu-id="25bc9-143">The domain entities do not belong directly to the ViewModel.</span></span> <span data-ttu-id="25bc9-144">相反，还需要在 Viewmodel 和域实体之间进行相互转换。</span><span class="sxs-lookup"><span data-stu-id="25bc9-144">Instead, you need to translate between ViewModels and domain entities and vice versa.</span></span>

<span data-ttu-id="25bc9-145">为解决复杂性，务必要通过聚合根来控制一个域模型，以确保与该组实体（聚合）相关的所有固定协定和规则通过单个入口点或入口（聚合根）来执行。</span><span class="sxs-lookup"><span data-stu-id="25bc9-145">When tackling complexity, it is important to have a domain model controlled by aggregate roots that make sure that all the invariants and rules related to that group of entities (aggregate) are performed through a single entry-point or gate, the aggregate root.</span></span>

<span data-ttu-id="25bc9-146">图 7-5 演示如何在 eShopOnContainers 应用程序中实现分层设计。</span><span class="sxs-lookup"><span data-stu-id="25bc9-146">Figure 7-5 shows how a layered design is implemented in the eShopOnContainers application.</span></span>

![DDD 微服务（例如，订购）中的三个层。](./media/image6.png)

<span data-ttu-id="25bc9-149">**图 7-5**。</span><span class="sxs-lookup"><span data-stu-id="25bc9-149">**Figure 7-5**.</span></span> <span data-ttu-id="25bc9-150">eShopOnContainers 订单微服务中的 DDD 层</span><span class="sxs-lookup"><span data-stu-id="25bc9-150">DDD layers in the ordering microservice in eShopOnContainers</span></span>

<span data-ttu-id="25bc9-151">需要将系统设计为每个层只与某个其他层进行通信。</span><span class="sxs-lookup"><span data-stu-id="25bc9-151">You want to design the system so that each layer communicates only with certain other layers.</span></span> <span data-ttu-id="25bc9-152">为了更轻松的实现这种设计，应将层实现为不同的类库，因为这样可以清楚地确定库之间的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="25bc9-152">That may be easier to enforce if layers are implemented as different class libraries, because you can clearly identify what dependencies are set between libraries.</span></span> <span data-ttu-id="25bc9-153">例如，域模型层不应在任何其他层（域模型类应为普通旧 CLR 对象（简称 [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)）类）上设置依赖关系。</span><span class="sxs-lookup"><span data-stu-id="25bc9-153">For instance, the domain model layer should not take a dependency on any other layer (the domain model classes should be Plain Old CLR Objects, or [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object), classes).</span></span> <span data-ttu-id="25bc9-154">如图 7-6 所示，**Ordering.Domain** 层库只在 .NET Core 库或 NuGet 包上具有依赖项，而在任何其他自定义库（如数据库或持久性库）上不具有依赖项。</span><span class="sxs-lookup"><span data-stu-id="25bc9-154">As shown in Figure 7-6, the **Ordering.Domain** layer library has dependencies only on the .NET Core libraries or NuGet packages, but not on any other custom library, such as data library or persistence library.</span></span>

![显示 Ordering.Domain 依赖项仅依赖于 .NET Core 库的解决方案资源管理器视图。](./media/image7.png)

<span data-ttu-id="25bc9-156">**图7-6**。</span><span class="sxs-lookup"><span data-stu-id="25bc9-156">**Figure 7-6**.</span></span> <span data-ttu-id="25bc9-157">通过作为库实现的层，可以更好地控制各层之间的依赖关系</span><span class="sxs-lookup"><span data-stu-id="25bc9-157">Layers implemented as libraries allow better control of dependencies between layers</span></span>

### <a name="the-domain-model-layer"></a><span data-ttu-id="25bc9-158">域模型层</span><span class="sxs-lookup"><span data-stu-id="25bc9-158">The domain model layer</span></span>

<span data-ttu-id="25bc9-159">关于域模型层和应用层，Eric Evans 在其大作[域驱动设计](https://domainlanguage.com/ddd/)一书中作出了以下描述。</span><span class="sxs-lookup"><span data-stu-id="25bc9-159">Eric Evans's excellent book [Domain Driven Design](https://domainlanguage.com/ddd/) says the following about the domain model layer and the application layer.</span></span>

<span data-ttu-id="25bc9-160">**域模型层**：负责表示业务概念、有关业务状况的信息和业务规则。</span><span class="sxs-lookup"><span data-stu-id="25bc9-160">**Domain Model Layer**: Responsible for representing concepts of the business, information about the business situation, and business rules.</span></span> <span data-ttu-id="25bc9-161">反映业务状况的状态是通过这个层进行控制和利用的，但有关状态存储的具体技术细节则由基础结构负责实施。</span><span class="sxs-lookup"><span data-stu-id="25bc9-161">State that reflects the business situation is controlled and used here, even though the technical details of storing it are delegated to the infrastructure.</span></span> <span data-ttu-id="25bc9-162">这一层是业务软件的核心。</span><span class="sxs-lookup"><span data-stu-id="25bc9-162">This layer is the heart of business software.</span></span>

<span data-ttu-id="25bc9-163">域模型层是表述业务的地方。</span><span class="sxs-lookup"><span data-stu-id="25bc9-163">The domain model layer is where the business is expressed.</span></span> <span data-ttu-id="25bc9-164">在 .NET 中实现微服务域模型层时，该层被编码为类库，具有用于捕获数据和行为（方法和逻辑）的域实体。</span><span class="sxs-lookup"><span data-stu-id="25bc9-164">When you implement a microservice domain model layer in .NET, that layer is coded as a class library with the domain entities that capture data plus behavior (methods with logic).</span></span>

<span data-ttu-id="25bc9-165">为遵循[持久性忽略](https://deviq.com/persistence-ignorance/)（Persistence Ignorance）和[基础结构忽略](https://ayende.com/blog/3137/infrastructure-ignorance)（Infrastructure Ignorance）原则，此层必须完全忽略数据持久性细节。</span><span class="sxs-lookup"><span data-stu-id="25bc9-165">Following the [Persistence Ignorance](https://deviq.com/persistence-ignorance/) and the [Infrastructure Ignorance](https://ayende.com/blog/3137/infrastructure-ignorance) principles, this layer must completely ignore data persistence details.</span></span> <span data-ttu-id="25bc9-166">应由基础结构层来执行这些持久性任务。</span><span class="sxs-lookup"><span data-stu-id="25bc9-166">These persistence tasks should be performed by the infrastructure layer.</span></span> <span data-ttu-id="25bc9-167">因此，此层不应在基础结构上设置直接的依赖关系，这意味着存在一个重要规则，即域模型实体类应为普通旧 CLR 对象（[POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)）。</span><span class="sxs-lookup"><span data-stu-id="25bc9-167">Therefore, this layer should not take direct dependencies on the infrastructure, which means that an important rule is that your domain model entity classes should be [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)s.</span></span>

<span data-ttu-id="25bc9-168">域实体不应在任何数据访问基础结构框架（如 Entity Framework 或 NHibernate）上具有任何直接的依赖关系（如派生自基类）。</span><span class="sxs-lookup"><span data-stu-id="25bc9-168">Domain entities should not have any direct dependency (like deriving from a base class) on any data access infrastructure framework like Entity Framework or NHibernate.</span></span> <span data-ttu-id="25bc9-169">理想情况下，域实体不应派生自或实现任何在基础结构框架中定义的类型。</span><span class="sxs-lookup"><span data-stu-id="25bc9-169">Ideally, your domain entities should not derive from or implement any type defined in any infrastructure framework.</span></span>

<span data-ttu-id="25bc9-170">大多数新式 ORM 框架（如 Entity Framework Core）允许使用这种方法，以确保域模型类不会耦合到基础结构。</span><span class="sxs-lookup"><span data-stu-id="25bc9-170">Most modern ORM frameworks like Entity Framework Core allow this approach, so that your domain model classes are not coupled to the infrastructure.</span></span> <span data-ttu-id="25bc9-171">但是使用某些 NoSQL 数据库和框架（如 Azure Service Fabric 中的执行组件和可靠集合）时，并不总是能够获得 POCO 实体。</span><span class="sxs-lookup"><span data-stu-id="25bc9-171">However, having POCO entities is not always possible when using certain NoSQL databases and frameworks, like Actors and Reliable Collections in Azure Service Fabric.</span></span>

<span data-ttu-id="25bc9-172">即使在需要为域模型遵循持久性无感知原则的情况下，也不应忽略对持久性的关注。</span><span class="sxs-lookup"><span data-stu-id="25bc9-172">Even when it is important to follow the Persistence Ignorance principle for your Domain model, you should not ignore persistence concerns.</span></span> <span data-ttu-id="25bc9-173">仍然十分有必要了解物理数据模型和模型映射到实体对象模型的方式。</span><span class="sxs-lookup"><span data-stu-id="25bc9-173">It is still very important to understand the physical data model and how it maps to your entity object model.</span></span> <span data-ttu-id="25bc9-174">否则就不可能创建设计。</span><span class="sxs-lookup"><span data-stu-id="25bc9-174">Otherwise you can create impossible designs.</span></span>

<span data-ttu-id="25bc9-175">但这并不意味可以采用为关系数据库设计的模型，并将其直接移到 NoSQL 或面向文档的数据库。</span><span class="sxs-lookup"><span data-stu-id="25bc9-175">Also, this does not mean you can take a model designed for a relational database and directly move it to a NoSQL or document-oriented database.</span></span> <span data-ttu-id="25bc9-176">在某些实体模型中，该模型或许适用，但通常是不适用的。</span><span class="sxs-lookup"><span data-stu-id="25bc9-176">In some entity models, the model might fit, but usually it does not.</span></span> <span data-ttu-id="25bc9-177">实体模型仍须遵循某些约束，这些约束基于存储技术和 ORM 技术。</span><span class="sxs-lookup"><span data-stu-id="25bc9-177">There are still constraints that your entity model must adhere to, based both on the storage technology and ORM technology.</span></span>

### <a name="the-application-layer"></a><span data-ttu-id="25bc9-178">应用层</span><span class="sxs-lookup"><span data-stu-id="25bc9-178">The application layer</span></span>

<span data-ttu-id="25bc9-179">现在我们来了解应用层，这里可以再次引用 Eric Evans 的[域驱动设计](https://domainlanguage.com/ddd/)一书中的内容：</span><span class="sxs-lookup"><span data-stu-id="25bc9-179">Moving on to the application layer, we can again cite Eric Evans's book [Domain Driven Design](https://domainlanguage.com/ddd/):</span></span>

<span data-ttu-id="25bc9-180">**应用程序层：** 定义软件要完成的作业并指导富有表现力的域对象解决问题。</span><span class="sxs-lookup"><span data-stu-id="25bc9-180">**Application Layer:** Defines the jobs the software is supposed to do and directs the expressive domain objects to work out problems.</span></span> <span data-ttu-id="25bc9-181">这一层负责执行对业务具有意义的任务或与其他系统的应用层进行交互时需执行的任务。</span><span class="sxs-lookup"><span data-stu-id="25bc9-181">The tasks this layer is responsible for are meaningful to the business or necessary for interaction with the application layers of other systems.</span></span> <span data-ttu-id="25bc9-182">这一层很“薄”。</span><span class="sxs-lookup"><span data-stu-id="25bc9-182">This layer is kept thin.</span></span> <span data-ttu-id="25bc9-183">它不包含业务规则或知识，仅针对下一层中域对象之间的协作，协调任务和委派工作。</span><span class="sxs-lookup"><span data-stu-id="25bc9-183">It does not contain business rules or knowledge, but only coordinates tasks and delegates work to collaborations of domain objects in the next layer down.</span></span> <span data-ttu-id="25bc9-184">它不具有反映业务状况的状态，但它可以具有状态，用于反映用户或程序的任务的进度。</span><span class="sxs-lookup"><span data-stu-id="25bc9-184">It does not have state reflecting the business situation, but it can have state that reflects the progress of a task for the user or the program.</span></span>

<span data-ttu-id="25bc9-185">.NET 中微服务的应用层通常被编码为 ASP.NET Core Web API 项目。</span><span class="sxs-lookup"><span data-stu-id="25bc9-185">A microservice’s application layer in .NET is commonly coded as an ASP.NET Core Web API project.</span></span> <span data-ttu-id="25bc9-186">该项目实现微服务的交互、远程网络访问和从 UI 或客户端应用中使用的外部 Web API。</span><span class="sxs-lookup"><span data-stu-id="25bc9-186">The project implements the microservice’s interaction, remote network access, and the external Web APIs used from the UI or client apps.</span></span> <span data-ttu-id="25bc9-187">它包括查询（如果使用 CQRS 方法）、微服务接受的命令，甚至是微服务之间的事件驱动的通信（集成事件）。</span><span class="sxs-lookup"><span data-stu-id="25bc9-187">It includes queries if using a CQRS approach, commands accepted by the microservice, and even the event-driven communication between microservices (integration events).</span></span> <span data-ttu-id="25bc9-188">表示应用程序层的 ASP.NET Core Web API 不能包含业务规则或域知识（尤其是用于事务或更新的域规则）；这些规则和知识应由域模型类库所有。</span><span class="sxs-lookup"><span data-stu-id="25bc9-188">The ASP.NET Core Web API that represents the application layer must not contain business rules or domain knowledge (especially domain rules for transactions or updates); these should be owned by the domain model class library.</span></span> <span data-ttu-id="25bc9-189">应用层须只能协调任务，不能保有或定义任何域状态（域模型）。</span><span class="sxs-lookup"><span data-stu-id="25bc9-189">The application layer must only coordinate tasks and must not hold or define any domain state (domain model).</span></span> <span data-ttu-id="25bc9-190">它将业务规则的执行委托给域模型类自身（聚合根和域实体），最终在这些域实体内更新数据。</span><span class="sxs-lookup"><span data-stu-id="25bc9-190">It delegates the execution of business rules to the domain model classes themselves (aggregate roots and domain entities), which will ultimately update the data within those domain entities.</span></span>

<span data-ttu-id="25bc9-191">基本上是在应用逻辑中实现所有依赖于某个前端的用例。</span><span class="sxs-lookup"><span data-stu-id="25bc9-191">Basically, the application logic is where you implement all use cases that depend on a given front end.</span></span> <span data-ttu-id="25bc9-192">例如，与某个 Web API 服务相关的实现。</span><span class="sxs-lookup"><span data-stu-id="25bc9-192">For example, the implementation related to a Web API service.</span></span>

<span data-ttu-id="25bc9-193">目标是域模型层中的域逻辑、其固定协定、数据模型和相关业务规则，必须完全独立于表示层和应用层。</span><span class="sxs-lookup"><span data-stu-id="25bc9-193">The goal is that the domain logic in the domain model layer, its invariants, the data model, and related business rules must be completely independent from the presentation and application layers.</span></span> <span data-ttu-id="25bc9-194">最重要的是，域模型层不能直接依赖于任何基础结构框架。</span><span class="sxs-lookup"><span data-stu-id="25bc9-194">Most of all, the domain model layer must not directly depend on any infrastructure framework.</span></span>

### <a name="the-infrastructure-layer"></a><span data-ttu-id="25bc9-195">基础结构层</span><span class="sxs-lookup"><span data-stu-id="25bc9-195">The infrastructure layer</span></span>

<span data-ttu-id="25bc9-196">基础结构层是关于如何将最初存放在域实体中的数据（内存中）持久保存在数据库或另一个持久性存储区中。</span><span class="sxs-lookup"><span data-stu-id="25bc9-196">The infrastructure layer is how the data that is initially held in domain entities (in memory) is persisted in databases or another persistent store.</span></span> <span data-ttu-id="25bc9-197">一个例子是使用 Entity Framework Core 代码实现存储库模式类，该类使用 DBContext 将数据持久保存在关系数据库中。</span><span class="sxs-lookup"><span data-stu-id="25bc9-197">An example is using Entity Framework Core code to implement the Repository pattern classes that use a DBContext to persist data in a relational database.</span></span>

<span data-ttu-id="25bc9-198">为遵循之前提到的[持久性忽略](https://deviq.com/persistence-ignorance/)和[基础结构忽略](https://ayende.com/blog/3137/infrastructure-ignorance)原则，基础结构层不得“污染”域模型层。</span><span class="sxs-lookup"><span data-stu-id="25bc9-198">In accordance with the previously mentioned [Persistence Ignorance](https://deviq.com/persistence-ignorance/) and [Infrastructure Ignorance](https://ayende.com/blog/3137/infrastructure-ignorance) principles, the infrastructure layer must not “contaminate” the domain model layer.</span></span> <span data-ttu-id="25bc9-199">必须通过不在框架上设置硬依赖关系，让域模型实体类对用于保存数据的基础结构（EF 或任何其他框架）保持为不可知状态。</span><span class="sxs-lookup"><span data-stu-id="25bc9-199">You must keep the domain model entity classes agnostic from the infrastructure that you use to persist data (EF or any other framework) by not taking hard dependencies on frameworks.</span></span> <span data-ttu-id="25bc9-200">域模型层类库应只具有域代码，且仅为 [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) 实体类，用于实现软件核心，并完全脱离基础结构技术。</span><span class="sxs-lookup"><span data-stu-id="25bc9-200">Your domain model layer class library should have only your domain code, just [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) entity classes implementing the heart of your software and completely decoupled from infrastructure technologies.</span></span>

<span data-ttu-id="25bc9-201">因此，层或类库和项目应最终依赖于域模型层（库），但反之则不成立，如图 7-7 所示。</span><span class="sxs-lookup"><span data-stu-id="25bc9-201">Thus, your layers or class libraries and projects should ultimately depend on your domain model layer (library), not vice versa, as shown in Figure 7-7.</span></span>

![DDD 服务中的依赖项（应用层）依赖于域和基础结构，基础结构依赖于域，但域不依赖于任何层。](./media/image8.png)

<span data-ttu-id="25bc9-203">**图 7-7**。</span><span class="sxs-lookup"><span data-stu-id="25bc9-203">**Figure 7-7**.</span></span> <span data-ttu-id="25bc9-204">DDD 中层之间的依赖关系</span><span class="sxs-lookup"><span data-stu-id="25bc9-204">Dependencies between layers in DDD</span></span>

<span data-ttu-id="25bc9-205">这一层设计对每个微服务应是独立的。</span><span class="sxs-lookup"><span data-stu-id="25bc9-205">This layer design should be independent for each microservice.</span></span> <span data-ttu-id="25bc9-206">如之前所述，可以实现遵循 DDD 模式的最复杂的微服务，也可以使用简单的方法实现简单的数据驱动微服务（单个层中的简单 CRUD）。</span><span class="sxs-lookup"><span data-stu-id="25bc9-206">As noted earlier, you can implement the most complex microservices following DDD patterns, while implementing simpler data-driven microservices (simple CRUD in a single layer) in a simpler way.</span></span>

#### <a name="additional-resources"></a><span data-ttu-id="25bc9-207">其他资源</span><span class="sxs-lookup"><span data-stu-id="25bc9-207">Additional resources</span></span>

- <span data-ttu-id="25bc9-208">**DevIQ.Persistence Ignorance principle** \（持久性无感知原则）</span><span class="sxs-lookup"><span data-stu-id="25bc9-208">**DevIQ. Persistence Ignorance principle** \</span></span>
  <https://deviq.com/persistence-ignorance/>

- <span data-ttu-id="25bc9-209">**Oren Eini。Infrastructure Ignorance** \（基础结构无感知）</span><span class="sxs-lookup"><span data-stu-id="25bc9-209">**Oren Eini. Infrastructure Ignorance** \</span></span>
  <https://ayende.com/blog/3137/infrastructure-ignorance>

- <span data-ttu-id="25bc9-210">**Angel Lopez。Layered Architecture In Domain-Driven Design** \（域驱动设计中的分层体系结构）</span><span class="sxs-lookup"><span data-stu-id="25bc9-210">**Angel Lopez. Layered Architecture In Domain-Driven Design** \</span></span>
  <https://ajlopez.wordpress.com/2008/09/12/layered-architecture-in-domain-driven-design/>

>[!div class="step-by-step"]
><span data-ttu-id="25bc9-211">[上一页](cqrs-microservice-reads.md)
>[下一页](microservice-domain-model.md)</span><span class="sxs-lookup"><span data-stu-id="25bc9-211">[Previous](cqrs-microservice-reads.md)
[Next](microservice-domain-model.md)</span></span>