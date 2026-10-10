# **Testing retinues of materials**

## Python Markdown Extensions
so let me start by testing Abbreviations 

### Abbreviations `abbr`
*[Pymdownx]: Python Markdown Extensions
*[HTML]: Hyper Link Text

When we talk about HTML and Pymdownx there's are lots of things that makes them synonymous

### Admonitions 
??? damage inline end "Lorem ipsum"
    Lorem ipsum dolor sit amet, consectetur
    adipiscing elit. Nulla et euismod nulla.
    Curabitur feugiat, tortor non consequat
    finibus, justo purus auctor massa, nec
    semper lorem quam in massa.


!!! Quote "Creed"
    Lorem ipsum dolor sit amet, consectetur
    adipiscing elit. Nulla et euismod nulla.
    Curabitur feugiat, tortor non consequat
    finibus, justo purus auctor massa, nec
    semper lorem quam in massa.

???+ example inline "Lorem ipsum"
    Lorem ipsum dolor sit amet, consectetur
    adipiscing elit. Nulla et euismod nulla.
    Curabitur feugiat, tortor non consequat
    finibus, justo purus auctor massa, nec
    semper lorem quam in massa.


!!! note "Phasellus posuere in sem ut cursus"

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

    !!! note ""

      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
      nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
      massa, nec semper lorem quam in massa.

---
### Superfences

!!! warning "Run this command"

    Before continuing, run:

    ```bash
    pip install mkdocs-material
    ```
1. Install MkDocs.

2. Configure your site.

    !!! note
        Your configuration belongs in `mkdocs.yml`.

3. Build the site.

---

1. Install MkDocs.

2. Configure your site.

    !!! example "Configuration"

        Add this to `mkdocs.yml`:

        ```yaml
        site_name: My Documentation
        theme:
          name: material
        ```

3. Build the site.

---

=== "Windows"

    ```powershell
    mkdocs serve
    ```

=== "Linux/macOS"

    ```bash
    mkdocs serve
    ```

---
### Buttons

#### Fiiled
[Check out Navs](../Material%20for%20mkdocs%20sample/Nav%20&%20Theme.md){ .md-button .md-button--primary }

[Send :fontawesome-solid-paper-plane:](./test2.md){ .md-button .md-button--primary }

### Unfilled or Outline
To check for Previews controls and formatting  
[:simple-youtube:](../Material%20for%20mkdocs%20sample/3.%20Preview%20Controls%20and%20Formatting%20Option.md){ .md-button }
