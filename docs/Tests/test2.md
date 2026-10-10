
## **Annotations**

### Configuration
This configuration allows to add annotations to all inline- and block-level elements, as well as code blocks, and nest annotations inside each other. Add the following lines to mkdocs.yml:

#### Annotation in a code block

=== "Tab 1"

``` {.yaml .annotate}
markdown_extensions: 
  - attr_list 
  - md_in_html
  - pymdownx.superfences
  - pymdownx.highlight      # (1)!
```

1. :man_raising_hand: `pymdownx.highlight` Added this bit, ai suggestion  


=== "Tab 2"

``` {.yaml .annotate}
markdown_extensions:
  - attr_list               # (1)!
  - md_in_html
  - pymdownx.superfences
  - pymdownx.highlight
```

1. :fontawesome-solid-people-roof:`attr_list` enables Markdown attributes.

### Inside of an Admonition block
- Between adding the marker and its definition, the word "annotate" must be inserted immediately after the type set before your proposed admonition header. Example below:

!!! Note annotate "Important command"

    Run this command before continuing, (1) very important
    
   1. Run

### On normal texts or block of texts
- Append {: .annotate } below the paragraph. 
- To group a list to an annotation marker. Indent every item under the list by 4 spaces examples below
below

More details (1)  
{: .annotate }

1.  - good
    - bad 
    - ugly
---

#### Annotations Nested inside another Annotation
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