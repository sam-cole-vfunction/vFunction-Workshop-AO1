---
title: "Order Controller"
chapter: true
weight: 3
---

![vFunction Logo](/images/vFunction.png)

### Order Controller

In this section, we will focus on the OrderController domain:

1. Click on the OrderController domain on the right pane. The names of the domains are derived from the classes that own the end points through which clients access the domain. These endpoints are called *entry points*.

2. The domain has three entry points. To review them, click EXPLORE TREE next to the Entry Points of the OrderController domain. On the left pane, you see the three endpoints. In the center pane, you see the call tree provided by the dynamic analysis. Clicking the left-most arrow in the call tree will take you up the tree, meaning it shows the calling nodes.

    | ![Order Controller Call Tree](/images/Order-Controller-Calltree.png) |
    | :--: |
    | Viewing the Entry Points and their calltree of OrderController domain |

3. Click on Back to domains. The dynamic class exclusivity is 100% - this means that the classes identified by the dynamic analysis flows are used exclusively by this domain. Click VIEW next to the Dynamic classes. You will see two exclusive classes (OrderController and OrderService), 0 non-exclusive classes (classes used by other domains), and 1 Common class (Logger). Common classes are classes marked to be included in a jar used by all domains as a library. We name this jar  *common jar*.

4. Click Back to domains – the static class exclusivity is 20%. Click on VIEW next to it. We see two exclusive classes and 8 non-exclusive classes, hence the 20% value (2 out of 10).

5. Click on the NON filter to view the non-exclusive classes. Click on the BillToAddress class - it belongs to the package com.oms.entity, which contains the @Entity classes that correspond to the database. We will add all the entity classes into the common jar. This is of course a design decision, and one may decide to try and add only the relevant entities to each domain. Click MARK PACKAGE AS COMMON, leave the package name as com.oms.entity, and click SUBMIT. The analysis will automatically re-run, and we will remain with a single non-exclusive class – SalesOrderRepository.

6. After the analysis is complete, click SalesOrderRepository and then on the class DETAILS (next to *Not exclusive to domain*). You will see that this class is being used by two domains: OrderController and ModifyFulfillmentController. Click on OrderController - a graph like the one below appears in the center pane. It shows that the class Orderdomain is referencing SalesOrderRepository as a bean (dashed arrow), and it is referencing SalesOrder as a compile-time dependency (solid arrow). Also, SalesOrderRepository references SalesOrder. 

    | ![Order Controller Static](/images/Order-Controller-Static-1.png) |
    | :--: |
    | Static Dependencies of SalesOrderRepository |
    
7. Click again on Details of SalesOrderRepository, but now select ModifyFulfillmentController to see how that domain references the SalesOrderRepository class. You will see a similar usage.

8. Since SalesOrderRepository is a JPA repository used only by these two domains (OrderController and ModifyFulfillmentController), keep this class non-exclusive in the analysis. This means the class will be duplicated into both domains.

9. Go back to the domains list and open click VIEW next to Order Controller Resources. Resources are external virtual or physical objects used by the domains such as database tables, files, transactions, spring beans, network sockets, and more. 
OrderController has ten exclusive resources (The resource used exclusively by this domain) and eight non-exclusive resources (resources shared with other domains). 

10. Click on the NON filter to view the non-exclusive resources. You can see a list of database tables used by the Order Controller domain, which are shared by others. Click Details on a table to see which other domains use it. 

11. At the bottom of the list, you see a file resource (oms-log.txt). Clicking Details will show that many domains write to this file. Mark this file as common to indicate that this resource is shared across all domains and should not affect resource exclusivity.

12. Click Back to domains

![vFunction Logo](/images/vFunction.png)
