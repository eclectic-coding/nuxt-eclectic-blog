---
title: Gridsome Pagination
date: 2020-04-09
published: true
tags: ['Gridsome', 'Coding']
series: false
cover_image: images/pagination.jpg
canonical_rule: false
description: When ever I set up a blog, there are a few features which are must haves, linkable taxonomy (i.e. tags or categories) and pagination. No one wants an eternal page. These tasks can be challenging but it is super easy in Gridsome.
---

When ever I set up a blog, there are a few features which are must haves, linkable taxonomy (i.e. tags or categories) and pagination. No one wants an eternal page. These tasks can be challenging but it is super easy in Gridsome.

## What is Gridsome
[Gridsome](https://gridsome.org/) is the [Gatsby](https://www.gatsbyjs.org/) alternative for [Vue.js](https://gridsome.org/) that aims to provide the tech stack to build blazing fast statically generated websites. It’s data-driven, using a GraphQL layer to get data from different sources in order to dynamically generate pages from it. <sup id="a1">[1](#f1)<!-- @IGNORE PREVIOUS: anchor --></sup> Adding pagination to this blog was the easiest implementation I have experienced. It requires basically three separate parts.
- GraphQL
- New Component
- Add CSS Styling

### GraphQL
In Gridsome, the GraphQL query handles the data collection. Where is a standard Vue.js application you might handle pagination in the Script section, via data and methods, in Gridsome all of the heavy lifting is done in the data later, which in this case in GraphQL. Notice below in my `page-query` there are three differences from the standard query:
- Declare a `$page` query variable
- Define the number of items `perPage`
- Add `@pagination` directive
- Include `totalCount` and the `pageInfo` section as shown below

```
query ($page: Int) {
  posts: allPost(perPage: 5, page: $page, filter: { published: 
    { eq: true }}) @paginate {
    totalCount
    pageInfo { 
      totalPages 
      currentPage 
      isFirst 
      isLast 
      } 
    edges { 
      node { 
      id 
      title 
      date (format: "MMM DD, YYYY")
      timeToRead 
      description 
      cover_image (width: 770, height: 380, blur: 10) 
      path tags { 
      id 
      title 
      path 
          } 
        } 
      } 
    } 
  }
  ```

### Add Component
Next task is to add the Pager component from Gridsome. In the script section:
```
import { Pager } from 'gridsome'

export default {
  components: {
    Pager
  }
 }
```
Then at the bottom of the template section add the `Pager` component:
`<Pager :info="$page.posts.pageInfo" />`

### Add Styling
Now, the output has no styling so we need to handle that with [properties](https://gridsome.org/docs/pagination/#pager-component) available on the Pager Component.
First to style the links, you can add a `:linkClass` to style the pagination links. But you will need to also include a second class to style the pagination container. So, my Pager element looks so:
```
<Pager :info="$page.posts.pageInfo" 
       linkClass="pager__link" 
       class="pager" />
```
My styling to match my theme looks so:
```
<style lang="scss">
  .pager {
    display: inline-block;
    width: 100%;
    text-align: center;

    &__link {
      color: var(--link-color);
      text-align: center;
      text-decoration: none;
      padding: .5rem 1rem;

      &:hover:not(.active) {
        background-color: var(--bg-content-color);
        border-radius: 5px;
        color: var(--link-color);
      }
    }
  }

  .active {
    background-color: var(--bg-content-color);
    border-radius: 5px;
  }

</style>
```
All Done!
Notice the styling? What I like about the [Gridsome Blog Starter](https://gridsome.org/starters/gridsome-blog-starter/) is that all the styles use SCSS and Block Element Modifier (BEM) naming convention, so my styling follows this convention.
Enjoy and I hope this helps.

<a id="f1">1</a>. Credit to [Building a blog with [Gridsome](https://alligator.io/vuejs/gridsome-blog/) by [Alex Morales](https://alligator.io/author/alex-jover-morales).[↩](#a1)<!-- @IGNORE PREVIOUS: anchor -->
