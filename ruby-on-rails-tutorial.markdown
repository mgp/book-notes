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
* Running the `rails server` command will run a local development web server.
* Using `git commit -a` commits all modifications to existing files or files created using `git mv`, which don't count as new files to Git.

