First clone this repo and switch to the `raw` branch:
```
$ git clone https://PATH/TO/blog blog
$ git fetch --all
$ git checkout raw
```

Install Icarus theme:
```
$ git clone https://github.com/digitalcraftsman/hugo-icarus-theme themes/icarus
$ cp -pr themes/icarus/exampleSite/data .
```

Then for development,
```
$ hugo server --buildDrafts --theme=icarus
```

Before building, first set up a [worktree](http://blog.jenkster.com/2016/02/git-for-static-sites.html) to `gh-pages`, which contains rendered HTML:
```
$ mkdir ../dist && git worktree add ../dist gh-pages
```

Then to build,
```
$ hugo --theme=icarus && cp -pr public/* ../dist/
```

Commit both directories (`blog/`’s `raw` branch and `dist/`’s `gh-pages` branch).