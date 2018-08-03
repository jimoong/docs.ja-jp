---
title: Ocelot を使った API ゲートウェイの実装
description: Ocelot を使用して API ゲートウェイを実装する方法と、コンテナー ベースの環境で Ocelot を使用する方法について説明します。
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 07/03/2018
ms.openlocfilehash: 7400603aa11b2a741db727c97c2e4b2a17268ac0
ms.sourcegitcommit: 59b51cd7c95c75be85bd6ef715e9ef8c85720bac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2018
ms.locfileid: "37878741"
---
# <a name="implementing-api-gateways-with-ocelot"></a><span data-ttu-id="37312-103">Ocelot を使った API ゲートウェイの実装</span><span class="sxs-lookup"><span data-stu-id="37312-103">Implementing API Gateways with Ocelot</span></span>

<span data-ttu-id="37312-104">参照マイクロサービス アプリケーション [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) では、[Ocelot ](https://github.com/ThreeMammals/Ocelot)を使用しています。これは、Ocelot がマイクロサービス/コンテナーと共に展開できるシンプルかつ軽量な API ゲートウェイで、eShopOnContainers で使用される次のような環境であれば、どこにでも展開できるからです。</span><span class="sxs-lookup"><span data-stu-id="37312-104">The reference microservice application [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) is using [Ocelot](https://github.com/ThreeMammals/Ocelot) because Ocelot is a simple and lightweight API Gateway that you can deploy anywhere along with your microservices/containers such as in any of the following environments used by eShopOnContainers.</span></span>

- <span data-ttu-id="37312-105">Docker ホスト、ローカル開発用 PC、オンプレミス、またはクラウド。</span><span class="sxs-lookup"><span data-stu-id="37312-105">Docker host, in your local dev PC, on-premises or in the cloud.</span></span>
- <span data-ttu-id="37312-106">Kubernetes クラスター、オンプレミスまたは Azure Kubernetes Service (AKS) などのマネージド クラウド。</span><span class="sxs-lookup"><span data-stu-id="37312-106">Kubernetes cluster, on-premises or in managed cloud such as Azure Kubernetes Service (AKS).</span></span>
- <span data-ttu-id="37312-107">Service Fabric クラスター、オンプレミス、またはクラウド。</span><span class="sxs-lookup"><span data-stu-id="37312-107">Service Fabric cluster, on-premises or in the cloud.</span></span>
- <span data-ttu-id="37312-108">Service Fabric メッシュ、Azure で PaaS/Serverless として。</span><span class="sxs-lookup"><span data-stu-id="37312-108">Service Fabric mesh, as PaaS/Serverless in Azure.</span></span>

## <a name="architect-and-design-your-api-gateways"></a><span data-ttu-id="37312-109">API ゲートウェイのアーキテクチャとデザイン</span><span class="sxs-lookup"><span data-stu-id="37312-109">Architect and design your API Gateways</span></span>

<span data-ttu-id="37312-110">次のアーキテクチャ図は、eShopOnContainers で Ocelot を使用した API ゲートウェイの実装方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="37312-110">The following architecture diagram shows how API Gateways are implemented with Ocelot in eShopOnContainers.</span></span>

![クライアント アプリ、マイクロサービス、およびその間の API ゲートウェイを示す eShopOnContainers アーキテクチャ図](./media/image28.png)

<span data-ttu-id="37312-112">**図 8-27**</span><span class="sxs-lookup"><span data-stu-id="37312-112">**Figure 8-27.**</span></span> <span data-ttu-id="37312-113">API ゲートウェイを使用した eShopOnContainers アーキテクチャ</span><span class="sxs-lookup"><span data-stu-id="37312-113">eShopOnContainers architecture with API Gateways</span></span>

<span data-ttu-id="37312-114">この図では、アプリケーション全体を 1 つの Docker ホスト、または "Docker for Windows" または "Docker for Mac" を搭載した開発 PC に配置する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="37312-114">That diagram shows how the whole application is deployed into a single Docker host or development PC with “Docker for Windows” or “Docker for Mac”.</span></span> <span data-ttu-id="37312-115">ただし、任意のオーケストレーターに配置することはよく似ていますが、図の任意のコンテナーがオーケストレーターでスケールアウトする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="37312-115">However, deploying into any orchestrator would be pretty similar but any container in the diagram could be scaled-out in the orchestrator.</span></span> 

<span data-ttu-id="37312-116">さらに、データベース、キャッシュ、メッセージ ブローカーなどのインフラストラクチャ資産は、オーケストレーターからオフロードして、Azure SQL Database、Azure Cosmos DB、Azure Redis、Azure Service Bus、またはオンプレミスの任意の HA クラスタリング ソリューションのような、インフラストラクチャの可用性が高いシステムに配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="37312-116">In addition, the infrastructure assets such as databases, cache, and message brokers should be offloaded from the orchestrator and deployed into high available systems for infrastructure, like Azure SQL Database, Azure Cosmos DB, Azure Redis, Azure Service Bus, or any HA clustering solution on-premises.</span></span>

<span data-ttu-id="37312-117">また、図からわかるように、複数の API ゲートウェイを持つことで、複数の開発チームがマイクロサービスと独自の関連する API ゲートウェイを開発して配置するときに、自律的に行うことができるようになります (この例では、マーケティング機能とショッピング機能)。</span><span class="sxs-lookup"><span data-stu-id="37312-117">As you can also notice in the diagram, having several API Gateways allows multiple development teams to be autonomous (in this case Marketing features vs. Shopping features) when developing and deploying their microservices plus their own related API Gateways.</span></span> 

<span data-ttu-id="37312-118">1 つのポイントが複数の開発チームによって更新されることを意味するモノリシック API ゲートウェイがあれば、アプリケーションの 1 つの部分にすべてのマイクロサービスを組み合わせることができたかもしれません。</span><span class="sxs-lookup"><span data-stu-id="37312-118">If you had a single monolithic API Gateway that would mean a single point to be updated by several development teams, which could couple all the microservices with a single part of the application.</span></span>

<span data-ttu-id="37312-119">デザインにさらに踏み込むと、きめ細かい API ゲートウェイを、選択したアーキテクチャに応じて、1 つのビジネス マイクロサービスに制限できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="37312-119">Going much further in the design, sometimes a fine-grained API Gateway can also be limited to a single business microservice depending on the chosen architecture.</span></span> <span data-ttu-id="37312-120">ビジネスまたはドメインで API ゲートウェイの境界を示すことは、より優れた設計にするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="37312-120">Having the API Gateway’s boundaries dictated by the business or domain will help you to get a better design.</span></span> 

<span data-ttu-id="37312-121">たとえば、API ゲートウェイ層を細分化することは、マイクロサービスに基づくより高度な複合 UI アプリケーションで特に役立つ場合があります。これは、きめ細かい API ゲートウェイの概念が UI コンポジション サービスと似ているためです。</span><span class="sxs-lookup"><span data-stu-id="37312-121">For instance, fine granularity in the API Gateway tier can be especially useful for more advanced composite UI applications that are based on microservices, because the concept of a fine-grained API Gateway is similar to a UI composition service.</span></span> 

<span data-ttu-id="37312-122">UI コンポジション サービスに関する詳細については、[マイクロサービスに基づく複合 UI の作成](https://docs.microsoft.com/dotnet/standard/microservices-architecture/architect-microservice-container-applications/microservice-based-composite-ui-shape-layout)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="37312-122">For more information about UI composition services, see [Creating composite UI based on microservices](https://docs.microsoft.com/dotnet/standard/microservices-architecture/architect-microservice-container-applications/microservice-based-composite-ui-shape-layout).</span></span>

<span data-ttu-id="37312-123">重要な点は、多くの中規模および大規模なアプリケーションの場合、カスタム ビルドの API ゲートウェイ製品の使用は、通常は有効なアプローチですが、API ゲートウェイで自立型マイクロサービスを作成している複数の開発チームに対して、複数の独立した構成領域を許可している場合を除き、単一のモノシリック アグリゲーターや一意の中央カスタム API ゲートウェイとしては適切なアプローチではないことです。</span><span class="sxs-lookup"><span data-stu-id="37312-123">As key takeaway, for many medium- and large-size applications, using a custom-built API Gateway product is usually a good approach, but not as a single monolithic aggregator or unique central custom API Gateway unless that API Gateway allows multiple independent configuration areas for the several development teams creating autonomous microservices.</span></span>

### <a name="sample-microservicescontainers-to-reroute-through-the-api-gateways"></a><span data-ttu-id="37312-124">API ゲートウェイ経由で経路変更するマイクロサービス/コンテナーのサンプル</span><span class="sxs-lookup"><span data-stu-id="37312-124">Sample microservices/containers to reroute through the API Gateways</span></span>

<span data-ttu-id="37312-125">例として、`eShopOnContainers` には、次の図に示すように、API ゲートウェイ経由で公開する必要がある約 6 種類の内部のマイクロサービスがあります。</span><span class="sxs-lookup"><span data-stu-id="37312-125">As an example, `eShopOnContainers` has around six internal microservice-types that have to be published through the API Gateways, as shown in the following image.</span></span>
 
![](./media/image29.png)

<span data-ttu-id="37312-126">**図 8-28**</span><span class="sxs-lookup"><span data-stu-id="37312-126">**Figure 8-28.**</span></span> <span data-ttu-id="37312-127">Visual Studio の eShopOnContainers ソリューション内のマイクロサービス フォルダー</span><span class="sxs-lookup"><span data-stu-id="37312-127">Microservice folders in eShopOnContainers solution in Visual Studio</span></span>

<span data-ttu-id="37312-128">ID サービスについては、システム内の唯一の横断的関心事のため、デザインでは API ゲートウェイのルーティングから除外されていますが、Ocelot を使用すると、経路変更リストの一部として含めることができます。</span><span class="sxs-lookup"><span data-stu-id="37312-128">About the Identity service, in the design it is left out of the API Gateway routing because it is the only cross-cutting concern in the system, but with Ocelot it is also possible to include it as part of the rerouting lists.</span></span>

<span data-ttu-id="37312-129">コードからわかるように、これらのサービスはすべて、ASP.NET Core Web API サービスとして現在実装されています。</span><span class="sxs-lookup"><span data-stu-id="37312-129">All those services are currently implemented as ASP.NET Core Web API services, as you can tell because of the code.</span></span> <span data-ttu-id="37312-130">Catalog マイクロサービス コードなどのマイクロサービスのいずれかに注目してみましょう。</span><span class="sxs-lookup"><span data-stu-id="37312-130">Let’s focus on one of the microservices like the Catalog microservice code.</span></span>

![](./media/image30.png)

<span data-ttu-id="37312-131">**図 8-29**</span><span class="sxs-lookup"><span data-stu-id="37312-131">**Figure 8-29.**</span></span> <span data-ttu-id="37312-132">サンプル Web API マイクロサービス (Catalog マイクロサービス)</span><span class="sxs-lookup"><span data-stu-id="37312-132">Sample Web API microservice (Catalog microservice)</span></span>

<span data-ttu-id="37312-133">Catalog マイクロサービスは、複数のコントローラーと次のコードにあるようなメソッドを持つ一般的な ASP.NET Core Web API プロジェクトであることがわかります。</span><span class="sxs-lookup"><span data-stu-id="37312-133">You can see that the Catalog microservice is a typical ASP.NET Core Web API project with several controllers and methods like in the following code.</span></span>

```csharp
[HttpGet]
[Route("items/{id:int}")]
[ProducesResponseType((int)HttpStatusCode.NotFound)]
[ProducesResponseType(typeof(CatalogItem),(int)HttpStatusCode.OK)]
public async Task<IActionResult> GetItemById(int id)
{
    if (id <= 0)
    {
        return BadRequest();
    }
    var item = await _catalogContext.CatalogItems.
                                          SingleOrDefaultAsync(ci => ci.Id == id);
    //…

    if (item != null)
    {
        return Ok(item);
    }
    return NotFound();
}
```
<span data-ttu-id="37312-134">HTTP 要求は、マイクロサービス データベースと追加のアクションにアクセスするような C# コードの実行で終了します。</span><span class="sxs-lookup"><span data-stu-id="37312-134">The HTTP request will end up running that kind of C# code accessing the microservice database plus any additional action.</span></span>

<span data-ttu-id="37312-135">マイクロサービス URL については、コンテナーがローカル開発用 PC (ローカル Docker ホスト) に配置されると、各マイクロサービスのコンテナーは、次の dockerfile にあるように、その dockerfile で指定された内部ポート (通常はポート 80) を常に持ちます。</span><span class="sxs-lookup"><span data-stu-id="37312-135">In regards to the microservice URL, when the containers are deployed in your local development PC (local Docker host), each microservice’s container has always an internal port, usually port 80, specified in its dockerfile, as in the following dockerfile:</span></span>

```
FROM microsoft/aspnetcore:2.0.5 AS base
WORKDIR /app
EXPOSE 80
```

<span data-ttu-id="37312-136">コードに示されているポート 80 は、Docker ホスト内の内部ポートであるため、クライアント アプリが到達することはできません。</span><span class="sxs-lookup"><span data-stu-id="37312-136">The port 80 shown in the code is internal within the Docker host, so it cannot be reached by the client apps.</span></span> <span data-ttu-id="37312-137">クライアント アプリがアクセスできるのは、`docker-compose` を使用して配置したときに公開された外部ポート (ある場合) だけです。</span><span class="sxs-lookup"><span data-stu-id="37312-137">The client apps can access only to the external ports (if any) published when deploying with `docker-compose`.</span></span>

<span data-ttu-id="37312-138">運用環境に配置するときに、これらの外部ポートを公開しないでください。</span><span class="sxs-lookup"><span data-stu-id="37312-138">Those external ports shouldn't be published when deploying into a production environment.</span></span> <span data-ttu-id="37312-139">クライアント アプリとマイクロサービス間の直接通信を回避するために、API ゲートウェイを使用する理由がここにあります。</span><span class="sxs-lookup"><span data-stu-id="37312-139">This is precisely why you want to use the API Gateway, to avoid the direct communication between the client apps and the microservices.</span></span>

<span data-ttu-id="37312-140">ただし、開発時には、マイクロサービス/コンテナーに直接アクセスして、Swagger を経由して実行できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="37312-140">However, when developing, you want to access the microservice/container directly and run it through Swagger.</span></span> <span data-ttu-id="37312-141">そのため、eShopOnContainers では、外部ポートが API ゲートウェイまたはクライアント アプリで使われなくなっても、指定されたままにしています。</span><span class="sxs-lookup"><span data-stu-id="37312-141">That’s why in eShopOnContainers, the external ports are still specified even when they won’t be used by the API Gateway or the client apps.</span></span>

<span data-ttu-id="37312-142">Catalog マイクロサービスの `docker-compose.override.yml` ファイルの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="37312-142">Here’s an example of the `docker-compose.override.yml` file for the Catalog microservice:</span></span>

```
catalog.api:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - ASPNETCORE_URLS=http://0.0.0.0:80
    - ConnectionString=YOUR_VALUE
    - ... Other Environment Variables
  ports:
    - "5101:80"   # Important: In a production environment you should remove the external port (5101) kept here for microservice debugging purposes. 
                  # The API Gateway redirects and access through the internal port (80).
```

<span data-ttu-id="37312-143">docker-compose.override.yml の構成では、Catalog コンテナーの内部ポートがポート 80 になっていますが、外部アクセスのポートは 5101 になっていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="37312-143">You can see how in the docker-compose.override.yml configuration the internal port for the Catalog container is port 80, but the port for external access is 5101.</span></span> <span data-ttu-id="37312-144">ただし、このポートは、API ゲートウェイを使用する場合には、アプリケーションでは使用せずに、Catalog マイクロサービスのデバッグ、実行、およびテストのためにだけ使用します。</span><span class="sxs-lookup"><span data-stu-id="37312-144">But this port shouldn’t be used by the application when using an API Gateway, only to debug, run and test just the Catalog microservice.</span></span>

<span data-ttu-id="37312-145">マイクロサービスの適切な運用配置環境は、Kubernetes や Service Fabric などのオーケストレーターであるため、通常は docker-compose を使用して運用環境に配置することはありません。</span><span class="sxs-lookup"><span data-stu-id="37312-145">Normally, you won’t be deploying with docker-compose into a production environment because the right production deployment environment for microservices is an orchestrator like Kubernetes or Service Fabric.</span></span> <span data-ttu-id="37312-146">これらの環境に配置する場合は、マイクロサービスに対してどの外部ポートも直接公開しない別の構成ファイルを使用しますが、API ゲートウェイからのリバース プロキシは常に使用します。</span><span class="sxs-lookup"><span data-stu-id="37312-146">When deploying to those environments you use different configuration files where you won’t publish directly any external port for the microservices but, you'll always use the reverse proxy from the API Gateway.</span></span>

<span data-ttu-id="37312-147">ローカルの Docker ホストで Catalog マイクロサービスを実行します。これには、Visual Studio から完全な eShopOnContainers ソリューションを実行する (docker-compose ファイル内のすべてのサービスを実行する) か、`docker-compose.yml` と docker-compose.override.yml が配置されているフォルダーにある CMD または PowerShell で次の docker-compose コマンドを使用して、単に Catalog マイクロサービスを起動します。</span><span class="sxs-lookup"><span data-stu-id="37312-147">Run the catalog microservice in your local Docker host either by running the full eShopOnContainers solution from Visual Studio (it’ll run all the services in the docker-compose files) or just starting the Catalog microservice with the following docker-compose command in CMD or PowerShell positioned at the folder where the `docker-compose.yml` and docker-compose.override.yml are placed.</span></span>

```
docker-compose run --service-ports catalog.api
```

<span data-ttu-id="37312-148">このコマンドは、catalog.api サービス コンテナーと docker-compose.yml で指定されている依存関係のみを実行します。</span><span class="sxs-lookup"><span data-stu-id="37312-148">This command only runs the catalog.api service container plus dependencies that are specified in the docker-compose.yml.</span></span> <span data-ttu-id="37312-149">この例では、SQL Server コンテナーと RabbitMQ コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="37312-149">In this case, the SQL Server container and RabbitMQ container.</span></span>

<span data-ttu-id="37312-150">こうすると、"外部" ポート (この例では `http://localhost:5101`) を経由して直接アクセスする Swagger UI を使用して、Catalog マイクロサービスに直接アクセスして、そのメソッドを表示できるようになります。</span><span class="sxs-lookup"><span data-stu-id="37312-150">Then, you can directly access the Catalog microservice and see its methods through the Swagger UI accessing directly through that “external” port, in this case `http://localhost:5101`.</span></span> 

![](./media/image31.png)

<span data-ttu-id="37312-151">**図 8-30**</span><span class="sxs-lookup"><span data-stu-id="37312-151">**Figure 8-30.**</span></span> <span data-ttu-id="37312-152">Swagger UI を使用した Catalog マイクロサービスのテスト</span><span class="sxs-lookup"><span data-stu-id="37312-152">Testing the Catalog microservice with its Swagger UI</span></span>

<span data-ttu-id="37312-153">この時点で、Visual Studio で C# コードにブレークポイントを設定し、Swagger UI で公開されるメソッドを使用してマイクロサービスをテストし、最後に `docker-compose down` コマンドを使用してすべてをクリーンアップすることは可能です。</span><span class="sxs-lookup"><span data-stu-id="37312-153">At this point, you could set a breakpoint in C# code in Visual Studio, test the microservice with the methods exposed in Swagger UI, and finally clean-up everything with the `docker-compose down` command.</span></span>

<span data-ttu-id="37312-154">しかし、マイクロサービスへのこの直接アクセス通信 (この例では外部ポート 5101 を経由) こそが、アプリケーションで回避したいことなのです。</span><span class="sxs-lookup"><span data-stu-id="37312-154">However, this direct-access communication to the microservice, in this case through the external port 5101, is precisely what you want to avoid in your application.</span></span> <span data-ttu-id="37312-155">また、API ゲートウェイ (この例では Ocelot ) の間接参照の追加レベルを設定することで、これを回避できます。</span><span class="sxs-lookup"><span data-stu-id="37312-155">And you can avoid that by setting the additional level of indirection of the API Gateway (Ocelot, in this case).</span></span> <span data-ttu-id="37312-156">これにより、クライアント アプリがマイクロサービスに直接アクセスすることはなくなります。</span><span class="sxs-lookup"><span data-stu-id="37312-156">That way, the client app won’t directly access the microservice.</span></span>

## <a name="implementing-your-api-gateways-with-ocelot"></a><span data-ttu-id="37312-157">Ocelot を使った API ゲートウェイの実装</span><span class="sxs-lookup"><span data-stu-id="37312-157">Implementing your API Gateways with Ocelot</span></span>

<span data-ttu-id="37312-158">Ocelot は、基本的に特定の順序で適用できるミドルウェアのセットです。</span><span class="sxs-lookup"><span data-stu-id="37312-158">Ocelot is basically a set of middlewares that you can apply in a specific order.</span></span>

<span data-ttu-id="37312-159">Ocelot は、ASP.NET Core でのみ動作するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="37312-159">Ocelot is designed to work with ASP.NET Core only.</span></span> <span data-ttu-id="37312-160">これは netstandard 2.0 をターゲットにしているため、.NET Standard 2.0 がサポートされていれば、どこでも使用できます。これには、.NET Core 2.0 ランタイムと .NET Framework 4.6.1 ランタイム以降が含まれます。</span><span class="sxs-lookup"><span data-stu-id="37312-160">It targets netstandard2.0 so it can be used anywhere .NET Standard 2.0 is supported, including .NET Core 2.0 runtime and .NET Framework 4.6.1 runtime and up.</span></span>

<span data-ttu-id="37312-161">[Ocelot の NuGet パッケージ](https://www.nuget.org/packages/Ocelot/)を使用して、Visual Studio から ASP.NET Core プロジェクトに Ocelot とその依存関係をインストールします。</span><span class="sxs-lookup"><span data-stu-id="37312-161">You install Ocelot and its dependencies in your ASP.NET Core project with [Ocelot's NuGet package](https://www.nuget.org/packages/Ocelot/), from Visual Studio.</span></span>

```
Install-Package Ocelot
```

<span data-ttu-id="37312-162">eShopOnContainers では、その API ゲートウェイの実装はシンプルな ASP.NET Core WebHost プロジェクトで、次の図に示すように、Ocelot のミドルウェアがすべての API ゲートウェイ機能を処理します。</span><span class="sxs-lookup"><span data-stu-id="37312-162">In eShopOnContainers, its API Gateway implementation is a simple ASP.NET Core WebHost project, and Ocelot’s middlewares handle all the API Gateway features, as shown in the following image:</span></span>

![](./media/image32.png)

<span data-ttu-id="37312-163">**図 8-31**</span><span class="sxs-lookup"><span data-stu-id="37312-163">**Figure 8-31.**</span></span> <span data-ttu-id="37312-164">eShopOnContainers の OcelotApiGw 基本プロジェクト</span><span class="sxs-lookup"><span data-stu-id="37312-164">The OcelotApiGw base project in eShopOnContainers</span></span>

<span data-ttu-id="37312-165">この ASP.NET Core WebHost プロジェクトは、`Program.cs` と `Startup.cs` の 2 つのシンプルなファイルで構成されています。</span><span class="sxs-lookup"><span data-stu-id="37312-165">This ASP.NET Core WebHost project is made by two simple files, the `Program.cs` and `Startup.cs`.</span></span>

<span data-ttu-id="37312-166">Program.cs では、一般的な ASP.NET Core BuildWebHost を作成して構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="37312-166">The Program.cs just needs to create and configure the typical ASP.NET Core BuildWebHost.</span></span> 

```csharp
namespace OcelotApiGw
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args)
        {
            var builder = WebHost.CreateDefaultBuilder(args);

            builder.ConfigureServices(s => s.AddSingleton(builder))                
                                                          .ConfigureAppConfiguration(
                              ic => ic.AddJsonFile(Path.Combine("configuration",
                                                                "configuration.json")))
                                                                .UseStartup<Startup>();
            var host = builder.Build();
            return host;
        }
    }
}
```

<span data-ttu-id="37312-167">ここで Ocelot について重要な点は、`AddJsonFile()` メソッドを通じてビルダーに提供する必要がある `configuration.json` ファイルです。</span><span class="sxs-lookup"><span data-stu-id="37312-167">The important point here for Ocelot is the `configuration.json` file that you must provide to the builder through the `AddJsonFile()` method.</span></span> <span data-ttu-id="37312-168">その `configuration.json` で、すべての API ゲートウェイの ReRoute を指定します。つまり、特定のポートを持つ外部エンドポイントと、通常は別のポートを使用する相関内部エンドポイントを指定します。</span><span class="sxs-lookup"><span data-stu-id="37312-168">That `configuration.json` is where you specify all the API Gateway ReRoutes, meaning the external endpoints with specific ports and the correlated internal endpoints, usually using different ports.</span></span>

```
{
    "ReRoutes": [],
    "GlobalConfiguration": {}
}
```

<span data-ttu-id="37312-169">構成には 2 つのセクションがあります。</span><span class="sxs-lookup"><span data-stu-id="37312-169">There are two sections to the configuration.</span></span> <span data-ttu-id="37312-170">ReRoute の配列と GlobalConfiguration です。</span><span class="sxs-lookup"><span data-stu-id="37312-170">An array of Re-Routes and a GlobalConfiguration.</span></span> <span data-ttu-id="37312-171">ReRoute は、Ocelot にアップストリーム要求の処理方法を指示するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="37312-171">The Re-Routes are the objects that tell Ocelot how to treat an upstream request.</span></span> <span data-ttu-id="37312-172">グローバル構成を使用すると、ReRoute 固有の設定をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="37312-172">The Global configuration allows overrides of Re-Route specific settings.</span></span> <span data-ttu-id="37312-173">多くの ReRoute 固有の設定を管理したくない場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="37312-173">It’s useful if you don’t want to manage lots of Re-Route specific settings.</span></span>

<span data-ttu-id="37312-174">eShopOnContainers のうちの 1 つの API ゲートウェイから、[ReRoute 構成](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/ApiGateways/Web.Bff.Shopping/apigw/configuration.json)ファイルの簡素化した例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="37312-174">Here’s a simplified example of [ReRoute configuration](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/ApiGateways/Web.Bff.Shopping/apigw/configuration.json) file from one of the API Gateways from eShopOnContainers.</span></span>

```
{
  "ReRoutes": [
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "catalog.api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/c/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ]
    },
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket.api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }
    
  ],
    "GlobalConfiguration": {
      "RequestIdKey": "OcRequestId",
      "AdministrationPath": "/administration"
    }
  }
```

<span data-ttu-id="37312-175">Ocelot API ゲートウェイの主な機能は、HTTP 要求を受け取り、それらを (現在は別の HTTP 要求として) ダウンストリーム サービスに転送することです。</span><span class="sxs-lookup"><span data-stu-id="37312-175">The main functionality of an Ocelot API Gateway is to take incoming HTTP requests and forward them on to a downstream service, currently as another HTTP request.</span></span> <span data-ttu-id="37312-176">Ocelot では、ある要求を別の場所にルーティングすることを ReRoute と説明しています。</span><span class="sxs-lookup"><span data-stu-id="37312-176">Ocelot’s describes the routing of one request to another as a Re-Route.</span></span>

<span data-ttu-id="37312-177">例として、Basket マイクロサービスの構成である、上記の configuration.json の ReRoute の 1 つに注目してみましょう。</span><span class="sxs-lookup"><span data-stu-id="37312-177">For instance, let’s focus on one of the Re-Routes in the configuration.json from above, the configuration for the Basket microservice.</span></span>

```
{
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket.api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
}
```

<span data-ttu-id="37312-178">DownstreamPathTemplate、Scheme、および DownstreamHostAndPorts が、この要求の転送先となる内部マイクロサービスの URL を構成しています。</span><span class="sxs-lookup"><span data-stu-id="37312-178">The DownstreamPathTemplate, Scheme, and DownstreamHostAndPorts make the internal microservice URL that this request will be forwarded to.</span></span> 

<span data-ttu-id="37312-179">ポートは、サービスで使用される内部ポートです。</span><span class="sxs-lookup"><span data-stu-id="37312-179">The port is the internal port used by the service.</span></span> <span data-ttu-id="37312-180">コンテナーを使用している場合、ポートはその dockerfile で指定されます。</span><span class="sxs-lookup"><span data-stu-id="37312-180">When using containers, the port specified at its dockerfile.</span></span>

<span data-ttu-id="37312-181">`Host` は、使用するサービスの名前解決に依存するサービス名です。</span><span class="sxs-lookup"><span data-stu-id="37312-181">The `Host` is a service name that depends on the service name resolution you are using.</span></span> <span data-ttu-id="37312-182">docker-compose を使用している場合、サービス名は、docker-compose ファイルで提供されるサービス名を使用する、Docker ホストにより提供されます。</span><span class="sxs-lookup"><span data-stu-id="37312-182">When using docker-compose, the services names are provided by the Docker Host, which is using the service names provided in the docker-compose files.</span></span> <span data-ttu-id="37312-183">Kubernetes や Service Fabric などのオーケストレーターを使用している場合、その名前は、DNS または各オーケストレーターで提供される名前解決で解決される必要があります。</span><span class="sxs-lookup"><span data-stu-id="37312-183">If using an orchestrator like Kubernetes or Service Fabric, that name should be resolved by the DNS or name resolution provided by each orchestrator.</span></span>

<span data-ttu-id="37312-184">DownstreamHostAndPorts は、要求の転送先となるすべてのダウンストリーム サービスのホストとポートを含む配列です。</span><span class="sxs-lookup"><span data-stu-id="37312-184">DownstreamHostAndPorts is an array that contains the host and port of any downstream services that you wish to forward requests to.</span></span> <span data-ttu-id="37312-185">通常、これには 1 つのエントリだけが含まれますが、要求をダウンストリーム サービスに負荷分散したい場合には、Ocelot を使用することで、複数のエントリを追加してロード バランサーを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="37312-185">Usually this will just contain one entry but sometimes you might want to load balance requests to your downstream services and Ocelot lets you add more than one entry and then select a load balancer.</span></span> <span data-ttu-id="37312-186">ただし、Azure と任意のオーケストレーターを使用している場合は、クラウドとオーケストレーター インフラストラクチャを使用して負荷分散した方が良い場合があります。</span><span class="sxs-lookup"><span data-stu-id="37312-186">But if using Azure and any orchestrator it is probably a better idea to load balance with the cloud and orchestrator infrastructure.</span></span>

<span data-ttu-id="37312-187">UpstreamPathTemplate は、クライアントから特定の要求に対し、どの DownstreamPathTemplate を使用するかを識別するために Ocelot によって使用される URL です。</span><span class="sxs-lookup"><span data-stu-id="37312-187">The UpstreamPathTemplate is the URL that Ocelot will use to identify which DownstreamPathTemplate to use for a given request from the client.</span></span> <span data-ttu-id="37312-188">最後に、Ocelot が異なる要求 (GET、POST、PUT) を同じ URL に区別できるように、UpstreamHttpMethod が使用されます。</span><span class="sxs-lookup"><span data-stu-id="37312-188">Finally, the UpstreamHttpMethod is used so Ocelot can distinguish between different requests (GET, POST, PUT) to the same URL.</span></span>

<span data-ttu-id="37312-189">この時点で、1 つまたは[複数のマージされた configuration.json ファイル](http://ocelot.readthedocs.io/en/latest/features/configuration.html#merging-configuration-files)を使用して 1 つの Ocelot API ゲートウェイ (ASP.NET Core WebHost) を持つことも、[Consul KV ストアに構成を格納](http://ocelot.readthedocs.io/en/latest/features/configuration.html#store-configuration-in-consul)することもできます。</span><span class="sxs-lookup"><span data-stu-id="37312-189">At this point, you could have a single Ocelot API Gateway (ASP.NET Core WebHost) using one or [multiple merged configuration.json files](http://ocelot.readthedocs.io/en/latest/features/configuration.html#merging-configuration-files) or you can also store the [configuration in a Consul KV store](http://ocelot.readthedocs.io/en/latest/features/configuration.html#store-configuration-in-consul).</span></span> 

<span data-ttu-id="37312-190">ただし、アーキテクチャとデザインのセクションで紹介したように、自律型のマイクロサービスが本当に必要な場合は、その 1 つのモノリシック API ゲートウェイを複数の API ゲートウェイや Backend for Frontend (BFF) に分割した方がよい場合があります。</span><span class="sxs-lookup"><span data-stu-id="37312-190">But as introduced in the architecture and design sections, if you really want to have autonomous microservices, it might be better to split that single monolithic API Gateway into multiple API Gateways and/or BFF (Backend for Frontend).</span></span> <span data-ttu-id="37312-191">そのために、Docker コンテナーを使用してこのアプローチを実装する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="37312-191">For that purpose, let’s see how to implement that approach with Docker containers.</span></span>

### <a name="using-a-single-docker-container-image-to-run-multiple-different-api-gateway--bff-container-types"></a><span data-ttu-id="37312-192">1 つの Docker コンテナー イメージを使用して、複数の異なる API ゲートウェイ/BFF コンテナーの種類を実行する</span><span class="sxs-lookup"><span data-stu-id="37312-192">Using a single Docker container image to run multiple different API Gateway / BFF container types</span></span> 

<span data-ttu-id="37312-193">eShopOnContainers のデザインでは、Ocelot API ゲートウェイを使用して 1 つの Docker コンテナー イメージが実装されますが、その後、Docker に配置するときに、各コンテナーに異なる configuration.json ファイルを提供することで、API ゲートウェイ/BFF コンテナーの種類ごとに異なるサービス/コンテナーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="37312-193">The design in eShopOnContainers implements a single Docker container image with the Ocelot API Gateway but then, when deploying to Docker, it creates different services/containers for each type of API-Gateway/BFF by providing a different configuration.json file for each container.</span></span>

![](./media/image33.png)

<span data-ttu-id="37312-194">**図 8-32**</span><span class="sxs-lookup"><span data-stu-id="37312-194">**Figure 8-32.**</span></span> <span data-ttu-id="37312-195">複数の API ゲートウェイの種類で 1 つの Ocelot Docker イメージを再利用する</span><span class="sxs-lookup"><span data-stu-id="37312-195">Reusing a single Ocelot Docker image across multiple API Gateway types</span></span>

<span data-ttu-id="37312-196">eShopOnContainers では、'OcelotApiGw' という名前のプロジェクトと、docker-compose.yml ファイルで指定されたイメージ名 “eshop/ocelotapigw” を使用して、“汎用 Ocelot API ゲートウェイ Docker イメージ” が作成されます。</span><span class="sxs-lookup"><span data-stu-id="37312-196">In eShopOnContainers, the “Generic Ocelot API Gateway Docker Image” is created with the project named 'OcelotApiGw' and the image name “eshop/ocelotapigw” that is specified in the docker-compose.yml file.</span></span> <span data-ttu-id="37312-197">次に、Docker に配置すると、次の docker-compose.yml ファイルからの抜粋に示されているように、同じ Docker イメージから作成された 4 つの API ゲートウェイ コンテナーができます。</span><span class="sxs-lookup"><span data-stu-id="37312-197">Then, when deploying to Docker, there will be four API-Gateway containers created from that same Docker image, as shown in the following extract from the docker-compose.yml file.</span></span>

```
PARTIAL DOCKER-COMPOSE.YML

  mobileshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
 
  mobilemarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
 
  webshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  webmarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
```

<span data-ttu-id="37312-198">さらに、API ゲートウェイのこれらのコンテナーの唯一の違いは、Ocelot 構成ファイルです。次の docker-compose.override.yml ファイルからわかるように、これはサービス コンテナーごとに異なり、Docker ボリュームにより実行時に指定されます。</span><span class="sxs-lookup"><span data-stu-id="37312-198">Additionally, and as you can see in the docker-compose.override.yml file, the only difference between those API Gateway containers is the Ocelot configuration file, which is different for each service container and is specified at runtime through a Docker volume, as shown in the following docker-compose.override.yml file.</span></span>

```
mobileshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity.api              
  ports:
    - "5200:80"   
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Shopping/apigw:/app/configuration
 
mobilemarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity.api              
  ports:
    - "5201:80"   
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Marketing/apigw:/app/configuration

webshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity.api              
  ports:
    - "5202:80"   
  volumes:
    - ./src/ApiGateways/Web.Bff.Shopping/apigw:/app/configuration

webmarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity.api              
  ports:
    - "5203:80"   
  volumes:
    - ./src/ApiGateways/Web.Bff.Marketing/apigw:/app/configuration
```

<span data-ttu-id="37312-199">次の Visual Studio エクスプローラーに示されているように、前のコードにより、特定のビジネス/BFF API ゲートウェイをそれぞれ定義するために必要な唯一のファイルは、configuration.json ファイルだけです。これは、4 つの API ゲートウェイが同じ Docker イメージに基づいているためです。</span><span class="sxs-lookup"><span data-stu-id="37312-199">Because of that previous code, and as shown in the Visual Studio Explorer below, the only file needed to define each specific business/BFF API Gateway is just a configuration.json file, because the four API Gateways are based on the same Docker image.</span></span>

![](./media/image34.png)

<span data-ttu-id="37312-200">**図 8-33**</span><span class="sxs-lookup"><span data-stu-id="37312-200">**Figure 8-33.**</span></span> <span data-ttu-id="37312-201">Ocelot で各 API ゲートウェイ/BFF を定義するために必要な唯一のファイル</span><span class="sxs-lookup"><span data-stu-id="37312-201">The only file needed to define each API Gateway / BFF with Ocelot is a configuration file</span></span> 

<span data-ttu-id="37312-202">API ゲートウェイを複数の API ゲートウェイに分割することで、マイクロサービスの異なるサブセットに重点を置いているさまざまな開発チームが、独立した Ocelot 構成ファイルを使用して、独自の API ゲートウェイを管理できます。</span><span class="sxs-lookup"><span data-stu-id="37312-202">By splitting the API Gateway into multiple API Gateways, different development teams focusing on different subsets of microservices can manage their own API Gateways by using independent Ocelot configuration files.</span></span> <span data-ttu-id="37312-203">くわえて、同時に同じ Ocelot Docker イメージを再利用することができます。</span><span class="sxs-lookup"><span data-stu-id="37312-203">Plus, at the same time they can reuse the same Ocelot Docker image.</span></span> 

<span data-ttu-id="37312-204">ここで、(eShopOnContainers ServicesAndWebApps.sln ソリューションを開くとき、または “docker-compose up” を実行している場合に、VS に既定で含まれる) API ゲートウェイを使用して eShopOnContainers を実行すると、次のサンプル ルートが実行されます。</span><span class="sxs-lookup"><span data-stu-id="37312-204">Now, if you run eShopOnContainers with the API Gateways (included by default in VS when opening eShopOnContainers-ServicesAndWebApps.sln solution or if running “docker-compose up”), the following sample routes will be performed.</span></span> 

<span data-ttu-id="37312-205">たとえば、webshoppingapigw API ゲートウェイによって提供されるアップストリーム URL http://localhost:5202/api/v1/c/catalog/items/2/ にアクセスすると、次のブラウザーにあるように、Docker ホスト内の内部ダウンストリーム URL http://catalog.api/api/v1/2 からの結果が得られます。</span><span class="sxs-lookup"><span data-stu-id="37312-205">For instance, when visiting the upstream URL http://localhost:5202/api/v1/c/catalog/items/2/ served by the webshoppingapigw API Gateway, you get the result from the internal Downstream URL http://catalog.api/api/v1/2 within the Docker host, as in the following browser.</span></span>

![](./media/image35.png)

<span data-ttu-id="37312-206">**図 8-34**</span><span class="sxs-lookup"><span data-stu-id="37312-206">**Figure 8-34.**</span></span> <span data-ttu-id="37312-207">API ゲートウェイによって提供される URL からマイクロサービスにアクセスする</span><span class="sxs-lookup"><span data-stu-id="37312-207">Accessing a microservice through a URL provided by the API Gateway</span></span> 

<span data-ttu-id="37312-208">テストまたはデバッグの理由から、API ゲートウェイを通過せずに Catalog Docker コンテナーに直接アクセスしたい場合 (開発環境でのみ)、'catalog.api' は Docker ホスト (docker-compose サービス名により処理されるサービス検索) 内部の DNS 解決であるため、コンテナーに直接アクセスする唯一の方法は、次のブラウザーにある http://localhost:5101/api/v1/Catalog/items/1 のように、開発テスト用にのみ提供される docker-compose.override.yml で公開されている外部ポートを経由することです。</span><span class="sxs-lookup"><span data-stu-id="37312-208">Because of testing or debugging reasons, if you wanted to directly access to the Catalog Docker container (only at the development environment) without passing through the API Gateway, since 'catalog.api' is a DNS resolution internal to the Docker host (service discovery handled by docker-compose service names), the only way to directly access the container is through the external port published in the docker-compose.override.yml, which is provided only for development tests, such as http://localhost:5101/api/v1/Catalog/items/1 in the following browser.</span></span>

![](./media/image36.png)

<span data-ttu-id="37312-209">**図 8-35**</span><span class="sxs-lookup"><span data-stu-id="37312-209">**Figure 8-35.**</span></span> <span data-ttu-id="37312-210">テスト目的でのマイクロサービスへの直接アクセス</span><span class="sxs-lookup"><span data-stu-id="37312-210">Direct access to a microservice for testing purposes</span></span> 

<span data-ttu-id="37312-211">ただし、アプリケーションは、ダイレクト ポートの "ショートカット" を経由してではなく、API ゲートウェイを経由してすべてのマイクロサービスにアクセスするように構成されます。</span><span class="sxs-lookup"><span data-stu-id="37312-211">But the application is configured so it accesses all the microservices through the API Gateways, not though the direct port “shortcuts”.</span></span> 

### <a name="the-gateway-aggregation-pattern-in-eshoponcontainers"></a><span data-ttu-id="37312-212">eShopOnContainers でのゲートウェイ集計パターン</span><span class="sxs-lookup"><span data-stu-id="37312-212">The Gateway aggregation pattern in eShopOnContainers</span></span>

<span data-ttu-id="37312-213">前述のとおり、要求の集計を実装する柔軟な方法は、コードでカスタム サービスを使用することです。</span><span class="sxs-lookup"><span data-stu-id="37312-213">As introduced previously, a flexible way to implement requests aggregation is with custom services, by code.</span></span> <span data-ttu-id="37312-214">Ocelot で要求の集計機能を使用して、要求の集計を実装することもできますが、十分な柔軟性が得られない場合があります。</span><span class="sxs-lookup"><span data-stu-id="37312-214">You could also implement request aggregation with the Request Aggregation feature in Ocelot, but it might not be as flexible as you need.</span></span> <span data-ttu-id="37312-215">そのため、eShopOnContainers で集計を実装するために選択した方法は、各アグリゲーターに明示的な ASP.NET Core Web API サービスを使用することです。</span><span class="sxs-lookup"><span data-stu-id="37312-215">Therefore, the selected way to implement aggregation in eShopOnContainers is with an explicit ASP.NET Core Web API services for each aggregator.</span></span> 

<span data-ttu-id="37312-216">このアプローチに従うと、前出の簡略化されたグローバル アーキテクチャの図に示されていないアグリゲーター サービスを考慮に入れた場合、API ゲートウェイ コンポジション図は、実際にはもう少し拡張されます。</span><span class="sxs-lookup"><span data-stu-id="37312-216">According to that approach, the API Gateway composition diagram is in reality a bit more extended when taking into account the aggregator services that are not shown in the simplified global architecture diagram shown previously.</span></span> 

<span data-ttu-id="37312-217">次の図では、アグリゲーター サービスがその関連する API ゲートウェイとどのように連携するかも確認できます。</span><span class="sxs-lookup"><span data-stu-id="37312-217">In the following diagram, you can also see how the aggregator services work with their related API Gateways.</span></span>

![](./media/image37.png)

<span data-ttu-id="37312-218">**図 8-36**</span><span class="sxs-lookup"><span data-stu-id="37312-218">**Figure 8-36.**</span></span> <span data-ttu-id="37312-219">アグリゲーター サービスを使用した eShopOnContainers アーキテクチャ</span><span class="sxs-lookup"><span data-stu-id="37312-219">eShopOnContainers architecture with aggregator services</span></span>

<span data-ttu-id="37312-220">次の図は、"ショッピング" ビジネス分野をさらに拡大して、API ゲートウェイの領域でこれらのアグリゲーター サービスを使用する場合に、マイクロサービスとの頻繁な通信がどのように削減できるかを確認できるようにしたものです。</span><span class="sxs-lookup"><span data-stu-id="37312-220">The following image is zooming in further, so you can notice how for the “Shopping” business area, the client apps could be improved by reducing chattiness with microservices when using those aggregator services under the realm of the API Gateways.</span></span> 

 ![](./media/image38.png)

<span data-ttu-id="37312-221">**図 8-37**</span><span class="sxs-lookup"><span data-stu-id="37312-221">**Figure 8-37.**</span></span> <span data-ttu-id="37312-222">アグリゲーター サービスのビジョンの拡大表示</span><span class="sxs-lookup"><span data-stu-id="37312-222">Zoom in vision of the Aggregator services</span></span>

<span data-ttu-id="37312-223">API ゲートウェイから送信される可能性がある要求を図に示すと、非常に複雑になる可能性があることがわかります。</span><span class="sxs-lookup"><span data-stu-id="37312-223">You can notice how when the diagram shows the possible requests coming from the API Gateways it can get pretty complex.</span></span> <span data-ttu-id="37312-224">しかし、クライアント アプリの視点かラ見て、青の矢印がどのように簡略化されるかがわかります。アグリゲーター パターンを使用すると、頻繁な通信と通信の待機時間を減らすことで、最終的にユーザー エクスペリエンス、とりわけリモート アプリ (モバイルおよび SPA アプリ) のユーザー エクスペリエンスが大幅に向上します。</span><span class="sxs-lookup"><span data-stu-id="37312-224">Although you can see how the arrows in blue would be simplified, from a client apps perspective, when using the aggregator pattern by reducing chattiness and latency in the communication, ultimately significantly improving the user experience for the remote apps (mobile and SPA apps), especially.</span></span> 

<span data-ttu-id="37312-225">"マーケティング" ビジネス領域とマイクロサービスの場合は、非常にシンプルなユース ケースであるため、アグリゲーターを使用する必要はありませんでしたが、必要に応じて使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="37312-225">In the case of the “Marketing” business area and microservices, it is a very simple use case so there was no need to use aggregators, but it could also be possible, if needed.</span></span>

### <a name="authentication-and-authorization-in-ocelot-api-gateways"></a><span data-ttu-id="37312-226">Ocelot API ゲートウェイでの認証と承認</span><span class="sxs-lookup"><span data-stu-id="37312-226">Authentication and authorization in Ocelot API Gateways</span></span>

<span data-ttu-id="37312-227">Ocelot API ゲートウェイでは、認証トークンを提供する [IdentityServer](http://identityserver.io/) を使用して、ASP.NET Core Web API サービスなどの認証サービスを API ゲートウェイの内側または外側に配置することができます。</span><span class="sxs-lookup"><span data-stu-id="37312-227">In an Ocelot API Gateway you can sit the authentication service, such as an ASP.NET Core Web API service using [IdentityServer](http://identityserver.io/) providing the auth token, either out or inside the API Gateway.</span></span>

<span data-ttu-id="37312-228">eShopOnContainers では、BFF に基づく境界とビジネス領域を持つ複数の API ゲートウェイを使用しているため、ID/認証サービス (次の図に黄色で強調表示) は、API ゲートウェイから除外されています。</span><span class="sxs-lookup"><span data-stu-id="37312-228">Since eShopOnContainers is using multiple API Gateways with boundaries based on BFF and business areas, the Identity/Auth service is left out of the API Gateways, as highlighted in yellow in the following diagram.</span></span>

 ![](./media/image39.png)

<span data-ttu-id="37312-229">**図 8-38**</span><span class="sxs-lookup"><span data-stu-id="37312-229">**Figure 8-38.**</span></span> <span data-ttu-id="37312-230">eShopOnContainers 内の ID サービスの位置</span><span class="sxs-lookup"><span data-stu-id="37312-230">Position of the Identity service in eShopOnContainers</span></span>

<span data-ttu-id="37312-231">ただし、Ocelot では、この別の図に示されているように、ID/認証マイクロサービスを API ゲートウェイの境界内に配置することもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="37312-231">However, Ocelot also supports to sit the Identity/Auth microservice within the API Gateway boundary, as in this other diagram.</span></span>

 ![](./media/image40.png)

<span data-ttu-id="37312-232">**図 8-39**</span><span class="sxs-lookup"><span data-stu-id="37312-232">**Figure 8-39.**</span></span> <span data-ttu-id="37312-233">Ocelot API ゲートウェイでの認証</span><span class="sxs-lookup"><span data-stu-id="37312-233">Authentication in Ocelot API Gateway</span></span>

<span data-ttu-id="37312-234">eShopOnContainers アプリケーションは API ゲートウェイを複数の Backend for Frontend (BFF) とビジネス領域の API ゲートウェイに分割しているため、もう 1 つのオプションは、横断的関心事に対して追加の API ゲートウェイを作成することでした。</span><span class="sxs-lookup"><span data-stu-id="37312-234">Because eShopOnContainers application has split the API Gateway into multiple BFF (Backend for Frontend) and business areas API Gateways, another option would had been to create an additional API Gateway for cross-cutting concerns.</span></span> <span data-ttu-id="37312-235">その選択は、複数の横断的関心事のマイクロサービスを持つより複雑なマイクロサービス ベースのアーキテクチャでは適切でしょう。</span><span class="sxs-lookup"><span data-stu-id="37312-235">That choice would be fair in a more complex microservice based architecture with multiple cross-cutting concerns microservices.</span></span> <span data-ttu-id="37312-236">eShopOnContainers には横断的関心事は 1 つしかないので、簡素化するため、API ゲートウェイ領域外のセキュリティ サービスだけを処理すると判断されました。</span><span class="sxs-lookup"><span data-stu-id="37312-236">Since there's only one cross-cutting concern in eShopOnContainers, it was decided to just handle the security service out of the API Gateway realm, for simplicity’s sake.</span></span>

<span data-ttu-id="37312-237">いずれの場合も、アプリが API ゲートウェイ レベルでセキュリティ保護されている場合は、任意のセキュリティで保護されたマイクロサービスを使用するときに、最初にアクセスされるのは、Ocelot API ゲートウェイの認証モジュールです。</span><span class="sxs-lookup"><span data-stu-id="37312-237">In any case, if the app is secured at the API Gateway level, the authentication module of the Ocelot API Gateway is visited at first when trying to use any secured microservice.</span></span> <span data-ttu-id="37312-238">これは HTTP 要求をリダイレクトして、ID/認証マイクロサービスにアクセスして、access_token で保護されたサービスにアクセスできるように、アクセス トークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="37312-238">That re-directs the HTTP request to visit the Identity or auth microservice to get the access token so so you can visit the protected services with the access_token.</span></span>

<span data-ttu-id="37312-239">認証を使用して API ゲートウェイ レベルで任意のサービスをセキュリティで保護する方法は、configuration.json で、その関連する設定に AuthenticationProviderKey を設定することで行います。</span><span class="sxs-lookup"><span data-stu-id="37312-239">The way you secure with authentication any service at the API Gateway level is by setting the AuthenticationProviderKey in its related settings at the configuration.json.</span></span>

```
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket.api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }
```

<span data-ttu-id="37312-240">Ocelot により実行時に Re-Routes AuthenticationOptions.AuthenticationProviderKey が調べられ、指定したキーに登録されている認証プロバイダーがあることが確認されます。</span><span class="sxs-lookup"><span data-stu-id="37312-240">When Ocelot runs, it will look at the Re-Routes AuthenticationOptions.AuthenticationProviderKey and check that there is an Authentication Provider registered with the given key.</span></span> <span data-ttu-id="37312-241">ない場合は、Ocelot が起動しません。</span><span class="sxs-lookup"><span data-stu-id="37312-241">If there isn't, then Ocelot will not start up.</span></span> <span data-ttu-id="37312-242">ある場合、ReRoute の実行時にそのプロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="37312-242">If there is, then the ReRoute will use that provider when it executes.</span></span>

<span data-ttu-id="37312-243">Ocelot WebHost は `authenticationProviderKey = "IdentityApiKey"` で構成されているため、そのサービスに認証トークンがない任意の要求がある場合には常に、認証が必要になります。</span><span class="sxs-lookup"><span data-stu-id="37312-243">Because the Ocelot WebHost is configured with the `authenticationProviderKey = "IdentityApiKey"`, that will require authentication whenever that service has any requests without any auth token.</span></span> 

```csharp
namespace OcelotApiGw
{
    public class Startup
    {
        private readonly IConfiguration _cfg;

        public Startup(IConfiguration configuration) => _cfg = configuration;

        public void ConfigureServices(IServiceCollection services)
        {
            var identityUrl = _cfg.GetValue<string>("IdentityUrl");
            var authenticationProviderKey = "IdentityApiKey";
                         //…
            services.AddAuthentication()
                .AddJwtBearer(authenticationProviderKey, x =>
                {
                    x.Authority = identityUrl;
                    x.RequireHttpsMetadata = false;
                    x.TokenValidationParameters = new Microsoft.IdentityModel.Tokens.TokenValidationParameters()
                    {
                        ValidAudiences = new[] { "orders", "basket", "locations", "marketing", "mobileshoppingagg", "webshoppingagg" }
                    };                   
                });
            //...
```

<span data-ttu-id="37312-244">次に、次の Basket マイクロサービス コントローラーのように、マイクロサービスのようにアクセスされる任意のリソースで、[Authorize] 属性を使用して承認を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="37312-244">Then, you also need to set authorization with the [Authorize] attribute on any resource to be accessed like the microservices, such as in the following Basket microservice controller.</span></span>

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class BasketController : Controller
    {   
      //...
    }
}
```

<span data-ttu-id="37312-245">“basket” などの ValidAudiences は、次のコードのように、Startup クラスの ConfigureServices() で、`AddJwtBearer()` を使用して各マイクロサービスで定義されている対象ユーザーと関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="37312-245">The ValidAudiences such as “basket” are correlated with the audience defined in each microservice with `AddJwtBearer()` at the ConfigureServices() of the Startup class, such as in the code below.</span></span>

```csharp
// prevent from mapping "sub" claim to nameidentifier.
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

var identityUrl = Configuration.GetValue<string>("IdentityUrl"); 
                
services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;

}).AddJwtBearer(options =>
{
    options.Authority = identityUrl;
    options.RequireHttpsMetadata = false;
    options.Audience = "basket";
});
```

<span data-ttu-id="37312-246">次の手順として、http://localhost:5202/api/v1/b/basket/1 のような API ゲートウェイに基づく ReRoute URL を使用して、セキュリティで保護された任意のマイクロサービスにアクセスしようしたときに、有効なトークンを提供しないと、401 未承認エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="37312-246">As next step, if you try to access any secured microservice like the Basket microservice with a Re-Route URL based on the API Gateway like http://localhost:5202/api/v1/b/basket/1 then you’ll get a 401 Unauthorized unless you provide a valid token.</span></span> <span data-ttu-id="37312-247">その一方で、ReRoute URL が認証されると、(内部マイクロサービス URL に) 関連付けられているダウンストリーム スキーマが何であれ、Ocelot によって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="37312-247">On the other hand, if a Re-Route URL is authenticated, Ocelot will invoke whatever downstream scheme is associated with it (the internal microservice URL).</span></span>

<span data-ttu-id="37312-248">**Ocelot の ReRoute 層での承認。**</span><span class="sxs-lookup"><span data-stu-id="37312-248">**Authorization at Ocelot’s Re-Routes tier.**</span></span>  <span data-ttu-id="37312-249">Ocelot では、認証後に評価されたクレーム ベースの承認をサポートします。</span><span class="sxs-lookup"><span data-stu-id="37312-249">Ocelot supports claims-based authorization evaluated after the authentication.</span></span> <span data-ttu-id="37312-250">次のコードを ReRoute 構成に追加することで、ルート レベルで承認を設定します。</span><span class="sxs-lookup"><span data-stu-id="37312-250">You set the authorization at a route level by adding the following code to the re-route configuration.</span></span> 

```
"RouteClaimsRequirement": {
    "UserType": "employee"
}
```

<span data-ttu-id="37312-251">この例では、承認ミドルウェアが呼び出されると、ユーザーがトークンに要求の種類 'UserType' を持っているかどうか、および要求の値が 'employee' かどうかが Ocelot によって確認されます。</span><span class="sxs-lookup"><span data-stu-id="37312-251">In that example, when the authorization middleware is called, Ocelot will find if the user has the claim type 'UserType' in the token and if the value of that claim is 'employee'.</span></span> <span data-ttu-id="37312-252">そうでない場合、ユーザーは承認されず、403 アクセス不可の応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="37312-252">If it isn’t, then the user will not be authorized and the response will be 403 forbidden.</span></span> 

## <a name="using-kubernetes-ingress-plus-ocelot-api-gateways"></a><span data-ttu-id="37312-253">Kubernetes イングレスと Ocelot API ゲートウェイを使用する</span><span class="sxs-lookup"><span data-stu-id="37312-253">Using Kubernetes Ingress plus Ocelot API Gateways</span></span>

<span data-ttu-id="37312-254">(Azure Kubernetes Service クラスターで使用されているように) Kubernetes を使用する場合、通常は、*Nginx* に基づく [Kuberentes イングレス層](https://kubernetes.io/docs/concepts/services-networking/ingress/)を通じて、すべての HTTP 要求を統合します。</span><span class="sxs-lookup"><span data-stu-id="37312-254">When using Kubernetes (like in an Azure Kubernetes Service cluster),, you usually unify all the HTTP requests through the [Kuberentes Ingress tier](https://kubernetes.io/docs/concepts/services-networking/ingress/) based on *Nginx*.</span></span> 

<span data-ttu-id="37312-255">Kuberentes では、どのイングレス アプローチも使用しない場合、サービスとポッドには、クラスター ネットワークによるルーティングのみが可能な IP が与えられます。</span><span class="sxs-lookup"><span data-stu-id="37312-255">In Kuberentes, if you don’t use any ingress approach, then your services and pods have IPs only routable by the cluster network.</span></span> 

<span data-ttu-id="37312-256">ただし、イングレス アプローチを使用する場合は、リバース プロキシとして機能する、インターネットとサービス (API ゲートウェイを含む) の間の中間層が与えられます。</span><span class="sxs-lookup"><span data-stu-id="37312-256">But if you use an ingress approach, you'll have a middle tier between the Internet and your services (including your API Gateways), acting as a reverse proxy.</span></span>

<span data-ttu-id="37312-257">定義としては、イングレスはクラスター サービスに到達する受信接続を許可するルールのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="37312-257">As a definition, an Ingress is a collection of rules that allow inbound connections to reach the cluster services.</span></span> <span data-ttu-id="37312-258">イングレスは、サービスに外部から到達可能な URL の提供、トラフィックの負荷分散、SSL 終了などを提供するために構成されます。</span><span class="sxs-lookup"><span data-stu-id="37312-258">An ingress is configured to provide services externally reachable URLs, load balance traffic, SSL termination and more.</span></span> <span data-ttu-id="37312-259">ユーザーは、イングレス リソースを API サーバーに POST することで、イングレスを要求します。</span><span class="sxs-lookup"><span data-stu-id="37312-259">Users request ingress by POSTing the Ingress resource to the API server.</span></span>

<span data-ttu-id="37312-260">eShopOnContainers では、ローカルで開発し、開発用コンピューターだけを Docker ホストとして使用する場合は、どのイングレスも使用せず、複数の API ゲートウェイのみを使用します。</span><span class="sxs-lookup"><span data-stu-id="37312-260">In eShopOnContainers, when developing locally and using just your development machine as the Docker host, you are not using any ingress but only the multiple API Gateways.</span></span> 

<span data-ttu-id="37312-261">ただし、Kuberentes に基づいて "運用" 環境をターゲットにする場合は、eShopOnCOntainers では、API ゲートウェイの前にイングレスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="37312-261">However, when targeting a “production” environment based on Kuberentes, eShopOnCOntainers is using an ingress in front of the API gateways.</span></span> <span data-ttu-id="37312-262">これにより、クライアントは引き続き同じベース URL を呼び出しますが、要求は複数の API ゲートウェイまたは BFF にルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="37312-262">That way, the clients still call the same base URL but the requests are routed to multiple API Gateways or BFF.</span></span> 

<span data-ttu-id="37312-263">API ゲートウェイは、サービスのみに接するフロント エンドまたはファサードで、Web アプリケーションではなく、通常はその範囲外であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="37312-263">Note that API Gateways are front-ends or facades surfacing only the services but not the web applications that are usually out of their scope.</span></span> <span data-ttu-id="37312-264">さらに、API ゲートウェイは、特定の内部マイクロサービスを非表示にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="37312-264">In addition, the API Gateways might hide certain internal microservices.</span></span> 

<span data-ttu-id="37312-265">ただし、イングレスは、HTTP 要求をリダイレクトするだけですが、どのマイクロサービスまたは Web アプリも非表示にはしません。</span><span class="sxs-lookup"><span data-stu-id="37312-265">The ingress, however, is just redirecting HTTP requests but not trying to hide any microservice or web app.</span></span>

<span data-ttu-id="37312-266">次の図に示すように、Web アプリケーションの前の Kuberentes にイングレス Nginx 層と、複数の Ocelot API ゲートウェイ/BFF を配置するのが理想的なアーキテクチャです。</span><span class="sxs-lookup"><span data-stu-id="37312-266">Having an ingress Nginx tier in Kuberentes in front of the web applications plus the several Ocelot API Gateways / BFF is the ideal architecture, as shown in the following diagram.</span></span>

 ![](./media/image41.png)

<span data-ttu-id="37312-267">**図 8-40**</span><span class="sxs-lookup"><span data-stu-id="37312-267">**Figure 8-40.**</span></span> <span data-ttu-id="37312-268">Kubernetes に配置するときの eShopOnContainers のイングレス層</span><span class="sxs-lookup"><span data-stu-id="37312-268">The ingress tier in eShopOnContainers when deployed into Kubernetes</span></span>

<span data-ttu-id="37312-269">Kuberentes に eShopOnContainers を配置すると、少数のサービスまたはエンドポイントだけが_イングレス_経由で公開されます。基本的に、URL の接尾辞は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="37312-269">When you deploy eShopOnContainers into Kuberentes, it exposes just a few services or endpoints via _ingress_, basically the following list of postfixes on the URLs:</span></span>

-   <span data-ttu-id="37312-270">`/`: クライアント SPA Web アプリケーション用</span><span class="sxs-lookup"><span data-stu-id="37312-270">`/` for the client SPA web application</span></span>
-   <span data-ttu-id="37312-271">`/webmvc`: クライアント MVC Web アプリケーション用</span><span class="sxs-lookup"><span data-stu-id="37312-271">`/webmvc` for the client MVC web application</span></span>
-   <span data-ttu-id="37312-272">`/webstatus`: 状態/正常性チェックを示すクライアント Web アプリ用</span><span class="sxs-lookup"><span data-stu-id="37312-272">`/webstatus` for the client web app showing the status/healchecks</span></span>
-   <span data-ttu-id="37312-273">`/webshoppingapigw`: Web BFF とショッピング ビジネス プロセス用</span><span class="sxs-lookup"><span data-stu-id="37312-273">`/webshoppingapigw` for the web BFF and shopping business processes</span></span>
-   <span data-ttu-id="37312-274">`/webmarketingapigw`: Web BFF とマーケティング ビジネス プロセス用</span><span class="sxs-lookup"><span data-stu-id="37312-274">`/webmarketingapigw` for the web BFF and marketing business processes</span></span>
-   <span data-ttu-id="37312-275">`/mobileshoppingapigw`: モバイル BFF とショッピング ビジネス プロセス用</span><span class="sxs-lookup"><span data-stu-id="37312-275">`/mobileshoppingapigw` for the mobile BFF and shopping business processes</span></span>
-   <span data-ttu-id="37312-276">`/mobilemarketingapigw`: モバイル BFF とマーケティング ビジネス プロセス用</span><span class="sxs-lookup"><span data-stu-id="37312-276">`/mobilemarketingapigw` for the mobile BFF and marketing business processes</span></span>

<span data-ttu-id="37312-277">Kubernetes に配置するときに、各 Ocelot API ゲートウェイは、API ゲートウェイを実行している_ポッド_ごとに異なる "configuration.json" ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="37312-277">When deploying to Kubernetes, each Ocelot API Gateway is using a different “configuration.json” file for each _pod_ running the API Gateways.</span></span> <span data-ttu-id="37312-278">これらの "configuration.json" ファイルは、‘ocelot’ という名前の Kuberentes _構成マップ_に基づいて作成されたボリュームを (もともとは deploy.ps1 スクリプトを使用して) マウントすることによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="37312-278">Those “configuration.json” files are provided by mounting (originally with the deploy.ps1 script) a volume created based on a Kuberentes _config map_ named ‘ocelot’.</span></span> <span data-ttu-id="37312-279">各コンテナーにより、関連する構成ファイルが `/app/configuration` という名前のコンテナーのフォルダーにマウントされます。</span><span class="sxs-lookup"><span data-stu-id="37312-279">Each container mounts its related configuration file in the container’s folder named `/app/configuration`.</span></span>

<span data-ttu-id="37312-280">eShopOnContainers のソース コード ファイルでは、元の "configuration.json" ファイルは `k8s/ocelot/` フォルダー内にあります。</span><span class="sxs-lookup"><span data-stu-id="37312-280">In the source code files of eShopOnContainers, the original “configuration.json” files can be found within the `k8s/ocelot/` folder.</span></span> <span data-ttu-id="37312-281">BFF/API ゲートウェイごとに 1 つのファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="37312-281">There’s one file for each BFF/APIGateway.</span></span>


## <a name="additional-cross-cutting-features-in-an-ocelot-api-gateway"></a><span data-ttu-id="37312-282">Ocelot API ゲートウェイで追加された横断機能</span><span class="sxs-lookup"><span data-stu-id="37312-282">Additional cross-cutting features in an Ocelot API Gateway</span></span>

<span data-ttu-id="37312-283">Ocelot API ゲートウェイを使用する場合に、調査および使用するための重要な機能は他にもあり、次のリンクで説明されています。</span><span class="sxs-lookup"><span data-stu-id="37312-283">There are other important features to research and use, when using an Ocelot API Gateway, described in the following links.</span></span>

-   <span data-ttu-id="37312-284">**Ocelot と Consul または Eureka を統合するクライアント側でのサービス検出** 
    [*http://ocelot.readthedocs.io/en/latest/features/servicediscovery.html*](http://ocelot.readthedocs.io/en/latest/features/servicediscovery.html)</span><span class="sxs-lookup"><span data-stu-id="37312-284">**Service discovery in the client side integrating Ocelot with Consul or Eureka** 
[*http://ocelot.readthedocs.io/en/latest/features/servicediscovery.html*](http://ocelot.readthedocs.io/en/latest/features/servicediscovery.html)</span></span>

-   <span data-ttu-id="37312-285">**API ゲートウェイ層でのキャッシュ** 
    [*http://ocelot.readthedocs.io/en/latest/features/caching.html*](http://ocelot.readthedocs.io/en/latest/features/caching.html)</span><span class="sxs-lookup"><span data-stu-id="37312-285">**Caching at the API Gateway tier** 
[*http://ocelot.readthedocs.io/en/latest/features/caching.html*](http://ocelot.readthedocs.io/en/latest/features/caching.html)</span></span>

-   <span data-ttu-id="37312-286">**API ゲートウェイ層でのログ記録** 
    [*http://ocelot.readthedocs.io/en/latest/features/logging.html*](http://ocelot.readthedocs.io/en/latest/features/logging.html)</span><span class="sxs-lookup"><span data-stu-id="37312-286">**Logging at the API Gateway tier** 
[*http://ocelot.readthedocs.io/en/latest/features/logging.html*](http://ocelot.readthedocs.io/en/latest/features/logging.html)</span></span>

-   <span data-ttu-id="37312-287">**API ゲートウェイ層でのサービスの品質 (再試行、サーキット ブレーカー)** 
    [*http://ocelot.readthedocs.io/en/latest/features/qualityofservice.html*](http://ocelot.readthedocs.io/en/latest/features/qualityofservice.html)</span><span class="sxs-lookup"><span data-stu-id="37312-287">**Quality of Service (Retries and Circuit breakers) at the API Gateway tier** 
[*http://ocelot.readthedocs.io/en/latest/features/qualityofservice.html*](http://ocelot.readthedocs.io/en/latest/features/qualityofservice.html)</span></span>

-   <span data-ttu-id="37312-288">**速度の制限** 
    [*http://ocelot.readthedocs.io/en/latest/features/ratelimiting.html*](http://ocelot.readthedocs.io/en/latest/features/ratelimiting.html )</span><span class="sxs-lookup"><span data-stu-id="37312-288">**Rate limiting** 
[*http://ocelot.readthedocs.io/en/latest/features/ratelimiting.html*](http://ocelot.readthedocs.io/en/latest/features/ratelimiting.html )</span></span>




>[!div class="step-by-step"]
<span data-ttu-id="37312-289">[前へ] (background-tasks-with-ihostedservice.md) [次へ] (../microservice-ddd-cqrs-patterns/index.md)</span><span class="sxs-lookup"><span data-stu-id="37312-289">[Previous] (background-tasks-with-ihostedservice.md) [Next] (../microservice-ddd-cqrs-patterns/index.md)</span></span>