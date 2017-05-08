## Information Rights Management

### Overview
The thought process around permission management is the same as that of building a tree-like folder structure where
RESOURCES (e.g. records in a database or anything with a unique id) can be thought of as files and RESOURCE TYPES can be thought of as folders/sub-folders of a RESOURCE.
So, the only difference to a traditional folder tree lies in that a RESOURCE/"file" can contain RESOURCE TYPES/"sub-folder" which in turn contains resources, that in turn can have resource types and it's turtles all the way down.
We can refer to this as the CONTENT tree of a (reference) RESOURCE.

The CONTENT tree of a RESOURCE can be mirrored with a similar structure for USERS and GROUPS, all of which will become MEMBERS of the RESOURCE and so we can refer to this as the MEMBERSHIP tree.

PERMISSIONS form the link between the MEMBERSHIP and CONTENT trees, and the shortest route between a USER and a RESOURCE or RESOURCE TYPE along this link resolves to the USER's PERMISSION to the RESOURCE
Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

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
