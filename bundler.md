When calling one Ruby script from another using backticks, the nested script was
picking up the calling script's bundler environment.  I first tried to fix this
by manually loading the second scripts Gemfile:

```ruby
require "bundler/inline"

gemfile do
  eval(IO.read(File.expand_path('Gemfile', __dir__)), binding)
end
```

This still ran into errors.  My fix was to have the nested script delete the
`BUNDLER_GEMFILE` environment variable:

```ruby
ENV.delete('BUNDLER_GEMFILE')
`bundle install`
require 'bundler/setup'
```
