# FasterPath

This project "WILL BE" a rewrite of Ruby's STDLIB **Pathname** optimized for speed and performance.
I am considering making it a \*nix OS gem, but that may likely just be an early phase.  Windows support
can be added much later.

The primary **GOAL** of this project is to improve performance in the Rails environment as path relation
and file lookup is a huge bottleneck in performance.  As this is the case the path performance updates
will likely not be limited to just changing Pathname but also will be offering changes in related methods
and classes.

Users will have the option to write their apps directly for this library, or they can choose to either
refine or monkeypatch the existing library.  Refinements are narrowed to scope and monkeypatching will
be a sledge hammer ;-)

**NOTE**: Refinements and monkeypatch methods are highly likely to be changed and renamed pre version
0.1.0 so keep that in mind!

## Why

I did a check on Rails on what methods were being called the most and where the application spend
most of its time.  It turns out roughly 80% of the time spent and calls made are file Path handling.
This is shocking, but it only gets worse when handling assets.  **That is why we need to deal with
these load heavy methods in the most efficient manner!**

## Installation

Ensure Rust is installed:

[Rust Downloads](https://www.rust-lang.org/downloads.html)

```
curl -sSf https://static.rust-lang.org/rustup.sh | sudo sh -s -- --channel=nightly
```

Add this line to your application's Gemfile:

```ruby
gem 'faster_path'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install faster_path

## Usage

Current methods implemented:

|Rust Implementation|Ruby Implementation|Performance Improvemant|
|---|---|---|
| `FasterPath.absolute?` | `Pathname#absolute?` | 545% to 1450% |
| `FasterPath.chop_basename` | `Pathname#chop_basename` | 6.7% to 54.6% |
| `FasterPath.blank?` | | |

You may choose to use the methods directly, or scope change to rewrite behavior on the
standard library with the included refinements, or even call a method to monkeypatch 
everything everywhere.

**Note:** `Pathname#chop_basename` in Ruby STDLIB has a bug with blank strings, that is the
only difference in behavior against FasterPath's implementation.

For the scoped **refinements** you will need to

```
require "faster_path/optional/refinements"
using FasterPath::RefinePathname
```

And for the sldeghammer of monkey patching you can do

```
require "faster_path/optional/monkeypatching"
FasterPath.sledgehammer_everything!
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/danielpclark/faster_path.


## License

[MIT License](http://opensource.org/licenses/MIT) or APACHE 2.0 at your pleasure.

