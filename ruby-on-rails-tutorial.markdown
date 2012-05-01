## Ruby on Rails Tutorial

by Michael Hartl

### Chapter 1: From zero to deploy
* The "rails root" is the root directory for any given application, and is not the root directory for Rails itself.
* Since gems with different version numbers sometimes conflict, it is often convenient to create separate gemsets, which are self-contained bundles of gems.
* The `app` subdirectory contains all models, views, controllers, and helpers.
* The `Gemfile` lists Gem requirements for an app, while `Gemfile.lock` is a list of gems used to ensure that all copies of the app use the same gem versions.
* Unless you specify a version number to the gem command in a `Gemfile`, Bundler will automatically install the latest version, which may cause minor breakage.
* The `>=` notation always performs upgrades, whereas the `~>` notation only performs upgrades to minor point releases but not to major point releases.
* After assembling the Gemfile, install the gems by running `bundle install`.
* Running the `rails server` command, or its shortcut `rails s`, will run a local development web server.
* Using `git commit -a` commits all modifications to existing files or files created using `git mv`, which don't count as new files to Git.

### Chapter 2: A demo app
* If a gem is intended for production with `group: production` in your `Gemfile`, you can skip installing it locally by running `bundle install --without production`.
* To use the version of Rake corresponding to your Gemfile, run it using `bundle exec`, for example `bundle exec rake db:migrate`.
* The `POST`, `PUT`, and `DELETE` methods are routed to the `create`, `update`, and `destroy` actions, and unlike the other actions do not render pages.
* In a one-to-many relationship, the model on the one-side uses `has_many`, while the model on the many-side uses `belongs_to`.

### Chapter 3: Mostly static pages
* Passing the `--skip-unit-test` option to the `rails new` command will skip generating a test directory associated with the `Test::Unit` framework.
* The Capybara gem allows you to simulate a user's interaction with an application using a natural English-like syntax.
* The `--without production` option passed to `bundle install` is a remembered option, and so can be omitted on future invocations.
* Any files in the `public` directly are served directly from the filesystem and don't even hit the Rails stack.
* After using `rails generate rspec:install` to use `generate` with RSpec, passing `--no-test-framework` to `rails generate` will suppress generating default RSpec tests.
* Web browsers are incapable of sending `PUT` and `DELETE` methods natively, but web frameworks like Rails make it seem like browsers are issuing such requests.
* The TDD practice of writing a failing test, making it pass, and then cleaning up is called "red, green, refactor."
* The `have_selector` method performs a partial match given whatever string is mapped to by `:text` in the given hash.
* In embedded Ruby, `<% ... %>` executes the code inside, while `<%= ... %>` executes the code and inserts the result into the template.
* Calling `<%= yield %>` in the special layout file `application.html.erb` inserts the content of the page into the layout; other values are passed using `provide`.

### Chapter 4: Rails flavored Ruby
* Functions for use in views are called helpers, and are found in the `app/helpers` directory.
* The Rails console, accessed with `rails console` or `rails c`, is built on top of interactive ruby, or `irb`.
* The `puts` method appends a newline character `\n` to the output, while the `print` method does not.
* Single-quoted strings are truly literal, and so do not interpolate values using the `#{}` syntax, and do not form escape sequences.
* Any method that returns a boolean ends with `?`.
* The `nil` object is the only Ruby object that is false in a boolean context, apart from `false` itself. Even `0` is true.
* The `include` statement mixes in a module. Rails automatically mixes the `ApplicationHelper` into all views so its methods are available everywhere.
* Bang methods, which end with `!`, mutate a value instead of returning a different value.
* For blocks, it is typical to use curly braces only for short one-line blocks, and the `do ... end` syntax for anything longer.
* It is a convention to put an extra space at the two ends of a hash literal, and spaces around each "hashrocket", or `=>`.
* When using symbols as hash keys, the symbol/hashrocket combination can be replaced with the key name followed by a colon and a value.
* The `inspect` method returns the string literal for an object, and the shortcut `p` function is equivalent to calling `inspect` on its argument and using `puts`.
* When a hash is the last argument in a function call, its curly braces are optional.

