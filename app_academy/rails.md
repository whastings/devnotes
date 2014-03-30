# Rails Curriculum Notes - Will Hastings

## Week 4 - Day 1
In a migration, you can use the `references` type to set up a `belongs_to` relationship with an index
- e.g. `t.references :post, index: true`

The `PUT` and `PATCH` HTTP verbs are for updating a resource
The **RestClient** gem makes it easy to make http requests
The **Addressable** gem makes it easy to parse and generate URLs
- Especially good for query strings
- Do with `Addressable::URI` class

## Week 4 - Day 2
**Routing**:
- Can get all seven RESTful routes with `resources` method
    + e.g. `resources :users`
    + Can limit which routes are created with `:only` option
- **Route Helpers**:
    + Two types:
        * Ending in `_url` provides full url with domain name
        * Ending in `_path` provides just path
    + Four `resources`, you'll get four helpers
        * e.g. `users_url`, `new_user_url`, `user_url`, `edit_user_url`
            - Helpers `user_url` and `edit_user_url` need an object argument
                + e.g. `user_url(@user)`
- You can run `rake routes` to see all your app's routes
- Can specify home page location with `root to: "controller#action`
- Can manually specify a route with an http verb method
    + e.g. `get '/add-user' => 'users#new', as: :add_user`
        * `:as` specifies helper name
    + You can specify url fragment wildcards, which will be available in controller
        * e.g. `/follow-user/:id` will map user id to `:id`
    + Path parts can be optional if placed in parentheses
- Can make a singular route by using `resource` instead of `resources`
- Rails 4 uses `PATCH` for update instead of `PUT`
    + `PUT` is supposed to send a whole resource, whereas `PATCH` is used for partial updates
- Matches entries in `config/routes.rb` top-down
- Can *nest routes* so they belong to parent routes
    + Should only nest routes that explicitly need the parent resource
        * e.g. For index, create, and new
    + Try to never nest more than two levels deep
    + Ex: A user's posts accessible at `/users/:id/posts`
```
resources :users do
  resources :posts, only: :index
end
```

**Controllers**
- Always name with plural of model name (e.g. `UsersController`)
- They all extend your `ApplicationController`, which extends `ActionController::Base`
- Can redirect to a different url with `redirect_to`
- Can render a template with `render :template_name`
    + Will render template with same name as action by default
- Can render JSON with `render json: object`
    + Will object's `to_json` method
- Can specify a response code with `render... status: code_or_alias`
    + e.g. `render :template, status: 403`
    + e.g. `render :template, status: :unprocessable_entity`
- Can generate with `rails generate controller ControllerName`
- Make sure to never `render` or `redirect_to` more than once in the same code path
- **Strong Parameters**
    + Way to whitelist parameters that can be passed to mass assignment
        * **Mass Assignment** is passing all parameters to model's `new` or `create`
    + e.g. `params.require(:required_param).permit(:allowed1, :allowed2)`
    + Good to place in a private method of the controller
        * e.g. `post_params`
- Can send just a status instead of rendering with `head :status_alias`
- The `params` method retrieves hash-like object of parameters in controller
    + Parameters can come from query string, post body, or path segments

## Week 4 - Day 3
**ERB**: *Embedded Ruby*
- Use `<% %>` for non-printing Ruby execution
- Use `<%= %>` for Ruby execution that prints the result
- **Partials**
    + File names start with an underscore
    + Use `render` to add it to another template
        * e.g. `render partial: "path/partial"`
    + Can pass variables to partials with `:object`
        * e.g. `<%= render partial: "product", object: @item %>`
            - `@item` will be available in partial as `product`
    + Can render all items in a collection with a partial using `:collection`
        * e.g. `<%= render partial: "product", collection: @products %>`

The **button_to** helper is a nice alternative to having non-GET links
- e.g. `button_to(user_url(@user), method: :delete)`

**Forms**:
- Rails will create nested hashes from parameter names that use the *key-like* syntax
    + e.g. `user[name]` will become `{ user: { name: 'value' } }`
- Rails will create arrays from parameter names that end in `[]`
    + e.g. Multiple `user[numbers][]` will become `{ user: { numbers: [...] } }`
- All forms need to use `POST`, but you can specify a different method for Rails to use with `_method`
    + e.g. `<input type="hidden" name="_method" value="PATCH">`

Awesome debugging gems:
- **Better Errors**
- **binding_of_caller**

**Error Handling**:
- Is about the only time you'd want to render a view for a different page
    + e.g. Rendering `new` from `#create` because there were validation errors

## Week 4 - Day 4
Use `before_action` in controllers to run methods before controller actions
- e.g. `before_action :require_signed_in`
- A `redirect_to` call in a `before_action` method will cancel the actual action
    + Can limit which actions it runs before with `:only` and `:except`
**Authentication**:
- Store password hash in DB in `password_digest` column
- `SecureRandom::urlsafe_base64` is great for generating random tokens
- **BCrypt** is great for password hashing
    + `BCrypt::Password.new(password_hash).is_password?(submitted_password)` will check if the password submitted is correct
    + `BCrypt::Password.create(unencrypted_password)` will create a password object whose `to_s` returns the BCrypt hash
        * Will create a different hash for same string because it uses a different *salt* every time
- Good to persist authentication by storing a **session token** in db and cookie on client
+ Can write *helper methods* in helper modules
    * `current_user` can look up user via session token
    * `sign_in_user` can assign the user's session token to a session or cookie variable
        - Probably a good idea to make a new token on every sign in
        - Also, the `reset_session` method will reset the Rails session token as well to prevent session fixation
            + But don't do it if you have a non-loggged in shopping cart!
    * `sign_out_user` can reset the user's session token and remove the session token from the cookie or session variable

Can set and get **cookies** with controllers' **cookie** method
- Can set cookie attributes by assigning a hash
    + Ex:

    cookies.permanent[:token] = {
      value: token,
      httponly: true,
      expires: 1.week.from_now
    }

Can set and get **session data** with controllers' **session** method
- Stored in cookie by default
    + Cookie is marked `:httponly`

Can make a method available to views with `helper_method :method_name`

## Week 4 - Day 5
**ActionMailer**: Rails's mechanism for sending out emails
- You create classes in `app/mailers` that extend from `ActionMailer::Base`
- Email content goes in views that live in `app/views/[mailer_name]`
    + View files should be named after the method names they're for
    + Good to have HTML and text versions of each email so Rails can send both
        * Create `email_name.html.erb`
        * Create `email_name.text.erb`
- Can generate with `rails generate mailer`
- Can specify defaults with the `default` method
    + e.g. `default from: 'admin@example.com'`
- Each email has its own method in the mailer class (like controller actions)
    + Should return a `Mail::Message` object created with the `mail` method
        * e.g. `mail(to: user.email, subject: 'Welcome to My Awesome Site')`
        * Message isn't actually sent until caller calls `deliver` on the returned message object
- You can call Mailer methods as if they were class methods
    + e.g. `message = UserMailer.welcome_email`
- To use url helpers in your message content, you need to set the following setting:
    + `config.action_mailer.default_url_options = { host: 'example.com' }`

Rails *escapes* HTML output by default
- If you want a string with html to output unescaped, call `String#html_safe` on it before printing
    + If you want to escape parts of the string manually, you can call the `html_escape` (alias `h`) helper method on it

The **content_for** method allows you to specify content for a **named yield** in a layout
- e.g. If your layout has `<%= yield :footer %>`, you can specify in your view:

```
<% content_for :footer do %>
  <%= javascript_include_tag "app" %>
<% end %>
```

You can use the `simple_format` helper method to automatically wrap text in paragraph tags

## Week 5 - Day 1
Creating objects of a has_many when creating the parent
- e.g. Creating group memberships for a user when creating the user, having a has_many :through to groups
- The has_many :through will add a `***_ids=` method to the parent
    + e.g. `group_ids=`
    + Assigning IDs to such a method will cause the intermediary records of the has_many (e.g. `GroupMembership`) to be created
    + You'll need to permit the `***_ids=` method to be mass assigned
- Can use checkboxes for the form

You can *nest whitelisted parameters* in strong parameters
- e.g. `params.require(:user).permit(:name, { addresses: [] }`
    + User can have one name and multiple addresses
- e.g. `params.permit(addresses: [:city, :country]).require(:addresses)`
    + Allows an array of addresses, each with a city and country parameter
    + Notice you need to call `require` after `permit`

Creating parent and has_many objects at the same time:
- Can do so via the association's `new` method
    + Ex: This will create the user and all its addresses, *within a transaction*, when you call `@user.save` and will set addresses' user IDs automatically

```
@user = User.new(user_params)
@user.addresses.build(address_params)
```

You can reject blank submitted parameters with strong parameters' `reject` method
- e.g. `params.permit(...).require(...).values.reject { |data| data.all?(&:blank?) }`
    + But this might not work if you have a checkbox that the user doesn't check

Removing related items that use checkboxes:
- If you uncheck all checkboxes, it won't remove the associations because no value will be sent for that parameter
- You need to include an extra empty hidden input of the same name
- e.g. `<input type="hidden" name="user[group][]" value="">`
    + So if no checkboxes are checked, this will cause the associated objects to be removed

You can cause an object deletion with the **_destroy** parameter with a value of `true` or `1`
- Usually done with a checkbox

You can prebuild related objects using the association's `build` method
- `3.times { @user.addresses.build }`

The **:inverse_of** association option
- Necessary if you're validating a belongs_to association or id and trying to build the parent and belongs_to object at the same time
- Don't validate association ID; validate associated instead
    + e.g. `validates :user, presence: true` instead of `validates :user_id, presence: true`
- Then, you need to add `inverse_of` on the has_many association
    + e.g. `has_many :addresses, :inverse_of => :user`
    + Points to the association name on the other side/object

With forms, you can't send an *array of hashes*, so send a hash with numbered keys
- e.g. `<input type="checkbox" name="user[groups][0][id]...`

Don't use the `:presence` validation on a boolean attribute
- Use `validates boolean_attribute, inclusion: { in: [true, false] }`

When you're editing a nested object in a nested form, you need to send its id in a hidden field
- e.g. `<input type="hidden" name="albums[<%= id %>][id]" value="<%= album.id %>">`
- In your `update` action, you then need to check for it
- Ex:

```
@albums = albums_params.map do |album_data|
  if album_data[:id]
    album = Album.find(album_data[:id])
    # ... Update with update_attributes
    if album_data[:_destroy]
      album.destroy
    end
  else
    @band.albums.build(album_data)
  end
end
```

- Can add `accepts_nested_attributes_for` to the parent model with the has_many to save a lot of the above manual work
    + Would add a `album_attributes=` method

## Week 5 - Day 2
With validations, you can validate that a pair of attributes are together unique
- e.g. `validates :name, :uniqueness => { :scope => :band_id }` 

**rescue_from**: Lets you catch exceptions with a custom method in a controller for any actions in that controller or any of its subclasses
- e.g. `rescue_from ActiveRecord::RecordNotFound, with: :record_not_found`

## Week 5 - Day 4
To use RSpec with Rails:
- Load the gem **rspec-rails**
- Make sure you have your test database settings configured
- Run `rails g rspec:install` in your app's directory
- Useful to set `format --documentation` in the `.rspec` file
- Set up your main test config in `config/application.rb`:

```
config.generators do |g|
  g.test_framework :rspec, 
    :fixtures => true, 
    :view_specs => false, 
    :helper_specs => false, 
    :routing_specs => false, 
    :controller_specs => true, 
    :request_specs => true
end
```

Testing models
- Use the **shoulda-matchers** gem to test model validations and associations
    + e.g. `have_many`, `belongs_to`, `have_db_column`, `have_db_index`

Use **Factory Girl** gem for building model objects to test
- Can create and save model objects with `FactoryGirl::create` or just instantiate them with `FactoryGirl::build`
- Has a simple syntax:

```
FactoryGirl.define do
  factory :post do
    title "It's a title!"
  end
end
```
- In `config/application.rb`, set `g.fixture_replacement :factory_girl, :dir => "spec/factories" `
- You set attributes via methods of the same name as those attributes
    + e.g. `username 'Bob'`
- Can also set attributes' values dynamically via blocks
    + e.g. `username { Faker::Internet.user_name }`
- Can use `sequence` for an attribute with a block and each block will get an incremented counter
    + e.g. `sequence :num_attribute { |n| n }`
- Can nest factories to make different versions

```
  factory :user do
    username 'Bob'

    factory :admin_user do
      admin true
    end
  end
```

**Guard** is a helpful gem for running tests automatically
**Spork** is a helpful gem for speeding up your tests by keeping your app loaded
- Use the 'spork-rails' gem
- Can set it up with `bundle exec spork --bootstrap`
- Then in `spec/spec_helper.rb`
    + `require 'spork'`
    + Put rspec config in a `Spork.prefork do...` block
- Add `--drb` to `.rspec`
- To start server run `bundle exec spork`

## Week 5 - Day 5
Integration testing with **Capybara**:
- Provides methods for interacting with web pages like a browser does
- Includes: `click_link`, `click_button`, `visit`
- Also has methods for making expectations about page content
    + e.g. `expect(page).to have_content('something')`
- Need to `require 'capybara/rspec'` in `spec_helper.rb`
- Can fill in form fields, e.g. `fill_in 'Label Name', with: 'value'`
- Capybara tests should go in **feature** blocks in spec files in `spec/features`
    + Doesn't use a `describe` block
- If you have the Launchy gem available, you can use the **save_and_open_page** method to open the page you're testing in a browser

**Concerns**:
- Are modules that have:
    + `extend ActiveSupport::Concern`
- Can execute statements within the context of the class including it with **included do...end**
    + Ex:
```
included do
  has_many :posts
end
```
- Can define class methods on the class including the concern by creating a nested module called **ClassMethods**
    + ActiveSupport will automatically make all methods in it class methods in the including class (no need to use `def self...`)

**Polymorphic Associations**:
- Is a `belongs_to` that allows the object to belong to one of multiples types of associated objects
- Add `polymorphic: true` to the `belongs_to` declaration
    + e.g. `belongs_to :commentable, polymorphic: true`
- Needs an extra column in the database table of the belongs_to object to save the type of the object it belongs to
    + e.g. Declares `t.integer :commentable_id` and `t.string  :commentable_type`
        * Or `t.references :commentable, polymorphic: true`
        * Good idea to add indexes to these columns
- Then, on the model with the `has_many`, you need to refer to the polymorphic association name using `:as`
    + e.g. `has_many :comments, as: :commentable`
