---
layout: post
title:  "Specifying Different Gem Sources"
categories: Ruby
---
# Specifying Different Gem Sources

You can't specify a source in a .gemspec file. The Ruby Gem and Bundler community
[agreed long ago](https://github.com/rubygems/rubygems/issues/2731#issuecomment-699558463)
that source specification would be handled entirely by bundler in the Gemfile.

This also gets a little into different source locations for gems.

/Gemfile
```ruby
source 'https://rubygems.org'

# Specify your gem's dependencies in webservices.gemspec
gemspec
```

/my_gem.gemspec
```ruby
require 'my_gem/version'

Gem::Specification.new do |spec|

  spec.add_dependency 'private_gem', '1.0.0'
end
```

You would normally install that gem on the command line
```
gem install private_gem:1.0.0 \
--source https://<USERNAME>:<TOKEN>@rubygems.pkg.github.com/<OWNER>
```

Then install your .gemspec with `bundle install`. Bundler first looks to see
if any gems on your LOCAL system matches the names of gems in the .gemspec.
