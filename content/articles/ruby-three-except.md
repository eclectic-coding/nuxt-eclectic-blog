---
title: Ruby 3.0 adds #except
date: 2020-10-04
published: true
tags: ['ruby', 'webdev']
series: false
cover_image: images/ruby-gems.jpg
canonical_rul: false
description: After the last four or five years of the promises of a new major version of Ruby (Ruby 3), it looks like this new version is set to release on December 25, 2020. This article will look at one new feature.
---
After the last four or five years of the promises of a new major version of Ruby (Ruby 3), it looks like this new version is set to release on December 25, 2020. This article will look at one new feature.

## Goals
There are three main goals for this future release.
- Speed: implementing its long-awaited async i/o “fibers” as a better way to control asynchronous threads.
- Concurrency: implementing “Ractors” (“Ruby Actors”) — similar to the way JavaScript offers background “web worker” scripts.
- Being Correct: Ruby 3 will ship with type signatures for its core libraries, available both for type checking as well as for enhancing future IDEs.

## New Hash method
Ruby 3 will add a new method for a hash `#except`, which returns a new hash.

Assuming the hash: `{food: "Burger", beverage: "Coffee", fries: "large"}`, in Rails 6, we can exclude an item like so:
```
{food: "Burger", beverage: "Coffee", fries: "large"}.except(:fries)
```
and it returns the hash: `{food: "Burger", beverage: "Coffee"}`

Currently in the latest version of Ruby, you can use `slice` to build the new hash you want, a different approach to yield the same results:
```
{food: "Burger", beverage: "Coffee", fries: "large"}.slice(:food, :beverage)
```
and it returns the hash: `{food: "Burger", beverage: "Coffee"}`

**Ruby 3**

Now with Ruby 3, you can use the new `#except` method: `{food: "Burger", beverage: "Coffee", fries: "large"}.except(:fries)`

Also, you can exclude mulitple items: `{food: "Burger", beverage: "Coffee", fries: "large"}.except(:fries, :food)`

Nice edition to the Ruby language with more to come. Stay tuned for more.

## Footnote
This has been fun. Leave a comment or send me a DM on [Twitter](http://twitter.com/EclecticCoding).

Shameless Plug: If you work at a great company, and you are in the market for a Software Developer with a varied skill set and life experiences, send me a message on [Twitter](http://twitter.com/EclecticCoding) and check out my [LinkedIn](http://www.linkedin.com/in/dev-chuck-smith).

