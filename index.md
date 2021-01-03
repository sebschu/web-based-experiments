---
layout: home
title: Running web-based experiments
nav_exclude: true
seo:
  type: Course
  name: Running web-based experiments
---

# {{ site.title }}
{: .mb-2 }
{{ site.description }}
{: .fs-4 .fw-300 }

## Schedule

_(all times are PST)_

1. 9:00-10:30: **Introduction**
  - General introduction
  - How do experiments on Mechanical Turk and Prolific work?
  - What kind of experiments can be run on Prolific?
  - Using GitHub for research projects
  - Preregistration and open science

2. 10:30-10:40: **Break**

3. 10:40-12:30: **Tutorials**
  - GitHub
  - Preregistering an experiment
  - Modifying an existing experiment

4. 12:30-1:00: **Lunch break**

5. 1:00-2:00: **Tutorials (continued)**
  - Posting the experiment
  - Testing the experiment
  - Downloading and visualizing the data
 
 6. 2:00-2:15 **Final discussion and Q&A**
  



## Just the Class

Just the Class is a GitHub Pages template developed for the purpose of quickly deploying course websites. In addition to serving plain web pages and files, it provides a boilerplate for:

- a [course calendar](calendar.md),
- a [staff](staff.md) page,
- and a weekly [schedule](schedule.md).

Just the Class is built on top of [Just the Docs](https://github.com/pmarsceill/just-the-docs), making it easy to extend for your own special use cases while providing sane defaults for most everything else. This means that you also get:

- automatic [navigation structure](https://pmarsceill.github.io/just-the-docs/docs/navigation-structure/),
- instant, full-text [search](https://pmarsceill.github.io/just-the-docs/docs/search/) and page indexing,
- and a small but powerful set of [UI components](https://pmarsceill.github.io/just-the-docs/docs/ui-components) and authoring [utilities](https://pmarsceill.github.io/just-the-docs/docs/utilities).

## Getting Started

Getting started with Just the Class is simple.

1. Create a [new repository based on Just the Class](https://github.com/kevinlin1/just-the-class/generate).
1. Update `_config.yml` and `index.md` with your course information.
1. Configure a [publishing source for GitHub Pages](https://help.github.com/en/articles/configuring-a-publishing-source-for-github-pages). Your course website is now live!
1. Edit and create `.md` [Markdown files](https://guides.github.com/features/mastering-markdown/) to add your content.

For a few open-source examples, see the following course websites and their source code.

- [CSE 390HA](https://courses.cs.washington.edu/courses/cse390ha/20au/) is an example of a single-page website: [source code](https://gitlab.cs.washington.edu/cse390ha/20au/website).
- [CSE 143](https://courses.cs.washington.edu/courses/cse143/20au/) hosts an entire online textbook with full-text search: [source code](https://gitlab.cs.washington.edu/cse143/20au/website).

Continue reading to learn how to setup a development environment on your local computer. This allows you to make incremental changes without directly modifying the live website.

### Local development environment

Just the Class is built for [Jekyll](https://jekyllrb.com), a static site generator. View the [quick start guide](https://jekyllrb.com/docs/) for more information. Just the Docs requires no special Jekyll plugins and can run on GitHub Pages' standard Jekyll compiler.

1. Follow the GitHub documentation for [Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll).
1. Start your local Jekyll server.
```bash
$ bundle exec jekyll serve
```
1. Point your web browser to [http://localhost:4000](http://localhost:4000)
1. Reload your web browser after making a change to preview its effect.

For more information, refer to [Just the Docs](https://pmarsceill.github.io/just-the-docs/).
