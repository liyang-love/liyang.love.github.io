## Entity Framework

---

实体框架(**Entity Framework** )是 **ADO.NET** 中的一套支持开发面向数据的软件应用程序的技术。

 **LINQ to Entities** 提供语言集成查询 (**LINQ**) 支持，它允许开发人员使用 **Visual Basic** 或 **Visual C#** 根据实体框架概念模型编写查询。针对实体框架的查询由针对对象上下文执行的命令目录树查询表示。**LINQ to Entities** 将语言集成查询 (**LINQ**) 查询转换为命令目录树查询，针对实体框架执行这些查询，并返回可同时由实体框架和 **LINQ** 使用的对象。

下面列出一些基于方法的查询语法示例(C#)：

> 投影 Select | SelectMany
>
> 筛选 Where | Where…Contains
>
> 排序 ThenBy | ThenByDescending
>
> 聚合运算符 Average | Count | LongCount | Max | Min | Sum
>
> 分区 Skip | Take
>
> 转换 ToArray | ToDictionary | ToList
>
> 联接运算符 GroupJoin | Join
>
> 元素运算符 First
>
> 分组 GroupBy
>
> 导航关系

**Select**

以下示例使用 **Select** 方法以将 Product.Name 和 Product.ProductID 属性投影到一系列匿名类型。

```C# 
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var query = AWEntities.Products
        .Select(product => new
        {
            ProductId = product.ProductID,
            ProductName = product.Name
        });
 
    Console.WriteLine("Product Info:");
    foreach (var productInfo in query)
    {
        Console.WriteLine("Product Id: {0} Product name: {1} ",
            productInfo.ProductId, productInfo.ProductName);
    }
}
```

以下示例使用 **Select** 方法以只返回一系列产品名称。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    IQueryable<string> productNames = AWEntities.Products
        .Select(p => p.Name);
 
    Console.WriteLine("Product Names:");
    foreach (String productName in productNames)
    {
        Console.WriteLine(productName);
    }
}
```



**SelectMany**

以下示例使用 **SelectMany** 方法以选择 TotalDue 低于 500.00 的所有订单。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
    contacts.SelectMany(
        contact => orders.Where(order =>
            (contact.ContactID == order.Contact.ContactID)
                && order.TotalDue < 500.00M)
            .Select(order => new
            {
                ContactID = contact.ContactID,
                LastName = contact.LastName,
                FirstName = contact.FirstName,
                OrderID = order.SalesOrderID,
                Total = order.TotalDue
            }));
 
    foreach (var smallOrder in query)
    {
        Console.WriteLine("Contact ID:{0} Name:{1},{2} Order ID:{3} Total Due: ${4} ",
            smallOrder.ContactID, smallOrder.LastName, smallOrder.FirstName,
            smallOrder.OrderID, smallOrder.Total);
    }
}
```



以下示例使用 **SelectMany** 方法以选择在 2002 年 10 月 1 或此日期之后发出的所有订单。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
    contacts.SelectMany(
        contact => orders.Where(order =>
            (contact.ContactID == order.Contact.ContactID)
                && order.OrderDate >= new DateTime(2002, 10, 1))
            .Select(order => new
            {
                ContactID = contact.ContactID,
                LastName = contact.LastName,
                FirstName = contact.FirstName,
                OrderID = order.SalesOrderID,
                OrderDate = order.OrderDate
            }));
 
    foreach (var order in query)
    {
        Console.WriteLine("Contact ID:{0} Name:{1},{2} Order ID:{3} Order date: {4:d} ",
            order.ContactID, order.LastName, order.FirstName,
            order.OrderID, order.OrderDate);
    }
}
```



**Where**

以下示例返回所有联机订单。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var onlineOrders = AWEntities.SalesOrderHeaders
        .Where(order => order.OnlineOrderFlag == true)
        .Select(s => new { s.SalesOrderID, s.OrderDate, s.SalesOrderNumber });
 
    foreach (var onlineOrder in onlineOrders)
    {
        Console.WriteLine("Order ID: {0} Order date: {1:d} Order number: {2}",
            onlineOrder.SalesOrderID,
            onlineOrder.OrderDate,
            onlineOrder.SalesOrderNumber);
    }
}
```



以下示例返回订单数量大于 2 且小于 6 的订单。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var query = AWEntities.SalesOrderDetails
        .Where(order => order.OrderQty > 2 && order.OrderQty < 6)
        .Select(s => new { s.SalesOrderID, s.OrderQty });
 
    foreach (var order in query)
    {
        Console.WriteLine("Order ID: {0} Order quantity: {1}",
            order.SalesOrderID, order.OrderQty);
    }
}
```



以下示例返回所有红色产品。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var query = AWEntities.Products
        .Where(product => product.Color == "Red")
        .Select(p => new { p.Name, p.ProductNumber, p.ListPrice });
 
    foreach (var product in query)
    {
        Console.WriteLine("Name: {0}", product.Name);
        Console.WriteLine("Product number: {0}", product.ProductNumber);
        Console.WriteLine("List price: ${0}", product.ListPrice);
        Console.WriteLine("");
    }
}
```



**Where…Contains**

以下示例将一个数组用作 **Where¡Contains** 子句的一部分，以查找 ProductModelID 与数组中的值匹配的所有产品。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    int?[] productModelIds = { 19, 26, 118 };
    var products = AWEntities.Products.
        Where(p => productModelIds.Contains(p.ProductModelID));
 
    foreach (var product in products)
    {
        Console.WriteLine("{0}: {1}", product.ProductModelID, product.ProductID);
    }
}
```



> 作为 **Where¡Contains** 子句中谓词的一部分，您可以使用 **Array**、**List<(Of <(<'T>)>)>** 或实现 **IEnumerable<(Of <(<'T>)>)>** 接口的任何类型的集合。 还可以在 **LINQ to Entities** 查询中声明和初始化集合。

以下示例声明并初始化 Where¡Contains 子句中的数组，以查找 ProductModelID 或 Size 与数组中的值匹配的所有产品。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var products = AWEntities.Products.
        Where(p => (new int?[] { 19, 26, 18 }).Contains(p.ProductModelID) || 
                   (new string[] { "L", "XL" }).Contains(p.Size));
 
    foreach (var product in products)
    {
        Console.WriteLine("{0}: {1}, {2}", product.ProductID, 
                                           product.ProductModelID, 
                                           product.Size);
    }
}
```



**ThenBy**

采用基于方法的查询语法的以下示例使用 **OrderBy** 和 **ThenBy** 以返回先按姓氏后按名字排序的联系人列表。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    IQueryable<Contact> sortedContacts = AWEntities.Contacts
        .OrderBy(c => c.LastName)
        .ThenBy(c => c.FirstName)
        .Select(c => c);
 
    Console.WriteLine("The list of contacts sorted by last name then by first name:");
    foreach (Contact sortedContact in sortedContacts)
    {
        Console.WriteLine(sortedContact.LastName + ", " + sortedContact.FirstName);
    }
}
```



**ThenByDescending**

以下示例使用 **OrderBy** 和 **ThenByDescending** 方法以首先按标价排序，然后执行产品名称的降序排序。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Product> products = AWEntities.Products;
 
    IOrderedQueryable<Product> query = products
        .OrderBy(product => product.ListPrice)
        .ThenByDescending(product => product.Name);
 
    foreach (Product product in query)
    {
        Console.WriteLine("Product ID: {0} Product Name: {1} List Price {2}",
            product.ProductID,
            product.Name,
            product.ListPrice);
    }
}
```



**Average**

以下示例使用 **Average** 方法来查找产品的平均标价。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Product> products = AWEntities.Products;
 
    Decimal averageListPrice =
        products.Average(product => product.ListPrice);
 
    Console.WriteLine("The average list price of all the products is ${0}",
        averageListPrice);
}
```



以下示例使用 **Average** 方法以查找每种样式的产品的平均标价。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Product> products = AWEntities.Products;
 
    var query = from product in products
                group product by product.Style into g
                select new
                {
                    Style = g.Key,
                    AverageListPrice =
                        g.Average(product => product.ListPrice)
                };
 
    foreach (var product in query)
    {
        Console.WriteLine("Product style: {0} Average list price: {1}",
            product.Style, product.AverageListPrice);
    }
}
```



以下示例使用 **Average** 方法以查找平均应付款总计。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    Decimal averageTotalDue = orders.Average(order => order.TotalDue);
    Console.WriteLine("The average TotalDue is {0}.", averageTotalDue);
}
```



以下示例使用 **Average** 方法以获取每个联系人 ID 的平均应付款总计。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
        from order in orders
        group order by order.Contact.ContactID into g
        select new
        {
            Category = g.Key,
            averageTotalDue = g.Average(order => order.TotalDue)
        };
 
    foreach (var order in query)
    {
        Console.WriteLine("ContactID = {0} \t Average TotalDue = {1}",
            order.Category, order.averageTotalDue);
    }
}
```



以下示例使用 **Average** 方法以针对每个联系人获取具有平均应付款总计的订单。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
        from order in orders
        group order by order.Contact.ContactID into g
        let averageTotalDue = g.Average(order => order.TotalDue)
        select new
        {
            Category = g.Key,
            CheapestProducts =
                g.Where(order => order.TotalDue == averageTotalDue)
        };
 
    foreach (var orderGroup in query)
    {
        Console.WriteLine("ContactID: {0}", orderGroup.Category);
        foreach (var order in orderGroup.CheapestProducts)
        {
            Console.WriteLine("Average total due for SalesOrderID {1} is: {0}",
                order.TotalDue, order.SalesOrderID);
        }
        Console.Write("\n");
    }
}
```



**Count**

以下示例使用 **Count** 方法以返回 Product 表中的产品数量。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Product> products = AWEntities.Products;
 
    int numProducts = products.Count();
 
    Console.WriteLine("There are {0} products.", numProducts);
}
```



以下示例使用 **Count** 方法以返回联系人 ID 的列表和每个联系人 ID 所具有的订单数。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
 
    //Can't find field SalesOrderContact
    var query =
        from contact in contacts
        select new
        {
            CustomerID = contact.ContactID,
            OrderCount = contact.SalesOrderHeaders.Count()
        };
 
    foreach (var contact in query)
    {
        Console.WriteLine("CustomerID = {0} \t OrderCount = {1}",
            contact.CustomerID,
            contact.OrderCount);
    }
}
```



以下示例按颜色对产品进行分组，并使用 **Count** 方法以返回每个颜色组中的产品数量。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Product> products = AWEntities.Products;
 
    var query =
        from product in products
        group product by product.Color into g
        select new { Color = g.Key, ProductCount = g.Count() };
 
    foreach (var product in query)
    {
        Console.WriteLine("Color = {0} \t ProductCount = {1}",
            product.Color,
            product.ProductCount);
    }
}
```



**LongCount**

以下示例以长整型获取联系人计数。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
 
    long numberOfContacts = contacts.LongCount();
    Console.WriteLine("There are {0} Contacts", numberOfContacts);
}
```



**Max**

以下示例使用 **Max** 方法以获取最大应付款总计。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    Decimal maxTotalDue = orders.Max(w => w.TotalDue);
    Console.WriteLine("The maximum TotalDue is {0}.",
        maxTotalDue);
}
```



以下示例使用 **Max** 方法以获取每个联系人 ID 的最大应付款总计。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
        from order in orders
        group order by order.Contact.ContactID into g
        select new
        {
            Category = g.Key,
            maxTotalDue =
                g.Max(order => order.TotalDue)
        };
 
    foreach (var order in query)
    {
        Console.WriteLine("ContactID = {0} \t Maximum TotalDue = {1}",
            order.Category, order.maxTotalDue);
    }
}
```



以下示例使用 **Max** 方法以针对每个联系人 ID 获取具有最大应付款总计的订单。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
        from order in orders
        group order by order.Contact.ContactID into g
        let maxTotalDue = g.Max(order => order.TotalDue)
        select new
        {
            Category = g.Key,
            CheapestProducts =
                g.Where(order => order.TotalDue == maxTotalDue)
        };
 
    foreach (var orderGroup in query)
    {
        Console.WriteLine("ContactID: {0}", orderGroup.Category);
        foreach (var order in orderGroup.CheapestProducts)
        {
            Console.WriteLine("MaxTotalDue {0} for SalesOrderID {1}: ",
                order.TotalDue,
                order.SalesOrderID);
        }
        Console.Write("\n");
    }
}
```



**Min**

以下示例使用 **Min** 方法以获取最小应付款总计。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    Decimal smallestTotalDue = orders.Min(totalDue => totalDue.TotalDue);
    Console.WriteLine("The smallest TotalDue is {0}.",
        smallestTotalDue);
}
```



以下示例使用 **Min** 方法以获取每个联系人 ID 的最小应付款总计。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
        from order in orders
        group order by order.Contact.ContactID into g
        select new
        {
            Category = g.Key,
            smallestTotalDue =
                g.Min(order => order.TotalDue)
        };
 
    foreach (var order in query)
    {
        Console.WriteLine("ContactID = {0} \t Minimum TotalDue = {1}",
            order.Category, order.smallestTotalDue);
    }
}
```



以下示例使用 **Min** 方法以针对每个联系人获取具有最小应付款总计的订单。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
        from order in orders
        group order by order.Contact.ContactID into g
        let minTotalDue = g.Min(order => order.TotalDue)
        select new
        {
            Category = g.Key,
            smallestTotalDue =
                g.Where(order => order.TotalDue == minTotalDue)
        };
 
 
    foreach (var orderGroup in query)
    {
        Console.WriteLine("ContactID: {0}", orderGroup.Category);
        foreach (var order in orderGroup.smallestTotalDue)
        {
            Console.WriteLine("Mininum TotalDue {0} for SalesOrderID {1}: ",
                order.TotalDue,
                order.SalesOrderID);
        }
        Console.Write("\n");
    }
}
```



**Sum**

以下示例使用 **Sum** 方法以获取 SalesOrderDetail 表中订单数量的总数。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderDetail> orders = AWEntities.SalesOrderDetails;
 
    double totalOrderQty = orders.Sum(o => o.OrderQty);
    Console.WriteLine("There are a total of {0} OrderQty.",
        totalOrderQty);
}
```



以下示例使用 **Sum** 方法以获取每个联系人 ID 的应付款总计。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
        from order in orders
        group order by order.Contact.ContactID into g
        select new
        {
            Category = g.Key,
            TotalDue = g.Sum(order => order.TotalDue)
        };
 
    foreach (var order in query)
    {
        Console.WriteLine("ContactID = {0} \t TotalDue sum = {1}",
            order.Category, order.TotalDue);
    }
}
```



**Skip**

以下示例使用 **Skip<(Of <<'(TSource>)>>)** 方法以获取 Contact 表中除前三个联系人之外的所有联系人。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    // LINQ to Entities only supports Skip on ordered collections.
    IOrderedQueryable<Product> products = AWEntities.Products
            .OrderBy(p => p.ListPrice);
 
    IQueryable<Product> allButFirst3Products = products.Skip(3);
 
    Console.WriteLine("All but first 3 products:");
    foreach (Product product in allButFirst3Products)
    {
        Console.WriteLine("Name: {0} \t ID: {1}",
            product.Name,
            product.ProductID);
    }
}
```



以下示例使用 **Skip<(Of <<'(TSource>)>>)** 方法以获取 Seattle 的前两个地址之外的所有地址。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Address> addresses = AWEntities.Addresses;
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    //LINQ to Entities only supports Skip on ordered collections.
    var query = (
        from address in addresses
        from order in orders
        where address.AddressID == order.Address.AddressID
             && address.City == "Seattle"
        orderby order.SalesOrderID
        select new
        {
            City = address.City,
            OrderID = order.SalesOrderID,
            OrderDate = order.OrderDate
        }).Skip(2);
 
    Console.WriteLine("All but first 2 orders in Seattle:");
    foreach (var order in query)
    {
        Console.WriteLine("City: {0} Order ID: {1} Total Due: {2:d}",
            order.City, order.OrderID, order.OrderDate);
    }
}
```



**Take**

以下示例使用 **Take<(Of <<'(TSource>)>>)** 方法以只从 Contact 表中获取前五个联系人。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
 
    IQueryable<Contact> first5Contacts = contacts.Take(5);
 
    Console.WriteLine("First 5 contacts:");
    foreach (Contact contact in first5Contacts)
    {
        Console.WriteLine("Title = {0} \t FirstName = {1} \t Lastname = {2}",
            contact.Title,
            contact.FirstName,
            contact.LastName);
    }
}
```



以下示例使用 **Take<(Of <<'(TSource>)>>)** 方法以获取 Seattle 的前三个地址。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Address> addresses = AWEntities.Addresses;
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query = (
        from address in addresses
        from order in orders
        where address.AddressID == order.Address.AddressID
             && address.City == "Seattle"
        select new
        {
            City = address.City,
            OrderID = order.SalesOrderID,
            OrderDate = order.OrderDate
        }).Take(3);
    Console.WriteLine("First 3 orders in Seattle:");
    foreach (var order in query)
    {
        Console.WriteLine("City: {0} Order ID: {1} Total Due: {2:d}",
            order.City, order.OrderID, order.OrderDate);
    }
}
```



**ToArray**

下面的示例使用 **ToArray<(Of <<'(TSource>)>>)** 方法立即将序列计算为数组。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Product> products = AWEntities.Products;
 
    Product[] prodArray = (
        from product in products
        orderby product.ListPrice descending
        select product).ToArray();
 
    Console.WriteLine("Every price from highest to lowest:");
    foreach (Product product in prodArray)
    {
        Console.WriteLine(product.ListPrice);
    }
}
```



**ToDictionary**

以下示例使用 **ToDictionary** 方法以立即将序列和相关的键表达式转换为字典。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Product> products = AWEntities.Products;
 
    Dictionary<String, Product> scoreRecordsDict = products.
            ToDictionary(record => record.Name);
 
    Console.WriteLine("Top Tube's ProductID: {0}",
            scoreRecordsDict["Top Tube"].ProductID);
}
```



**ToList**

以下示例使用 **ToList<(Of <<'(TSource>)>>)** 方法以立即将序列转换为 **List<(Of <(<'T>)>)>**，其中，T 属于类型 DataRow。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Product> products = AWEntities.Products;
 
    List<Product> query =
        (from product in products
         orderby product.Name
         select product).ToList();
 
    Console.WriteLine("The product list, ordered by product name:");
    foreach (Product product in query)
    {
        Console.WriteLine(product.Name.ToLower(CultureInfo.InvariantCulture));
    }
}
```



**GroupJoin**

以下示例针对 SalesOrderHeader 表和 SalesOrderDetail 表执行 **GroupJoin** 以查找每个客户的订单数。 组联接等效于左外部联接，它返回第一个（左侧）数据源的每个元素（即使其他数据源中没有关联元素）。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
    ObjectSet<SalesOrderDetail> details = AWEntities.SalesOrderDetails;
 
    var query = orders.GroupJoin(details, 
        order => order.SalesOrderID,
        detail => detail.SalesOrderID,
        (order, orderGroup) => new
        {
            CustomerID = order.SalesOrderID,
            OrderCount = orderGroup.Count()
        });
 
    foreach (var order in query)
    {
        Console.WriteLine("CustomerID: {0}  Orders Count: {1}",
            order.CustomerID,
            order.OrderCount);
    }
 
}
```



下面的示例对 Contact 和 SalesOrderHeader 表执行 **GroupJoin** 以查找每个联系人的订单数。 将显示每个联系人的订单数和 ID。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query = contacts.GroupJoin(orders, 
        contact => contact.ContactID,
        order => order.Contact.ContactID,
        (contact, contactGroup) => new
        {
            ContactID = contact.ContactID,
            OrderCount = contactGroup.Count(),
            Orders = contactGroup.Select(order => order)
        });
 
    foreach (var group in query)
    {
        Console.WriteLine("ContactID: {0}", group.ContactID);
        Console.WriteLine("Order count: {0}", group.OrderCount);
        foreach (var orderInfo in group.Orders)
        {
            Console.WriteLine("   Sale ID: {0}", orderInfo.SalesOrderID);
        }
        Console.WriteLine("");
    }
}
```



**Join**

以下示例针对 Contact 表和 SalesOrderHeader 表执行联接。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query =
        contacts.Join(
            orders,
            order => order.ContactID,
            contact => contact.Contact.ContactID,
            (contact, order) => new
            {
                ContactID = contact.ContactID,
                SalesOrderID = order.SalesOrderID,
                FirstName = contact.FirstName,
                Lastname = contact.LastName,
                TotalDue = order.TotalDue
            });
 
    foreach (var contact_order in query)
    {
        Console.WriteLine("ContactID: {0} "
                        + "SalesOrderID: {1} "
                        + "FirstName: {2} "
                        + "Lastname: {3} "
                        + "TotalDue: {4}",
            contact_order.ContactID,
            contact_order.SalesOrderID,
            contact_order.FirstName,
            contact_order.Lastname,
            contact_order.TotalDue);
    }
}
```



以下示例针对 Contact 表和 SalesOrderHeader 表执行联接，同时按联系人 ID 对结果分组。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    var query = contacts.Join(
        orders,
        order => order.ContactID,
        contact => contact.Contact.ContactID,
        (contact, order) => new
        {
            ContactID = contact.ContactID,
            SalesOrderID = order.SalesOrderID,
            FirstName = contact.FirstName,
            Lastname = contact.LastName,
            TotalDue = order.TotalDue
        })
            .GroupBy(record => record.ContactID);
 
    foreach (var group in query)
    {
        foreach (var contact_order in group)
        {
            Console.WriteLine("ContactID: {0} "
                            + "SalesOrderID: {1} "
                            + "FirstName: {2} "
                            + "Lastname: {3} "
                            + "TotalDue: {4}",
                contact_order.ContactID,
                contact_order.SalesOrderID,
                contact_order.FirstName,
                contact_order.Lastname,
                contact_order.TotalDue);
        }
    }
}
```



**First**

以下示例使用 **First** 方法查找以“caroline”开头的第一个电子邮件地址。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<Contact> contacts = AWEntities.Contacts;
 
    Contact query = contacts.First(contact =>
        contact.EmailAddress.StartsWith("caroline"));
 
    Console.WriteLine("An email address starting with 'caroline': {0}",
        query.EmailAddress);
}
```



**GroupBy**

下面的示例使用 **GroupBy** 方法来返回按邮政编码分组的 Address 对象。 这些结果将投影到一个匿名类型。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var query = AWEntities.Addresses
        .GroupBy( address => address.PostalCode)
        .Select( address => address);                     
 
    foreach (IGrouping<string, Address> addressGroup in query)
    {
        Console.WriteLine("Postal Code: {0}", addressGroup.Key);
        foreach (Address address in addressGroup)
        {
            Console.WriteLine("\t" + address.AddressLine1 +
                address.AddressLine2);
        }
    }
}
```



下面的示例使用 **GroupBy** 方法来返回按联系人姓氏的首字母分组的 Contact 对象。 这些结果还按姓氏的首字母进行排序，并投影到一个匿名类型。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var query = AWEntities.Contacts
        .GroupBy(c => c.LastName.Substring(0,1))
        .OrderBy(c => c.Key)
        .Select(c => c);
 
    foreach (IGrouping<string, Contact> group in query)
    {
        Console.WriteLine("Last names that start with the letter '{0}':",
            group.Key);
        foreach (Contact contact in group)
        {
            Console.WriteLine(contact.LastName);
        }
 
    }
}
```



下面的示例使用 **GroupBy** 方法来返回按客户 ID 分组的 SalesOrderHeader 对象。 同时还返回每个客户的销售数量。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var query = AWEntities.SalesOrderHeaders
        .GroupBy(order => order.CustomerID);
 
    foreach (IGrouping<int, SalesOrderHeader> group in query)
    {
        Console.WriteLine("Customer ID: {0}", group.Key);
        Console.WriteLine("Order count: {0}", group.Count());
 
        foreach (SalesOrderHeader sale in group)
        {
            Console.WriteLine("   Sale ID: {0}", sale.SalesOrderID);
        }
        Console.WriteLine("");                    
    }
}
```



**导航关系**

> 实体框架中的导航属性是快捷方式属性，用于定位位于关联各端的实体。导航属性允许用户通过关联集从一个实体导航到另一个实体或从一个实体导航到多个相关的实体。

以下示例采用基于方法的查询语法，使用 **SelectMany** 方法以获取其姓氏为“Zhou”的联系人的所有订单。 Contact.SalesOrderHeader 导航属性用于获取每个联系人的 SalesOrderHeader 对象的集合。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    IQueryable<SalesOrderHeader> ordersQuery = AWEntities.Contacts
        .Where(c => c.LastName == "Zhou")
        .SelectMany(c => c.SalesOrderHeaders);
 
    foreach (var order in ordersQuery)
    {
        Console.WriteLine("Order ID: {0}, Order date: {1}, Total Due: {2}",
            order.SalesOrderID, order.OrderDate, order.TotalDue);
    }
}
```



采用基于方法的查询语法的以下示例使用 **Select** 方法以获取所有联系人 ID 和姓氏为“Zhou”的每个联系人的应付款总计。 Contact.SalesOrderHeader 导航属性用于获取每个联系人的 SalesOrderHeader 对象的集合。 **Sum** 方法使用 Contact.SalesOrderHeader 导航属性以汇总每个联系人的所有订单的应付款总计。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var ordersQuery = AWEntities.Contacts
        .Where(c => c.LastName == "Zhou")
        .Select(c => new
        {
            ContactID = c.ContactID,
            Total = c.SalesOrderHeaders.Sum(o => o.TotalDue)
        });
 
    foreach (var contact in ordersQuery)
    {
        Console.WriteLine("Contact ID: {0} Orders total: {1}", 
                  contact.ContactID, contact.Total);
    }
}
```



采用基于方法的查询语法的以下示例获取其姓氏为“Zhou”的联系人的所有订单。 Contact.SalesOrderHeader 导航属性用于获取每个联系人的 SalesOrderHeader 对象的集合。 联系人的姓名和订单以匿名类型返回。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var ordersQuery = AWEntities.Contacts
        .Where(c => c.LastName == "Zhou")
        .Select(c => new { LastName = c.LastName, Orders = c.SalesOrderHeaders });
 
    foreach (var order in ordersQuery)
    {
        Console.WriteLine("Name: {0}", order.LastName);
        foreach (SalesOrderHeader orderInfo in order.Orders)
        {
            Console.WriteLine("Order ID: {0}, Order date: {1}, Total Due: {2}",
                orderInfo.SalesOrderID, orderInfo.OrderDate, orderInfo.TotalDue);
        }
        Console.WriteLine("");
    }
}
```



以下示例使用 SalesOrderHeader.Address 和 SalesOrderHeader.Contact 导航属性以获取与每个订单关联的 Address 对象和 Contact 对象的集合。 Seattle 城市发生的每个订单的联系人姓氏、街道地址、销售订单号和应付款总计将以匿名类型返回。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    var ordersQuery = AWEntities.SalesOrderHeaders
        .Where(o => o.Address.City == "Seattle")
        .Select(o => new
        {
            ContactLastName = o.Contact.LastName,
            ContactFirstName = o.Contact.FirstName,
            StreetAddress = o.Address.AddressLine1,
            OrderNumber = o.SalesOrderNumber,
            TotalDue = o.TotalDue
        });
 
    foreach (var orderInfo in ordersQuery)
    {
        Console.WriteLine("Name: {0}, {1}", orderInfo.ContactLastName, 
                            orderInfo.ContactFirstName);
        Console.WriteLine("Street address: {0}", orderInfo.StreetAddress);
        Console.WriteLine("Order number: {0}", orderInfo.OrderNumber);
        Console.WriteLine("Total Due: {0}", orderInfo.TotalDue);
        Console.WriteLine("");
    }
}
```



以下示例使用 **Where** 方法以查找在 2003 年 12 月 1 日之后生成的订单，然后使用 order.SalesOrderDetail 导航属性以获取每个订单的详细信息。

```c#
using (AdventureWorksEntities AWEntities = new AdventureWorksEntities())
{
    ObjectSet<SalesOrderHeader> orders = AWEntities.SalesOrderHeaders;
 
    IQueryable<SalesOrderHeader> query =
        from order in orders
        where order.OrderDate >= new DateTime(2003, 12, 1)
        select order;
 
 
    Console.WriteLine("Orders that were made after December 1, 2003:");
    foreach (SalesOrderHeader order in query)
    {
        Console.WriteLine("OrderID {0} Order date: {1:d} ",
            order.SalesOrderID, order.OrderDate);
        foreach (SalesOrderDetail orderDetail in order.SalesOrderDetails)
        {
            Console.WriteLine("  Product ID: {0} Unit Price {1}",
                orderDetail.ProductID, orderDetail.UnitPrice);
        }
    }
}
```

