# MobilePagination

## Description
Rack gem for producing minimal pagination links.

![<Display Name>](http://i.imgur.com/qZcqfx8.png)


## Installation

Add this line to your application's Gemfile:

    gem 'mobile_pagination'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install mobile_pagination

## Usage
Initializes with 4 options: `current_page, total_pages, query, path`

```ruby
require 'mobile_pagination'

# http://local.m.newhomeguide.com/New-Homes/Georgia/Atlanta/?page=2

def opts
  {
    :current_page => params[:page],         # => 2
    :total_pages  => total_pages,           # => Int for total pages
    :query        => request.query_string,  # => 'page=2'
    :path         => request.path           # => '/New-Homes/Georgia/Atlanta/'
  }
end

def pagination
  MobilePagination::Paginate.new(opts).html
end

# in your view, you may have something like this:
ol
  = pagination
```

### Configuration
Configuration is optional. Say for example you were paginating slides on a slideshow - instead of using page, you could configure the gem to use a different key.

```ruby
MobilePagination.configure do |config|
  config.page_key = 'slide'
end
```

Configuration must run prior to initialization. The resulting pagination links will now contain `/?slide=2, /?slide=3` to suit your custom url structure. The default page_key is `page`.

### Overrides

If you don't like list items, you will need to override `MobilePagination::Templates`

Just add this to the bottom of one of your view helpers:

```ruby
module MobilePagination
  module Templates

    def first_page_html
      "<p'>
        <a title='First Page' href='#{first_page_link}'>Luke's Page</a> |
      <p>"
    end

    def previous_page_html
      "<h1>
        <a title='Previous Page' href='#{previous_page_link}'>Previous Page</a> |
      </h1>"
    end

    def next_page_html
      "<span>
        <a title='Next Page' href='#{next_page_link}'>Next Page</a> |
      </span>"
    end

    def last_page_html
      "<h3>
        <a title='Last Page' href='#{last_page_link}'>Last Page</a>
      </h3>"
    end

  end
end
```

Please note, any methods ending in `_link` will need to remain, as these methods are responsible for generating the paginated urls.

### Testing
`rspec spec`

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
