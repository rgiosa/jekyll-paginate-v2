# Example 02::Category pagination
This example website contains information about sports cars and has a single main paginated front page that lists all entries in the system. It also contains separate paginated category pages that list only _Porche_ cars an another that lists only _Toyota_ cars.

<p align="center">
  <img src="https://raw.githubusercontent.com/sverrirs/jekyll-paginate-v2/master/examples/img/02-example-screenshot-main.png" />
</p>

The site is generated using the jekyll built in new command `jekyll new myblog` and it uses the [default `minima` theme](https://github.com/jekyll/minima).

After generating the pagination gem was installed using
```
gem install jekyll-paginate-v2
```

## Structure
The blog has one type of posts that can be found under the `cars/_posts` folder. This folder structure automatically assigns the category `cars` to all posts under it.

Users can then assign other categories to their posts by editing each post's front-matter as they see fit.

This site uses the same `_layouts/home.html` and `_includes/header.html` overrides as Example 01 does (please refer to that example project for details).

Additionally this project adds additional pagination pages `toyota/index.md`, `categories/porsche.md`, `categories/byname.md`.

> It is not advisable to have multiple pagination pages stored directly under the main site root. It makes it easy to mis-configure resulting in overwritten pagination pages unless developers remember to specify a unique pagination: permalink: '' override for each pagination page.


### Toyota/index.md
Simple category pagination page that only paginates cars having the category `sports` assigned to them. The configuration is simple being only

``` yml
---
layout: home
title: Toyota
pagination: 
  enabled: true
  category: sports
---
```

This configuration draws the paging permalink structure from the `_config.yml` file. Therefore the generated paging urls will look like:

```
Page 1: http://localhost:4000/toyota/
Page 2: http://localhost:4000/toyota/page/2/
Page N: http://localhost:4000/toyota/page/N/
```

### Categories/porsche.md
This is a more complex category pagination including a permalink override for both the main page url and the pagination pages. The configuration for the page looks like this

``` yml
---
layout: home
title: Only Porsche
permalink: /porsches/
pagination: 
  enabled: true
  category: porsche
  permalink: /:num/
---
```

The paging will only list posts in the category `porsche`. The main difference here is that the url structure generated will look different because of the combined page and category permalink config in the main front-matter.

```
Page 1: http://localhost:4000/porsches/
Page 2: http://localhost:4000/porsches/2/
Page N: http://localhost:4000/porsches/N/
```

### Categories/byname.md
This pagination page expands on the the first two examples by adding a custom sorting for the category pagination. It displays all cars but by alphabetical order by Car name.

This configuration is achieved with the following front-matter configuration
---
layout: home
title: By Name
permalink: /name/
pagination: 
  enabled: true
  category: cars
  permalink: /:num/
  sort_field: 'title'
  sort_reverse: false
---

The paging url structure will look similar to the example before

```
Page 1: http://localhost:4000/name/
Page 2: http://localhost:4000/name/2/
Page N: http://localhost:4000/name/N/
```

## Setup Configuration
The gem is added to the `_config.yml` file under
``` yml
gems:
  - jekyll-paginate-v2
```

as well as to the `Gemfile` into the main loop
``` ruby
group :jekyll_plugins do
  gem "jekyll-paginate-v2"
  gem "jekyll-feed"
end
```

At this point is is advisable to delete the `Gemfile.lock` file to clear out any potential issues with gem caching and dependency issues (no worries this file will be auto generated for you again).

## Configuring the pagination

Add the pagination configuration to `_config.yml`

``` yml
# Pagination Settings
pagination:
  enabled: true
  per_page: 3
  permalink: '/page/:num/'
  title_suffix: ' - page :num'
  limit: 0
  sort_field: 'date'
  sort_reverse: true
```


Due to the way the entries in the blog utilize multiple categories it is also good to explicitly state the permalink format to avoid excessive nesting of the post pages. So place the following line into the `_config.yml` file as well

``` yml
# Produces a cleaner folder structure when using categories
permalink: /:year/:month/:title.html
```

Try building the site yourself using `jekyll build` or `jekyll serve`.

Cheers :heart: