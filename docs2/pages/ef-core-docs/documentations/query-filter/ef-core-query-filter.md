---
Permalink: ef-core-query-filter
---

# Query Filter

## Introduction

In almost every application, there are some tables which contains inactive or soft deleted data. This kind of data should not be shown to the client or used anymore every time we query these tables, we must remove them with a WHERE clause. Major ORM like NHibernate have a filter feature to select records based on a predefined filter but, unfortunately for Entity Framework users, Query Filter is only available through third party library.

**EF+ Query Filter** lets you change the predefined query from the context generated by Entity Framework for your own Query.

 - You can filter the query with a predicate to exclude soft deleted records:

{% include template-example.html %} 
```csharp

// using Z.EntityFramework.Plus; // Don't forget to include this.
var ctx = new EntitiesContext();

ctx.Filter<Post>(q => q.Where(x => !x.IsSoftDeleted));

// SELECT * FROM Post WHERE IsSoftDeleted = false
var list = ctx.Posts.ToList();

```

[Try it](https://dotnetfiddle.net/m38JPM)

 - You can control the default query to add a default sorting:

{% include template-example.html %} 
```csharp

// using Z.EntityFramework.Plus; // Don't forget to include this.
var ctx = new EntitiesContext();

ctx.Filter<Post>(q => q.Where(x => !x.IsSoftDeleted)
                       .OrderByDescending(x => x.ViewCount));

// SELECT * FROM Post WHERE IsSoftDeleted = false ORDER BY ViewCount
var list = ctx.Posts.ToList();

```

[Try it](https://dotnetfiddle.net/AjRuQO)

 - You can use a predefined filter and enable it only for a specific query:

{% include template-example.html %} 
```csharp

// using Z.EntityFramework.Plus; // Don't forget to include this.
var ctx = new EntitiesContext();

ctx.Filter<Post>(MyEnum.EnumValue, q => q.Where(x => !x.IsSoftDeleted)).Disable();

// SELECT * FROM Post WHERE IsSoftDeleted = false
var list = ctx.Posts.Filter(MyEnum.EnumValue).ToList();
```

[Try it](https://dotnetfiddle.net/1tnpPm)

## Options

 - [Global](options/ef-core-query-filter-global.md)
 - [By Instance](options/ef-core-query-filter-by-instance.md)
 - [By Query](options/ef-core-query-filter-by-query.md)
 - [By Inheritance/Interface](options/ef-core-query-filter-by-inheritance-interface.md)
 - [By Enable/Disable](options/ef-core-query-filter-by-enable-disable.md)
 - [By AsNoFilter](options/ef-core-query-filter-by-as-no-filter.md)

## Real Life Scenarios

 - [Logical Data Partitioning](scenarios/ef-core-query-filter-logical-data-partitioning.md)
 - [Multi-Tenancy](scenarios/ef-core-query-filter-multi-tenancy.md)
 - [Object State](scenarios/ef-core-query-filter-object-state.md)
 - [Security Access](scenarios/ef-core-query-filter-security-access.md)
 - [Default Ordering](scenarios/ef-core-query-filter-default-ordering.md)
 
## Limitations

 - Entity Framework Core
   - Doesn't work with LazyLoading
   - Doesn't work with Include
   - **DO NOT** support filter by inheritance/interface (Will be supported when EntityFramework team will fix this [issue](https://github.com/aspnet/EntityFramework/issues/3736))

#### Entity Framework Core - Limitations

A **ForceCast** option has been added to support temporary inheritance, but some LINQ method will no longer be working in combination with ForceCast.

{% include template-example.html %} 
```csharp

QueryFilterManager.ForceCast = true;

```

Here is a list of known method that no longer work with query filtered with the ForceCast options enabled:

 - Aggregate
 - Max
 - Min
 - Sum

## Requirements

 - **EF+ Query Filter:** Full version or Standalone version
 - **Database Provider:** All supported
 - **Entity Framework Version:** EF5, EF6
 - **Minimum Framework Version:** .NET Framework 4

## Conclusion

**EF+ Query Filter** is very powerful and very easy to use. Our filter version covers all kinds of requirements an application could have.

Need help getting started? [info@zzzprojects.com](mailto:info@zzzprojects.com)

We welcome all comments, ideas and suggestions to improve our library.