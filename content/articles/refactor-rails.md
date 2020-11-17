---
title: Refactor Rails
date: 2020-08-14
published: true
tags: ['rails', 'webdev', 'ruby']
series: false
cover_image: images/rails.jpg
canonical_rul: false
description: As any one journeys through any profession, one thing holds true. Hopefully, the longer you work at it, the better your skills will develop, at least that should be the goal. As we look back at our code from months ago, we should see a progression of maturity.
---
As any one journeys through any profession, one thing holds true. Hopefully, the longer you work at it, the better your skills will develop, at least that should be the goal. As we look back at our code from months ago, we should see a progression of maturity. This blog post addresses my observations in my own journey as I reviewed my ongoing [Rails][RAILS] project.

So, my journey, well, has been an Eclectic one. You can read at your leisure about my [Passionate Journey][JOURNEY] for background. For the last two decades my hobby has been web development as a self taught developer, from HTML, to Joomla, to WordPress, and StudioPress' Genesis framework. Since starting at [Flatiron][] in August of last year I have been immersed in Ruby, Sinatra, and more specifically Ruby on Rails, which I love.

I have spent a lot of time trying to level up my skills as I look for that first Professional Development position. I have moved away from the endless full project tutorials to learning deeper small task from great sites like [GoRails][GORAILS] and [Drifting Ruby][DRIFT]. Through supporting Open Source, reading source code, and my own side projects I know I am progressing. So, I have set out to refactor a side project.

## Existing Project details
The side project is called [Your Congress][CONGRESS]. I wanted to create this project to hopefully provide a site that users can stay up to date with what is happening with their US Congress in Washington DC. The data is freely available from the Propublica Congress API. Right now the site includes:
- User authentication (not Devise) using tokens, email authentication, remember me link, and reset passwords.
- Includes pages for the Senate and the House
- Allows the user to create a follow list of Senators and Representatives, which includes all contact information and social media accounts.

In the future I plan to add:
- Searching
- Current Bills and voting
- Current Spending
- Committees

## Beginning Structure
I want to start by explaining my thought process, over two months ago, when I designed the backend of this full-stack Ruby on Rails web application.

First, I needed to access the [Members API][MEMBERS], which is the endpoint for both the House and the Senate. I could have created two API calls: House and Senate, using the `chamber` value and created two resources and two database tables, but I decided to not do that. Instead, I created a seed file that made two API calls, but wrote all the data to one table called `Members`.

To DRY out the views, I created a series of class methods, that I called in the seed file when the seed actions were complete, to build and save data strings into the database table in new columns. For instance:
- `calculate_age` method to use the date of birth from the database, and compare it to the current time, and write to the database table column a Members current age.
- `social_media_links` method to use the social media user accounts and build a usable URL
- `full_state_name` method to use the two character state abbreviation and output the complete state name
- `full_party_name` method to use the initial of the party returned by the API (i.e. R,D, ID) and output the complete party name
- `clickable_phone_number` method to convert the phone number string returned by the API and format to make the phone number clickable in the views
- `set_title_and_name` method to build a string with a `short_title`, `first_name`, and `last_name` to use in the views

It was these methods, that I logically concluded, if I created separate resource for Senators and Representatives, would have to be replicated and that is not very DRY.

**Problems**

As the application developed this approach created problems. I ended up creating controllers for Senators and Representatives, for routing and views. I realized the card for the index view was the same code so I created a partial which only worked if it was a `member` partial. So, the Members controller started to look real messy:
```ruby
def index
    @members = Member.all
    @representatives = Member.where(chamber: 'house')
    @senators = Member.where(chamber: 'senate')
 end
```
Overall, the application starting becoming unnecessarily messy. As I learned more and viewed other projects, I realized there was a better way.

## Refactoring Opportunities
To refactor there were a few tasks that I had to accomplish, which included creating complete resources for Senators and Representatives. This approach meant refactoring the class methods into helpers.

I am not going to review here every method refactor but I want to look specifically at `calculate_age` method as an example of my methodology.

The existing class method looked like so:
```ruby
def self.calculate_age
    Member.all.each do |member|
      now = Time.now.utc.to_date
      dob = member.date_of_birth.to_date

      age = now.year - dob.year - (now.month > dob.month || (now.month == dob.month && now.day >= dob.day) ? 0 : 1)

      member.update(age: age)
    end
end
```
This method read each row of the table, over 500 rows, and used the `date_of_birth` value to calculate the Members age, and then write the value to a new column.

The refactor uses a helper method that can be freely used in the views. I created `app/helpers/age_helper.rb`:
```ruby
module AgeHelper
  def age_helper(member_dob)
    now = Time.now.utc.to_date
    dob = member_dob.to_date

    now.year - dob.year - (now.month > dob.month || (now.month == dob.month && now.day >= dob.day) ? 0 : 1)

  end
end
```
In the HAML view I can use the helper like so:
```
= age_helper(senator.date_of_birth)
```
After I replaced all the class methods with helpers, I addressed a few remaining refactoring problems:
- Removed methods from seed file
- Removed additional database table columns that are do longer needed
- Rebuild the database based on the new design
- Completely removed the `Member` resource

## Still more to go
It is not totally complete. I still have one outstanding issue. Previously I had a Follow List in the Users Dashboard of all Senators and Representatives that the user had followed. This table previously used the Members resource which now on longer exist. I am currently working at implementing a Favorites / Likes list for both resources in one dashboard.

Eventually I will add a schedule of API updates based on the API update schedule. For instance the Bills data endpoint updates 6 times a day.

The bottom line is your code from past project should always look like "*what was I thinking*." Progress and maturing as a developer means we should always be ready to go back, refactor, and grow more.

## Footnote
This has been fun. Leave a comment or send me a DM on [Twitter](http://twitter.com/EclecticCoding).

Shameless Plug: If you work at a great company and you are in the market for a Software Developer with a varied skill set and life experiences, send me a message on [Twitter](http://twitter.com/EclecticCoding) and check out my [LinkedIn](http://www.linkedin.com/in/dev-chuck-smith).

[RAILS]: https://rubyonrails.org/
[JOURNEY]: passionate-journey.md
[FLATIRON]: https://flatironschool.com/
[GORAILS]: https://gorails.com/
[DRIFT]: https://www.driftingruby.com/
[CONGRESS]: https://yourcongress.co
[MEMBERS]: https://projects.propublica.org/api-docs/congress-api/members/
