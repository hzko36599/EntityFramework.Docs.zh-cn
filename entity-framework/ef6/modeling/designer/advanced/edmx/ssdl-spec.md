---
title: SSDL 规范-EF6
description: 实体框架6中的 SSDL 规范
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/advanced/edmx/ssdl-spec
ms.openlocfilehash: 33052967bb543048fc0885957cf1f37abfac9cd2
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066299"
---
# <a name="ssdl-specification"></a><span data-ttu-id="c688f-103">SSDL 规范</span><span class="sxs-lookup"><span data-stu-id="c688f-103">SSDL Specification</span></span>
<span data-ttu-id="c688f-104">存储架构定义语言 (SSDL) 是一种基于 XML 的语言，用于描述实体框架应用程序的存储模型。</span><span class="sxs-lookup"><span data-stu-id="c688f-104">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="c688f-105">在实体框架应用程序中，将使用在 StoreItemCollection 中) 编写的 ssdl 文件 (将存储模型元数据加载到，并可通过使用 system.exception 类中的方法来访问该实例。</span><span class="sxs-lookup"><span data-stu-id="c688f-105">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="c688f-106">实体框架使用存储模型元数据将针对概念模型的查询转换为特定于存储的命令。</span><span class="sxs-lookup"><span data-stu-id="c688f-106">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="c688f-107">Entity Framework Designer (EF 设计器) 在设计时将存储模型信息存储在 .edmx 文件中。</span><span class="sxs-lookup"><span data-stu-id="c688f-107">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="c688f-108">在生成时，Entity Designer 使用 .edmx 文件中的信息来创建运行时实体框架所需的 ssdl 文件。</span><span class="sxs-lookup"><span data-stu-id="c688f-108">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="c688f-109">SSDL 的版本按 XML 命名空间进行区分。</span><span class="sxs-lookup"><span data-stu-id="c688f-109">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="c688f-110">SSDL 版本</span><span class="sxs-lookup"><span data-stu-id="c688f-110">SSDL Version</span></span> | <span data-ttu-id="c688f-111">XML 命名空间</span><span class="sxs-lookup"><span data-stu-id="c688f-111">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="c688f-112">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="c688f-112">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="c688f-113">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="c688f-113">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="c688f-114">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="c688f-114">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="c688f-115">Association 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-115">Association Element (SSDL)</span></span>

<span data-ttu-id="c688f-116">存储架构定义语言 (SSDL) 中的 **Association** 元素指定参与基础数据库中外键约束的表列。</span><span class="sxs-lookup"><span data-stu-id="c688f-116">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="c688f-117">两个必需的子 End 元素指定位于关联两端的表和各端的重数。</span><span class="sxs-lookup"><span data-stu-id="c688f-117">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="c688f-118">一个可选的 ReferentialConstraint 元素指定关联的主体端和依赖端以及参与列。</span><span class="sxs-lookup"><span data-stu-id="c688f-118">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="c688f-119">如果不存在 **ReferentialConstraint** 元素，则必须使用 AssociationSetMapping 元素来指定关联的列映射。</span><span class="sxs-lookup"><span data-stu-id="c688f-119">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="c688f-120">**Association**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-120">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-121">Documentation（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-121">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c688f-122">End（正好两个）</span><span class="sxs-lookup"><span data-stu-id="c688f-122">End (exactly two)</span></span>
-   <span data-ttu-id="c688f-123">ReferentialConstraint（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-123">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="c688f-124">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-124">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-125">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-125">Applicable Attributes</span></span>

<span data-ttu-id="c688f-126">下表介绍可应用于 **Association** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-126">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="c688f-127">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-127">Attribute Name</span></span> | <span data-ttu-id="c688f-128">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-128">Is Required</span></span> | <span data-ttu-id="c688f-129">值</span><span class="sxs-lookup"><span data-stu-id="c688f-129">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-130">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-130">**Name**</span></span>       | <span data-ttu-id="c688f-131">是</span><span class="sxs-lookup"><span data-stu-id="c688f-131">Yes</span></span>         | <span data-ttu-id="c688f-132">基础数据库中相应外键约束的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-132">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-133">可以将任意数量的批注属性 (自定义 XML 特性) 应用于 **Association** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-133">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="c688f-134">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-134">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-135">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-135">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-136">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-136">Example</span></span>

<span data-ttu-id="c688f-137">下面的示例演示一个 **Association** 元素，该元素使用 **ReferentialConstraint** 元素指定参与 **FK \_ CustomerOrders** 外键约束的列：</span><span class="sxs-lookup"><span data-stu-id="c688f-137">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="c688f-138">AssociationSet 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-138">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="c688f-139">存储架构定义语言 (SSDL) 中的 **AssociationSet** 元素表示基础数据库中两个表之间的外键约束。</span><span class="sxs-lookup"><span data-stu-id="c688f-139">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="c688f-140">参与外键约束的表列在 Association 元素中指定。</span><span class="sxs-lookup"><span data-stu-id="c688f-140">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="c688f-141">与给定**associationset**元素对应的**association**元素是在**AssociationSet**元素的**association**特性中指定的。</span><span class="sxs-lookup"><span data-stu-id="c688f-141">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="c688f-142">SSDL 关联集通过 AssociationSetMapping 元素映射到 CSDL 关联集。</span><span class="sxs-lookup"><span data-stu-id="c688f-142">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="c688f-143">但是，如果使用 ReferentialConstraint 元素定义给定 CSDL 关联集的 CSDL 关联，则不需要相应的 **AssociationSetMapping** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-143">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="c688f-144">在这种情况下，如果存在 **AssociationSetMapping** 元素，该元素定义的映射将被 **ReferentialConstraint** 元素重写。</span><span class="sxs-lookup"><span data-stu-id="c688f-144">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="c688f-145">**AssociationSet**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-145">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-146">Documentation（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-146">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c688f-147">End（零个或两个）</span><span class="sxs-lookup"><span data-stu-id="c688f-147">End (zero or two)</span></span>
-   <span data-ttu-id="c688f-148">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-148">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-149">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-149">Applicable Attributes</span></span>

<span data-ttu-id="c688f-150">下表介绍可应用于 **AssociationSet** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-150">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="c688f-151">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-151">Attribute Name</span></span>  | <span data-ttu-id="c688f-152">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-152">Is Required</span></span> | <span data-ttu-id="c688f-153">值</span><span class="sxs-lookup"><span data-stu-id="c688f-153">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-154">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-154">**Name**</span></span>        | <span data-ttu-id="c688f-155">是</span><span class="sxs-lookup"><span data-stu-id="c688f-155">Yes</span></span>         | <span data-ttu-id="c688f-156">关联集表示的外键约束的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-156">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="c688f-157">**关联**</span><span class="sxs-lookup"><span data-stu-id="c688f-157">**Association**</span></span> | <span data-ttu-id="c688f-158">是</span><span class="sxs-lookup"><span data-stu-id="c688f-158">Yes</span></span>         | <span data-ttu-id="c688f-159">定义参与外键约束的列的关联的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-159">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-160">任意数量的批注属性 (自定义 XML 特性) 可以应用于 **AssociationSet** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-160">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="c688f-161">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-161">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-162">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-162">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-163">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-163">Example</span></span>

<span data-ttu-id="c688f-164">下面的示例演示了一个 **AssociationSet** 元素，该元素表示 `FK_CustomerOrders` 基础数据库中的外键约束：</span><span class="sxs-lookup"><span data-stu-id="c688f-164">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="c688f-165">CollectionType 元素 (SSDL) </span><span class="sxs-lookup"><span data-stu-id="c688f-165">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="c688f-166">存储架构定义语言 (SSDL) 中的 **CollectionType** 元素指定函数的返回类型是集合。</span><span class="sxs-lookup"><span data-stu-id="c688f-166">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="c688f-167">**CollectionType**元素是 ReturnType 元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-167">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="c688f-168">集合的类型是通过使用 RowType 子元素指定的：</span><span class="sxs-lookup"><span data-stu-id="c688f-168">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="c688f-169">可以将任意数量的批注属性 (自定义 XML 特性) 应用于 **CollectionType** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-169">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="c688f-170">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-170">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-171">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-171">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-172">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-172">Example</span></span>

<span data-ttu-id="c688f-173">下面的示例演示一个函数，该函数使用 **CollectionType** 元素来指定该函数返回行的集合。</span><span class="sxs-lookup"><span data-stu-id="c688f-173">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="c688f-174">CommandText 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-174">CommandText Element (SSDL)</span></span>

<span data-ttu-id="c688f-175">存储架构定义语言 (SSDL) 中的 **CommandText** 元素是 Function 元素的子元素，它允许您定义在数据库中执行的 SQL 语句。</span><span class="sxs-lookup"><span data-stu-id="c688f-175">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="c688f-176">**CommandText**元素允许添加类似于数据库中的存储过程的功能，但在存储模型中定义**CommandText**元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-176">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="c688f-177">**CommandText**元素不能有子元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-177">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="c688f-178">**CommandText**元素的正文必须是基础数据库的有效 SQL 语句。</span><span class="sxs-lookup"><span data-stu-id="c688f-178">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="c688f-179">无特性适用于 **CommandText** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-179">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-180">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-180">Example</span></span>

<span data-ttu-id="c688f-181">下面的示例演示具有子**CommandText**元素的**函数**元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-181">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="c688f-182">通过将 **UpdateProductInOrder** 函数导入到概念模型中，将其作为 ObjectContext 的方法公开。</span><span class="sxs-lookup"><span data-stu-id="c688f-182">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="c688f-183">DefiningQuery 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-183">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="c688f-184">使用存储架构定义语言 (SSDL) 中的 **DefiningQuery** 元素，你可以直接在基础数据库中执行 SQL 语句。</span><span class="sxs-lookup"><span data-stu-id="c688f-184">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="c688f-185">**DefiningQuery**元素通常用于数据库视图，但该视图是在存储模型中定义的，而不是在数据库中定义的。</span><span class="sxs-lookup"><span data-stu-id="c688f-185">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="c688f-186">在 **DefiningQuery** 元素中定义的视图可以通过 EntitySetMapping 元素映射到概念模型中的实体类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-186">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="c688f-187">这些映射是只读的。</span><span class="sxs-lookup"><span data-stu-id="c688f-187">These mappings are read-only.</span></span>  

<span data-ttu-id="c688f-188">下面的 SSDL 语法显示了 **EntitySet** 的声明，后跟包含用于检索视图的查询的 **DefiningQuery** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-188">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="c688f-189">您可以使用实体框架中的存储过程来对视图启用读写方案。</span><span class="sxs-lookup"><span data-stu-id="c688f-189">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="c688f-190">您可以使用数据源视图或实体 SQL 视图作为基表来检索数据和存储过程的更改处理。</span><span class="sxs-lookup"><span data-stu-id="c688f-190"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="c688f-191">你可以使用 **DefiningQuery** 元素将 Microsoft SQL Server Compact 为3.5。</span><span class="sxs-lookup"><span data-stu-id="c688f-191">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="c688f-192">尽管 SQL Server Compact 3.5 不支持存储过程，但你可以通过 **DefiningQuery** 元素实现类似功能。</span><span class="sxs-lookup"><span data-stu-id="c688f-192">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="c688f-193">另一个有用之处在于，可以创建存储过程以克服编程语言中使用的数据类型与数据源的数据类型的不匹配。</span><span class="sxs-lookup"><span data-stu-id="c688f-193">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="c688f-194">您可以编写一个使用一组特定参数的 **DefiningQuery** ，然后使用一组不同的参数调用存储过程，例如，删除数据的存储过程。</span><span class="sxs-lookup"><span data-stu-id="c688f-194">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="c688f-195">Dependent 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-195">Dependent Element (SSDL)</span></span>

<span data-ttu-id="c688f-196">存储架构定义语言 (SSDL) 中的 **依赖** 元素是 ReferentialConstraint 元素的子元素，用于定义外键约束 (也称为引用约束) 的依赖端。</span><span class="sxs-lookup"><span data-stu-id="c688f-196">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="c688f-197">**Dependent**元素指定表中)  (或列的列，这些列引用主键列 (或列) 。</span><span class="sxs-lookup"><span data-stu-id="c688f-197">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="c688f-198">**PropertyRef** 元素指定要引用的列。</span><span class="sxs-lookup"><span data-stu-id="c688f-198">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="c688f-199">主体元素指定 **依赖** 元素中指定的列所引用的主键列。</span><span class="sxs-lookup"><span data-stu-id="c688f-199">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="c688f-200">**依赖**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-200">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-201">PropertyRef（一个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-201">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="c688f-202">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-202">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-203">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-203">Applicable Attributes</span></span>

<span data-ttu-id="c688f-204">下表介绍可应用于 **依赖** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-204">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="c688f-205">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-205">Attribute Name</span></span> | <span data-ttu-id="c688f-206">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-206">Is Required</span></span> | <span data-ttu-id="c688f-207">值</span><span class="sxs-lookup"><span data-stu-id="c688f-207">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-208">**角色**</span><span class="sxs-lookup"><span data-stu-id="c688f-208">**Role**</span></span>       | <span data-ttu-id="c688f-209">是</span><span class="sxs-lookup"><span data-stu-id="c688f-209">Yes</span></span>         | <span data-ttu-id="c688f-210">与 **Role** 特性相同的值 (如果使用) 对应的结束元素，则为; 否则为。否则为包含引用列的表的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-210">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-211">可以将任意数量的批注属性 (自定义 XML 特性) 应用于 **依赖** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-211">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="c688f-212">然而，自定义特性可能不属于为 CSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-212">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c688f-213">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-213">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-214">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-214">Example</span></span>

<span data-ttu-id="c688f-215">下面的示例演示一个 Association 元素，该元素使用 **ReferentialConstraint** 元素指定参与 **FK \_ CustomerOrders** 外键约束的列。</span><span class="sxs-lookup"><span data-stu-id="c688f-215">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="c688f-216">**Dependent**元素将**Order**表的**CustomerId**列指定为约束的依赖端。</span><span class="sxs-lookup"><span data-stu-id="c688f-216">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="c688f-217">Documentation 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-217">Documentation Element (SSDL)</span></span>

<span data-ttu-id="c688f-218">存储架构定义语言 (SSDL) 中的 **文档** 元素可用于提供有关在父元素中定义的对象的信息。</span><span class="sxs-lookup"><span data-stu-id="c688f-218">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="c688f-219">**文档**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-219">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-220">**摘要**：父元素的简要说明。</span><span class="sxs-lookup"><span data-stu-id="c688f-220">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="c688f-221">（零个或一个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-221">(zero or one element)</span></span>
-   <span data-ttu-id="c688f-222">**LongDescription**：父元素的详细说明。</span><span class="sxs-lookup"><span data-stu-id="c688f-222">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="c688f-223">（零个或一个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-223">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-224">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-224">Applicable Attributes</span></span>

<span data-ttu-id="c688f-225">可以将任意数量的批注属性 (自定义 XML 特性) 可以应用于 **文档** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-225">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="c688f-226">然而，自定义特性可能不属于为 CSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-226">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c688f-227">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-227">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-228">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-228">Example</span></span>

<span data-ttu-id="c688f-229">下面的示例将 **文档** 元素显示为 EntityType 元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-229">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="c688f-230">End 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-230">End Element (SSDL)</span></span>

<span data-ttu-id="c688f-231">存储架构定义语言中的 **结束** 元素 (SSDL) 指定基础数据库中外键约束一端的表和行数。</span><span class="sxs-lookup"><span data-stu-id="c688f-231">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="c688f-232">**End**元素可以是 Association 元素或 AssociationSet 元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-232">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="c688f-233">在每种情况下，可能的子元素以及适用的特性都不同。</span><span class="sxs-lookup"><span data-stu-id="c688f-233">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="c688f-234">End 元素作为 Association 元素的子元素</span><span class="sxs-lookup"><span data-stu-id="c688f-234">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="c688f-235">**End**元素 (作为**Association**元素的子元素) 使用**Type**和**多重性**特性指定 foreign key 约束末尾的表和行数。</span><span class="sxs-lookup"><span data-stu-id="c688f-235">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="c688f-236">外键约束的两端定义为 SSDL 关联的一部分；SSDL 关联必须仅具有两端。</span><span class="sxs-lookup"><span data-stu-id="c688f-236">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="c688f-237">**End**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-237">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-238">Documentation（零个或一个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-238">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="c688f-239">OnDelete（零个或一个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-239">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="c688f-240">批注元素（零个或多个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-240">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="c688f-241">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-241">Applicable Attributes</span></span>

<span data-ttu-id="c688f-242">下表描述了当 **结束** 元素是 **Association** 元素的子元素时，可应用于该元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-242">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="c688f-243">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-243">Attribute Name</span></span>   | <span data-ttu-id="c688f-244">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-244">Is Required</span></span> | <span data-ttu-id="c688f-245">值</span><span class="sxs-lookup"><span data-stu-id="c688f-245">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-246">类型</span><span class="sxs-lookup"><span data-stu-id="c688f-246">**Type**</span></span>         | <span data-ttu-id="c688f-247">是</span><span class="sxs-lookup"><span data-stu-id="c688f-247">Yes</span></span>         | <span data-ttu-id="c688f-248">处于外键约束端的 SSDL 实体集的完全限定名。</span><span class="sxs-lookup"><span data-stu-id="c688f-248">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="c688f-249">**角色**</span><span class="sxs-lookup"><span data-stu-id="c688f-249">**Role**</span></span>         | <span data-ttu-id="c688f-250">否</span><span class="sxs-lookup"><span data-stu-id="c688f-250">No</span></span>          | <span data-ttu-id="c688f-251">如果) 使用，则为相应 ReferentialConstraint (元素的主体或从属元素中的 **Role** 特性的值。</span><span class="sxs-lookup"><span data-stu-id="c688f-251">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="c688f-252">**多重性**</span><span class="sxs-lookup"><span data-stu-id="c688f-252">**Multiplicity**</span></span> | <span data-ttu-id="c688f-253">是</span><span class="sxs-lookup"><span data-stu-id="c688f-253">Yes</span></span>         | <span data-ttu-id="c688f-254">**1**、 **0 或 1**或， **\*** 具体取决于外键约束末尾的行数。</span><span class="sxs-lookup"><span data-stu-id="c688f-254">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="c688f-255">**1** 指示在外键约束端刚好存在一行。</span><span class="sxs-lookup"><span data-stu-id="c688f-255">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="c688f-256">**0 .0** 表示外键约束端处存在零个或一行。</span><span class="sxs-lookup"><span data-stu-id="c688f-256">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="c688f-257">**\*** 指示外键约束端处存在零行、一行或多行。</span><span class="sxs-lookup"><span data-stu-id="c688f-257">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-258">可以将任意数量的批注属性 (自定义 XML 特性) 应用于 **结束** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-258">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="c688f-259">然而，自定义特性可能不属于为 CSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-259">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c688f-260">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-260">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="c688f-261">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-261">Example</span></span>

<span data-ttu-id="c688f-262">下面的示例演示了一个定义**FK \_ CustomerOrders**外键约束的**Association**元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-262">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="c688f-263">在每个**End**元素上指定的**多重性**值指示**Orders**表中的多个行可以与**Customers**表中的某一行相关联，但**customers**表中只有一个行可以与**Orders**表中的一行相关联。</span><span class="sxs-lookup"><span data-stu-id="c688f-263">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="c688f-264">此外， **OnDelete**元素指示如果删除**customers**表中的行，则将删除**Orders**表中引用**customers**表中的特定行的所有行。</span><span class="sxs-lookup"><span data-stu-id="c688f-264">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="c688f-265">End 元素作为 AssociationSet 元素的子元素</span><span class="sxs-lookup"><span data-stu-id="c688f-265">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="c688f-266">**End**元素 (为**AssociationSet**元素的子元素) 在基础数据库中的外键约束的一端指定一个表。</span><span class="sxs-lookup"><span data-stu-id="c688f-266">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="c688f-267">**End**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-267">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-268">Documentation（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-268">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c688f-269">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-269">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="c688f-270">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-270">Applicable Attributes</span></span>

<span data-ttu-id="c688f-271">下表描述了当 **结束** 元素为 **AssociationSet** 元素的子元素时，可应用于该元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-271">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="c688f-272">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-272">Attribute Name</span></span> | <span data-ttu-id="c688f-273">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-273">Is Required</span></span> | <span data-ttu-id="c688f-274">值</span><span class="sxs-lookup"><span data-stu-id="c688f-274">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-275">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="c688f-275">**EntitySet**</span></span>  | <span data-ttu-id="c688f-276">是</span><span class="sxs-lookup"><span data-stu-id="c688f-276">Yes</span></span>         | <span data-ttu-id="c688f-277">处于外键约束一端的 SSDL 实体集的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-277">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="c688f-278">**角色**</span><span class="sxs-lookup"><span data-stu-id="c688f-278">**Role**</span></span>       | <span data-ttu-id="c688f-279">否</span><span class="sxs-lookup"><span data-stu-id="c688f-279">No</span></span>          | <span data-ttu-id="c688f-280">在对应的 Association 元素的一个**End**元素上指定的一个**角色**特性的值。</span><span class="sxs-lookup"><span data-stu-id="c688f-280">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-281">可以将任意数量的批注属性 (自定义 XML 特性) 应用于 **结束** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-281">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="c688f-282">然而，自定义特性可能不属于为 CSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-282">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c688f-283">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-283">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="c688f-284">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-284">Example</span></span>

<span data-ttu-id="c688f-285">下面的示例演示一个具有**AssociationSet**元素的**EntityContainer**元素，其中包含两个**End**元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-285">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="c688f-286">EntityContainer 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-286">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="c688f-287">存储架构定义语言中的 **EntityContainer** 元素 (SSDL) 描述实体框架应用程序中的基础数据源的结构：在 EntitySet 元素中定义的 ssdl 实体 (集) 表示数据库中的表、在 EntityType 元素中定义的 ssdl 实体类型 (表示表中的行，以及在 AssociationSet 元素中定义的关联集) 表示数据库中的外键约束。</span><span class="sxs-lookup"><span data-stu-id="c688f-287">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="c688f-288">存储模型实体容器通过 EntityContainerMapping 元素映射到概念模型实体容器。</span><span class="sxs-lookup"><span data-stu-id="c688f-288">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="c688f-289">**EntityContainer**元素可以有零个或一个文档元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-289">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="c688f-290">如果存在 **文档** 元素，则它必须位于所有其他子元素之前。</span><span class="sxs-lookup"><span data-stu-id="c688f-290">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="c688f-291">**EntityContainer**元素可以具有零个或多个下列子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-291">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-292">EntitySet</span><span class="sxs-lookup"><span data-stu-id="c688f-292">EntitySet</span></span>
-   <span data-ttu-id="c688f-293">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="c688f-293">AssociationSet</span></span>
-   <span data-ttu-id="c688f-294">批注元素</span><span class="sxs-lookup"><span data-stu-id="c688f-294">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-295">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-295">Applicable Attributes</span></span>

<span data-ttu-id="c688f-296">下表介绍可应用于 **EntityContainer** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-296">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="c688f-297">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-297">Attribute Name</span></span> | <span data-ttu-id="c688f-298">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-298">Is Required</span></span> | <span data-ttu-id="c688f-299">值</span><span class="sxs-lookup"><span data-stu-id="c688f-299">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="c688f-300">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-300">**Name**</span></span>       | <span data-ttu-id="c688f-301">是</span><span class="sxs-lookup"><span data-stu-id="c688f-301">Yes</span></span>         | <span data-ttu-id="c688f-302">实体容器的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-302">The name of the entity container.</span></span> <span data-ttu-id="c688f-303">此名称不能包含句点 (.)。</span><span class="sxs-lookup"><span data-stu-id="c688f-303">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-304">可以将任意数量的批注属性 (自定义 XML 特性) 可以应用于 **EntityContainer** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-304">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="c688f-305">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-305">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-306">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-306">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-307">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-307">Example</span></span>

<span data-ttu-id="c688f-308">下面的示例演示一个 **EntityContainer** 元素，该元素定义了两个实体集和一个关联集。</span><span class="sxs-lookup"><span data-stu-id="c688f-308">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="c688f-309">注意，实体类型和关联类型名称由概念模型命名空间名称限定。</span><span class="sxs-lookup"><span data-stu-id="c688f-309">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="c688f-310">EntitySet 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-310">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="c688f-311">存储架构定义语言 (SSDL) 中的 **EntitySet** 元素表示基础数据库中的表或视图。</span><span class="sxs-lookup"><span data-stu-id="c688f-311">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="c688f-312">以 SSDL 表示的 EntityType 元素表示表或视图中的行。</span><span class="sxs-lookup"><span data-stu-id="c688f-312">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="c688f-313">**EntitySet**元素的**EntityType**特性指定了表示 ssdl 实体集中的行的特定 SSDL 实体类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-313">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="c688f-314">CSDL 实体集和 SSDL 实体集之间的映射在 EntitySetMapping 元素中指定。</span><span class="sxs-lookup"><span data-stu-id="c688f-314">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="c688f-315">**EntitySet**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-315">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-316">Documentation（零个或一个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-316">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="c688f-317">DefiningQuery（零个或一个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-317">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="c688f-318">批注元素</span><span class="sxs-lookup"><span data-stu-id="c688f-318">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-319">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-319">Applicable Attributes</span></span>

<span data-ttu-id="c688f-320">下表介绍可应用于 **EntitySet** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-320">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="c688f-321">此处未列出 (一些属性) 可以用 **存储** 别名进行限定。</span><span class="sxs-lookup"><span data-stu-id="c688f-321">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="c688f-322">在更新模型时，模型更新向导会使用这些特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-322">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="c688f-323">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-323">Attribute Name</span></span> | <span data-ttu-id="c688f-324">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-324">Is Required</span></span> | <span data-ttu-id="c688f-325">值</span><span class="sxs-lookup"><span data-stu-id="c688f-325">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-326">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-326">**Name**</span></span>       | <span data-ttu-id="c688f-327">是</span><span class="sxs-lookup"><span data-stu-id="c688f-327">Yes</span></span>         | <span data-ttu-id="c688f-328">实体集的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-328">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="c688f-329">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="c688f-329">**EntityType**</span></span> | <span data-ttu-id="c688f-330">是</span><span class="sxs-lookup"><span data-stu-id="c688f-330">Yes</span></span>         | <span data-ttu-id="c688f-331">实体集包含其实例的实体类型的完全限定名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-331">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="c688f-332">**架构**</span><span class="sxs-lookup"><span data-stu-id="c688f-332">**Schema**</span></span>     | <span data-ttu-id="c688f-333">否</span><span class="sxs-lookup"><span data-stu-id="c688f-333">No</span></span>          | <span data-ttu-id="c688f-334">数据库架构。</span><span class="sxs-lookup"><span data-stu-id="c688f-334">The database schema.</span></span>                                                                     |
| <span data-ttu-id="c688f-335">**表**</span><span class="sxs-lookup"><span data-stu-id="c688f-335">**Table**</span></span>      | <span data-ttu-id="c688f-336">否</span><span class="sxs-lookup"><span data-stu-id="c688f-336">No</span></span>          | <span data-ttu-id="c688f-337">数据库表。</span><span class="sxs-lookup"><span data-stu-id="c688f-337">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="c688f-338">可以将任意数量的批注属性 (自定义 XML 特性) 应用于 **EntitySet** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-338">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="c688f-339">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-339">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-340">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-340">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-341">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-341">Example</span></span>

<span data-ttu-id="c688f-342">下面的示例演示一个具有两个**EntitySet**元素和一个**AssociationSet**元素的**EntityContainer**元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-342">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="c688f-343">EntityType 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-343">EntityType Element (SSDL)</span></span>

<span data-ttu-id="c688f-344">存储架构定义语言 (SSDL) 中的 **EntityType** 元素表示基础数据库的表或视图中的行。</span><span class="sxs-lookup"><span data-stu-id="c688f-344">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="c688f-345">以 SSDL 表示的 EntitySet 元素表示行所在的表或视图。</span><span class="sxs-lookup"><span data-stu-id="c688f-345">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="c688f-346">**EntitySet**元素的**EntityType**特性指定了表示 ssdl 实体集中的行的特定 SSDL 实体类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-346">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="c688f-347">SSDL 实体类型与 CSDL 实体类型之间的映射在 EntityTypeMapping 元素中指定。</span><span class="sxs-lookup"><span data-stu-id="c688f-347">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="c688f-348">**EntityType**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-348">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-349">Documentation（零个或一个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-349">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="c688f-350">Key（零个或一个元素）</span><span class="sxs-lookup"><span data-stu-id="c688f-350">Key (zero or one element)</span></span>
-   <span data-ttu-id="c688f-351">批注元素</span><span class="sxs-lookup"><span data-stu-id="c688f-351">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-352">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-352">Applicable Attributes</span></span>

<span data-ttu-id="c688f-353">下表介绍可应用于 **EntityType** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-353">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="c688f-354">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-354">Attribute Name</span></span> | <span data-ttu-id="c688f-355">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-355">Is Required</span></span> | <span data-ttu-id="c688f-356">值</span><span class="sxs-lookup"><span data-stu-id="c688f-356">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-357">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-357">**Name**</span></span>       | <span data-ttu-id="c688f-358">是</span><span class="sxs-lookup"><span data-stu-id="c688f-358">Yes</span></span>         | <span data-ttu-id="c688f-359">实体类型的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-359">The name of the entity type.</span></span> <span data-ttu-id="c688f-360">此值通常与以实体类型表示行的表的名称相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-360">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="c688f-361">此值可以不包含句点 (.)。</span><span class="sxs-lookup"><span data-stu-id="c688f-361">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-362">可以将任意数量的批注属性 (自定义 XML 特性) 应用于 **EntityType** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-362">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="c688f-363">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-363">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-364">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-364">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-365">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-365">Example</span></span>

<span data-ttu-id="c688f-366">下面的示例演示具有两个属性的 **EntityType** 元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-366">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="c688f-367">Function 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-367">Function Element (SSDL)</span></span>

<span data-ttu-id="c688f-368">存储架构定义语言 (SSDL) 中的 **Function** 元素指定基础数据库中存在的存储过程。</span><span class="sxs-lookup"><span data-stu-id="c688f-368">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="c688f-369">**函数**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-369">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-370">Documentation（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-370">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c688f-371">参数 (零个或多个) </span><span class="sxs-lookup"><span data-stu-id="c688f-371">Parameter (zero or more)</span></span>
-   <span data-ttu-id="c688f-372">CommandText (零个或一个) </span><span class="sxs-lookup"><span data-stu-id="c688f-372">CommandText (zero or one)</span></span>
-   <span data-ttu-id="c688f-373">ReturnType (零个或多个) </span><span class="sxs-lookup"><span data-stu-id="c688f-373">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="c688f-374">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-374">Annotation elements (zero or more)</span></span>

<span data-ttu-id="c688f-375">函数的返回类型必须与 **returntype** 元素或 **returntype** 属性一起指定 (参见下面) ，但不能同时指定两者。</span><span class="sxs-lookup"><span data-stu-id="c688f-375">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="c688f-376">可以将在存储模型中指定的存储过程导入应用程序的概念模型。</span><span class="sxs-lookup"><span data-stu-id="c688f-376">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="c688f-377">有关详细信息，请参阅 [用存储过程查询](xref:ef6/modeling/designer/stored-procedures/query)。</span><span class="sxs-lookup"><span data-stu-id="c688f-377">For more information, see [Querying with Stored Procedures](xref:ef6/modeling/designer/stored-procedures/query).</span></span> <span data-ttu-id="c688f-378">**函数**元素还可用于在存储模型中定义自定义函数。</span><span class="sxs-lookup"><span data-stu-id="c688f-378">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-379">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-379">Applicable Attributes</span></span>

<span data-ttu-id="c688f-380">下表介绍可应用于 **Function** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-380">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="c688f-381">此处未列出 (一些属性) 可以用 **存储** 别名进行限定。</span><span class="sxs-lookup"><span data-stu-id="c688f-381">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="c688f-382">在更新模型时，模型更新向导会使用这些特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-382">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="c688f-383">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-383">Attribute Name</span></span>             | <span data-ttu-id="c688f-384">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-384">Is Required</span></span> | <span data-ttu-id="c688f-385">值</span><span class="sxs-lookup"><span data-stu-id="c688f-385">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-386">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-386">**Name**</span></span>                   | <span data-ttu-id="c688f-387">是</span><span class="sxs-lookup"><span data-stu-id="c688f-387">Yes</span></span>         | <span data-ttu-id="c688f-388">存储过程的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-388">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="c688f-389">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="c688f-389">**ReturnType**</span></span>             | <span data-ttu-id="c688f-390">否</span><span class="sxs-lookup"><span data-stu-id="c688f-390">No</span></span>          | <span data-ttu-id="c688f-391">存储过程的返回类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-391">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="c688f-392">**聚合**</span><span class="sxs-lookup"><span data-stu-id="c688f-392">**Aggregate**</span></span>              | <span data-ttu-id="c688f-393">否</span><span class="sxs-lookup"><span data-stu-id="c688f-393">No</span></span>          | <span data-ttu-id="c688f-394">如果存储过程返回聚合值，**则为 True** ;否则**为 False**。</span><span class="sxs-lookup"><span data-stu-id="c688f-394">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="c688f-395">**L**</span><span class="sxs-lookup"><span data-stu-id="c688f-395">**BuiltIn**</span></span>                | <span data-ttu-id="c688f-396">否</span><span class="sxs-lookup"><span data-stu-id="c688f-396">No</span></span>          | <span data-ttu-id="c688f-397">如果函数是内置的<sup>1</sup>函数，则为**True** ; 否则为。否则**为 False**。</span><span class="sxs-lookup"><span data-stu-id="c688f-397">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="c688f-398">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="c688f-398">**StoreFunctionName**</span></span>      | <span data-ttu-id="c688f-399">否</span><span class="sxs-lookup"><span data-stu-id="c688f-399">No</span></span>          | <span data-ttu-id="c688f-400">存储过程的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-400">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="c688f-401">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="c688f-401">**NiladicFunction**</span></span>        | <span data-ttu-id="c688f-402">否</span><span class="sxs-lookup"><span data-stu-id="c688f-402">No</span></span>          | <span data-ttu-id="c688f-403">如果函数是 niladic<sup>2</sup>函数，则为**True** ;否则**为 False** 。</span><span class="sxs-lookup"><span data-stu-id="c688f-403">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="c688f-404">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="c688f-404">**IsComposable**</span></span>           | <span data-ttu-id="c688f-405">否</span><span class="sxs-lookup"><span data-stu-id="c688f-405">No</span></span>          | <span data-ttu-id="c688f-406">如果函数是可组合的<sup>3</sup>函数，则为**True** ;否则**为 False** 。</span><span class="sxs-lookup"><span data-stu-id="c688f-406">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="c688f-407">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="c688f-407">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="c688f-408">否</span><span class="sxs-lookup"><span data-stu-id="c688f-408">No</span></span>          | <span data-ttu-id="c688f-409">定义用于解析函数重载的类型语义的枚举。</span><span class="sxs-lookup"><span data-stu-id="c688f-409">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="c688f-410">该枚举是在提供程序清单中根据函数定义来定义的。</span><span class="sxs-lookup"><span data-stu-id="c688f-410">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="c688f-411">默认值为 **AllowImplicitConversion**。</span><span class="sxs-lookup"><span data-stu-id="c688f-411">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="c688f-412">**架构**</span><span class="sxs-lookup"><span data-stu-id="c688f-412">**Schema**</span></span>                 | <span data-ttu-id="c688f-413">否</span><span class="sxs-lookup"><span data-stu-id="c688f-413">No</span></span>          | <span data-ttu-id="c688f-414">在其中定义存储过程的架构的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-414">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="c688f-415"><sup>1</sup> 内置函数是在数据库中定义的函数。</span><span class="sxs-lookup"><span data-stu-id="c688f-415"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="c688f-416">有关在存储模型中定义的函数的信息，请参阅 CommandText 元素 (SSDL) 。</span><span class="sxs-lookup"><span data-stu-id="c688f-416">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="c688f-417"><sup>2</sup> niladic 函数是不接受任何参数的函数，并且在调用时不需要括号。</span><span class="sxs-lookup"><span data-stu-id="c688f-417"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="c688f-418"><sup>3</sup> 如果一个函数的输出可以是另一个函数的输入，则这两个函数是可组合的。</span><span class="sxs-lookup"><span data-stu-id="c688f-418"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="c688f-419">可以将任意数量的批注属性 (自定义 XML 特性) 可以应用于 **Function** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-419">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="c688f-420">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-420">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-421">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-421">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-422">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-422">Example</span></span>

<span data-ttu-id="c688f-423">下面的示例显示了与**UpdateOrderQuantity**存储过程对应的**Function**元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-423">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="c688f-424">该存储过程接受两个参数，且不返回值。</span><span class="sxs-lookup"><span data-stu-id="c688f-424">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="c688f-425">Key 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-425">Key Element (SSDL)</span></span>

<span data-ttu-id="c688f-426">存储架构定义语言中 (SSDL) 的 **Key** 元素表示基础数据库中的表的主键。</span><span class="sxs-lookup"><span data-stu-id="c688f-426">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="c688f-427">**Key** 是 EntityType 元素的子元素，它表示表中的行。</span><span class="sxs-lookup"><span data-stu-id="c688f-427">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="c688f-428">通过引用**EntityType**元素中定义的一个或多个属性元素，在**key**元素中定义主键。</span><span class="sxs-lookup"><span data-stu-id="c688f-428">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="c688f-429">**Key**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-429">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-430">PropertyRef（一个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-430">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="c688f-431">批注元素</span><span class="sxs-lookup"><span data-stu-id="c688f-431">Annotation elements</span></span>

<span data-ttu-id="c688f-432">没有任何特性适用于 **该键** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-432">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-433">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-433">Example</span></span>

<span data-ttu-id="c688f-434">下面的示例演示一个具有引用一个属性的键的 **EntityType** 元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-434">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="c688f-435">OnDelete 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-435">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="c688f-436"> (SSDL) 存储架构定义语言中的 **OnDelete** 元素反映了参与外键约束的行被删除时的数据库行为。</span><span class="sxs-lookup"><span data-stu-id="c688f-436">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="c688f-437">如果将操作设置为 **Cascade**，则还将删除引用要删除的行的行。</span><span class="sxs-lookup"><span data-stu-id="c688f-437">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="c688f-438">如果将操作设置为 " **无**"，则不会同时删除引用要删除行的行。</span><span class="sxs-lookup"><span data-stu-id="c688f-438">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="c688f-439">**OnDelete**元素是结束元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-439">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="c688f-440">**OnDelete**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-440">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-441">Documentation（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-441">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c688f-442">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-442">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-443">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-443">Applicable Attributes</span></span>

<span data-ttu-id="c688f-444">下表介绍可应用于 **OnDelete** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-444">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="c688f-445">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-445">Attribute Name</span></span> | <span data-ttu-id="c688f-446">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-446">Is Required</span></span> | <span data-ttu-id="c688f-447">值</span><span class="sxs-lookup"><span data-stu-id="c688f-447">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-448">**操作**</span><span class="sxs-lookup"><span data-stu-id="c688f-448">**Action**</span></span>     | <span data-ttu-id="c688f-449">是</span><span class="sxs-lookup"><span data-stu-id="c688f-449">Yes</span></span>         | <span data-ttu-id="c688f-450">**Cascade** 或 **None**。</span><span class="sxs-lookup"><span data-stu-id="c688f-450">**Cascade** or **None**.</span></span> <span data-ttu-id="c688f-451"> (**限制** 值有效，但行为与 **None**相同。 ) </span><span class="sxs-lookup"><span data-stu-id="c688f-451">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-452">可以将任意数量的批注属性 (自定义 XML 特性) 应用到 **OnDelete** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-452">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="c688f-453">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-453">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-454">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-454">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-455">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-455">Example</span></span>

<span data-ttu-id="c688f-456">下面的示例演示了一个定义**FK \_ CustomerOrders**外键约束的**Association**元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-456">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="c688f-457">**OnDelete**元素指示在删除**customers**表中的某一行时，该**订单**表中引用**该特定**行的所有行都将被删除。</span><span class="sxs-lookup"><span data-stu-id="c688f-457">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="c688f-458">Parameter 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-458">Parameter Element (SSDL)</span></span>

<span data-ttu-id="c688f-459">存储架构定义语言 (SSDL) 中的 **Parameter** 元素是 Function 元素的子元素，用于指定数据库中存储过程的参数。</span><span class="sxs-lookup"><span data-stu-id="c688f-459">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="c688f-460">**Parameter**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-460">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-461">Documentation（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-461">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c688f-462">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-462">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-463">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-463">Applicable Attributes</span></span>

<span data-ttu-id="c688f-464">下表描述了可应用于 **参数** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-464">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="c688f-465">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-465">Attribute Name</span></span> | <span data-ttu-id="c688f-466">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-466">Is Required</span></span> | <span data-ttu-id="c688f-467">值</span><span class="sxs-lookup"><span data-stu-id="c688f-467">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-468">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-468">**Name**</span></span>       | <span data-ttu-id="c688f-469">是</span><span class="sxs-lookup"><span data-stu-id="c688f-469">Yes</span></span>         | <span data-ttu-id="c688f-470">参数的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-470">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="c688f-471">类型</span><span class="sxs-lookup"><span data-stu-id="c688f-471">**Type**</span></span>       | <span data-ttu-id="c688f-472">是</span><span class="sxs-lookup"><span data-stu-id="c688f-472">Yes</span></span>         | <span data-ttu-id="c688f-473">参数类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-473">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="c688f-474">**模式**</span><span class="sxs-lookup"><span data-stu-id="c688f-474">**Mode**</span></span>       | <span data-ttu-id="c688f-475">否</span><span class="sxs-lookup"><span data-stu-id="c688f-475">No</span></span>          | <span data-ttu-id="c688f-476">**In**、 **Out**或 **InOut** ，具体取决于参数是输入、输出还是输入/输出参数。</span><span class="sxs-lookup"><span data-stu-id="c688f-476">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="c688f-477">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="c688f-477">**MaxLength**</span></span>  | <span data-ttu-id="c688f-478">否</span><span class="sxs-lookup"><span data-stu-id="c688f-478">No</span></span>          | <span data-ttu-id="c688f-479">参数的最大长度。</span><span class="sxs-lookup"><span data-stu-id="c688f-479">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="c688f-480">**精度**</span><span class="sxs-lookup"><span data-stu-id="c688f-480">**Precision**</span></span>  | <span data-ttu-id="c688f-481">否</span><span class="sxs-lookup"><span data-stu-id="c688f-481">No</span></span>          | <span data-ttu-id="c688f-482">参数的精度。</span><span class="sxs-lookup"><span data-stu-id="c688f-482">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="c688f-483">**缩放**</span><span class="sxs-lookup"><span data-stu-id="c688f-483">**Scale**</span></span>      | <span data-ttu-id="c688f-484">否</span><span class="sxs-lookup"><span data-stu-id="c688f-484">No</span></span>          | <span data-ttu-id="c688f-485">参数的确定位数。</span><span class="sxs-lookup"><span data-stu-id="c688f-485">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="c688f-486">SRID </span><span class="sxs-lookup"><span data-stu-id="c688f-486">**SRID**</span></span>       | <span data-ttu-id="c688f-487">否</span><span class="sxs-lookup"><span data-stu-id="c688f-487">No</span></span>          | <span data-ttu-id="c688f-488">空间系统引用标识符。</span><span class="sxs-lookup"><span data-stu-id="c688f-488">Spatial System Reference Identifier.</span></span> <span data-ttu-id="c688f-489">仅对空间类型的参数有效。</span><span class="sxs-lookup"><span data-stu-id="c688f-489">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="c688f-490">有关详细信息，请参阅 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c688f-490">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-491">可以将任意数量的批注属性 (自定义 XML 特性) 可应用于 **参数** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-491">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="c688f-492">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-492">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-493">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-493">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-494">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-494">Example</span></span>

<span data-ttu-id="c688f-495">下面的示例演示一个具有两个参数元素的 **函数** 元素，这些 **参数** 元素指定输入参数：</span><span class="sxs-lookup"><span data-stu-id="c688f-495">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="c688f-496">Principal 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-496">Principal Element (SSDL)</span></span>

<span data-ttu-id="c688f-497">存储架构定义语言 (SSDL) 中的 **主体** 元素是 ReferentialConstraint 元素的子元素，用于定义外键约束 (也称为引用约束) 的主体端。</span><span class="sxs-lookup"><span data-stu-id="c688f-497">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="c688f-498">**主体**元素指定主键列 (或列) 在另一个列 (或列) 引用的表中。</span><span class="sxs-lookup"><span data-stu-id="c688f-498">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="c688f-499">**PropertyRef** 元素指定要引用的列。</span><span class="sxs-lookup"><span data-stu-id="c688f-499">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="c688f-500">Dependent 元素指定引用 **主体** 元素中指定的主键列的列。</span><span class="sxs-lookup"><span data-stu-id="c688f-500">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="c688f-501">**主体**元素可以具有以下子元素 (按列出的顺序) ：</span><span class="sxs-lookup"><span data-stu-id="c688f-501">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c688f-502">PropertyRef（一个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-502">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="c688f-503">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-503">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-504">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-504">Applicable Attributes</span></span>

<span data-ttu-id="c688f-505">下表介绍可应用于 **主体** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-505">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="c688f-506">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-506">Attribute Name</span></span> | <span data-ttu-id="c688f-507">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-507">Is Required</span></span> | <span data-ttu-id="c688f-508">值</span><span class="sxs-lookup"><span data-stu-id="c688f-508">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-509">**角色**</span><span class="sxs-lookup"><span data-stu-id="c688f-509">**Role**</span></span>       | <span data-ttu-id="c688f-510">是</span><span class="sxs-lookup"><span data-stu-id="c688f-510">Yes</span></span>         | <span data-ttu-id="c688f-511">与 **Role** 特性相同的值 (如果使用) 对应的结束元素，则为; 否则为。否则，为包含被引用列的表的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-511">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-512">可以将任意数量的批注属性 (自定义 XML 特性) 应用于 **主体** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-512">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="c688f-513">然而，自定义特性可能不属于为 CSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-513">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c688f-514">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-514">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-515">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-515">Example</span></span>

<span data-ttu-id="c688f-516">下面的示例演示一个 Association 元素，该元素使用 **ReferentialConstraint** 元素指定参与 **FK \_ CustomerOrders** 外键约束的列。</span><span class="sxs-lookup"><span data-stu-id="c688f-516">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="c688f-517">**主体**元素将**Customer**表的**CustomerId**列指定为约束的主体端。</span><span class="sxs-lookup"><span data-stu-id="c688f-517">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="c688f-518">Property 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-518">Property Element (SSDL)</span></span>

<span data-ttu-id="c688f-519">存储架构定义语言 (SSDL) 中的 **属性** 元素表示基础数据库中的表中的列。</span><span class="sxs-lookup"><span data-stu-id="c688f-519">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="c688f-520">**属性** 元素是 EntityType 元素的子元素，表示表中的行。</span><span class="sxs-lookup"><span data-stu-id="c688f-520">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="c688f-521">**EntityType**元素上定义的每个**属性**元素都表示一列。</span><span class="sxs-lookup"><span data-stu-id="c688f-521">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="c688f-522">**属性**元素不能具有任何子元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-522">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-523">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-523">Applicable Attributes</span></span>

<span data-ttu-id="c688f-524">下表介绍可应用于 **属性** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-524">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="c688f-525">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-525">Attribute Name</span></span>            | <span data-ttu-id="c688f-526">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-526">Is Required</span></span> | <span data-ttu-id="c688f-527">值</span><span class="sxs-lookup"><span data-stu-id="c688f-527">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-528">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-528">**Name**</span></span>                  | <span data-ttu-id="c688f-529">是</span><span class="sxs-lookup"><span data-stu-id="c688f-529">Yes</span></span>         | <span data-ttu-id="c688f-530">对应列的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-530">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="c688f-531">类型</span><span class="sxs-lookup"><span data-stu-id="c688f-531">**Type**</span></span>                  | <span data-ttu-id="c688f-532">是</span><span class="sxs-lookup"><span data-stu-id="c688f-532">Yes</span></span>         | <span data-ttu-id="c688f-533">对应列的类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-533">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="c688f-534">**可以为 Null**</span><span class="sxs-lookup"><span data-stu-id="c688f-534">**Nullable**</span></span>              | <span data-ttu-id="c688f-535">否</span><span class="sxs-lookup"><span data-stu-id="c688f-535">No</span></span>          | <span data-ttu-id="c688f-536"> (默认值**为 True**) 或**False** ，具体取决于对应列是否可以具有 null 值。</span><span class="sxs-lookup"><span data-stu-id="c688f-536">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="c688f-537">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="c688f-537">**DefaultValue**</span></span>          | <span data-ttu-id="c688f-538">否</span><span class="sxs-lookup"><span data-stu-id="c688f-538">No</span></span>          | <span data-ttu-id="c688f-539">对应列的默认值。</span><span class="sxs-lookup"><span data-stu-id="c688f-539">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="c688f-540">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="c688f-540">**MaxLength**</span></span>             | <span data-ttu-id="c688f-541">否</span><span class="sxs-lookup"><span data-stu-id="c688f-541">No</span></span>          | <span data-ttu-id="c688f-542">对应列的最大长度。</span><span class="sxs-lookup"><span data-stu-id="c688f-542">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="c688f-543">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="c688f-543">**FixedLength**</span></span>           | <span data-ttu-id="c688f-544">否</span><span class="sxs-lookup"><span data-stu-id="c688f-544">No</span></span>          | <span data-ttu-id="c688f-545">**True** 或 **False** ，具体取决于相应的列值是否将作为固定长度字符串存储。</span><span class="sxs-lookup"><span data-stu-id="c688f-545">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="c688f-546">**精度**</span><span class="sxs-lookup"><span data-stu-id="c688f-546">**Precision**</span></span>             | <span data-ttu-id="c688f-547">否</span><span class="sxs-lookup"><span data-stu-id="c688f-547">No</span></span>          | <span data-ttu-id="c688f-548">对应列的精度。</span><span class="sxs-lookup"><span data-stu-id="c688f-548">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="c688f-549">**缩放**</span><span class="sxs-lookup"><span data-stu-id="c688f-549">**Scale**</span></span>                 | <span data-ttu-id="c688f-550">否</span><span class="sxs-lookup"><span data-stu-id="c688f-550">No</span></span>          | <span data-ttu-id="c688f-551">对应列的小数位数。</span><span class="sxs-lookup"><span data-stu-id="c688f-551">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="c688f-552">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="c688f-552">**Unicode**</span></span>               | <span data-ttu-id="c688f-553">否</span><span class="sxs-lookup"><span data-stu-id="c688f-553">No</span></span>          | <span data-ttu-id="c688f-554">**True** 或 **False** ，具体取决于对应列值是否将存储为 Unicode 字符串。</span><span class="sxs-lookup"><span data-stu-id="c688f-554">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="c688f-555">**排序规则**</span><span class="sxs-lookup"><span data-stu-id="c688f-555">**Collation**</span></span>             | <span data-ttu-id="c688f-556">否</span><span class="sxs-lookup"><span data-stu-id="c688f-556">No</span></span>          | <span data-ttu-id="c688f-557">指定要在数据源中使用的排序的字符串。</span><span class="sxs-lookup"><span data-stu-id="c688f-557">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="c688f-558">SRID </span><span class="sxs-lookup"><span data-stu-id="c688f-558">**SRID**</span></span>                  | <span data-ttu-id="c688f-559">否</span><span class="sxs-lookup"><span data-stu-id="c688f-559">No</span></span>          | <span data-ttu-id="c688f-560">空间系统引用标识符。</span><span class="sxs-lookup"><span data-stu-id="c688f-560">Spatial System Reference Identifier.</span></span> <span data-ttu-id="c688f-561">仅对空间类型的属性有效。</span><span class="sxs-lookup"><span data-stu-id="c688f-561">Valid only for properties of spatial types.</span></span> <span data-ttu-id="c688f-562">有关详细信息，请参阅 [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c688f-562">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="c688f-563">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="c688f-563">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="c688f-564">否</span><span class="sxs-lookup"><span data-stu-id="c688f-564">No</span></span>          | <span data-ttu-id="c688f-565">**无**， **标识** (如果相应的列值是在数据库) 中生成的标识，则为; 如果在数据库) 中计算相应列值，则为 **计算** (。</span><span class="sxs-lookup"><span data-stu-id="c688f-565">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="c688f-566">对于 RowType 属性无效。</span><span class="sxs-lookup"><span data-stu-id="c688f-566">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-567">可以将任意数量的批注属性 (自定义 XML 特性) 可以应用于 **Property** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-567">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="c688f-568">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-568">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-569">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-569">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-570">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-570">Example</span></span>

<span data-ttu-id="c688f-571">下面的示例演示一个具有两个子**属性**元素的**EntityType**元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-571">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="c688f-572">PropertyRef 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-572">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="c688f-573">存储架构定义语言 (SSDL) 中的 **PropertyRef** 元素引用 EntityType 元素上定义的属性，以指示该属性将执行以下角色之一：</span><span class="sxs-lookup"><span data-stu-id="c688f-573">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="c688f-574">是 **EntityType** 表示的表的主键的一部分。</span><span class="sxs-lookup"><span data-stu-id="c688f-574">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="c688f-575">一个或多个 **PropertyRef** 元素可用于定义主键。</span><span class="sxs-lookup"><span data-stu-id="c688f-575">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="c688f-576">有关更多信息，请参见 Key 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-576">For more information, see Key element.</span></span>
-   <span data-ttu-id="c688f-577">是引用约束的依赖端或主体端。</span><span class="sxs-lookup"><span data-stu-id="c688f-577">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="c688f-578">有关更多信息，请参见 ReferentialConstraint 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-578">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="c688f-579">**PropertyRef**元素只能具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-579">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="c688f-580">Documentation（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-580">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c688f-581">批注元素</span><span class="sxs-lookup"><span data-stu-id="c688f-581">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-582">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-582">Applicable Attributes</span></span>

<span data-ttu-id="c688f-583">下表介绍可应用于 **PropertyRef** 元素的特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-583">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="c688f-584">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-584">Attribute Name</span></span> | <span data-ttu-id="c688f-585">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-585">Is Required</span></span> | <span data-ttu-id="c688f-586">值</span><span class="sxs-lookup"><span data-stu-id="c688f-586">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="c688f-587">**名称**</span><span class="sxs-lookup"><span data-stu-id="c688f-587">**Name**</span></span>       | <span data-ttu-id="c688f-588">是</span><span class="sxs-lookup"><span data-stu-id="c688f-588">Yes</span></span>         | <span data-ttu-id="c688f-589">所引用属性的名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-589">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="c688f-590">可以将任意数量的批注属性 (自定义 XML 特性) 应用到 **PropertyRef** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-590">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="c688f-591">然而，自定义特性可能不属于为 CSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c688f-592">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-593">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-593">Example</span></span>

<span data-ttu-id="c688f-594">下面的示例演示了一个 **PropertyRef** 元素，该元素用于通过引用 **EntityType** 元素上定义的属性来定义主键。</span><span class="sxs-lookup"><span data-stu-id="c688f-594">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="c688f-595">ReferentialConstraint 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-595">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="c688f-596">存储架构定义语言 (SSDL) 中的 **ReferentialConstraint** 元素表示一个外键约束， (在基础数据库中也称为 "引用完整性) 约束"。</span><span class="sxs-lookup"><span data-stu-id="c688f-596">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="c688f-597">约束的主体端和依赖端分别由 Principal 和 Dependent 子元素指定。</span><span class="sxs-lookup"><span data-stu-id="c688f-597">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="c688f-598">通过 PropertyRef 元素引用参与主体端和依赖端的列。</span><span class="sxs-lookup"><span data-stu-id="c688f-598">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="c688f-599">**ReferentialConstraint**元素是 Association 元素的可选子元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-599">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="c688f-600">如果未使用 **ReferentialConstraint** 元素映射 **Association** 元素中指定的 foreign key 约束，则必须使用 AssociationSetMapping 元素来执行此操作。</span><span class="sxs-lookup"><span data-stu-id="c688f-600">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="c688f-601">**ReferentialConstraint**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-601">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="c688f-602">Documentation（零个或一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-602">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c688f-603">Principal（恰好一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-603">Principal (exactly one)</span></span>
-   <span data-ttu-id="c688f-604">Dependent（恰好一个）</span><span class="sxs-lookup"><span data-stu-id="c688f-604">Dependent (exactly one)</span></span>
-   <span data-ttu-id="c688f-605">批注元素（零个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-605">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-606">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-606">Applicable Attributes</span></span>

<span data-ttu-id="c688f-607">可以将任意数量的批注属性 (自定义 XML 特性) 应用到 **ReferentialConstraint** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-607">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="c688f-608">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-608">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-609">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-609">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-610">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-610">Example</span></span>

<span data-ttu-id="c688f-611">下面的示例演示一个 **Association** 元素，该元素使用 **ReferentialConstraint** 元素指定参与 **FK \_ CustomerOrders** 外键约束的列：</span><span class="sxs-lookup"><span data-stu-id="c688f-611">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="c688f-612">ReturnType 元素 (SSDL) </span><span class="sxs-lookup"><span data-stu-id="c688f-612">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="c688f-613">存储架构定义语言中的 **ReturnType** 元素 (SSDL) 指定 **函数** 元素中定义的函数的返回类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-613">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="c688f-614">还可以使用 **ReturnType** 特性指定函数返回类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-614">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="c688f-615">函数的返回类型是使用 **type** 特性或 **ReturnType** 元素指定的。</span><span class="sxs-lookup"><span data-stu-id="c688f-615">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="c688f-616">**ReturnType**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-616">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="c688f-617">CollectionType (一个) </span><span class="sxs-lookup"><span data-stu-id="c688f-617">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="c688f-618">可以将任意数量的批注属性 (自定义 XML 特性) 可应用于 **ReturnType** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-618">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="c688f-619">然而，自定义特性可能不属于为 SSDL 保留的任何 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-619">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c688f-620">任何两个自定义特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-620">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-621">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-621">Example</span></span>

<span data-ttu-id="c688f-622">下面的示例使用一个 **函数** ，该函数返回行的集合。</span><span class="sxs-lookup"><span data-stu-id="c688f-622">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="c688f-623"> (SSDL) 的 RowType 元素</span><span class="sxs-lookup"><span data-stu-id="c688f-623">RowType Element (SSDL)</span></span>

<span data-ttu-id="c688f-624">存储架构定义语言 (SSDL) 中的 **RowType** 元素将未命名的结构定义为在存储区中定义的函数的返回类型。</span><span class="sxs-lookup"><span data-stu-id="c688f-624">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="c688f-625">**RowType**元素是**CollectionType**元素的子元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-625">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="c688f-626">**RowType**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-626">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="c688f-627">Property（一个或多个）</span><span class="sxs-lookup"><span data-stu-id="c688f-627">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="c688f-628">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-628">Example</span></span>

<span data-ttu-id="c688f-629">下面的示例演示了一个存储函数，该函数使用 **CollectionType** 元素来指定函数返回 (在 **RowType** 元素) 中指定的行的集合。</span><span class="sxs-lookup"><span data-stu-id="c688f-629">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="c688f-630">Schema 元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-630">Schema Element (SSDL)</span></span>

<span data-ttu-id="c688f-631">存储架构定义语言 (SSDL) 中的 **架构** 元素是存储模型定义的根元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-631">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="c688f-632">它包括构成存储模型的对象、函数和容器的定义。</span><span class="sxs-lookup"><span data-stu-id="c688f-632">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="c688f-633">**Schema**元素可包含零个或多个下列子元素：</span><span class="sxs-lookup"><span data-stu-id="c688f-633">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="c688f-634">关联</span><span class="sxs-lookup"><span data-stu-id="c688f-634">Association</span></span>
-   <span data-ttu-id="c688f-635">EntityType</span><span class="sxs-lookup"><span data-stu-id="c688f-635">EntityType</span></span>
-   <span data-ttu-id="c688f-636">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="c688f-636">EntityContainer</span></span>
-   <span data-ttu-id="c688f-637">函数</span><span class="sxs-lookup"><span data-stu-id="c688f-637">Function</span></span>

<span data-ttu-id="c688f-638">**Schema**元素使用**namespace**属性为存储模型中的实体类型和关联对象定义命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-638">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="c688f-639">在命名空间内，任何两个对象都不能同名。</span><span class="sxs-lookup"><span data-stu-id="c688f-639">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="c688f-640">存储模型命名空间不同于 **Schema** 元素的 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-640">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="c688f-641">**命名空间**特性定义的存储模型命名空间 () 是实体类型和关联类型的逻辑容器。</span><span class="sxs-lookup"><span data-stu-id="c688f-641">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="c688f-642">**Schema**元素的**xmlns**特性) 指示 (XML 命名空间是**schema**元素的子元素和属性的默认命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-642">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="c688f-643">格式为 (的 XML 命名空间 https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl （YYYY 和 MM 分别表示年份和月份）) 为 SSDL 预留。</span><span class="sxs-lookup"><span data-stu-id="c688f-643">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="c688f-644">自定义元素和特性不能位于具有此格式的命名空间中。</span><span class="sxs-lookup"><span data-stu-id="c688f-644">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c688f-645">适用的属性</span><span class="sxs-lookup"><span data-stu-id="c688f-645">Applicable Attributes</span></span>

<span data-ttu-id="c688f-646">下表描述了可对 **Schema** 元素应用的属性。</span><span class="sxs-lookup"><span data-stu-id="c688f-646">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="c688f-647">属性名称</span><span class="sxs-lookup"><span data-stu-id="c688f-647">Attribute Name</span></span>            | <span data-ttu-id="c688f-648">是否必需</span><span class="sxs-lookup"><span data-stu-id="c688f-648">Is Required</span></span> | <span data-ttu-id="c688f-649">值</span><span class="sxs-lookup"><span data-stu-id="c688f-649">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-650">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="c688f-650">**Namespace**</span></span>             | <span data-ttu-id="c688f-651">是</span><span class="sxs-lookup"><span data-stu-id="c688f-651">Yes</span></span>         | <span data-ttu-id="c688f-652">存储模型的命名空间。</span><span class="sxs-lookup"><span data-stu-id="c688f-652">The namespace of the storage model.</span></span> <span data-ttu-id="c688f-653">**命名空间**属性的值用于构成类型的完全限定名称。</span><span class="sxs-lookup"><span data-stu-id="c688f-653">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="c688f-654">例如，如果名为*Customer*的**entitytype**在位于 examplemodel.store 命名空间中，则**entitytype**的完全限定名称为位于 examplemodel.store。</span><span class="sxs-lookup"><span data-stu-id="c688f-654">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="c688f-655">以下字符串不能用作 **Namespace** 特性的值： **System**、 **暂时性**或 **Edm**。</span><span class="sxs-lookup"><span data-stu-id="c688f-655">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="c688f-656">**Namespace**属性的值不能与 CSDL Schema 元素中**namespace**属性的值相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-656">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="c688f-657">**Alias**</span><span class="sxs-lookup"><span data-stu-id="c688f-657">**Alias**</span></span>                 | <span data-ttu-id="c688f-658">否</span><span class="sxs-lookup"><span data-stu-id="c688f-658">No</span></span>          | <span data-ttu-id="c688f-659">用于取代命名空间名称的标识符。</span><span class="sxs-lookup"><span data-stu-id="c688f-659">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="c688f-660">例如，如果名为*Customer*的**EntityType**位于位于 examplemodel.store 命名空间中，并且**Alias**属性的值为*Storagemodel.customer*，则可以使用 storagemodel.customer 作为 EntityType 的完全限定名称 **。**</span><span class="sxs-lookup"><span data-stu-id="c688f-660">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="c688f-661">**提供程序**</span><span class="sxs-lookup"><span data-stu-id="c688f-661">**Provider**</span></span>              | <span data-ttu-id="c688f-662">是</span><span class="sxs-lookup"><span data-stu-id="c688f-662">Yes</span></span>         | <span data-ttu-id="c688f-663">数据提供程序。</span><span class="sxs-lookup"><span data-stu-id="c688f-663">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="c688f-664">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="c688f-664">**ProviderManifestToken**</span></span> | <span data-ttu-id="c688f-665">是</span><span class="sxs-lookup"><span data-stu-id="c688f-665">Yes</span></span>         | <span data-ttu-id="c688f-666">一个标记，该标记指示提供程序清单返回到的提供程序。</span><span class="sxs-lookup"><span data-stu-id="c688f-666">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="c688f-667">没有为该标记定义格式。</span><span class="sxs-lookup"><span data-stu-id="c688f-667">No format for the token is defined.</span></span> <span data-ttu-id="c688f-668">标记的值由提供程序定义。</span><span class="sxs-lookup"><span data-stu-id="c688f-668">Values for the token are defined by the provider.</span></span> <span data-ttu-id="c688f-669">有关 SQL Server 提供程序清单令牌的信息，请参阅 SqlClient for 实体框架。</span><span class="sxs-lookup"><span data-stu-id="c688f-669">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="c688f-670">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-670">Example</span></span>

<span data-ttu-id="c688f-671">下面的示例演示一个 **架构** 元素，该元素包含一个 **EntityContainer** 元素、两个 **EntityType** 元素和一个 **Association** 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-671">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="c688f-672">Annotation 特性</span><span class="sxs-lookup"><span data-stu-id="c688f-672">Annotation Attributes</span></span>

<span data-ttu-id="c688f-673">以存储架构定义语言 (SSDL) 表示的批注特性在存储模型中是自定义 XML 特性，这些特性提供有关存储模型中元素的额外元数据。</span><span class="sxs-lookup"><span data-stu-id="c688f-673">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="c688f-674">除了具有有效的 XML 结构之外，以下约束也适用于批注特性：</span><span class="sxs-lookup"><span data-stu-id="c688f-674">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="c688f-675">批注特性不能位于为 SSDL 保留的任何 XML 命名空间中。</span><span class="sxs-lookup"><span data-stu-id="c688f-675">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="c688f-676">任何两个批注特性的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-676">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="c688f-677">可以将多个批注特性应用于一个给定的 SSDL 元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-677">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="c688f-678">可以在运行时通过使用 System.web 命名空间中的类访问批注元素中包含的元数据。</span><span class="sxs-lookup"><span data-stu-id="c688f-678">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-679">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-679">Example</span></span>

<span data-ttu-id="c688f-680">下面的示例演示一个 EntityType 元素，该元素具有应用于 " **订单 id** " 属性的批注特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-680">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="c688f-681">该示例还显示了添加到 **EntityType** 元素的批注元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-681">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="c688f-682">批注元素 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-682">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="c688f-683">以存储架构定义语言 (SSDL) 表示的批注元素是存储模型中的自定义 XML 元素，可提供有关存储模型的额外元数据。</span><span class="sxs-lookup"><span data-stu-id="c688f-683">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="c688f-684">除了具有有效的 XML 结构之外，批注元素还应满足以下约束：</span><span class="sxs-lookup"><span data-stu-id="c688f-684">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="c688f-685">批注元素不能位于为 SSDL 保留的任何 XML 命名空间中。</span><span class="sxs-lookup"><span data-stu-id="c688f-685">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="c688f-686">任何两个批注元素的完全限定名称都不能相同。</span><span class="sxs-lookup"><span data-stu-id="c688f-686">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="c688f-687">批注元素必须出现在给定 SSDL 元素的所有其他子元素之后。</span><span class="sxs-lookup"><span data-stu-id="c688f-687">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="c688f-688">多个批注元素可能是某个给定 SSDL 元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-688">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="c688f-689">从 .NET Framework 版本4开始，可以在运行时通过使用 System.web 命名空间中的类访问批注元素中包含的元数据。</span><span class="sxs-lookup"><span data-stu-id="c688f-689">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="c688f-690">示例</span><span class="sxs-lookup"><span data-stu-id="c688f-690">Example</span></span>

<span data-ttu-id="c688f-691">下面的示例演示一个 EntityType 元素，该元素具有一个 (**CustomElement**) 的批注元素。</span><span class="sxs-lookup"><span data-stu-id="c688f-691">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="c688f-692">该示例还显示了应用于 " **订单 id** " 属性的批注特性。</span><span class="sxs-lookup"><span data-stu-id="c688f-692">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="c688f-693">方面 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c688f-693">Facets (SSDL)</span></span>

<span data-ttu-id="c688f-694">以存储架构定义语言 (SSDL) 表示的方面表示对于 Property 元素中指定的列类型的约束。</span><span class="sxs-lookup"><span data-stu-id="c688f-694">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="c688f-695">Facet 作为 **属性** 元素上的 XML 特性实现。</span><span class="sxs-lookup"><span data-stu-id="c688f-695">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="c688f-696">下表描述了 SSDL 中支持的方面：</span><span class="sxs-lookup"><span data-stu-id="c688f-696">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="c688f-697">方面</span><span class="sxs-lookup"><span data-stu-id="c688f-697">Facet</span></span>           | <span data-ttu-id="c688f-698">说明</span><span class="sxs-lookup"><span data-stu-id="c688f-698">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c688f-699">**排序规则**</span><span class="sxs-lookup"><span data-stu-id="c688f-699">**Collation**</span></span>   | <span data-ttu-id="c688f-700">指定在对属性值执行比较和排序操作时要使用的排序序列。</span><span class="sxs-lookup"><span data-stu-id="c688f-700">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="c688f-701">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="c688f-701">**FixedLength**</span></span> | <span data-ttu-id="c688f-702">指定列值的长度是否可变。</span><span class="sxs-lookup"><span data-stu-id="c688f-702">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="c688f-703">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="c688f-703">**MaxLength**</span></span>   | <span data-ttu-id="c688f-704">指定列值的最大长度。</span><span class="sxs-lookup"><span data-stu-id="c688f-704">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="c688f-705">**精度**</span><span class="sxs-lookup"><span data-stu-id="c688f-705">**Precision**</span></span>   | <span data-ttu-id="c688f-706">对于 **Decimal**类型的属性，指定属性值可以具有的位数。</span><span class="sxs-lookup"><span data-stu-id="c688f-706">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="c688f-707">对于类型为 **Time**、 **DateTime**和 **DateTimeOffset**的属性，指定列值的秒小数部分的位数。</span><span class="sxs-lookup"><span data-stu-id="c688f-707">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="c688f-708">**缩放**</span><span class="sxs-lookup"><span data-stu-id="c688f-708">**Scale**</span></span>       | <span data-ttu-id="c688f-709">指定列值小数点右侧的位数。</span><span class="sxs-lookup"><span data-stu-id="c688f-709">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="c688f-710">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="c688f-710">**Unicode**</span></span>     | <span data-ttu-id="c688f-711">指示是否将列值存储为 Unicode。</span><span class="sxs-lookup"><span data-stu-id="c688f-711">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
