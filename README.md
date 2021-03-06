Spree Split Payments [![Code Climate](https://codeclimate.com/github/vinsol/spree-split-payments.png)](https://codeclimate.com/github/vinsol/spree-split-payments) [![Build Status](https://travis-ci.org/vinsol/spree-split-payments.png?branch=master)](https://travis-ci.org/vinsol/spree-split-payments)
=========================

######THIS GEM IS NOT READY FOR PRODUCTION USE.

This extension provides the feature for a spree store to allow user to club payment methods to pay for the order.

Easily configurable from the admin end where one can select which payment methods should be allowed for clubbing and their priorities which can be used while creating payments and displaying them to the user.

Installation
------------

Add spree-split-payments to your `Gemfile`:

```ruby
gem 'spree-split-payments'
```

Bundle your dependencies and run the installation generator:

```shell
bundle
bundle exec rails g spree_split_payments:install
```

Integration
-----------
The extension needs a way to find out the maximum amount that can be made via a payment method. To do so it sends a message to the user object as:

```ruby
#{payment_method.class.name.demodulize.underscore}_for_partial_payments

# for example : for Spree::PaymentMethod::LoyaltyPoints it calls for
# loyalty_points_for_partial_payments
# on current_user
```

So you can either

1) Alias an exising method like
```ruby
# models/spree/user_decorator.rb
alias_method :loyalty_points_for_partial_payments, :loyalty_points_equivalent_currency

# where loyalty_points_equivalent_currency is the method provided by
# Spree::PaymentMethod::LoyaltyPoints extension.
```

2) Define a method under user class. For example for Spree::PaymentMethod::LoyaltyPoints)
```ruby
#models/spree/user_decorator.rb
Spree::User.class_eval do
  ...

  def loyalty_points_for_partial_payments
    # logic goes here
  end

  ...

end
```

Testing
-------

Be sure to bundle your dependencies and then create a dummy test app for the specs to run against.

```shell
bundle
bundle exec rake test_app
bundle exec rspec spec
```

When testing your applications integration with this extension you may use it's factories.
Simply add this require statement to your `spec_helper`:

```ruby
require 'spree-split-payments/factories'
```

Contributing
------------

1. Fork the repo.
2. Clone your repo.
3. Run `bundle install`.
4. Run `bundle exec rake test_app` to create the test application in `spec/test_app`.
5. Make your changes.
6. Ensure specs pass by running `bundle exec rspec spec`.
7. Submit your pull request.


Credits
-------

[![vinsol.com: Ruby on Rails, iOS and Android developers](http://vinsol.com/vin_logo.png "Ruby on Rails, iOS and Android developers")](http://vinsol.com)

Copyright (c) 2014 [vinsol.com](http://vinsol.com "Ruby on Rails, iOS and Android developers"), released under the New MIT License
