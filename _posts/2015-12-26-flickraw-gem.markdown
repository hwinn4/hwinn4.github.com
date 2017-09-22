---
layout: post
title: "Getting Started with the flickraw Gem"
categories: "gems Flickr APIs beginners Flatiron&nbspSchool"
---

For a recent code challenge I needed to use the Flickr API to find a bunch of images and incorporate them into the output of my program. Initially I wanted to use the official JavaScript Flickr API, which allows you to send a customized request to a URL endpoint, but later I decided I wanted to write my program in Ruby instead. Happily, I found the [flickraw gem](https://github.com/hanklords/flickraw), which made it simple to hit the same URL endpoint via Ruby!

To install the gem: ```gem install flickraw```

You will need a Flickr API key and secret, which you can apply for [here](https://www.flickr.com/services/api/misc.api_keys.html). You will need to set your key and secret on the FlickRaw object:

```
FlickRaw.api_key=ENV['flickraw_key']
FlickRaw.shared_secret=ENV['flickraw_secret']
```

Once you have your credentials, you can start exploring the flickraw methods that you can use to retrieve images from Flickr or post images to a Flickr account. Helpfully, these methods mimic the [official Flickr API methods](https://www.flickr.com/services/api/). For example, if you want to find photos of graffiti, you can write:

```ruby
flickr.photos.search(:text => 'graffiti')
```

This method takes a hash as an argument, and there are many other options you can pass into this method as well. If you only want to see images that are licensed for commercial and non-commercial reuse, you can filter your results by license 5. You can filter by [other](https://www.flickr.com/services/api/flickr.photos.licenses.getInfo.html) [licenses](https://www.flickr.com/creativecommons/) as well, depending on the purpose of your app. The "per_page" option allows you to define the number of results you get back from the API. The default is 100 images. 

```ruby
flickr.photos.search(:text => 'graffiti', :license => 5, :per_page => 50)
```

The ```flickr.photos.search``` method returns a variety of data about each image, including its id, secret, server, and farm, all of which you would need to retrieve the photo via its URL if using the official Flickr API. Conveniently, flickraw has a handful of methods that can build these URLs for you! You can use the [Flickr URL Helpers](https://github.com/hanklords/flickraw#flickr-url-helpers) to retrieve the URL for each image in the size most appropriate for your app, such as thumbnail, square, medium, etc. In the example below, I am iterating over 50 images and returning an array of their URLs for the square version of each image.

```ruby
images_data = flickr.photos.search(:text => 'graffiti', :license => 5, :per_page => 50)

image_urls = images_data.map do |image_data|
  FlickRaw.url_s(image_data)
end
```

Once you have a collection of URLs for images that you are legally licensed to use, the sky's the limit! 