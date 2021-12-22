Rake task should fail if minimum score is too low #214

Really good first commit

https://github.com/whitesmith/rubycritic/issues/214

https://ruby-doc.org/core-2.2.0/Kernel.html#method-i-exit

https://www.cyberciti.biz/faq/bash-get-exit-code-of-command/


https://github.com/whitesmith/rubycritic/pull/396/files


inside bin/rubycritic we find the actual thing that produces the exit code. this
file is what is installed on your path when you run the `$ rubycritic` command. Because the exit
code is found here and not in the rake task, the rake task is always returning 0.

I think a proper way to fix this is to follow what this file is doing and exit
```ruby
# Always look in the lib directory of this gem
# first when searching the load path
$LOAD_PATH.unshift File.expand_path('../lib', __dir__)

require 'rubycritic/cli/application'

exit RubyCritic::Cli::Application.new(ARGV).execute
```

it worked! i can't believe it worked! i'm a fucking genius!!!!!!

ah, but it was a false hope. it flat out exits from the rake command if you try to
run more than one task. A common pattern to run rubocop, your unit tests, and rubycritic
all from one rake task.

[Rubocop handles it like this:](https://github.com/rubocop/rubocop/blob/master/lib/rubocop/rake_task.rb#L43)
```ruby
def run_cli(verbose, options)
  # We lazy-load RuboCop so that the task doesn't dramatically impact the
  # load time of your Rakefile.
  require 'rubocop'

  cli = CLI.new
  puts 'Running RuboCop...' if verbose
  result = cli.run(options)
  abort('RuboCop failed!') if result.nonzero? && fail_on_error
end
```

[reek does the same thing](https://github.com/troessner/reek/blob/master/lib/reek/rake/task.rb#L108).
```ruby
def run_task
  puts "\n\n!!! Running 'reek' rake command: #{command}\n\n" if verbose
  system(*command)
  abort("\n\n!!! Reek has found smells - exiting!") if sys_call_failed? && fail_on_error
end
```

What is this `fail_on_error` method? Ah, [for reek](https://github.com/troessner/reek/blob/master/lib/reek/rake/task.rb#L60) it's an option flag for the Rake task to exit when a smell fails.

> Whether or not to fail Rake when an error occurs (typically when smells are found).

This is a good default so we don't break the expected behavior for everyone depending on this.

It's common i've seen to have the failures exit out of a multi command rake task.
But if everything passes - then it should continue on to the next command.


Github Actions doesn't support allowing failures https://github.com/actions/toolkit/issues/399

---
Simplecov is installed >= 0.17.0
at v 0.19.0 ruby 2.4 support was dropped.

`::SimpleCov::Result#from_hash` changed
https://github.com/simplecov-ruby/simplecov/issues/920
