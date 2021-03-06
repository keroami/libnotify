# Libnotify

[![Build Status](https://travis-ci.org/splattael/libnotify.png?branch=master)](https://travis-ci.org/splattael/libnotify) [![Gem Version](https://badge.fury.io/rb/libnotify.png)](http://badge.fury.io/rb/libnotify) [![Code Climate](https://codeclimate.com/github/splattael/libnotify.png)](https://codeclimate.com/github/splattael/libnotify) [![Inline docs](http://inch-pages.github.io/github/splattael/libnotify.png)](http://inch-pages.github.io/github/splattael/libnotify)

Ruby bindings for libnotify using FFI.

[Gem](https://rubygems.org/gems/libnotify) |
[Source](https://github.com/splattael/libnotify) |
[RDoc](http://rubydoc.info/github/splattael/libnotify/master)

![libnotify](http://github.com/splattael/libnotify/raw/master/libnotify.png)

## Usage

```ruby
require 'libnotify'

# Block syntax
n = Libnotify.new do |notify|
  notify.summary    = "hello"
  notify.body       = "world"
  notify.timeout    = 1.5         # 1.5 (s), 1000 (ms), "2", nil, false
  notify.urgency    = :critical   # :low, :normal, :critical
  notify.append     = false       # default true - append onto existing notification
  notify.transient  = true        # default false - keep the notifications around after display
  notify.icon_path  = "/usr/share/icons/gnome/scalable/emblems/emblem-default.svg"
end
n.show!

# Hash syntax
Libnotify.show(:body => "hello", :summary => "world", :timeout => 2.5)

# Mixed syntax
Libnotify.show(options) do |n|
  n.timeout = 1.5     # overrides :timeout in options
end

# Icon path auto-detection
Libnotify.icon_dirs << "/usr/share/icons/gnome/*/"
Libnotify.show(:icon_path => "emblem-default.png")
Libnotify.show(:icon_path => :"emblem-default")

# Update pre-existing notification then close it
n = Libnotify.new(:summary => "hello", :body => "world")
n.update # identical to show! if not shown before
Kernel.sleep 1
n.update(:body => "my love") do |notify|
  notify.summary = "goodbye"
end
Kernel.sleep 1
n.close
```

## Installation

```bash
gem install libnotify
```

You'll need libnotify. On Debian just type:

```bash
apt-get install libnotify1
```

## Testing

```bash
git clone git://github.com/splattael/libnotify.git
cd libnotify
(gem install bundler)
bundle install
rake
```

### Code coverage

```bash
COVERAGE=1 rake
```

## Caveats

### Ubuntu

`timeout` and `append` options might not work on Ubuntu.
See GH #21 for details.
https://github.com/splattael/libnotify/issues/21#issuecomment-19114127

## Authors

* Peter Suschlik (https://github.com/splattael)

## Contributors

* Dennis Collective (https://github.com/denniscollective)
* Daniel Mack (https://github.com/zonque)
* Nuisance of Cats (https://github.com/nuisanceofcats)
* Jason Staten (https://github.com/statianzo)

## License

[MIT License](http://www.opensource.org/licenses/MIT)

## TODO

* Unify show/update interface
  -> simplifies code

* Turn FFI module into a class
  -> more plugabble

* Support newer features of `libnotify`
  -> actions, hints?
  See https://developer-next.gnome.org/libnotify/0.7/NotifyNotification.html

* Support older versions of `libnotify`
  -> prior `0.4`?
