## Rails Forms

-   It may sound strange, but forms are possibly the most complicated thing about learning web development. Not necessarily because the code itself is difficult, but because you usually want to build forms that accomplish so many different things at once.

## To best understand Rails forms, we'll build a form and explain what everything is what's going on as we go along

# CUD
## Setting up our rails app

Fork and clone this repo and follow these steps:
- cd into `CatApp`
- run `bundle install`
- run `rails db:create`
- run `rails db:migrate`
- run `rails db:seed`
- run `rails s` and visit `localhost:3000`

# Creating a new Cat

So far, we've done `index` and `show` methods in our controller. But! We are going to need some others in order to create CRUD. What are we going to need? What do we get when we run `rails routes`?



# How to Write Rails Forms
https://launchacademy.com/codecabulary/learn-rails/writing-forms
When you write a form, it will usually correlate with a database table: a signup page will enter a new user in the users table Forms are thus a good way to begin thinking about Rails' RESTful architecture, which maps HTTP requests to CRUD queries.
## boxing off

Welcome to the world of Rails forms, which give users the ability to submit data into form fields.

# regular HTML form
```html
<form>
  <label>Post title:</label><br>
  <input type="text" id="post_title" name="post[title]"><br>
 
  <label>Post description:</label><br>
  <textarea id="post_description" name="post[description]"></textarea><br>
 
  <input type="submit" value="Submit Post">
</form>
```

# Form_For
it goes through and explains line by line what each thing is https://learn.co/tracks/full-stack-web-dev-with-react/rails/crud-with-rails/form_for-on-edit

```ruby
<%= form_for(@post) do |f| %>
  <label>Post title:</label><br>
  <%= f.text_field :title %><br>
 
  <label>Post description</label><br>
  <%= f.text_area :description %><br>
 
  <%= f.submit %>
<% end %>
```
https://learn.co/tracks/full-stack-web-dev-with-react/rails/crud-with-rails/rails-forms-overview



## paths vs route helpers
By design, Rails was meant to be flexible the result is that there are a number of ways to accomplish the same features. Routes are a great example of this - let's learn how to leverage built-in URL helper methods instead of hard coding route paths into an application.

![screen shot 2018-02-08 at 12 45 31 am](https://user-images.githubusercontent.com/6153182/35957431-86c65e7c-0c69-11e8-9d43-aecb9e4671d7.png)

## Rails URL helper

## `link_to` method
As you can see, even though we never added HTML code for the link –– e.g., <a href="..."></a> –– the link_to method rendered the correct tag for us.


# Rails: The CUD of CRUD

### Learning Objectives
-  Discuss forms in rails
-  form_tag vs form_for 
-  Use form helpers to generate forms in rails
-  Use partials to DRY up our views

# CUD
## Setting up our rails app

Fork and clone this repo and follow these steps:
- cd into `CatApp`
- run `bundle install`
- run `rails db:create`
- run `rails db:migrate`
- run `rails db:seed`
- run `rails s` and visit `localhost:3000`

# Creating a new Cat

So far, we've done `index` and `show` methods in our controller. But! We are going to need some others in order to create CRUD. What are we going to need? What do we get when we run `rails routes`?


## Our Cat controller

Before we can do anything with our cat form, we need two new controller methods: `new` and `create`. `new` won't do anything for now -- it'll just send back our `new.html.erb`. `create`, on the other hand, is going to create a new cat!

```rb
def create
  @cat = Cat.new(name: params[:name], breed: params[:breed])
  if @cat.save
    redirect_to cat_path(@cat)
  else
    render :new
  end
end
```
#### what is redirect_to & cat_path? What are they doing?

But, wouldn't it be better to check out our params first before we create the cat? We know what a cat needs to have in order to be created. Let's adjust that.

```rb
def create
  @cat = Cat.new(cat_params)
  if @cat.save
    redirect_to cat_path(@cat)
  else
    render :new
  end
end

private
def cat_params
  params.require(:cat).permit(:name, :breed)
end
```

We're saying that we expect there to be a `cat` in what we get back from the server, and that cat should have the fields `name` and `breed`.

### Using the `form_for` helper

So, this is pretty neat so far, right? We can add cats at will! But there is a _better way_ to write that form. Let's take a crack at that.

it goes through and explains line by line what each thing is https://learn.co/tracks/full-stack-web-dev-with-react/rails/crud-with-rails/form_for-on-edit

```erb
<%= form_for @cat do |f| %>
  <%= f.text_field :name, placeholder: "Name" %>
  <%= f.text_field :breed, placeholder: "Breed" %>
  <%= f.submit "Add new cat"%>
<% end %>
```

Instead of having to write the form ourselves, with the path and the method and the CSRF token and everything, it's now generated for us with this `form_for` tag! Yay!

We also have to add to our `new` method:

```rb
def new
  @cat = Cat.new
end
```

This creates a blank `cat` object that we're going to pass into our view.

## 🚀 LAB!

Add create functionality into your version of the cat app.

# Updating an existing cat

Same as before, just in our `edit` method we have to pass in the cat we're editing.

## 🚀 LAB!

Add update functionality into your version of the cat app.

# Deleting a cat (😿)

When we delete a cat, we can still use the `form_for` helper -- we just need to give it some additional information.

In the show view:

```html
<%= form_for @cat, html: {method: "delete"} do |f| %>
  <%= f.submit "Delete #{@cat.name}???" %>
<% end %>
```

We're telling the `form_for` helper that in the HTML for this particular form, the method should be `delete`.

## 🚀 LAB!

Add delete functionality into your version of the cat app.

# DRYing up our forms!

Take a look at our edit and new views. That form looks pretty similar, right? Let's abstract it out into a PARTIAL!

We're going to make a new file, `_form.html.erb`, within the `views/cats` directory. Then, we're going to take the form and put it in there.

Then, any time we want to include a form, we can just include this line in our erb file:

```html
<%= render partial: 'form'%>
```

Easy peasy!

## 🚀 LAB!

Make a form partial.
