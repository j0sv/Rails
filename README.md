1.  Create a new rails application

  ```
  $ rails new rails_demo --database=postgresql --skip-test --skip-bundle
  ```

  * The --database=postgresql selects PostgreSQL as the database (The out-of-the-box setting is SQLite)
  * The --skip-test option skips configuring for the default testing tool.
  * The --skip-bundle option prevents the generator from running bundle install automatically.  

2. Configuring the rails application  
  This will add:
  * [rspec](https://github.com/rspec/rspec-rails) - for unit test
  * [shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers) - to get shorter syntax in rspec test
  * [factory_girl_rails](https://github.com/thoughtbot/factory_girl) - to create inctances of object in test
  * [cucumber](https://github.com/cucumber/cucumber-rails) - for acceptance test
  * [database_cleaner](https://github.com/DatabaseCleaner/database_cleaner) - will remove data added during automated test execution
  
  Include by adding to the file: `gemfile`

  ```
  group :development, :test do
    gem 'rspec-rails'
    gem 'shoulda-matchers'
    gem 'factory_girl_rails'
    gem 'cucumber-rails', require: false
  end

  group :test do
    gem 'database_cleaner'
  end
  ``` 

  Run `$ bundle install` to install the added gems:
  * Shoulda-matchers configuration added to `spec/rails_helper.rb`

  ```
  Shoulda::Matchers.configure do |config|
    config.integrate do |with|
      with.test_framework :rspec
      with.library :rails
    end
  end
  ```

  * Configuration to avoid `rails generate` create selected files. Added to `config/application.rb`

  ```
  class Application < Rails::Application
    # Disable generation of helpers, javascripts, css, and view, helper, routing and controller specs
    config.generators do |generate|
      generate.helper false
      generate.assets false
      generate.view_specs false
      generate.helper_specs false
      generate.routing_specs false
      generate.controller_specs false
    end
    ...
  end
  ```
  
  Add to `.rspec` to format output from rspec.

  ```
  --color
  --format documentation
  ```
3. Initiate test tools installed above and create database

  Run the RSpec generator to add the testing framework to your rails application

  ```
  $ bundle exec rails generate rspec:install
  $ bundle exec rails generate cucumber:install
  ```

  Create database
  ```
  rails db:create
  rails db:migrate
  ```
  
4. Verify test tool configuration

  Run `$ bundle exec rspec`. The output you see should be something like:

  ```
  $ bundle exec rspec
  No examples found.

  Finished in 0.00023 seconds (files took 0.5029 seconds to load)
  0 examples, 0 failures
  ```
  Run `$ cucumber`. The output you see should be something like:

  ```
  $ bundle exec cucumber
  Using the default profile...
  0 scenarios
  0 steps
  0m0.000s
  ```
