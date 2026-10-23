# Nav and Theme 2
> **`nav` and `theme` are not two flat lists of settings. They are two different configuration structures with different grammars.**

So let's treat them like a language.

---

# 1. The "alphabet" of `nav`

Core MkDocs `nav` has a surprisingly small fundamental grammar. The official MkDocs configuration reference confirms that `nav` defines the format/layout of global navigation, with internal pages, nested sections, and external links. ([MkDocs][1])

At its simplest:

```yaml
nav:
  - item
  - item
  - item
```

`nav` is a **YAML sequence**.

An item can fundamentally be:

### A. A page

```yaml
- Home: index.md
```

### B. A section containing more items

```yaml
- Guides:
    - Installation: installation.md
    - Configuration: configuration.md
```

### C. An external link

```yaml
- MkDocs: https://www.mkdocs.org/
```

And those can recursively contain one another.

That's the grammar.

---

# 2. Think of `nav` as a recursive language

This is the part that was missing.

Conceptually:

```text
NAV
│
└── ITEM
     │
     ├── PAGE
     │    ├── label → file
     │    └── file
     │
     ├── SECTION
     │    └── ITEM
     │         └── ITEM
     │              └── ...
     │
     └── LINK
          ├── label
          └── URL
```

So:

```yaml
nav:
  - Home: index.md
```

is:

```text
NAV
└── PAGE
```

While:

```yaml
nav:
  - Guides:
      - Installation: installation.md
      - Configuration: configuration.md
```

is:

```text
NAV
└── SECTION
    ├── PAGE
    └── PAGE
```

And:

```yaml
nav:
  - Guides:
      - Configuration:
          - YAML: yaml.md
          - Markdown: markdown.md
```

is:

```text
NAV
└── SECTION
    └── SECTION
        ├── PAGE
        └── PAGE
```

**That's the mechanism.**

You're not memorizing a "nav tree."

You're learning a structure in which a **section can contain navigation items, and those items can themselves be sections.**

---

# 3. Now you can generate your own `nav`

Once you understand that grammar, you can invent this:

```yaml
nav:
  - Home: index.md

  - Learn:
      - Markdown: markdown.md
      - YAML:
          - Scalars: yaml/scalars.md
          - Sequences: yaml/sequences.md
          - Mappings: yaml/mappings.md

  - Projects:
      - Project A:
          - Overview: projects/a/index.md
          - Setup: projects/a/setup.md
      - Project B:
          - Overview: projects/b/index.md
          - Reference: projects/b/reference.md

  - External:
      - MkDocs: https://www.mkdocs.org/
      - Material: https://squidfunk.github.io/mkdocs-material/
```

You didn't memorize that configuration.

You **generated it from the grammar**.

That is what you were asking for.

---

# 4. `theme` is fundamentally different

Now we get to the more complicated one.

Core MkDocs defines:

```yaml
theme:
```

as a configuration object that selects a theme **and passes theme-specific configuration to it**. MkDocs itself defines things such as `name`, `locale`, `custom_dir`, and `static_templates`; additional keys belong to the selected theme. ([MkDocs][1])

So:

```yaml
theme:
  name: material
```

means:

```text
THEME
│
└── NAME
    └── MATERIAL
```

But once you say:

```yaml
name: material
```

you're effectively saying:

> "The rest of the vocabulary inside this `theme:` object can now include Material's vocabulary."

That's why `theme` cannot have one universal list of children.

---

# 5. The actual architecture

Think of it like this:

```text
mkdocs.yml
│
├── NAV
│
│   └── MkDocs defines the grammar
│
└── THEME
    │
    ├── MkDocs defines the theme interface
    │
    └── selected theme
         │
         └── Material
              │
              └── Material defines additional vocabulary
```

This is explicitly how MkDocs describes it: theme-specific keywords are allowed under `theme`, and you consult the documentation for the theme you're using. ([MkDocs][1])

So asking:

> "What are all the possible children of `theme`?"

has **two answers**:

### Core MkDocs

```yaml
theme:
  name:
  locale:
  custom_dir:
  static_templates:
```

### Material

Then Material adds its own configuration vocabulary.

---

# 6. Now let's map Material's vocabulary

Material's current documentation organizes its customization into areas including **navigation, colors, fonts, logo/icons, search, header, footer, analytics, repository integration, and more**. ([GitHub Pages][2])

A useful structural map is:

```text
theme:
│
├── name
│
├── palette
│   ├── scheme
│   ├── primary
│   ├── accent
│   ├── toggle
│   └── media
│
├── font
│   ├── text
│   └── code
│
├── logo
│
├── favicon
│
├── icon
│   ├── logo
│   ├── repo
│   ├── alternate
│   ├── previous
│   ├── next
│   └── ...
│
└── features
    └── FEATURE FLAGS
```

**But there is an important qualification:** Material's configuration surface is larger than this simplified map, and some behavior is configured outside `theme`, such as `extra`, Markdown extensions, plugins, CSS, and JavaScript. The Material documentation's setup/reference is therefore the actual authoritative catalogue. ([GitHub Pages][2])

---

# 7. `palette` — now let's REALLY explain it

Your original example:

```yaml
theme:
  name: material
  palette:
    scheme: default
    primary: blue
```

Let's parse it mechanically.

```text
theme
│
└── palette
```

`palette` is a **mapping**.

Inside it:

```text
palette
│
├── scheme
│
├── primary
│
└── accent
```

Those are different properties.

Material currently documents two color schemes:

```text
default → light
slate   → dark
```

and separate `primary` and `accent` color settings. ([GitHub Pages][3])

---

# 8. `primary` is not a YAML concept

This is another crucial distinction.

YAML doesn't know what this means:

```yaml
primary: blue
```

YAML merely understands:

```text
KEY → VALUE
```

Material gives the key `primary` its meaning.

So:

```yaml
primary: blue
```

means:

```text
Material's property:
primary

Value:
blue
```

Material then interprets `blue` as one of its supported color names.

Material's primary color affects things such as the header, sidebar, text links and other interface components. ([GitHub Pages][3])

---

# 9. What values can `primary` accept?

This is exactly the "alphabet" you're asking for.

Material's documented predefined color palette includes names such as:

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

The available choices depend on the particular property; Material documents the primary and accent palettes separately. ([GitHub Pages][3])

So:

```yaml
primary: blue
```

is legal.

And:

```yaml
primary: deep purple
```

is also legal.

But:

```yaml
primary: banana
```

is not a Material predefined color.

**That's the kind of information a configuration reference should give you.**

---

# 10. `accent`

Material also has:

```yaml
accent: orange
```

So:

```yaml
palette:
  primary: blue
  accent: orange
```

means:

```text
palette
│
├── primary → blue
│
└── accent → orange
```

Material uses the accent color for interactive/highlight elements. ([GitHub Pages][3])

And no, Material doesn't have a standard:

```yaml
secondary:
```

equivalent in this palette configuration.

---

# 11. `scheme`

Then:

```yaml
scheme: default
```

is another key/value pair.

The value must be one of Material's supported schemes:

```text
default
slate
```

So:

```yaml
palette:
  scheme: slate
```

means:

> Use the dark color scheme.

---

# 12. But then something fascinating happens

You can turn:

```yaml
palette:
  scheme: default
  primary: blue
```

into:

```yaml
palette:

  - scheme: default
    primary: blue
    accent: orange

  - scheme: slate
    primary: blue
    accent: orange
```

**The YAML data type changed.**

Before:

```text
palette → mapping
```

After:

```text
palette → sequence
              │
              ├── mapping
              └── mapping
```

Why?

Because Material interprets multiple palette entries as multiple selectable configurations. Its documentation explicitly shows the palette becoming a list when configuring a light/dark toggle. ([GitHub Pages][3])

That is **configuration grammar in action**.

---

# 13. `toggle` belongs to a palette entry

For example:

```yaml
palette:

  - scheme: default
    toggle:
      icon: material/brightness-7
      name: Switch to dark mode

  - scheme: slate
    toggle:
      icon: material/brightness-4
      name: Switch to light mode
```

Now the structure is:

```text
palette
│
├── ITEM 1
│   ├── scheme
│   └── toggle
│       ├── icon
│       └── name
│
└── ITEM 2
    ├── scheme
    └── toggle
        ├── icon
        └── name
```

Material documents `toggle` as part of this palette configuration. ([GitHub Pages][3])

Again, you're no longer memorizing snippets.

You're learning **which object can contain which other objects**.

---

# 14. Now `features`

This:

```yaml
theme:
  features:
    - navigation.tabs
    - navigation.top
```

has a completely different grammar.

`features` is a **sequence of strings**.

Think:

```text
features
│
├── "navigation.tabs"
└── "navigation.top"
```

Each string is a **feature flag**.

Material recognizes the feature names and activates the corresponding behavior.

For example:

```yaml
- navigation.tabs
```

activates navigation tabs.

```yaml
- navigation.sections
```

activates section grouping.

```yaml
- navigation.expand
```

expands collapsible subsections by default.

```yaml
- navigation.top
```

enables the back-to-top behavior.

```yaml
- navigation.path
```

adds navigation breadcrumbs.

These are all documented Material feature flags. ([GitHub Pages][4])

---

# 15. And here's the entire `navigation.*` vocabulary

This is much closer to the thing you originally wanted.

Material currently documents navigation-related feature flags including:

```text
navigation.instant
navigation.instant.prefetch
navigation.instant.progress
navigation.instant.preview

navigation.tracking

navigation.tabs
navigation.tabs.sticky

navigation.sections
navigation.expand

navigation.path
navigation.prune

navigation.indexes
navigation.top

navigation.footer
```

These aren't arbitrary words.

They are **feature identifiers defined by Material**. ([GitHub Pages][4])

And they have relationships.

For example:

```text
navigation.tabs
└── navigation.tabs.sticky
```

means:

> `sticky` is a refinement of the tabs behavior.

Likewise:

```text
navigation.instant
├── navigation.instant.prefetch
├── navigation.instant.progress
└── navigation.instant.preview
```

means the latter features extend the instant-navigation system. Material's documentation explicitly describes those relationships. ([GitHub Pages][4])

---

# 16. This is the distinction you kept asking me for

You don't need to think:

> "What does `navigation.tabs` mean?"

in isolation.

You need to understand:

```text
theme
└── features
    └── list of Material feature identifiers
```

Therefore:

```yaml
theme:
  features:
    - navigation.tabs
```

is grammatically:

```text
THEME
  contains FEATURES

FEATURES
  contains a LIST

LIST
  contains the VALUE "navigation.tabs"
```

Material then interprets that value as:

> activate navigation tabs.

**That's the mechanism.**

---

# 17. `nav` and `theme.features` now fit together

Suppose you write:

```yaml
nav:
  - Home: index.md

  - Guides:
      - Installation: install.md
      - Configuration: config.md

  - Reference:
      - YAML: yaml.md
```

That creates the **information architecture**:

```text
Home
Guides
├── Installation
└── Configuration
Reference
└── YAML
```

Then:

```yaml
theme:
  name: material
  features:
    - navigation.tabs
    - navigation.sections
```

does **not create those pages**.

It tells Material how to **render the navigation structure you've already defined**.

Material's documentation explicitly says `navigation.tabs` renders top-level sections as tabs and `navigation.sections` renders top-level sections as groups in the sidebar. ([GitHub Pages][4])

So:

```text
             nav
              │
              ▼
       WHAT EXISTS
              │
              ▼
        Navigation tree
              │
              │
              ▼
     Material features
              │
              ▼
       HOW IT APPEARS
```

That is the relationship.

---

# 18. And now you can invent your own site

For example:

```yaml
site_name: My Documentation

nav:
  - Home: index.md

  - Tutorials:
      - Getting Started: tutorials/getting-started.md
      - MkDocs:
          - Navigation: tutorials/mkdocs-navigation.md
          - Themes: tutorials/mkdocs-themes.md

  - Reference:
      - Configuration: reference/configuration.md
      - Commands: reference/commands.md

theme:
  name: material

  palette:
    - scheme: default
      primary: indigo
      accent: amber
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode

    - scheme: slate
      primary: indigo
      accent: amber
      toggle:
        icon: material/brightness-4
        name: Switch to light mode

  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - navigation.top
    - navigation.path
    - navigation.footer
```

And now you can **read every structural level**:

```text
site_name
└── scalar

nav
└── sequence
    ├── page
    ├── section
    │   ├── page
    │   └── section
    │       ├── page
    │       └── page
    └── section
        └── page

theme
└── mapping
    ├── name → scalar
    ├── palette → sequence
    │   ├── mapping
    │   └── mapping
    └── features → sequence
        ├── string
        ├── string
        └── string
```

**THIS is the level of understanding you were asking for.**

Not:

> "Here's an example of `navigation.tabs`."

But:

> **What type of thing is `navigation.tabs`? Where can it legally occur? What contains it? What does the containing structure expect? What other members exist beside it? What values can those members take? What relationships exist between them?**

That's the difference between **copying configuration** and **speaking the configuration language**.

And the official references are now useful in a completely different way: [MkDocs Configuration](https://www.mkdocs.org/user-guide/configuration/) gives you the **core language**, while [Material's Setup/Reference](https://squidfunk.github.io/mkdocs-material/setup/) gives you the **Material vocabulary layered on top of it**. ([MkDocs][1])

If you want the **literal A–Z treatment**, the next step is to take **`nav` and `theme` separately and build a configuration grammar/reference for each: every key → accepted value types → allowed children → what each value does → combinations → conflicts → working examples.** That is the document you were asking for, rather than another tour of example sites.

[1]: https://www.mkdocs.org/user-guide/configuration/?utm_source=chatgpt.com "Configuration - MkDocs"
[2]: https://squidfunk.github.io/mkdocs-material/setup/?utm_source=chatgpt.com "Setup - Material for MkDocs"
[3]: https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/?utm_source=chatgpt.com "Changing the colors - Material for MkDocs"
[4]: https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/?utm_source=chatgpt.com "Setting up navigation - Material for MkDocs"
