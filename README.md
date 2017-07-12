# Graphql::Preload

Provides a DSL for the [`graphql` gem](https://github.com/rmosolgo/graphql-ruby) that allows ActiveRecord associations to be preloaded in field definitions. Based on a [gist](https://gist.github.com/theorygeek/a1a59a2bf9c59e4b3706ac68d12c8434) by @theorygeek.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'graphql-preload'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install graphql-preload

## Usage

First, add the instrument to your `GraphQL::Schema`:

    Schema = GraphQL::Schema.define do
      instrument(:field, GraphQL::Preload::Instrument.new)
    end

Call your new instrument when defining your field:

    PostType = GraphQL::ObjectType.define do
      name 'Post'

      field :comments, !types[!CommentType] do
        preload comments: :author

        resolve ->(obj, args, ctx) { obj.comments }
      end

      # Or you can use the more terse syntax:
      field :comments, !types[!CommentType], preload: { comments: :author }, property: :comments
    end

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/ConsultingMD/graphql-preload.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
