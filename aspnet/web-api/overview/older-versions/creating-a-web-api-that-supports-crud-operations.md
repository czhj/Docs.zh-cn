---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "启用 ASP.NET Web API 1 中的 CRUD 操作 |Microsoft 文档"
author: MikeWasson
description: "本教程演示如何使用 ASP.NET Web API 的 HTTP 服务中支持 CRUD 操作。 在教程的 Visual Studio 2012 Web AP 中使用的软件版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a91bf065c9ce0fc5bd9b7115340edabea975a7e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="7c3ea-104">启用 ASP.NET Web API 1 中的 CRUD 操作</span><span class="sxs-lookup"><span data-stu-id="7c3ea-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="7c3ea-105">通过[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7c3ea-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7c3ea-106">下载已完成的项目</span><span class="sxs-lookup"><span data-stu-id="7c3ea-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="7c3ea-107">本教程演示如何使用 ASP.NET Web API 的 HTTP 服务中支持 CRUD 操作。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7c3ea-108">在本教程中使用的软件版本</span><span class="sxs-lookup"><span data-stu-id="7c3ea-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7c3ea-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7c3ea-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="7c3ea-110">Web API 1 （也适用于 Web API 2）</span><span class="sxs-lookup"><span data-stu-id="7c3ea-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="7c3ea-111">CRUD 代表&quot;创建、 读取、 更新和删除，&quot;哪些是四个基本数据库操作。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="7c3ea-112">许多 HTTP 服务还模型通过 REST 或类似于 REST 的 Api 的 CRUD 操作。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="7c3ea-113">在本教程中，你将生成一个非常简单的 web API 可以管理的产品列表。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="7c3ea-114">每个产品都将包含名称、 价格和类别 (如&quot;toys&quot;或&quot;硬件&quot;)，加上一个产品 id。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="7c3ea-115">产品 API 将公开以下方法。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="7c3ea-116">操作</span><span class="sxs-lookup"><span data-stu-id="7c3ea-116">Action</span></span> | <span data-ttu-id="7c3ea-117">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="7c3ea-117">HTTP method</span></span> | <span data-ttu-id="7c3ea-118">相对 URI</span><span class="sxs-lookup"><span data-stu-id="7c3ea-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7c3ea-119">获取所有产品的列表</span><span class="sxs-lookup"><span data-stu-id="7c3ea-119">Get a list of all products</span></span> | <span data-ttu-id="7c3ea-120">GET</span><span class="sxs-lookup"><span data-stu-id="7c3ea-120">GET</span></span> | <span data-ttu-id="7c3ea-121">/ api/产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-121">/api/products</span></span> |
| <span data-ttu-id="7c3ea-122">获取按 ID 的产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-122">Get a product by ID</span></span> | <span data-ttu-id="7c3ea-123">GET</span><span class="sxs-lookup"><span data-stu-id="7c3ea-123">GET</span></span> | <span data-ttu-id="7c3ea-124">/api/产品/*id*</span><span class="sxs-lookup"><span data-stu-id="7c3ea-124">/api/products/*id*</span></span> |
| <span data-ttu-id="7c3ea-125">按类别获取产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-125">Get a product by category</span></span> | <span data-ttu-id="7c3ea-126">GET</span><span class="sxs-lookup"><span data-stu-id="7c3ea-126">GET</span></span> | <span data-ttu-id="7c3ea-127">/ api/产品？ 类别 =*类别*</span><span class="sxs-lookup"><span data-stu-id="7c3ea-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="7c3ea-128">创建新产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-128">Create a new product</span></span> | <span data-ttu-id="7c3ea-129">发布</span><span class="sxs-lookup"><span data-stu-id="7c3ea-129">POST</span></span> | <span data-ttu-id="7c3ea-130">/ api/产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-130">/api/products</span></span> |
| <span data-ttu-id="7c3ea-131">将产品更新</span><span class="sxs-lookup"><span data-stu-id="7c3ea-131">Update a product</span></span> | <span data-ttu-id="7c3ea-132">PUT</span><span class="sxs-lookup"><span data-stu-id="7c3ea-132">PUT</span></span> | <span data-ttu-id="7c3ea-133">/api/产品/*id*</span><span class="sxs-lookup"><span data-stu-id="7c3ea-133">/api/products/*id*</span></span> |
| <span data-ttu-id="7c3ea-134">删除产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-134">Delete a product</span></span> | <span data-ttu-id="7c3ea-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="7c3ea-135">DELETE</span></span> | <span data-ttu-id="7c3ea-136">/api/产品/*id*</span><span class="sxs-lookup"><span data-stu-id="7c3ea-136">/api/products/*id*</span></span> |

<span data-ttu-id="7c3ea-137">请注意一些 Uri 在路径中包括的产品 ID。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="7c3ea-138">例如，若要获取其 ID 28 的产品，客户端发送 GET 请求`http://hostname/api/products/28`。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="7c3ea-139">资源</span><span class="sxs-lookup"><span data-stu-id="7c3ea-139">Resources</span></span>

<span data-ttu-id="7c3ea-140">产品 API 定义这两种资源类型的 Uri:</span><span class="sxs-lookup"><span data-stu-id="7c3ea-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="7c3ea-141">资源</span><span class="sxs-lookup"><span data-stu-id="7c3ea-141">Resource</span></span> | <span data-ttu-id="7c3ea-142">URI</span><span class="sxs-lookup"><span data-stu-id="7c3ea-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="7c3ea-143">所有产品的列表。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-143">The list of all the products.</span></span> | <span data-ttu-id="7c3ea-144">/ api/产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-144">/api/products</span></span> |
| <span data-ttu-id="7c3ea-145">单独的产品。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-145">An individual product.</span></span> | <span data-ttu-id="7c3ea-146">/api/产品/*id*</span><span class="sxs-lookup"><span data-stu-id="7c3ea-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="7c3ea-147">方法</span><span class="sxs-lookup"><span data-stu-id="7c3ea-147">Methods</span></span>

<span data-ttu-id="7c3ea-148">四个主要 HTTP 方法 （GET、 PUT、 POST 和 DELETE） 可以如下所示映射到 CRUD 操作：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="7c3ea-149">GET 操作将检索的表示形式指定 URI 处的资源。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="7c3ea-150">获取应在服务器上没有任何副作用。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="7c3ea-151">PUT 更新指定 URI 处的资源。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="7c3ea-152">PUT 还可用于创建新的资源在指定的 URI，如果服务器允许客户端指定新的 Uri。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="7c3ea-153">对于本教程，API 将不支持通过 PUT 创建。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="7c3ea-154">POST 创建新的资源。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-154">POST creates a new resource.</span></span> <span data-ttu-id="7c3ea-155">服务器分配新对象的 URI，并将此 URI 返回响应消息的一部分。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="7c3ea-156">删除： 删除指定 URI 处的资源。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="7c3ea-157">注意： PUT 方法将替换整个 product 实体。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="7c3ea-158">即，客户端需要发送更新的产品的完整表示。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="7c3ea-159">如果你想要支持部分更新，修补程序的方法是首选的。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="7c3ea-160">本教程未实现修补程序。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="7c3ea-161">创建新的 Web API 项目</span><span class="sxs-lookup"><span data-stu-id="7c3ea-161">Create a New Web API Project</span></span>

<span data-ttu-id="7c3ea-162">通过运行 Visual Studio 启动，然后选择**新项目**从**启动**页。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="7c3ea-163">或从**文件**菜单上，选择**新建**然后**项目**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="7c3ea-164">在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="7c3ea-165">下**Visual C#**，选择**Web**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="7c3ea-166">在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7c3ea-167">将项目&quot;ProductStore&quot;单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="7c3ea-168">在**新建 ASP.NET MVC 4 项目**对话框中，选择**Web API**单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="7c3ea-169">添加模型</span><span class="sxs-lookup"><span data-stu-id="7c3ea-169">Adding a Model</span></span>

<span data-ttu-id="7c3ea-170">模型是表示应用程序中的数据的对象。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="7c3ea-171">在 ASP.NET Web API 中，可以将强类型的 CLR 对象用作模型，并且它们将自动将序列化到 XML 或 JSON 为客户端。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="7c3ea-172">对于 ProductStore API 中，我们的数据包括的产品，因此我们将创建一个名为的新类`Product`。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="7c3ea-173">如果解决方案资源管理器不可见，请单击**视图**菜单，然后选择**解决方案资源管理器**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="7c3ea-174">在解决方案资源管理器，右键单击**模型**文件夹。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="7c3ea-175">从上下文 meny 中，选择**添加**，然后选择**类**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="7c3ea-176">将类&quot;产品&quot;。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="7c3ea-177">添加到以下属性`Product`类。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="7c3ea-178">添加存储库</span><span class="sxs-lookup"><span data-stu-id="7c3ea-178">Adding a Repository</span></span>

<span data-ttu-id="7c3ea-179">我们需要存储的产品的集合。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-179">We need to store a collection of products.</span></span> <span data-ttu-id="7c3ea-180">它是一个好办法分隔从我们的服务实现的集合。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="7c3ea-181">这样一来，我们可以无需重写服务类更改的后备存储。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="7c3ea-182">此类型的设计称为*存储库*模式。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="7c3ea-183">首先定义存储库中的泛型接口。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="7c3ea-184">在解决方案资源管理器，右键单击**模型**文件夹。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="7c3ea-185">选择**添加**，然后选择**新项**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="7c3ea-186">在**模板**窗格中，选择**已安装的模板**展开 C# 节点。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="7c3ea-187">在 C# 中，选择**代码**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-187">Under C#, select **Code**.</span></span> <span data-ttu-id="7c3ea-188">在代码模板列表中，选择**接口**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="7c3ea-189">命名接口&quot;IProductRepository&quot;。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="7c3ea-190">添加以下实现：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="7c3ea-191">现在将另一个类添加到 Models 文件夹中，名为&quot;化 ProductRepository。&quot;此类将实现 `IProductRespository` 接口。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="7c3ea-192">添加以下实现：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="7c3ea-193">存储库在本地内存中保留列表。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="7c3ea-194">这就是本教程，确定，但在实际应用中，您应将数据从外部看，这两个数据库存储或云存储中。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="7c3ea-195">存储库模式将使其更轻松地更改实现更高版本。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="7c3ea-196">添加一个 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="7c3ea-196">Adding a Web API Controller</span></span>

<span data-ttu-id="7c3ea-197">如果你使用了 ASP.NET MVC，然后你已熟悉控制器。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="7c3ea-198">在 ASP.NET Web API 中，*控制器*是处理来自客户端的 HTTP 请求的类。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="7c3ea-199">新项目向导创建项目时，会为您创建两个控制器。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="7c3ea-200">若要查看它们，展开解决方案资源管理器中的 Controllers 文件夹。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="7c3ea-201">HomeController 是传统的 ASP.NET MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="7c3ea-202">它负责提供服务的站点的 HTML 页面，并不直接与我们的 web API。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="7c3ea-203">ValuesController 是一个示例 WebAPI 控制器。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="7c3ea-204">请继续并右键单击解决方案资源管理器中的文件并选择删除 ValuesController，**删除。**</span><span class="sxs-lookup"><span data-stu-id="7c3ea-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="7c3ea-205">现在，如下所示添加新控制器：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="7c3ea-206">在**解决方案资源管理器**，右键单击 Controllers 文件夹。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-206">In **Solution Explorer**, right-click the the Controllers folder.</span></span> <span data-ttu-id="7c3ea-207">选择**添加**，然后选择**控制器**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="7c3ea-208">在**添加控制器**向导中，名称控制器&quot;ProductsController&quot;。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="7c3ea-209">在**模板**下拉列表中，选择**空 API 控制器**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="7c3ea-210">然后单击 **“添加”**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="7c3ea-211">不需要将你的控制器放入名为控制器的文件夹。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="7c3ea-212">文件夹名称并不重要;它是只需组织源文件一种简便方式。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="7c3ea-213">**添加控制器**向导将创建名为 ProductsController.cs 在控制器文件夹中的文件。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="7c3ea-214">如果没有打开此文件，则双击该文件以打开它。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="7c3ea-215">添加以下**使用**语句：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="7c3ea-216">添加保存的字段**IProductRepository**实例。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="7c3ea-217">调用`new ProductRepository()`控制器中不是最佳设计，因为它会绑定到特定的实现的控制器`IProductRepository`。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="7c3ea-218">更好的方法，请参阅[使用 Web API 依赖项解析程序](../advanced/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="7c3ea-219">获取资源</span><span class="sxs-lookup"><span data-stu-id="7c3ea-219">Getting a Resource</span></span>

<span data-ttu-id="7c3ea-220">ProductStore API 将公开多个&quot;读取&quot;作为 HTTP GET 方法的操作。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="7c3ea-221">每个操作将对应于中的方法`ProductsController`类。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="7c3ea-222">操作</span><span class="sxs-lookup"><span data-stu-id="7c3ea-222">Action</span></span> | <span data-ttu-id="7c3ea-223">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="7c3ea-223">HTTP method</span></span> | <span data-ttu-id="7c3ea-224">相对 URI</span><span class="sxs-lookup"><span data-stu-id="7c3ea-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7c3ea-225">获取所有产品的列表</span><span class="sxs-lookup"><span data-stu-id="7c3ea-225">Get a list of all products</span></span> | <span data-ttu-id="7c3ea-226">GET</span><span class="sxs-lookup"><span data-stu-id="7c3ea-226">GET</span></span> | <span data-ttu-id="7c3ea-227">/ api/产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-227">/api/products</span></span> |
| <span data-ttu-id="7c3ea-228">获取按 ID 的产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-228">Get a product by ID</span></span> | <span data-ttu-id="7c3ea-229">GET</span><span class="sxs-lookup"><span data-stu-id="7c3ea-229">GET</span></span> | <span data-ttu-id="7c3ea-230">/api/产品/*id*</span><span class="sxs-lookup"><span data-stu-id="7c3ea-230">/api/products/*id*</span></span> |
| <span data-ttu-id="7c3ea-231">按类别获取产品</span><span class="sxs-lookup"><span data-stu-id="7c3ea-231">Get a product by category</span></span> | <span data-ttu-id="7c3ea-232">GET</span><span class="sxs-lookup"><span data-stu-id="7c3ea-232">GET</span></span> | <span data-ttu-id="7c3ea-233">/ api/产品？ 类别 =*类别*</span><span class="sxs-lookup"><span data-stu-id="7c3ea-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="7c3ea-234">若要获取所有产品的列表，将以下方法添加到`ProductsController`类：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="7c3ea-235">方法名称开头&quot;获取&quot;，因此按照约定它映射到 GET 请求。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="7c3ea-236">此外，因为该方法不具有任何参数，它映射到一个 URI，不包含 *&quot;id&quot;* 路径中的段。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="7c3ea-237">若要获取的产品按 ID，将以下方法添加到`ProductsController`类：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="7c3ea-238">此方法名称还开头&quot;获取&quot;，但方法具有一个名为参数*id*。此参数映射到&quot;id&quot; URI 路径段。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="7c3ea-239">ASP.NET Web API 框架会自动将 ID 转换为正确的数据类型 (**int**) 的参数。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="7c3ea-240">GetProduct 方法将引发类型的异常**HttpResponseException**如果*id*无效。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="7c3ea-241">此异常将由框架转换为 404 （未找到） 错误。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="7c3ea-242">最后，添加一个方法以按类别查找产品：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="7c3ea-243">如果请求 URI 查询字符串，Web API 将尝试匹配的控制器方法参数的查询参数。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="7c3ea-244">因此，在窗体的 URI"api/产品？ 类别 =*类别*"将映射到此方法。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="7c3ea-245">创建资源</span><span class="sxs-lookup"><span data-stu-id="7c3ea-245">Creating a Resource</span></span>

<span data-ttu-id="7c3ea-246">接下来，我们将添加到一个方法`ProductsController`类，以创建新产品。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="7c3ea-247">下面是该方法的简单实现：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="7c3ea-248">请注意有关此方法的以下两项操作：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-248">Note two things about this method:</span></span>

- <span data-ttu-id="7c3ea-249">方法名称开头&quot;Post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="7c3ea-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="7c3ea-250">若要创建新的产品，客户端发送的 HTTP POST 请求。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="7c3ea-251">该方法采用类型产品的参数。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="7c3ea-252">在 Web API 中，复杂类型的参数反序列化从请求正文。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="7c3ea-253">因此，我们期望客户端以 XML 或 JSON 格式发送产品对象，序列化表示形式。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="7c3ea-254">此实现将正常工作，但很不完整。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="7c3ea-255">理想情况下，我们想要的 HTTP 响应包括以下各项：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="7c3ea-256">**响应代码：**默认情况下，Web API 框架将响应状态代码设置为 200 （正常）。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="7c3ea-257">但根据 HTTP/1.1 协议，POST 请求导致创建资源时服务器应回复状态 201 （已创建）。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="7c3ea-258">**位置：**时服务器创建的资源，它应在响应的位置标头中包含新资源的 URI。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="7c3ea-259">ASP.NET Web API，使得容易操作的 HTTP 响应消息。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="7c3ea-260">下面是改进的实现：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="7c3ea-261">请注意，方法的返回类型是现在**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="7c3ea-262">通过返回**HttpResponseMessage**而不是一种产品，我们可以控制的 HTTP 响应消息，包括状态代码和位置标头的详细信息。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="7c3ea-263">**CreateResponse**方法创建**HttpResponseMessage**并自动将写入正文的序列化的对象的表示形式产品 fo 响应消息。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="7c3ea-264">此示例不会验证`Product`。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="7c3ea-265">有关模型验证的信息，请参阅[ASP.NET Web API 中的模型验证](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="7c3ea-266">更新资源</span><span class="sxs-lookup"><span data-stu-id="7c3ea-266">Updating a Resource</span></span>

<span data-ttu-id="7c3ea-267">使用 PUT 更新产品非常简单：</span><span class="sxs-lookup"><span data-stu-id="7c3ea-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="7c3ea-268">方法名称开头&quot;放...&quot;，因此 Web API 将与其匹配的 PUT 请求。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="7c3ea-269">该方法采用两个参数，产品 ID 和更新的产品。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="7c3ea-270">*Id*参数则来自 URI 路径中，与*产品*从请求正文反序列化参数。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="7c3ea-271">默认情况下，ASP.NET Web API 框架将路由从简单的参数类型和请求正文中的复杂类型。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="7c3ea-272">删除资源</span><span class="sxs-lookup"><span data-stu-id="7c3ea-272">Deleting a Resource</span></span>

<span data-ttu-id="7c3ea-273">若要删除 resourse，定义"删除..."方法。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="7c3ea-274">如果 DELETE 请求成功，它可以准确地描述状态; 实体正文返回状态 200 （正常）状态为 202 （已接受） 如果删除仍然挂起;或状态 204 （无内容） 的任何实体正文。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="7c3ea-275">在这种情况下，`DeleteProduct`方法具有`void`返回类型，因此 ASP.NET Web API 会自动将此状态代码 204 （无内容）。</span><span class="sxs-lookup"><span data-stu-id="7c3ea-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>