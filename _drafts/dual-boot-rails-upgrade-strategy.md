try grouping the next rails version in a `group :next do` block

I use the rails_next gem because it comes with a couple nice to have features like an outdated dependency checker. This checker will check your dependencies against a specified version of rails and tell what will become outdated. Another nice feature is a deprecation warning shitlist tracker.

Add this line to your application's Gemfile:
`gem 'next_rails'`

And then install the gem with `bundle install.
`$ bundle install`

Finally to finish setup run:
`$ bundle exec next --init`

This creates 2 new files: Gemfile.next and a Gemfile.next.lock. Doing it this way you keep all the logic in the original Gemfile and you don't have to worry about other developers forgetting to modify a dependency in 2 different Gemfiles.

next thing is in the gemfile change your rails dependency line to:
```diff
diff --git a/Gemfile b/Gemfile
-gem 'rails', '~> 5.2.2'
+if next?
+ gem 'rails', '~> 6.0.0.beta3'
+else
+ gem 'rails', '~> 5.2.2'
+end
```


Install the new rails gem. Note: this might take some finangeling to work depending on your gems.

`next bundle update`
