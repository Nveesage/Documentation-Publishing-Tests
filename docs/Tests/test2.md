
## **Annotations**

### Configuration
This configuration allows to add annotations to all inline- and block-level elements, as well as code blocks, and nest annotations inside each other. Add the following lines to mkdocs.yml:

```yaml
markdown_extensions:
  - attr_list
  - md_in_html
  - pymdownx.superfences
```

### Annotation in a code block

``` {.yaml .annotate}
markdown_extensions: 
  - attr_list 
  - md_in_html
  - pymdownx.superfences
  - pymdownx.highlight      # (1)!
```

1. :man_raising_hand: `pymdownx.highlight` Added this bit, ai suggestion  


``` {.yaml .annotate}
markdown_extensions:
  - attr_list               # (1)!
  - md_in_html
  - pymdownx.superfences
  - pymdownx.highlight
```

1. :fontawesome-solid-people-roof:`attr_list` enables Markdown attributes.

### Annotation in an Admonition block
- Between adding the marker and its definition, the word "annotate" must be inserted immediately after the type set before your proposed admonition header. Example below:

!!! Note annotate "Important command"

    Run this command before continuing, (1) very important
    
   1. Run

### Annotation in a normal texts or block of texts
- Append {: .annotate } below the paragraph. 
- To group a list to an annotation marker. Indent every item under the list by 4 spaces examples below
below

More details (1)  
{: .annotate }

1.  - good
    - bad 
    - ugly
---

### Annotations Nested inside another Annotation
Lorem ipsum dolor sit amet, (1) consectetur adipiscing elit.
{ .annotate }

1.  :man_raising_hand: I'm an annotation! (1)
    { .annotate }

    1.  :woman_raising_hand: I'm an annotation as well!
        { .annotate }


```{.yaml . annotate}
theme: # (1)!
  name: material
```

1. Configure the theme. For example:
   ```yaml
   name: material
   ```
---

## Content Tab
### Definition and Configuration
Sometimes, it's desirable to group alternative content under different tabs, e.g. when describing how to access an API from different languages or environments. Material for MkDocs allows for beautiful and functional tabs, grouping code blocks and other content.

Configuration¶
This configuration enables content tabs, and allows to nest arbitrary content inside content tabs, including code blocks and ... more content tabs! Add the following lines to mkdocs.yml:

```yaml {.annotate}
markdown_extensions:
  - pymdownx.superfences      # This enables flexibility of nesting especially within blocks
  - pymdownx.tabbed:
      alternate_style: true   # For content tabs
      slugify: !!python/object/apply:pymdownx.slugs.slugify     # (1)!
        kwds:
          case: lower
```

1. Slugification: Python Markdown extensions adds support for slugification of content tabs, which produces nicer looking and more readable anchor links. Enable the slugify function with the following lines:


??? info "Indentation" 
    Content tabs works because the contents of each tab's are indented completely inside of it's tab headings along with all other functions that makes it work. believe me this fact about indentation in content tab nearly take me this morning lol

=== "Unordered list"

    * Sed sagittis eleifend rutrum
    * Donec vitae suscipit est
    * Nulla tempor lobortis orci

=== "Ordered list"

    1. Sed sagittis eleifend rutrum
    2. Donec vitae suscipit est
    3. Nulla tempor lobortis orci  

---


=== "Tab 1"

    ``` {.yaml .annotate}
    markdown_extensions:
      - attr_list
      - md_in_html
      - pymdownx.superfences
      - pymdownx.highlight # (1)!
    ```

    1. :man_raising_hand: `pymdownx.highlight` — added this bit.

=== "Tab 2"

    ``` {.yaml .annotate}
    markdown_extensions:
      - attr_list # (1)!
      - md_in_html
      - pymdownx.superfences
      - pymdownx.highlight
    ```

    1. :fontawesome-solid-people-roof: `attr_list` enables Markdown attributes.