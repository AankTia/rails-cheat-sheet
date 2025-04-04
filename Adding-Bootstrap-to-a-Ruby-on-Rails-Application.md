# Adding Bootstrap to a Ruby on Rails Application

Here are the most common methods to add Bootstrap to your Rails application:

## Method 1: Using the Bootstrap Gem

1. Add the bootstrap gem to your Gemfile:

```ruby
# Gemfile
gem 'bootstrap', '~> 5.3.0'
gem 'jquery-rails' # Required for some Bootstrap features
```

2. Install the gems:

```bash
bundle install
```

3. Import Bootstrap in your application.scss file:

```scss
// app/assets/stylesheets/application.scss
// First, rename application.css to application.scss

// Import Bootstrap
@import "bootstrap";
```

4. Add Bootstrap JavaScript by importing it in your application.js:

```javascript
// app/javascript/application.js
//= require jquery3
//= require popper
//= require bootstrap
```

## Method 2: Using Yarn/npm (Rails 6+ with Webpacker or importmap-rails)

1. Install Bootstrap using Yarn or npm:

```bash
yarn add bootstrap @popperjs/core
```
or
```bash
npm install bootstrap @popperjs/core
```

2. Import Bootstrap styles in your application.scss:

```scss
// app/assets/stylesheets/application.scss
@import "bootstrap/scss/bootstrap";
```

3. Import Bootstrap JavaScript in your application.js:

```javascript
// app/javascript/application.js
import "bootstrap"
```

4. For Rails 7 with importmap-rails, add Bootstrap to your importmap:

```bash
bin/importmap pin bootstrap @popperjs/core
```

## Method 3: Using CDN (Simplest approach)

Add the Bootstrap CDN links directly to your layout file:

```erb
<!-- app/views/layouts/application.html.erb -->
<head>
  <!-- ... -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
  <%= javascript_include_tag "application", "data-turbo-track": "reload", defer: true %>
  <!-- ... -->
</head>
<body>
  <%= yield %>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
```

## Method 4: Using the bootstrap-sass Gem (For older Bootstrap versions)

```ruby
# Gemfile
gem 'bootstrap-sass', '~> 3.4.1'
gem 'sassc-rails', '>= 2.1.0'
```

## Testing Bootstrap Installation

Create a quick test page to verify Bootstrap is working:

```bash
rails generate controller Home index
```

Then add some Bootstrap components to the view:

```erb
<!-- app/views/home/index.html.erb -->
<div class="container mt-5">
  <div class="row">
    <div class="col-md-6 offset-md-3">
      <div class="card">
        <div class="card-header bg-primary text-white">
          <h4>Bootstrap Test</h4>
        </div>
        <div class="card-body">
          <h5 class="card-title">Bootstrap is working!</h5>
          <p class="card-text">If you can see this styled card, Bootstrap is properly installed.</p>
          <button class="btn btn-primary">Test Button</button>
          <button class="btn btn-outline-secondary">Secondary Button</button>
        </div>
      </div>
    </div>
  </div>
</div>
```

Set this page as your root route:

```ruby
# config/routes.rb
Rails.application.routes.draw do
  root 'home#index'
end
```

Visit your application at http://localhost:3000 to verify Bootstrap is working correctly.