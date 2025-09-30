## The new LXQt Wiki

Based on [mkdocs-material](https://squidfunk.github.io/`mkdocs-material/), https://github.com/lxqt/lxqt/wiki/
and https://github.com/lxqt/pcmanfm-qt.wiki.

It does not display some documents meant mostly for the LXQt team.

Website: [lxqt-project.org/wiki/](https://lxqt-project.org/wiki/)

### Editing

See [lxqt-project.org/wiki/editing-wiki](https://lxqt-project.org/wiki/editing-wiki).

#### Merging PRs

Before merging edit PRs make sure that `update-wikis` is commented out in the workflow,
otherwise the new page will be overwritten during import. The modified page has to be copied
manually in the GH Wiki atm.

After editing the GH Wiki both workflows need to be run manually, with `update-wikis` enabled (uncommented).

## Files

* `update-wikis`: Script to import from GH wikis.
* `.github/workflows/deploy-material.yml`: GH action.
* `docs/mkdocs.yml`: Main config, exclude some pages, layout.
* `docs/lxqt.wiki`: Markdown files.
  * `index.md`: Home page of https://lxqt-project.org/wiki/.
  * `Home.md`: Home page of https://github.com/lxqt/lxqt/wiki/, not displayed int the new wiki.
  * `_assets`: Logos, css.
