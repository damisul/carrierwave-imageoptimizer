# Main difference of this fork is:
- to add CarrierWave 3+ compatibility (simply relaxed condition in gemspec)

# CarrierWave ImageOptimizer

This gem allows you to simply optimize CarrierWave images via jpegoptim or optipng using the [image_optimizer](https://github.com/jtescher/image_optimizer) gem.

Tested against 2.2.x, 2.3.x, and ruby-head

[![Build Status](https://secure.travis-ci.org/jtescher/carrierwave-imageoptimizer.svg)](http://travis-ci.org/jtescher/carrierwave-imageoptimizer)
[![Code Climate](https://codeclimate.com/github/jtescher/carrierwave-imageoptimizer.svg)](https://codeclimate.com/github/jtescher/carrierwave-imageoptimizer)
[![Coverage Status](https://coveralls.io/repos/jtescher/carrierwave-imageoptimizer/badge.svg)](https://coveralls.io/r/jtescher/carrierwave-imageoptimizer)
[![Gem Version](https://badge.fury.io/rb/carrierwave-imageoptimizer.svg)](http://badge.fury.io/rb/carrierwave-imageoptimizer)

## Installation

##### This gem uses the following utilities for optimizing images:

1. jpegoptim, which can be installed from the official [jpegoptim](https://github.com/tjko/jpegoptim) repository

2. OptiPNG, which can be installed from [sourceforge.net](http://optipng.sourceforge.net/)

Or install the utilities via homebrew:

```bash
$ brew install optipng jpegoptim
```

Then add this line to your application's Gemfile:

    gem 'carrierwave-imageoptimizer'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install carrierwave-imageoptimizer

### Installation on Heroku
If you are using Heroku for your production you must use [heroku buildpacks](https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app) in order to install `optipng` and `jpegoptim`. [jayzes](https://github.com/jayzes?tab=repositories) has some plug and play buildpacks to get you going in no time.

__On an existing Heroku app and using the Heroku CLI__

1. Add this gem to your gemfile.
2. Modify your code following the instrunctions on the [usage](#usage) section.
3. `heroku buildpacks:add --index 1 https://github.com/jayzes/heroku-buildpack-optipng`
4. `heroku buildpacks:add --index 1 https://github.com/jayzes/heroku-buildpack-jpegoptim`
5. `git push heroku` 
  * _It is important to do a push to heroku __after__ the buildpacks have been declared._

## Usage

To add image optimization to your CarrierWave uploader, first include the module:

```ruby
class MyUploader < CarrierWave::Uploader::Base
  include CarrierWave::ImageOptimizer
  ...
end
```

Then apply to all versions via:

```ruby
class MyUploader < CarrierWave::Uploader::Base
  ...
  process :optimize
end
```

Or to a single image version via:

```ruby
class MyUploader < CarrierWave::Uploader::Base
  ...
  version :thumbnail do
    process :optimize
  end
end
```

##### Lossy JPEG optimization

Pass an optional `quality` parameter to target a specific lossy JPG quality level (0-100), default is lossless
optimization. PNGs will ignore the quality setting.

```ruby
class MyUploader < CarrierWave::Uploader::Base
  ...
  version :thumbnail do
    process optimize: [{ quality: 50 }]
  end
end
```

##### Quiet Mode

Pass an optional `quiet` parameter to compress images without logging progress.

```ruby
class MyUploader < CarrierWave::Uploader::Base
  ...
  version :thumbnail do
    process optimize: [{ quiet: true }]
  end
end
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
