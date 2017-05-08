## Information Rights Management

### Overview
The thought process around permission management is the same as that of building a tree-like folder structure where
RESOURCES (e.g. records in a database or anything with a unique id) can be thought of as files and RESOURCE TYPES can be thought of as folders/sub-folders of a RESOURCE.
So, the only difference to a traditional folder tree lies in that a RESOURCE/"file" can contain RESOURCE TYPES/"sub-folder" which in turn contains resources, that in turn can have resource types and it's turtles all the way down.
We can refer to this as the CONTENT tree of a (reference) RESOURCE.

The CONTENT tree of a RESOURCE can be mirrored with a similar structure for USERS and GROUPS, all of which will become MEMBERS of the RESOURCE and so we can refer to this as the MEMBERSHIP tree.

PERMISSIONS form the link between the MEMBERSHIP and CONTENT trees, and the shortest route between a USER and a RESOURCE or RESOURCE TYPE along this link resolves to the USER's PERMISSION to the RESOURCE

<img href="https://github.com/Centular/docs/blob/master/images/permission-tree.png">

![Permission Tree Concept](https://github.com/Centular/docs/blob/master/images/permission-tree.png)

In the diagram above, Resource P213 is our reference node. It has one user group "User Group A" with two users, "User 24" and "User 87". Down its content tree it has a group of resources of type "Resource Type A". There can be multiple collections with different types under a resource as shown by "Resource Type n".
"Resource Type A" collection has two resources, "Resource A98" and "Resource A332" with "Resource Type X" and "Resource X46" in turn under "Resource A332".

Permissions are evaluated bottom-up, meaning, the content tree is traversed from the accessed resource in the the reverse direction until a PERMISSION link is found between the user MEMBERSHIP tree and the resource's CONTENT tree (or none is found, in which case acces is denied).

**Example 1:** "User 87" wants to access "Resource X46" way at the bottom, so the applied permission path will be:
```
("User 87") -> [PERMISSION {read,write,delete}] -> ("Resource A332") -> [CONTENT link] -> ("Resource Type X") -> [CONTENT link] -> ("Resource X46")
```
So "User 87" has "read", "write" and "delete" permission to "Resource X46", and for that matter any resource under "Resource A 322"

**Example 2:** "User 24" wants to access the same resource "Resource X46". In this case, "User 24" has no direct permission to any parent in the CONTENT tree, but it's a member of "User Group A" that does:
```
("User 24") -> [MEMBER_OF] -> ("User Group A") -> [PERMISSION {read}] -> ("Resource Type A") -> [CONTENT] -> ("Resource A332") -> [CONTENT] -> ("Resource Type X") -> [CONTENT] -> ("Resource X46")
```
So, "User 24" has "read" permission on "Resource X46" due to its membership of "User Group A".

So in conclusion, permissions are given to Users and/or Groups to Resources and/or Resource Types OF Resources. A permission on a resource affects the permission you have on the children of that resource as well.

## Your Content Root
The domain you've registered with Centular forms your content root and will be your starting point.
The domain itself is a Resource with an ID. This id can be found by calling GET /domains, or preferably finding the domain id within the current user's JWT token returned when authenticated.
**Authentication Process citation needed**

## Resource Types
In order to register any resource, you will need a resource type to identify the collection under which to put your new resource.
Resource Types can be found as content in your domain under the "system.type" resource type collection. **"system.type"** is the resource type id, in other words, the type identifying all other types.
Take note of the standard system types found there already:
- "system.type.user"
- "system.type.group"
- "system.type.permission"

The use of these will become clearer as we go through some examples.
To manage resource types, use the [Resource API](http://api-docs.centular.io/#/rights324532resource32types)
and register your own resource types under your domain as if they are normal resources, specifying **"system.type"** as the resource type:
```
POST /rights/resources
{
  id: {your-resource-type-id}, (e.g. "my-type-product")
  name: {some human readable name}, (Preferably in plural form as a resource type identifies a collection)
  resourceTypeId: "system.type",
  parentId: {your-domain-id}
}
```
## Registering Resources
## Creating Groups
## Joining Users to Groups
## Setting Permissions
## Checking Permissions



The numbering in these nodes are conceptual and will soon become IDs when you start using the system.


```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

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
