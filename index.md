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

## Resource Types
In order to register any resource, you will need a resource type to identify the collection under which to put your new resource.
Resource Types can be found as content in your domain under the "system.type" resource type collection. **"system.type"** is the resource type id, in other words, the type identifying all other types.
Take note of the standard system types found there already:
- "system.type.user"
- "system.type.group"
- "system.type.permission"

The use of these will become clearer as we go through some examples.
To manage resource types, use the [Resource API](http://api-docs.centular.io/#/rights324532resource32types)
and register your own resource types under your domain as if they are normal resources, specifying **"system.type"** as the resource type: e.g.
```
POST /rights/resources
{
  id: "organisation-type-branch"
  name: "Branches", (Preferably in plural form as a resource type identifies a collection)
  resourceTypeId: "system.type",
  parentId: "e4a68bbe-1cb7-42f4-8ab9-a3a7950128f5" (Your domain id will be a UUID)
}
```
## Registering Resources
Register resources using the same call above with appropriate parameters:



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
