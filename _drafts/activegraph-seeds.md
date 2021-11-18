---
layout: post
title:  "Seeding a neo4jrb Database"
categories: Ruby neo4j
---
[/railties/lib/rails/engine.rb](https://github.com/rails/rails/blob/9bb2512485d968a0d682e302b9c0831c43174a8f/railties/lib/rails/engine.rb#L564)
```ruby
# Load data from db/seeds.rb file. It can be used in to load engines'
# seeds, e.g.:
#
# Blog::Engine.load_seed
def load_seed
  seed_file = paths["db/seeds.rb"].existent.first
  run_callbacks(:load_seed) { load(seed_file) } if seed_file
end
```


[the task that creates rake tasks in neo4jrb](https://github.com/neo4jrb/activegraph/blob/master/lib/active_graph/tasks/migration.rake)
