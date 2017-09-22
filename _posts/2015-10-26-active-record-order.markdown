---
layout: post
title: "Active Record Order"
categories: "Flatiron&nbspSchool ordering database retrieval sort forms"
---

Today at Flatiron we completed a Ruby on Rails lab that allows a user to create and save blog posts. When creating a post, the user can enter a title as well as some content. Finally, the user can also select tags from a long list of checkboxes that are incorporated into our “new post” form. 

![before]({{ site.url }}/images/final_new_post_page.png)

However, when my pair programming partner and I  first checked our form in the browser, we noticed that the tag “adorable” appeared last when it should have appeared as the very first option! A closer look at our seed.rb file revealed that the “adorable” tag was created and persisted after all of the other tag objects, so its id was the highest out of all the tags. By default, the tag checkboxes in our form were ordered by id, from smallest to largest. 
 
Thus, this code...
```ruby
<%= f.collection_check_boxes :tag_ids, Tag.all, :id, :name %>
```

...created this outcome:
![before]({{ site.url }}/images/blog_categories_before.jpg)

We thought this was a really cool problem to fix, as it mimics real life. When you use any site that allows you to tag some content, it’s nice to be able to add your own tags and not just use the tags provided by the website itself. It’s also nice to be able to add the tag “sweet” after “awesome” but still see your tag options listed alphabetically. 

To figure out how to sort our tags, we visited our handy new tool, the Rails console. First we entered `Tag.all`, just to confirm we knew what that contained. Second, we typed ` Tag.all.methods.sort`. This provided us with a long list of all the methods that `Tag.all` could possibly receive. In this list I noticed our solution: `order`. [API Dock](http://apidock.com/rails/ActiveRecord/QueryMethods/order) revealed that `order` can take an argument, such as `:name`, and then organize your data by that attribute, instead of by id. We revised our code to include this new method, and that yielded the desired result!

Our new code...
```ruby
 <%= f.collection_check_boxes :tag_ids, Tag.all.order(:name), :id, :name %>
```

...created this ordered list of tags:
![before]({{ site.url }}/images/blog_categories_after.jpg)

When a class receives the method `order`, Active Record generates a SQL query that uses the `ORDER BY` clause, which we have used in past labs to decide if we want data to be retrieved in ascending or descending order:

```SQL
SELECT "tags".* FROM "tags"  ORDER BY "tags"."name" ASC
```

In other words, `order` tells SQL to retrieve items from the database in the sequence you specify. For more examples of how this method works, check out Ruby on Rails’ [own guide](http://guides.rubyonrails.org/active_record_querying.html#ordering), which also provides great examples of how `order` can be used to structure data retrieval by multiple attributes. 