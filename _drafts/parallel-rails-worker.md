---
layout: post
title:  "Parallel Workers in Rails"
categories: Ruby
---
> @dhh suggested the possibility of adapting the number of workers to the number of tests. E.g: 30 tests, 1 worker. 60 tests, 2 workers, etc. I think it would be great and it would be totally feasible with this implementation. I'd leave that for a future iteration though.

https://github.com/rails/rails/pull/42761#issuecomment-879020026

I got the rails vagrant box installed and then was able to run the entire test suite.

I'm surprised it only took about 60 seconds to run all 2200 tests.
