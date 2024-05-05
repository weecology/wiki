
## Weecology Wiki

The weecology wiki is where we collaboratively document how to navigate life in the lab (and beyond) from choosing and pursuing a career path, to writing and reviewing papers, to using the high performance computing system and the printer.

This is the GitHub repository that stores all of the relevant code and content. The wiki is available at <https://wiki.weecology.org>

The site is built using [Wowchemy](https://wowchemy.com/), the [Wowchemy Project Documentation template](https://github.com/wowchemy/hugo-documentation-theme), and the [Hugo static site generator](https://gohugo.io/).

## Editing the wiki

### Editing an existing page

All of the wiki content is stored in markdown files in the `content/docs/` directory.

* To edit from the GitHub repository find the associated page click the pencil icon to edit
* To edit from the website click the Edit this page link at the bottom of the page you want to edit and you’ll be redirected to the GitHub repository to edit

In both cases when you are done editing add a descriptive commit message at the bottom of the page, select `Commit directly to the main branch`, and click the `Commit changes` button. If you want someone else to look at your changes before they are added to the wiki you can select `Create a new branch for this commit and start a pull request` instead and click the `Propose changes` button and someone will review the changes before merging them into the wiki.

### Adding a new page

1. In the GitHub repo navigate to the appropriate folder in `content/docs/` and click `Add file` and then `Create new file`.
2. Add the following YAML to the top of the page and edit the text of the `title` and `summary` fields to match the new page.

```yaml
---
title: "Submitting a manuscript"
summary: >
  How to submit a manuscript to a journal
---
```

3. Add the text for the page
4. A descriptive commit message at the bottom of the page, and select `Commit directly to the main branch` (if you don't want the changes reviewed) or `Create a new branch for this commit and start a pull request` (if you want the changes reviewed), and click the `Commit changes` or `Propose changes` button.

### Adding a new topic

1. In the GitHub repo navigate to the appropriate folder in `content/docs/` and click `Add file` and then `Create new file`
2. Type the name of the directory you want to create for the topic, then `/`, and then `_index.md`
3. Add the following text to the page and the text of the `title`, `summary`, and the paragraph description to match the new topic.

```
---
title: "Career Guidance"
summary: >
  What career is right for you and how do you get there 
---

There are lots of different careers available for folks with science PhDs and former members of weecology work in lots of different careers including faculty at both research and teaching colleges/universities, software developers and data scientists (in both academia and industry), scientists and managers at NGOs, and a goat breader. This is a set of resources to help you figure out what type of career you want and how to get there.

{{< list_children >}}
```

4. Commit the changes as described above
5. You can now add a new page to the new topic

## License

All content is licensed [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/) and the Wowchemy infrastructure is licensed [MIT](https://mit-license.org/) by Wowchemy.
