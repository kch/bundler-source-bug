This demo app shows a bug with bundler where having one gem specified with a source and another with a path will cause bundler not to find gems with a source specified.

Start a local gemserver:

```
$ gem server -d gemserver 1>/dev/null 2>&1 &
```

Bundle:

```
$ bundle
Fetching source index from http://0.0.0.0:8808/
Fetching gem metadata from https://rubygems.org/...........
Fetching version metadata from https://rubygems.org/..
Resolving dependencies...
Using bar 0.0.1 from source at localgems
Installing foo 0.0.1
Using bundler 1.8.2
Bundle complete! 2 Gemfile dependencies, 3 gems now installed.
Use `bundle show [gemname]` to see where a bundled gem is installed.
```

Try to exec something:

```
$ bundle exec irb
Could not find gem 'foo (>= 0) ruby' in rubygems repository http://0.0.0.0:8808/.
Source does not contain any versions of 'foo (>= 0) ruby'
Run `bundle install` to install missing gems.
```

:(

comment out the `gem 'bar'…` line in the Gemfile, `bundle` again, try exec again:

```
$ bundle
Fetching source index from http://0.0.0.0:8808/
Fetching source index from http://0.0.0.0:8808/
Fetching gem metadata from https://rubygems.org/...........
Fetching version metadata from https://rubygems.org/..
Resolving dependencies...
Using foo 0.0.1
Using bundler 1.8.2
Bundle complete! 1 Gemfile dependency, 2 gems now installed.
Use `bundle show [gemname]` to see where a bundled gem is installed.
```

and it works:

```
$ bundle exec irb
>> 
```
