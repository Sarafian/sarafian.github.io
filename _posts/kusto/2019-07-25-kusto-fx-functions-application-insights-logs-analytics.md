---
title: Create/Save functions in application insights log analytics
excerpt: When in logs analytics of application insights, you might notice under the sources one "fx functions" without any UI options. So what is it?
categories:
- Azure
tags:
- kusto
- Application insights
- Logs analytics
---

# Introduction

I've recently started working with [Azure's application insights][1] and I'm really having fun with it. When I first started, I noticed that under the sources that there is something that reads as `fx functions`. 

![Azure application insights logs analytics fx functions](/assets/images/posts/kusto/2019-07-25-fx-functions.png "Functions")

Very recently I realized that the `fx` is actually an icon.
{: .notice--tip}

I thought this would be great as one could enhance the default query language with functions that are aligned with the organization's deployment language. These functions could be shared to help reduce the size of the queries while keeping everything cleaner. But, I first needed to master *kusto*, so I focused on building up big queries.

# Where is the save option?

Once I learned enough, I've decided it was time to break my big queries into smaller chunks that would also single source common functionality. I couldn't find how and as much as I searched online and read the [documentation][1], I couldn't find any useful leads. So, I raised a question on [stackoverflow][2].

It seems that my organization was blocking this through authorization and if that is the case with you, I hope this post offers you the leads to help you navigate the waters. I had a suspicion that this was the case, because I couldn't create a shared query as well.

What you need is the **Application Insights Component Contributor** and **Application Insights Snapshot Debugger** roles on the applications insights instance as described in the [Resources, roles, and access control in Application Insights][3] page. With these roles, you can create and edit existing functions as well as shared queries. 

# Saving a function

When you press **Save**, then the following form appears

![Azure application insights logs analytics save form](/assets/images/posts/kusto/2019-07-25-save-functions-form.png "Save form")

Without the **Application Insights Component Contributor** role, the **Function** option in the **Save as** drop-down is missing. After saving the function, it will show up both as a shared query with `fx` icon and under the functions as well. 

![Azure application insights logs shared query and function](/assets/images/posts/kusto/2019-07-25-shared-queries.png "Shared query and function")

You are allowed to have both a shared query and a function with the same name (e.g. `test`) but it is not advised because when trying to update the function an error is thrown.
{: .notice--tip}

The **name** in the form is how the function shows as a shared query on the right and the **alias** is how it shows withing the functions on the left. 

# Using a function

The alias is also the reference artefact in the queries, similar to e.g. `requests, `dependencies` etc. For example, the following kusto query would execute the `test` function:

```text
test
```

To edit/update the function, just do what you would to edit/update a query. After pressing **Save** just select the same type, name and alias. 

To delete a function you effectively delete an entry from the shared queries. Problem is that there you select based on the name and not the alias.

# What is really a function?

A function is the same as a query with the only difference being that it shows up on the left and can be referenced by other queries. You can't save a function for example like this

```text
let fServiceNameFromCloudRoleName = (name:string){
    iff(
        name<>"", @"true value",
        @"false value"
    )
}
```

and then use it like this

```text
union requests,dependencies
| extend Extra_In_ServiceName=fsn(cloud_RoleName)
```

Instead a function would contain both and create a new dataset by extending the union of `requests` and `dependencies`.

```text
let fServiceNameFromCloudRoleName = (name:string){
    iff(
        name<>"", @"true value",
        @"false value"
    )
}
union requests,dependencies
| extend Extra_In_ServiceName=fsn(cloud_RoleName)
```

This means that a function is more like a source like `requests`,`dependencies` etc are. If you would compare this with SQL, then a function in logs analytics is more like a view which combines and extends the data sets from the other sources.

# Suggestions

I believe that documentation for this particular topic can greatly be improved. 

- The [Resources, roles, and access control in Application Insights][3] doesn't properly explain how these roles affect the ability to save a function
- The UI in the application insights is not helpful. In contrast, when trying to save a shared query, the option is there and an error is thrown when the roles are not assigned. This is a clear indicator that the feature is there but permissions are missing. 
- The [documentation][1] has very little explanation over what a function is and when it does explain, it focuses on in script functions.
- The alias in a function is completed disconnected from the shared query name and it does not easily identify the one that needs to be edited or deleted.
- The term function is wrong and misleading.

[1]: https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview
[2]: https://stackoverflow.com/questions/57177895/what-is-the-fx-functions-in-azure-applications-insights-how-can-you-add-one/57178156
[3]: https://docs.microsoft.com/en-us/azure/azure-monitor/app/resources-roles-access-control