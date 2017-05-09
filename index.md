## Information Rights Management

### Overview
The thought process around permission management is the same as that of building a tree-like folder structure where
RESOURCES (e.g. records in a database or anything with a unique id) can be thought of as files and RESOURCE TYPES can be thought of as folders/sub-folders of a RESOURCE.
So, the only difference to a traditional folder tree lies in that a RESOURCE/"file" can contain RESOURCE TYPES/"sub-folder" which in turn contains resources, that in turn can have resource types and it's turtles all the way down.
We can refer to this as the CONTENT tree of a (reference) RESOURCE.

The CONTENT tree of a RESOURCE can be mirrored with a similar structure for USERS and GROUPS, all of which will become MEMBERS of the RESOURCE and so we can refer to this as the MEMBERSHIP tree.

PERMISSIONS form the link between the MEMBERSHIP and CONTENT trees, and the shortest route between a USER and a RESOURCE or RESOURCE TYPE along this link resolves to the USER's PERMISSION to the RESOURCE

<img src="http://s3-eu-west-1.amazonaws.com/centular-resources/doc-images/permission+tree.png" alt="permission-tree" class="inline"/>

In the diagram above, Resource 1 is our reference node. It has one user group "User Group A" with two users, "User 1" and "User 1". Down its content tree it has a group of resources of type "Resource Type A". There can be multiple collections with different types under a resource as shown by "Resource Type n".
"Resource Type A" collection has two resources, "Resource A1" and "Resource A2" with "Resource Type X" and "Resource X1" in turn under "Resource A2".

Permissions are evaluated bottom-up, meaning, the content tree is traversed from the accessed resource in the the reverse direction until a PERMISSION link is found between the user MEMBERSHIP tree and the resource's CONTENT tree (or none is found, in which case acces is denied).

**Example 1:** "User 1" wants to access "Resource X1" way at the bottom, so the applied permission path will be:
```
("User 1") -> [PERMISSION {read,write,delete}] -> ("Resource A2") -> [CONTENT] -> ("Resource Type X") -> [CONTENT] -> ("Resource X1")
```
So "User 1" has "read", "write" and "delete" permission to "Resource X1", and for that matter any resource under "Resource A2"

**Example 2:** "User 2" wants to access the same resource "Resource X1". In this case, "User 2" has no direct permission to any parent in the CONTENT tree, but it is a member of "User Group A" that does:
```
("User 2") -> [MEMBER_OF] -> ("User Group A") -> [PERMISSION {read}] -> ("Resource Type A") -> [CONTENT] -> ("Resource A2") -> [CONTENT] -> ("Resource Type X") -> [CONTENT] -> ("Resource X1")
```
So, "User 2" has "read" permission on "Resource X1" due to its membership of "User Group A".

So in conclusion, permissions are given to Users and/or Groups to Resources and/or Resource Types OF Resources. A permission on a resource affects the permission you have on the children of that resource as well.

## Your Content Root
The domain you've registered with Centular forms your content root and will be your starting point.
The domain itself is a Resource with an ID. This id can be found by calling GET /domains, or preferably finding the domain id within the current user's JWT token returned when authenticated.
**Authentication Process citation needed**

## Walkthrough: Restaurant Franching
## Resource Types
In order to register any resource, you will need a resource type to identify the collection under which to put your new resource.
Resource Types can be found as content in your domain under the "system.type" resource type collection. **"system.type"** is the resource type id, in other words, the type identifying all other types.
Take note of the standard system types found there already:
```
GET /rights/resources?parent_id={yout-domain-id}&resource_type_id=system.type:
```
- "system.type.user"
- "system.type.group"
- "system.type.permission"

The use of these will become clearer as we go through some examples.
To manage resource types, use the [Resource API](http://api-docs.centular.io/#/rights324532resource32types)
and register your own resource types under your domain as if they are normal resources, specifying **"system.type"** as the resource type.
Our company (The Burger Palace) will have a collection of franchises, each franchise will accept orders, each order will have items associated. So lets create these resource types:
```
POST /rights/resources
{
  parentId: "{your-domain-id}", (Your domain id will be a UUID e.g. "e4a68bbe-1cb7-42f4-8ab9-a3a7950128f5")
  resourceTypeId: "system.type",
  resources: [{
    id: "burgerpalice-type-franchise"
    name: "Franchises"
  },{
    id: "burgerpalice-type-order"
    name: "Orders"
  },{
    id: "burgerpalice-type-item"
    name: "Items"
  }]
}
```
Check if they've been created:
```
GET /rights/resources?parent_id={your-domain-id}&resource_type_id=system.type

Resonse body: 
{
  count: 6,
  pageNumber: 0,
  results: [{
    id: "system.type.user"
    name: "Users"
  },{
    id: "system.type.group"
    name: "Groups"
  },{
    id: "system.type.permission"
    name: "Permissions"
  },{
    id: "burgerpalice-type-franchise"
    name: "Franchises"
  },{
    id: "burgerpalice-type-order"
    name: "Orders"
  },{
    id: "burgerpalice-type-item"
    name: "Items"
  }],
  total: 6
}
```

## Registering Resources
Our company has two stores, one in New York and one in London:
```
POST /rights/resources
{
  parentId: "{your-domain-id}"
  resourceTypeId: "burgerpalice-type-franchise",
  resources: [{
    id: "9c0b2919-e5cc-447a-acd0-f5dc964d35d6",
    name: "Burger Palace, New York"
  },{
    id: "61c06c24-dccb-4c31-975b-d5f86283f6cf",
    name: "Burger Palace, London"
  }]
}
```

## Creating Groups
We are going to have staff, so lets create some groups at our franchises to put them in. 
Groups at New York branch:
```
POST /rights/groups
{
  parentId: "9c0b2919-e5cc-447a-acd0-f5dc964d35d6" (Burger Palace, New York's id)
  groupNames: ["Store Managers","Point of Sales","Kitchen Staff"]
}
```
Groups at Londen branch (here have cleaners as well):

```
POST /rights/groups
{
  parentId: "61c06c24-dccb-4c31-975b-d5f86283f6cf" (Burger Palace, London's id)
  groupNames: ["Store Managers","Point of Sales","Kitchen Staff","Cleaners"]
}
```
Lets check if these are now in place. Groups are also normal resources, so to get the groups at a certain place, use the normal find resources call and specify "system.type.group" as the type of sub-resources to get:
Lets look at New York's groups. The New York branch's id is used as the parent_id here:
```
GET /rights/resources?parent_id=9c0b2919-e5cc-447a-acd0-f5dc964d35d6&resource_type_id=system.type.group

Response body:
{
  count: 3,
  pageNumber: 0,
  results: [{
    id: "7fb8a67f-5e10-4607-bc71-d2cebbae1ee2"
    name: "Store Managers"
  },{
    id: "c7fe4129-550c-4961-84e8-e8c4b1ced44c"
    name: "Point of Sales"
  },{
    id: "49872e59-72fa-4a14-aed2-abe96ff30674"
    name: "Kitchen Staff"
  }],
  total: 3  
}
```
VS. London's groups:
```
GET /rights/resources?parent_id=61c06c24-dccb-4c31-975b-d5f86283f6cf&resource_type_id=system.type.group

Response body:
{
  count: 4,
  pageNumber: 0,
  results: [{
    id: "148a3a24-99f5-4eea-a85f-057e0bce0a38"
    name: "Store Managers"
  },{
    id: "4b8e1da6-b07e-4722-b870-ca93439d45c8"
    name: "Point of Sales"
  },{
    id: "fee7a235-e0c3-4d57-9be2-7cd26ec9c263"
    name: "Kitchen Staff"
  },{
    id: "07c35d31-4448-46fe-a04d-5384cb9d2749"
    name: "Cleaners"
  }],
  total: 4  
}
```
## Setting Permissions
### Permission Value Definition
Permission are represented by a 1 byte value. The first 4 bits are used to flag the actions permitted.
So if we look at a byte in binary form, the actions are listed from right to left where

- read   = decimal 1 = binary 0000 0001 = The resource can be viewed
- write  = decimal 2 = binary 0000 0010 = The resource can be changed
- delete = decimal 4 = binary 0000 0100 = The resource can be deleted
- permit = decimal 8 = binary 0000 1000 = The resource can be permitted to users/groups

The top 4 bits are unused and reserved for future use.
So a permission value of 15 (binary 0000 1111) means all actions are switched on, thus full permission.

Lets give our groups some permissions. Store Managers can do everything so they will get full permissions at their respective branches.
Lets set Store Managers of the New York branch permissions:
```
POST /rights/groups/7fb8a67f-5e10-4607-bc71-d2cebbae1ee2/resource-permissions
{
  resourceId: "9c0b2919-e5cc-447a-acd0-f5dc964d35d6", (The New York branch id)
  permission: 15
}
```
...and Store Managers at London:
```
POST /rights/groups/148a3a24-99f5-4eea-a85f-057e0bce0a38/resource-permissions
{
  resourceId: "61c06c24-dccb-4c31-975b-d5f86283f6cf", (The London branch id)
  permission: 15
}
```
So, Store Managers will have permission 15 (full permission) to any resources created under their respective branch.

Let continue with the other groups.
Point of Sales need to be able to create, view and remove orders at their respective branches.
For this we use the /resource-type-permissions call to set permissions on a collection of a certain type belonging to some resource, the parent. So its as good as saying; New York Point of Sales group can read, write and delete orders at the New York branch:
```
POST /rights/groups/c7fe4129-550c-4961-84e8-e8c4b1ced44c/resource-type-permissions
{
  parentId: "9c0b2919-e5cc-447a-acd0-f5dc964d35d6", (The New York branch id)
  resourceTypeId: "burgerpalice-type-order"
  permission: 7
}
```
...and Point of Sales at London:
```
POST /rights/groups/4b8e1da6-b07e-4722-b870-ca93439d45c8/resource-permissions
{
  resourceId: "61c06c24-dccb-4c31-975b-d5f86283f6cf", (The London branch id)
  permission: 7
}
```

Kitchen Staff should only be able to view orders so that they can prepare it.
New York Kitchen Staff:
```
POST /rights/groups/49872e59-72fa-4a14-aed2-abe96ff30674/resource-type-permissions
{
  parentId: "9c0b2919-e5cc-447a-acd0-f5dc964d35d6", (The New York branch id)
  resourceTypeId: "burgerpalice-type-order"
  permission: 1
}
```
...and London Kitchen Staff:
```
POST /rights/groups/fee7a235-e0c3-4d57-9be2-7cd26ec9c263/resource-permissions
{
  resourceId: "61c06c24-dccb-4c31-975b-d5f86283f6cf", (The London branch id)
  permission: 1
}
```
We will be leaving out Cleaners at London to illustrate denied access.




## Joining Users to Groups

## Checking Permissions



The numbering in these nodes are conceptual and will soon become IDs when you start using the system.


```markdown
Syntax highlighted code block

# Header 1
## Header 2


- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Centular/docs/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
