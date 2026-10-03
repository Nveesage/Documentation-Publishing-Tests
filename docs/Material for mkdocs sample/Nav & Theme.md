You're right about the problem: **`theme: name: material` is not an explanation of `theme`; it is one example of a theme configuration.** If your goal is to be able to construct and modify your own configuration, you need to understand the configuration *grammar* and the available keys, not memorize snippets.

I checked the current MkDocs documentation and the current Material for MkDocs documentation. The crucial thing is that **`nav` and `theme` belong to two different configuration layers**:

* `nav` is a **core MkDocs configuration setting**.
* `theme` is a **core MkDocs configuration setting**, but most of the things you put *inside* `theme:` are determined by the particular theme you're using. Material therefore has its own configuration vocabulary. ([MkDocs][1])

Let's teach them from the ground up.

---

# PART I — `nav`

## 1. What `nav` actually is

`nav` is a YAML value assigned to the MkDocs configuration key `nav`.

Its job is:

> **Define the pages and hierarchy of the site's global navigation.**

Officially, MkDocs describes it as determining the format and layout of the site's global navigation. ([MkDocs][1])

So:

```yaml
nav:
```

means:

> "MkDocs, I'm going to explicitly tell you how I want my site's navigation organized."

It is **not** telling MkDocs where your files physically have to be.

This distinction is important.

You can have:

```text
docs/
├── banana.md
├── apple.md
└── vegetables/
    └── carrot.md
```

and create:

```yaml
nav:
  - Introduction: banana.md
  - Fruit: apple.md
  - Vegetables:
      - Carrots: vegetables/carrot.md
```

The navigation can have completely different names and organization from your filesystem.

---

# 2. The fundamental `nav` grammar

There are essentially three things you need to understand:

### A. A page

```yaml
- Home: index.md
```

### B. A section

```yaml
- Guides:
    - Getting Started: getting-started.md
    - Configuration: configuration.md
```

### C. An external link

```yaml
- MkDocs: https://www.mkdocs.org/
```

That's the basic vocabulary.

---

# 3. Page entries

Consider:

```yaml
nav:
  - Home: index.md
  - About: about.md
```

The syntax:

```yaml
- Home: index.md
```

contains:

```text
Home
 ↓
navigation title

index.md
 ↓
source Markdown file
```

The `-` means this is an item in a YAML sequence.

The `:` separates the key from the value.

So:

```yaml
Home: index.md
```

is a YAML mapping.

You could write:

```yaml
nav:
  - Introduction: index.md
  - My First Page: about.md
  - Whatever I Want: getting-started.md
```

The Markdown filenames don't change.

Only the **navigation labels** change.

MkDocs uses the title in `nav` as the page's title throughout the site when one is supplied. ([MkDocs][2])

---

# 4. You don't actually have to provide the title

This is valid:

```yaml
nav:
  - index.md
  - about.md
```

MkDocs can infer the page title.

Its title-resolution order is essentially:

1. title supplied in `nav`
2. page front matter `title`
3. first level-1 Markdown heading
4. filename ([MkDocs][2])

So these are different:

```yaml
- About: about.md
```

and:

```yaml
- about.md
```

The first explicitly says:

> Display this as **About**.

The second says:

> MkDocs, figure out the title from the page.

---

# 5. Sections

Now we get to the thing people keep calling a **navigation tree**.

```yaml
nav:
  - Home: index.md

  - Guides:
      - Getting Started: guides/getting-started.md
      - Configuration: guides/configuration.md

  - Reference:
      - YAML: reference/yaml.md
      - Markdown: reference/markdown.md
```

Conceptually:

```text
Home

Guides
├── Getting Started
└── Configuration

Reference
├── YAML
└── Markdown
```

`Guides` isn't a Markdown file in this example.

It is a **section/container**.

MkDocs' terminology is important here:

> A section contains child navigation items.

A section itself isn't inherently a page/link in core MkDocs. Sections can contain pages and further sections, and they can be nested as deeply as you like. ([MkDocs][2])

---

# 6. You can nest sections

For example:

```yaml
nav:
  - Guides:
      - Getting Started:
          - Installation: guides/install.md
          - First Project: guides/first-project.md

      - Configuration:
          - Basic: guides/config/basic.md
          - Advanced: guides/config/advanced.md
```

The resulting structure is:

```text
Guides
│
├── Getting Started
│   ├── Installation
│   └── First Project
│
└── Configuration
    ├── Basic
    └── Advanced
```

This is what people mean by a **navigation tree**.

There is nothing magical happening.

It's simply nested YAML structures being interpreted as a hierarchy.

---

# 7. `nav` does NOT have a fixed maximum hierarchy

Core MkDocs allows sections to be nested arbitrarily deeply. ([MkDocs][2])

So theoretically:

```yaml
nav:
  - Level 1:
      - Level 2:
          - Level 3:
              - Level 4:
                  - Level 5:
                      - Page: page.md
```

is structurally possible.

**Whether a particular theme displays that hierarchy nicely is a separate question.**

That's one of the most important distinctions in this entire subject:

```text
MKDOCS
│
├── understands your navigation structure
│
└── THEME
     └── decides how that structure is presented visually
```

---

# 8. `nav` does not equal your folders

Suppose:

```text
docs/
├── index.md
├── stuff.md
├── banana.md
└── extremely-deep-folder/
    └── thing.md
```

You can make:

```yaml
nav:
  - Home: index.md
  - My Important Stuff: stuff.md
  - Fruit: banana.md
  - Secret:
      - Thing: extremely-deep-folder/thing.md
```

The navigation hierarchy doesn't have to mirror the directory hierarchy.

The only requirement is that the paths you specify point to the appropriate files relative to your `docs_dir`. ([MkDocs][1])

---

# 9. What happens if you don't use `nav`?

This is extremely important.

If you omit `nav`:

```yaml
site_name: My Site
```

MkDocs automatically discovers Markdown files in your documentation directory and creates navigation from them.

The automatic navigation is ordered alphanumerically, with index files receiving special treatment. ([MkDocs][2])

So:

```text
docs/
├── index.md
├── apple.md
├── banana.md
└── carrot.md
```

can automatically produce something like:

```text
Home
Apple
Banana
Carrot
```

When you add:

```yaml
nav:
```

you're essentially saying:

> **Stop leaving this organization to automatic discovery; I want to explicitly define it.**

---

# 10. Pages missing from `nav`

This is another subtle but important point.

Suppose:

```text
docs/
├── index.md
├── guide.md
└── secret.md
```

but:

```yaml
nav:
  - Home: index.md
  - Guide: guide.md
```

`secret.md` isn't necessarily deleted.

MkDocs can still build it.

It simply isn't connected to the **global navigation**.

The official documentation describes such pages as rendered but not linked from global navigation. ([MkDocs][2])

So:

```text
filesystem
    ≠
navigation
```

That's fundamental.

---

# PART II — `theme`

Now let's deal with the thing that was bothering you.

## 11. What `theme` actually is

Core MkDocs defines:

```yaml
theme:
```

as the configuration for:

> **which theme renders your documentation and how that theme is configured.**

MkDocs itself provides built-in themes, while third-party themes provide their own additional configuration options. ([MkDocs][1])

So this:

```yaml
theme:
  name: material
```

contains two levels:

```text
theme
└── name
    └── material
```

Meaning:

> Use the installed theme named `material`.

---

# 12. Why `theme` becomes confusing

Because this:

```yaml
theme:
  name: material
  palette:
    primary: blue
```

contains **both MkDocs concepts and Material concepts**.

Core MkDocs knows about the `theme` setting.

But **Material decides what `palette` means**.

Likewise:

```yaml
theme:
  name: material
  features:
    - navigation.tabs
```

`features` is a Material configuration concept.

Therefore you cannot ask:

> "What are all the possible `theme` keys in MkDocs?"

and get one universal list that applies to every theme.

The correct question is:

> **What does MkDocs define for the theme system, and what configuration does my chosen theme expose?**

That's why the documentation seems fragmented.

---

# 13. Core MkDocs `theme` vocabulary

At the MkDocs level, the important configuration keys include:

```yaml
theme:
  name: ...
  locale: ...
  custom_dir: ...
  static_templates: ...
  include_sidebar: ...
```

The exact supported options depend partly on the selected theme. MkDocs' own configuration documentation identifies `name`, `locale`, and `custom_dir` as core theme configuration concepts, while individual themes add their own options. ([MkDocs][1])

### `name`

```yaml
theme:
  name: material
```

Which installed theme?

Examples include:

```yaml
theme:
  name: mkdocs
```

or:

```yaml
theme:
  name: readthedocs
```

or, after installing Material:

```yaml
theme:
  name: material
```

MkDocs currently ships with `mkdocs` and `readthedocs` built-in, while third-party themes can be installed separately. ([MkDocs][3])

---

# 14. `locale`

```yaml
theme:
  name: material
  language: en
```

Careful here: **the exact key is theme-dependent**.

Core MkDocs has:

```yaml
theme:
  locale: en
```

Material has its own language configuration conventions/documentation.

This is another example of why you must distinguish **core MkDocs configuration** from **Material's configuration vocabulary**.

---

# 15. `custom_dir`

This is a core MkDocs mechanism for customizing/extending an existing theme.

For example:

```yaml
theme:
  name: material
  custom_dir: overrides
```

That means:

> "Use Material, but also look in my `overrides` directory for theme customizations."

MkDocs describes `custom_dir` as the directory containing files that override or supplement the parent theme. ([MkDocs][4])

This is significantly more advanced than merely changing colors, but it is part of the actual theme system.

---

# PART III — NOW MATERIAL

Now your original question:

> **What the hell is `primary`? Is there a secondary? What is the exhaustive list?**

Excellent question.

## 16. `palette` is Material's color configuration

Material gives you:

```yaml
theme:
  name: material
  palette:
    primary: blue
```

`palette` means:

> **Configure the site's color palette.**

Inside `palette`, Material currently gives you two major color roles:

```yaml
palette:
  primary: ...
  accent: ...
```

There is **not** a standard:

```yaml
secondary:
```

in Material's palette configuration.

The two principal configurable roles are:

```text
primary
accent
```

Material documents them separately. ([GitHub Pages][5])

---

# 17. What is `primary`?

`primary` is the **main color** of the interface.

Material uses it for things including:

* header
* sidebar
* text links
* various other interface components

For example:

```yaml
theme:
  name: material
  palette:
    primary: blue
```

means:

> Make the site's primary theme color blue.

Material provides a predefined palette of names including:

```text
red
pink
purple
deep purple
indigo
blue
light blue
cyan
teal
green
light green
lime
yellow
amber
orange
deep orange
brown
grey
blue grey
black
white
```

for `primary`. ([GitHub Pages][5])

---

# 18. What is `accent`?

`accent` is the color used for interactive/highlighted elements such as:

* hovered links
* buttons
* scrollbars
* other interactive elements

For example:

```yaml
theme:
  name: material
  palette:
    primary: blue
    accent: orange
```

Conceptually:

```text
BLUE
↓
main identity of site

ORANGE
↓
interactive/highlight emphasis
```

Material's documented accent choices are:

```text
red
pink
purple
deep purple
indigo
blue
light blue
cyan
teal
green
light green
lime
yellow
amber
orange
deep orange
```

Notice that the accent palette is slightly different from the primary palette. ([GitHub Pages][5])

---

# 19. `scheme`

Now:

```yaml
palette:
  scheme: default
```

This is **not a color**.

It specifies the **color scheme**.

Material currently provides:

```text
default
slate
```

where:

```text
default → light
slate   → dark
```

Material explicitly documents those two schemes. ([GitHub Pages][5])

So:

```yaml
palette:
  scheme: default
  primary: blue
  accent: orange
```

means:

```text
PALETTE
│
├── scheme
│   └── light
│
├── primary
│   └── blue
│
└── accent
    └── orange
```

---

# 20. And this is why your original example was misleading

Someone giving you:

```yaml
theme:
  name: material
  palette:
    scheme: default
    primary: blue
```

has effectively given you this:

```text
theme
│
└── Material
    │
    └── palette
        │
        ├── scheme → light/dark scheme
        │
        ├── primary → main color
        │
        └── accent → interactive color
```

But they didn't explain the **configuration vocabulary**.

That's the part you were missing.

---

# PART IV — THE BIG MATERIAL `theme` MAP

Here's the useful mental map.

Material's `theme:` configuration is broadly divided into several areas.

```text
theme:
│
├── name
│
├── language / localization
│
├── palette
│   ├── scheme
│   ├── primary
│   ├── accent
│   ├── media
│   └── toggle
│
├── font
│   ├── text
│   └── code
│
├── favicon
│
├── logo
│
├── icon
│
├── features
│   ├── navigation.*
│   ├── toc.*
│   ├── search.*
│   ├── content.*
│   ├── header.*
│   └── announce.*
│
├── custom_dir
│
└── other theme-specific configuration
```

Material's official setup documentation groups its configuration into areas including navigation, colors, fonts, icons/logo, search, header, footer, analytics, repository integration and other site behavior. ([GitHub Pages][6])

That gives you the **map**.

Now let's go deeper into the two areas you're actually studying: **navigation and theme configuration.**

---

# PART V — MATERIAL'S NAVIGATION CONTROLS

Here's where another source of confusion comes in.

You have:

```yaml
nav:
```

AND:

```yaml
theme:
  features:
    - navigation.tabs
```

Those are **not the same thing**.

### `nav`

defines:

> **What the navigation contains.**

### `theme.features`

defines:

> **How Material behaves/presents that navigation.**

That's an enormous distinction.

---

## 21. `navigation.tabs`

```yaml
theme:
  features:
    - navigation.tabs
```

This makes top-level navigation sections appear as tabs in Material's header area on sufficiently wide screens. ([GitHub Pages][7])

Your:

```yaml
nav:
  - Home: index.md
  - Guides:
      - Getting Started: getting-started.md
  - Reference:
      - YAML: yaml.md
```

defines:

```text
Home
Guides
Reference
```

Then:

```yaml
navigation.tabs
```

changes **how those top-level navigation items are presented**.

---

# 22. `navigation.tabs.sticky`

```yaml
theme:
  features:
    - navigation.tabs
    - navigation.tabs.sticky
```

The tabs remain visible while scrolling. ([GitHub Pages][7])

---

# 23. `navigation.sections`

```yaml
theme:
  features:
    - navigation.sections
```

Material renders top-level navigation sections as groups in the sidebar. ([GitHub Pages][7])

---

# 24. `navigation.expand`

```yaml
theme:
  features:
    - navigation.expand
```

Expandable navigation sections are opened by default. ([GitHub Pages][7])

---

# 25. `navigation.indexes`

This one is particularly important.

Normally:

```yaml
nav:
  - Guides:
      - Introduction: guides/index.md
      - Installation: guides/install.md
```

gives you a section and a separate introduction page.

With:

```yaml
theme:
  features:
    - navigation.indexes
```

Material allows a section itself to have an index page. ([GitHub Pages][7])

This is one of those features where **`nav` and `theme` interact**:

```text
nav
↓
defines the structure

navigation.indexes
↓
changes how Material interprets/presents sections
```

---

# 26. `navigation.top`

```yaml
theme:
  features:
    - navigation.top
```

Enables a back-to-top button when appropriate. ([GitHub Pages][7])

---

# 27. `navigation.path`

```yaml
theme:
  features:
    - navigation.path
```

Adds breadcrumb-like navigation above the page title. ([GitHub Pages][7])

Conceptually:

```text
Guides → Configuration → YAML

# YAML Configuration
```

---

# 28. `navigation.footer`

```yaml
theme:
  features:
    - navigation.footer
```

Enables previous/next navigation in the footer. ([GitHub Pages][8])

---

# 29. `navigation.prune`

```yaml
theme:
  features:
    - navigation.prune
```

Material can render only the navigation items currently relevant/visible, reducing the HTML generated for large documentation sites. It is not compatible with `navigation.expand`. ([GitHub Pages][7])

---

# 30. The pattern should now be obvious

Instead of memorizing:

```yaml
navigation.tabs
navigation.sections
navigation.expand
navigation.top
navigation.path
navigation.footer
navigation.indexes
```

read them literally:

```text
navigation.tabs
        │
        └── tabs

navigation.sections
        │
        └── sections

navigation.expand
        │
        └── expansion

navigation.top
        │
        └── back-to-top

navigation.path
        │
        └── breadcrumb/path

navigation.footer
        │
        └── footer navigation

navigation.indexes
        │
        └── section index pages
```

**This is the language of Material's configuration.**

---

# PART VI — MATERIAL'S OTHER THEME CONTROLS

Now let's make your original `theme` example genuinely useful.

## Colors

```yaml
theme:
  name: material

  palette:
    scheme: default
    primary: blue
    accent: orange
```

---

## Dark mode

Material's dark scheme is:

```yaml
theme:
  name: material

  palette:
    scheme: slate
```

Material also supports configuring multiple palette entries so users can switch between light and dark modes. ([GitHub Pages][5])

For example:

```yaml
theme:
  name: material

  palette:

    - scheme: default
      primary: blue
      accent: orange
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode

    - scheme: slate
      primary: blue
      accent: orange
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
```

Now `palette` has changed from a **mapping** into a **list of palette configurations**.

That's YAML.

And that's why understanding the structure matters.

---

# PART VII — FONTS

Material has:

```yaml
theme:
  font:
    text: Roboto
    code: Roboto Mono
```

The conceptual model is:

```text
font
├── text → normal documentation text
└── code → code/monospace text
```

Material supports a large set of Google Fonts and also allows self-hosted/custom fonts. ([GitHub Pages][9])

You can also disable automatic Google Font loading:

```yaml
theme:
  font: false
```

Material documents that as the way to fall back to system fonts rather than automatically loading fonts from Google. ([GitHub Pages][9])

---

# PART VIII — LOGO AND FAVICON

You can configure:

```yaml
theme:
  logo: images/logo.png
```

and:

```yaml
theme:
  favicon: images/favicon.png
```

Material's logo/icon documentation covers these and many other icon settings. ([GitHub Pages][10])

---

# PART IX — ICONS

Material has an `icon` configuration area.

For example:

```yaml
theme:
  icon:
    previous: fontawesome/solid/angle-left
    next: fontawesome/solid/angle-right
```

The important conceptual distinction is:

```text
logo
↓
brand/site logo

favicon
↓
browser tab icon

icon
↓
individual interface icons
```

Material provides a large icon set and allows many of the interface icons to be customized. ([GitHub Pages][10])

---

# PART X — FEATURES ARE SWITCHES

This is another thing that wasn't being explained properly.

When you see:

```yaml
theme:
  features:
    - navigation.tabs
    - navigation.top
    - content.code.copy
```

don't interpret that as some mysterious YAML.

It's essentially:

```text
features
↓
a list of switches
```

You're saying:

```text
TURN ON
    navigation.tabs

TURN ON
    navigation.top

TURN ON
    content.code.copy
```

Material's feature flags are organized by component/function, which is why names such as `navigation.tabs` and `navigation.instant` exist. ([GitHub Pages][11])

---

# PART XI — THE REALLY IMPORTANT "EXHAUSTIVE" DISTINCTION

You asked:

> "What is the exhaustive list of MkDocs or Material for MkDocs?"

There are **two different exhaustive lists**.

### List A — Core MkDocs

This is the configuration language understood by MkDocs itself.

The official MkDocs configuration reference is the authoritative list for that. ([MkDocs][1])

Among its categories are things like:

```text
Project information
├── site_name
├── site_url
├── repo_url
├── repo_name
└── ...

Documentation layout
├── nav
├── docs_dir
├── exclude_docs
├── draft_docs
└── not_in_nav

Theme
└── theme

Build directories
├── site_dir
├── ...
└── ...

Preview controls
├── watch
└── ...

Markdown extensions
├── markdown_extensions
└── ...

Plugins
└── plugins

Hooks
└── hooks

Extra files
├── extra_css
├── extra_javascript
└── extra_templates
```

That is **MkDocs' configuration universe**.

---

### List B — Material's theme configuration

Once you write:

```yaml
theme:
  name: material
```

you're entering **Material's configuration universe**.

Material adds its own configuration vocabulary for things such as:

```text
palette
font
icon
logo
favicon
features
language/localization
navigation behavior
search behavior
header behavior
footer behavior
```

Material's own setup documentation is therefore the authoritative reference for those. ([GitHub Pages][6])

---

# THE WHOLE THING AS ONE MODEL

This is the model I want you to retain:

```text
mkdocs.yml
│
├── nav
│   │
│   ├── pages
│   ├── sections
│   ├── nested sections
│   └── external links
│
└── theme
    │
    ├── MkDocs theme system
    │
    └── Material
        │
        ├── palette
        │   ├── scheme
        │   ├── primary
        │   ├── accent
        │   ├── media
        │   └── toggle
        │
        ├── font
        │   ├── text
        │   └── code
        │
        ├── logo
        ├── favicon
        │
        ├── icon
        │
        └── features
            │
            ├── navigation.tabs
            ├── navigation.sections
            ├── navigation.expand
            ├── navigation.indexes
            ├── navigation.top
            ├── navigation.path
            ├── navigation.footer
            ├── navigation.instant
            ├── navigation.tracking
            ├── navigation.prune
            └── ...
```

And **now** your original configuration:

```yaml
theme:
  name: material
  palette:
    scheme: default
    primary: blue
```

is no longer a magical incantation.

You can read it:

```text
theme
│
└── use Material
    │
    └── palette
        │
        ├── use light scheme
        │
        └── make blue the primary color
```

And if you wanted to construct your own:

```yaml
theme:
  name: material

  palette:
    scheme: slate
    primary: deep purple
    accent: amber

  font:
    text: Roboto
    code: Roboto Mono

  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - navigation.top
    - navigation.footer
```

you now have the **semantic vocabulary** to understand what every major line is doing.

---

## One final distinction that will save you a lot of confusion

There are **three different kinds of "navigation"** floating around:

```text
1. nav:
```

**MkDocs configuration**

> What pages/sections exist in my navigation, and in what hierarchy?

```text
2. theme.features:
     - navigation.tabs
```

**Material configuration**

> How should Material display/behave around that navigation?

```text
3. toc
```

**Table of contents**

> What headings exist *inside the current Markdown page?*

Those are three different systems.

If your page contains:

```markdown
# YAML

## Scalars

## Sequences

## Mappings
```

that's the **table of contents**.

If `mkdocs.yml` contains:

```yaml
nav:
  - YAML:
      - Basics: basics.md
      - Advanced: advanced.md
```

that's the **site navigation**.

And if you add:

```yaml
theme:
  features:
    - navigation.tabs
```

that's **Material changing the presentation/behavior of that site navigation**.

That separation is the foundation for genuinely understanding `nav` and `theme`, rather than just copying configuration snippets.

[1]: https://www.mkdocs.org/user-guide/configuration/?utm_source=chatgpt.com "Configuration - MkDocs"
[2]: https://www.mkdocs.org/user-guide/writing-your-docs/?utm_source=chatgpt.com "Writing Your Docs - MkDocs"
[3]: https://www.mkdocs.org/user-guide/choosing-your-theme/?utm_source=chatgpt.com "Choosing Your Theme - MkDocs"
[4]: https://www.mkdocs.org/user-guide/customizing-your-theme/?utm_source=chatgpt.com "Customizing Your Theme - MkDocs"
[5]: https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/?utm_source=chatgpt.com "Changing the colors - Material for MkDocs"
[6]: https://squidfunk.github.io/mkdocs-material/setup/?utm_source=chatgpt.com "Setup - Material for MkDocs"
[7]: https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/?utm_source=chatgpt.com "Setting up navigation - Material for MkDocs"
[8]: https://squidfunk.github.io/mkdocs-material/setup/setting-up-the-footer/?utm_source=chatgpt.com "Setting up the footer - Material for MkDocs"
[9]: https://squidfunk.github.io/mkdocs-material/setup/changing-the-fonts/?utm_source=chatgpt.com "Changing the fonts - Material for MkDocs"
[10]: https://squidfunk.github.io/mkdocs-material/setup/changing-the-logo-and-icons/?utm_source=chatgpt.com "Changing the logo and icons - Material for MkDocs"
[11]: https://squidfunk.github.io/mkdocs-material/upgrade/?utm_source=chatgpt.com "How to upgrade - Material for MkDocs"
