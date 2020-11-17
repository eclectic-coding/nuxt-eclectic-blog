---
title: Rails Multiple Seed Files
date: 2020-10-11
published: true
tags: ['rails', 'database', 'webdev']
series: false
cover_image: images/rails-seeds.jpg
canonical_rul: false
description: If you have a larger Rails project, or a project with many resources, managing one database seed file can get out of hand. In the name of DRY-ing out code, this article walks you through how to abstract your seed data into multiple files.
---

If you have a larger Rails project, or a project with many resources, managing one database seed file can get out of hand. In the name of DRY-ing out code, this article walks you through how to abstract your seed data into multiple files.

So, the approach is to create a new directory: `db/seeds`, and had your seed files in this location. I think the best approach would be to separate your concerns in your seed files. Create multiple seed files to manage easier each resource and place them in this new directory.

When you execute `rails db:migrate`, Rails will only run the file: `db/seeds.rb`. To fix this, we will use Ruby to parse the new directory for files. So, in the default `db/seeds.rb` file add the following:

```ruby
Dir[File.join(Rails.root, "db", "seeds", "*.rb")].sort.each do |seed|

  puts "seeding - #{seed}. loading seeds, for real!"
  
  load seed
end
```
Now, when you run the seed command, it will parse through all of your files and print to the command line exactly which file is loading.

## Footnote

This has been fun. Leave a comment or send me a DM on [Twitter](http://twitter.com/EclecticCoding).

Shameless Plug: If you work at a great company, and you are in the market for a Software Developer with a varied skill set and life experiences, send me a message on [Twitter](http://twitter.com/EclecticCoding) and check out my [LinkedIn](http://www.linkedin.com/in/dev-chuck-smith).
