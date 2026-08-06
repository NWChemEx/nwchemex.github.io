---
title: Contributing to the NWChemEx Community Website
author: ryan_richard
tags: website markdown
classes: wide
---

So you want to contribute to the NWChemEx Community website? That's awesome!!!
The point of this tutorial is to give you a crash course in writing content for
the website.

# Making an Author Profile

If this is your first time contributing you will need to update the website
author list. The list lives in `_data/authors.yml`. If you are already on the
list skip this step. It is recommended you simply copy/paste one of the existing
author profiles and change it appropriately. The layout is roughly:

```yaml
# Content in <> are placeholders which you should fill in
<tag>:
  name     : <your_name_as_you_want_it_displayed>
  avatar   : <path_to_an_image_which_should_be_used_as_your_avatar>
  bio      : <a_quick_blurb_about_who_you_are>
  location : <where_are_you_based_out_of>
  email    : <how_should_people_contact_you_by_email>
  links: # These are meant for linking to social media, personal websites
    - label : <name_of_link>
      icon  : <path_to_icon>
      url   : <where_the_link_should_take_them>
```

Take note of the value you use for `<tag>`, that's going to be needed to
associate posts and tutorials with you.

Please keep authors in alphabetical order by last name, for ease of finding
them.
{: .notice--warning}

# Creating a Post/Tutorial

For the most part the considerations for creating a post (news, update, and 
shout-outs) and a tutorial are similar so we will discuss them together created
following the same steps. They primarily differ in:

1. where the content lives (posts live in `_posts` and tutorials in
   `_tutorials`),
2. how they are named (posts should be named `YEAR-MONTH-DAY-title.md`,
   tutorials should just be labeled with their titles)

## Front Matter

Once you know where to put the file, and what to name it, we have to start
writing it. All posts and tutorials should minimally contain the following front
matter:

```markdown
---
title: <the_title_of_the_page>
author: <the_tag_associated_with_your_author_profile>
tags: <a_series_of_space_separated_keywords>
classes: wide # Should only be present when not using `toc: true` (vide infra)
---
```

Tags which are meant to be multiple keywords, e.g., `NWChemEx Community` should
be hyphenated, i.e. `NWChemEx-Community`.
{: .notice}

Some other useful front-matter options you may want to include are:

```markdown
toc: true     # Creates a table of contents from section headings
```

## Body

After the front matter you write the actual content of your post/tutorial using
markdown, specifically
[GitHub Flavored Markdown](https://github.github.com/gfm/), and the
[Liquid templating language](https://shopify.github.io/liquid/). We assume that
most readers will already be familiar with GitHub Flavored Markdown (GFM) from
their other interactions with GitHub, if not there already exists a number of
tutorials and cheat sheets; some examples:

- [Official GitHub Tutorial](https://docs.github.com/articles/markdown-basics)
- [markdown-cheatsheet](https://github.com/im-luka/markdown-cheatsheet)

Readers of this tutorial are likely less familiar with Liquid. Generally,
speaking, Liquid is used for slightly fancier things than GFM. One such thing
is for including notes and warnings

```markdown
This is a generic notice
{: .notice}

This is an informational notice
{: .notice--info}


This is a warning
{: .notice--warning}
```

This is a generic notice
{: .notice}

This is an informational notice
{: .notice--info}


This is a warning
{: .notice--warning}
