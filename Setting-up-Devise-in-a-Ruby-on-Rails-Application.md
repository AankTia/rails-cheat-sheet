# Setting up Devise in a Ruby on Rails Application

I'll walk you through the process of setting up authentication in a Ruby on Rails application using Devise, step by step.

## Step 1: Add Devise to your Gemfile

```ruby
# Gemfile
gem 'devise'
```

Then run:

```bash
bundle install
```

## Step 2: Install Devise

Run the generator:

```bash
rails generate devise:install
```

This will create:
- An initializer at `config/initializers/devise.rb`
- A locale file at `config/locales/devise.en.yml`

## Step 3: Follow the instructions in the terminal

Devise will give you instructions in the terminal to:

1. Set a default URL option in your environment configuration:
```ruby
# config/environments/development.rb
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

2. Ensure you have a root path in your routes:
```ruby
# config/routes.rb
root to: "home#index"
```

3. Set up flash messages in your layout:
```erb
<!-- app/views/layouts/application.html.erb -->
<p class="notice"><%= notice %></p>
<p class="alert"><%= alert %></p>
```

## Step 4: Generate your User model

```bash
rails generate devise User
```

This creates:
- A User model
- A migration file
- Routes for Devise

## Step 5: Customize your migration (optional)

You can add additional fields to the migration file before running it:

```ruby
# db/migrate/YYYYMMDDHHMMSS_devise_create_users.rb
class DeviseCreateUsers < ActiveRecord::Migration[6.1]
  def change
    create_table :users do |t|
      ## Database authenticatable
      t.string :email,              null: false, default: ""
      t.string :encrypted_password, null: false, default: ""
      
      # Add custom fields
      t.string :first_name
      t.string :last_name
      
      # Other Devise fields remain the same...
    end
  end
end
```

## Step 6: Run migrations

```bash
rails db:migrate
```

## Step 7: Configure your controllers

To restrict access to authenticated users:

```ruby
# app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  before_action :authenticate_user!
end
```

Or for specific controllers:

```ruby
# app/controllers/some_controller.rb
class SomeController < ApplicationController
  before_action :authenticate_user!, except: [:index, :show]
end
```

## Step 8: Customize views (optional)

Generate Devise views to customize:

```bash
rails generate devise:views
```

## Step 9: Add sign in/out links to your layout

```erb
<!-- app/views/layouts/application.html.erb -->
<% if user_signed_in? %>
  <li>
    <%= link_to "Sign out", destroy_user_session_path, method: :delete %>
  </li>
<% else %>
  <li>
    <%= link_to "Sign in", new_user_session_path %>
  </li>
  <li>
    <%= link_to "Sign up", new_user_registration_path %>
  </li>
<% end %>
```

## Step 10: Customize Devise (optional)

To allow additional fields in forms:

```ruby
# app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:first_name, :last_name])
    devise_parameter_sanitizer.permit(:account_update, keys: [:first_name, :last_name])
  end
end
```

That's it! You now have a working authentication system in your Rails application using Devise.