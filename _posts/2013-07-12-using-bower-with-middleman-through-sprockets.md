---
title: Using Bower With Middleman Through Sprockets
layout: post
---

[Bower](https://github.com/bower/bower) is a simple package management system for js/css similar to Bundler for Ruby or NPM for Node.

[Middleman](http://middlemanapp.com/) is a static website generator I use to create web app prototypes. Since it uses Ruby, Bundler, and Sprockets its fairly easy to migrate work done in Middleman to Ruby on Rails.

Normally I use Bundler to integrate with common js/css libraries such as jQuery or Bootstrap, but I also like to experiment with new js/css libraries, most of which aren’t bundled as Ruby Gems.

To get Bower to play nice with Middleman first you need to install your Bower packages in the root of your project. This will create a folder named bower_components. This folder won’t be in the Sprockets default search path, but it can be added by inserting the following line into config.rb

``` ruby
sprockets.append_path File.join "#{root}", "bower_components"
```

Now that Sprockets is set to search the bower_components directory you can use Sprockets to include the files using the following format

``` javascript
//= require component_folder/component_file.extension
```

Thanks to [Headcannon](https://github.com/headcanon) for the [Middleman Bower Template](https://github.com/headcanon/middleman-bower-template) which pointed me in the right direction.