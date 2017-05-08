## Information Rights Management

### Overview
The thought process around permission management is the same as that of building a tree-like folder structure where
RESOURCES (e.g. records in a database or anything with a unique id) can be thought of as files and RESOURCE TYPES can be thought of as folders/sub-folders of a RESOURCE.
So, the only difference to a traditional folder tree lies in that a RESOURCE/"file" can contain RESOURCE TYPES/"sub-folder" which in turn contains resources, that in turn can have resource types and it's turtles all the way down.
We can refer to this as the CONTENT tree of a (reference) RESOURCE.

The CONTENT tree of a RESOURCE can be mirrored with a similar structure for USERS and GROUPS, all of which will become MEMBERS of the RESOURCE and so we can refer to this as the MEMBERSHIP tree.

PERMISSIONS form the link between the MEMBERSHIP and CONTENT trees, and the shortest route between a USER and a RESOURCE or RESOURCE TYPE along this link resolves to the USER's PERMISSION to the RESOURCE

![Permission Tree Concept](https://github.com/Centular/docs/blob/master/images/permission-tree.png)

In the diagram above, Resource P213 is our reference node. It has one user group "User Group A" with two users, "User 24" and "User 87". Down its content tree it has a group of resources of type "Resource Type A". There can be multiple collections with different types under a resource as shown by "Resource Type n".
"Resource Type A" collection has two resources, "Resource A98" and "Resource A332" with "Resource Type X" and "Resource X46" in turn under "Resource A332".

Permissions are evaluated bottom-up, meaning, the content tree is traversed from the accessed resource in the the reverse direction until a PERMISSION link is found between the user MEMBERSHIP tree and the resource's CONTENT tree (or none is found, in which case acces is denied).

For example. "User 87" wants to access "Resource X46" way at the bottom, so the applied permission path will be:
```
"User 87" -> [PERMISSION {read,write,delete}] -> ("Resource A332") -> [CONTENT link] -> ("Resource Type X") -> [CONTENT link] -> ("Resource X46")
```
So "User 87" has "read", "write" and "delete" permission to "Resource X46", and for that matter any resource under "Resource Type X".










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
