<p align="center">
    <h2 align="center">Davide's blog - based on <a href="http://sergiokopplin.github.io/indigo/">Indigo Minimalist Jekyll Template</a></h2>
</p>

***

<p align="center">
    <b><a href="README.md#what-has-inside">What has inside</a></b>
    |
    <b><a href="README.md#setup">Setup</a></b>
</p>

## What has inside

- [Jekyll](https://jekyllrb.com/), [Sass](http://sass-lang.com/) and [SVG](https://www.w3.org/Graphics/SVG/)
- Google Speed: [98/100](https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Fdavide.dev);

## Setup

0. :star: to the project. :metal:
2. Fork the project [Indigo](https://github.com/sergiokopplin/indigo/fork)
3. Edit `_config.yml` with your data (check <a href="README.md#settings">settings</a> section)
4. Write some posts :bowtie:

If you want to test locally on your machine, do the following steps also:

1. Install [Jekyll](http://jekyllrb.com), [NodeJS](https://nodejs.org/) and [Bundler](http://bundler.io/).
2. Clone the forked repo on your machine
3. Enter the cloned folder via terminal and run `bundle install`
4. Then run `bundle exec jekyll serve --config _config.yml,_config-dev.yml`
5. Open it in your browser: `http://localhost:4000`
6. Test your app with `bundle exec htmlproofer ./_site`
7. Do you want to use the [jekyll-admin](https://jekyll.github.io/jekyll-admin/) plugin to edit your posts? Go to the admin panel: `http://localhost:4000/admin`. The admin panel will not work on GitHub Pages, [only locally](https://github.com/jekyll/jekyll-admin/issues/341#issuecomment-292739469).

---
[MIT](https://opensource.org/licenses/MIT) License © Davide Zordan

[MIT](http://kopplin.mit-license.org/) License © Sérgio Kopplin
