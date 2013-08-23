---
title: Using Multiple Gemfiles
layout: post
---

One of the major benefits of [Bundler](https://github.com/bundler/bundler) is being able to checkout a project and install its Gem dependencies using the ```bundle``` command. However, Gems that are only used locally shouldn't be added to the Gemfile because they unnecessarily alter Gem dependencies and in some cases create conflicts with project Gems.

I wanted users to be able to easily install my project's toolchain without creating conflicts so I decided to create a separate Gemfile named Gemfile.tools.

### Gemfile.tools
```ruby
source 'https://rubygems.org'

gem 'chef'
gem 'vagrant', '~>1.0.7'
gem 'knife-solo'
```

To install the Gems from this file you need to run

```bash
bundle install --gemfile Gemfile.tools
```

This will install the Gems and create a new file called Gemfile.tools.lock.

Finally to use the ```bundle exec``` command with the new Gemfile.tools you run

```bash
BUNDLE_GEMFILE=Gemfile.tools bundle exec ...
```

I find this to be a bit cumbersome so the other option is to install the Gems with binstubs

```bash
bundle install --gemfile Gemfile.tools --binstubs
```

Then you can access your gems through your project's bin directory

```bash
bin/command ...
```

By creating two separate Gemfiles I was able to harness the power of Bundler while keeping my project and toolchain Gems separate.