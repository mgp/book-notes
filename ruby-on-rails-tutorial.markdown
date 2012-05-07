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
* While `array.new` takes an initial value for the array, `Hash.new` takes a default value for the hash.
* The `blank?` method, added to the `String` class by Rails, returns whether a string is composed entirely of whitespace.
* A Rails convention is to pass a hash named `attributes` into the `initialize` method, or constructor, of a class.

### Chapter 5: Filling in the layout
* The `alt` attribute is passed in the optional options hash to the `image_tag` helper, but is in fact required by the HTML standard.
* The `bootstrap-sass` gem converts the LESS used by Bootstrap to the Sass language used by Rails.
* Filenames of partials start with an underscore, but are omitted in the call to `render`. Shared partials are found in the `app/views/layouts` directory.
* Static assets specific to the application are in `app/assets`; assets for libraries written by your dev team are in `lib/assets`; and assets from third-party vendors are in `vendor/assets`.
* Filename extensions of assets specify which preprocessor engines to use; if multiple are specified, the associated preprocessors are run from right to left.
* The asset pipeline puts all CSS in one `application.css` file, all Javascript in one `javascripts.js` file, and minimizes assets in all paths where possible.
* The `bootstrap-sass` gem provides Sass equivalents of all the LESS variables for colors found in Bootstrap.
* A named route ending with `_path` is just the pathname, while the one ending with `_url` includes the hostname and scheme.
* It's convention to route the root of your site using `root` instead of `match '/'`.
* Files included in the `spec/support` directory are automatically included by RSpec.
* If you just run `rake` by itself, the default behavior is to run the full test suite.

### Chapter 6: Modeling users
* In contrast to the plural convention for controller names, model names are singular, although the underlying table name is in the plural form.
* For an irreversible database migration like removing a database column, you must define separate `self.up` and `self.down` methods in place of the `change` method.
* The `annotate` gem precedes each model definition with a comment containing its field names and types.
* By default all model attributes are accessible, but `attr_accessible` ensures that only the provided attributes are accessible to outside users.
* Starting `rails console` with the `--sandbox` flag rolls back any changes on exit.
* The `create` method combines the `new` and `save` actions into one.
* The `update_attribute` method combines both the `update` and `save` actions into one, but can only modify those attributes defined as accessible using `attr_accessible`.
* The `db:test:prepare` Rake task ensures that the data model from the development database `db/development.sqlite3` is reflected in the test database `db/test.sqlite3`.
* In RSpec, whenever an object responds to a boolean method `foo?`, there is a corresponding test method called `be_foo`.
* Constants in Ruby have a name that starts with a capital letter.
* Using `validates: uniqueness` is a query-then-write approach subject to race conditions; to properly enforce uniqueness, use an index at the the database level.
* Not all database adapters use case-insensitive indices, and so you must use a database callback to lowercase a string before writing it.
* When using `rails generate migration`, to automatically create a migration for table `tablename`, end the migration name with `_to_tabename`.
* If the password confirmation field is `nil`, Rails doesn't run the confirmation validation, although this can never happen on the web, and only through the console.
* The `let` method in RSpec memoizes its value, and so the value is remembered from one invocation to the next.
* Given a `password_digest` column in the database, the `has_secure_password` method provides a secure way to create and authenticate new users.

### Chapter 7: Sign up
* The `console` command accepts the environment as an argument, `server` through the `--environment` flag, and `db:migrate` through the `RAILS_ENV` variable.
* To speed up your tests, redefine the BCrypt cost factor in `config/environments/test.rb` from its secure default value to its fast minimum value.
* Methods defined in any helper file are automatically available in any view, but strive for organization and convenience.
* To reset the database, use the `db:reset` Rake task. This may need to be followed with the `db:test:prepare` Rake task before testing.
* The `count` method on every Active Record class and the `change` method in RSpec can be used to verify that validations stopped a write from succeeding.
* Passing `-e` and a string to RSpec runs just the tests whose description strings match the given string.
* To display an empty form using `form_for`, simply create an empty Active Record object in the corresponding controller.
* Partials expected to be used in views across multiple controllers should be placed in the `app/views/shared` directory.
* The `any?` method is the opposite of `empty?`, returning `true` if an array is not empty, and `false` if it is.
* The `pluralize` method in `ActionView::Helpers::TextHelper` is backed by a powerful inflector that can pluralize almost any word.
* The `redirect_to` method can construct a RESTful URL from any Active Record object passed as a parameter.
* The `flash` variable is like a special hash whose values disappear upon visiting a second page or upon reloading.
* Embedded Ruby will automatically convert symbols into strings before inserting them into the template.
* To change the names of fields with missing values in error messages, edit the corresponding attribute in `config/locales/en.yml`.
* The `content_tag` helper method can improve readability wherever ERb is used to compute HTML attributes inside quotes.

### Chapter 8: Sign in, sign out
* Modeling sessions as a RESTful resource, signing in is a `POST` request to the `create` action, and signing out is a `DELETE` request to the `destroy` action.
* Passing the `only` option to `resources` in `config/routes.rb` restricts the RESTful routes generated.
* If you cannot provide a suitable model for `form_for`, you can pass the name of the resource and the corresponding URI as parameters.
* Display error messages on rendered pages using `flash.now` because its contents disappear upon an additional request; when using redirects, use `flash`.
* By default, helpers are available in views, but you must use `include` to make them available in controllers.
* Values stored in the `session` object are stored in a cookie that expires upon the browser closing.
* The `its` method of RSpec applies the subsequent test to the given attribute rather than the subject of the test.
* Inside private methods called by a model's `before_save` method, column names that are written to must be preceded by `self`.
* The `permanent` method of `cookies` is a shortcut for setting the optional expires date of a value to 20 years in the future, or `20.years.from_now.utc`.
* The `||=` operator can be used to assign a value to a variable if it is currently undefined.
* Passing `validate: false` to `save` skips the validations for the model, which is useful in `rails console`.
* File `spec/support/utilities.rb` can contain not just helper methods but custom matchers (like `have_selector` or `have_link`) using `RSpec::Matchers.define`.

