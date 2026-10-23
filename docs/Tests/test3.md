The fact about indentation in content tab nearly take me this morning lol

=== "Tab 1"

    ``` yaml {.annotate}
    markdown_extensions: 
    - attr_list 
    - md_in_html
    - pymdownx.superfences
    - pymdownx.highlight      # (1)!
    ```

    1. :man_raising_hand: `pymdownx.highlight` Added this bit, ai suggestion  



=== "Tab 2"

    ``` yaml {.annotate}
    markdown_extensions:
    - attr_list               # (1)!
    - md_in_html
    - pymdownx.superfences
    - pymdownx.highlight
    ```

    1. :fontawesome-solid-people-roof:`attr_list` enables Markdown attributes.

## Footnote
### Sample
**Why documentation matters**

Good documentation helps people understand and use a product.
Clear examples are especially useful when explaining complex
features.[^1]

Footnotes can also provide additional context without interrupting
the main explanation.[^2]

[^1]: Examples help readers apply concepts in practice.
[^2]: Use footnotes for supplementary information, sources, or clarifications.


### SVGs
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" fill="blue"/>
</svg>

<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" fill="red"/>
</svg> <svg width="100" height="100">
  <circle cx="50" cy="50" r="40" fill="pink"/>
</svg>

<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" fill="yellow"/>
</svg>




