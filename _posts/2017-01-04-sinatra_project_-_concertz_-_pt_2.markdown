---
layout: post
title:  "Sinatra Project - Concertz - Pt. 2"
date:   2017-01-04 16:16:23 +0000
---


*This is part 2 of a multi-part series. If you haven't already, check out [part 1](http://bjcantlupe.com/2016/12/30/sinatra_project_-_concertz_-_pt_1/)*.

**Paying attention to workflow**

The nice thing about using a model-view-controller design pattern within a CRUD (create, read, update, delete) application is that it is easy to jump into a workflow. First, you want to set up all of your models and their relationships. Then, you go back and forth between the views and controllers while creating each of the CRUD operations for your models. Sticking to this flow not only allows you to work efficiently, it also makes it easy to pick up where you left off. 

What screws up my workflow is a basic html view. What's the point of setting up routes with dynamic content if they all lead to ugly pages? I can't help wanting to spruce up every page with CSS styling as I go. In general, I don't think that's a bad thing. It does become a problem, though, when I go down the rabbit hole and can't remember where I left off. It also makes for some messy class/id assignments and stylesheets. This is why I think it's so important to pay attention to workflow. I'm using this post as an opportunity to remind myself to stay on task. Feel free to learn from my mistakes.

**Update & Updated Outline**

I have spent a couple days working on this project now, and it is actually nearly complete! Since my last post, I made some changes to my outline to reflect what I have done so far. The last feature I need to add before debugging is the DELETE operation for users, artists and concerts. The updated outline is below. Completed features are indicated with an 'x'.

# Concertz Outline

**MODELS**

**User**

[x]  has_many :artists 
  
[x]  has_many :concerts, through: :artists

[x]  instance variables
  
    email
    
    password
		
    username
    
 [x] instance methods
   
     authenticate

**Artist**  

[x] belongs_to :user  
  
[x] has_many :concerts  

[x] instance variables
  
    name
		
	bio

[x]  instance methods 
  
    slug
    
[x] class methods 
  
    find_by_slug

**Concert**
  
  [x] belongs_to :artist
  
  [x] has_one :user, through: artist

  [x] instance variables
  
    concert_date
    
    location
    
    ticket_price
		
	description

**VIEWS**

[x] `index.erb` - Show upcoming and past concerts

[x] `layout.erb` - layout template. Includes login/signup buttons if not logged in. If logged in, concerts sign out button.

**Users**

[x] `login_user.erb` - Login Page, includes form with email/password for login

[x] `create_user.erb` - Sign Up Page, includes form with email/password for 
sign up

[x] `show_user.erb` - show user information. Includes artists and concerts. Artists headers link to artist page. Include "create artists" button

[x] `edit_user.erb` - show form to edit user info, including username, email, password

**Artists**

[x] `artists.erb` - show listing of all artists. 

[x] `create_artist.erb` - show form to create a new artist. 

[x] `show_artist.erb` -  Show artist info, list concerts belonging to artist. if current_user.id == @artist.user.id, include edit button and delete button and form to add concerts. 

[x] `edit_artist.erb` - show form to edit artist

**Concerts**

[x] `show_concert.erb` - show info about a particular concert. If logged_in, include edit and delete buttons

[x] `edit_concert.erb` - show form to edit a particular concert

[x] `create_concert.erb` - show form to create a new concert

*Note: concerts index is represented in the homepage*


**CONTROLLERS**

**ApplicationController**

[x] `GET '/'` - Verify if logged_in. index.erb

[x] configure do 
   
	 set session secret
	 
	 set public_folder 
	 
	 set views

[x] helper methods

    current_user
  
    logged_in?

**UsersController**

[x] `GET '/login'` - Verify if logged_in. If true: login.erb. else: redirect to `'/'`.

[x] `POST '/login'` - authenticates login. if true: add user_id to session and redirect to `'/'`. if false: redirect to `'/login'`

[x] `GET '/signup'` - Verify if logged_in. If true: redirect to `"/users/#{current_user.id}"`, else: `create_user.erb`

[x] `POST '/signup'` - authenticate signup. if true: create user, add user_id to session and redirect to` '/'`. if false: redirect to `'/signup`'.

[x] `GET '/users/:id'` - verify `current_user.id` matches `params[:id]`. If true: `'users/show_user.erb'` . else: redirect to `'/'`.

[x] `GET '/users/:id/edit` - verify if logged_in && @user == current_user. If true: `'/users/edit_user.erb'`. else: redirect to `'/login'`

[x] `PATCH '/users/:id` - verify if logged_in && @user == current_user && @user.valid?. If true: @user.save updated information and redirect to `"/users/#{@user.id}"`. 

[x] `GET '/logout'` - verify if logged_in. If true: session.clear and redirect to `'/'`. else: redirect to `'/'`.

[ ] `DELETE '/users/:id'` - verify if logged_in && @user == current_user. If true: delete @user and all associated artists and concerts. Clear session and redirect to `'/'`. else: redirect to `'/'`.

**ConcertsController**

[x] `GET '/concerts'` - redirect to '/'

[x] `GET '/concerts/new` - verify if logged_in. If true: `'concerts/create_concert.erb'`. else: redirect to `'/'`.

[x] `GET '/concerts/:id'` - `'concerts/show_concert.erb'`

[!] `POST '/concerts'` - create new concert instance. If logged_in: proceed to validity check. else: redirect to `'/'`. If @concert.valid? && @concert.artist.valid? redirect to `"/concerts/#{@concert.id}"`. else: redirect to `'/concerts/new'`.  
*Note: Just realized I'm missing a logged_in check here.*

[x] `GET '/concerts/:id/edit` - if `logged_in && current_user.concerts.include?(@concert)`: `'/concerts/edit_concert.erb'`

[x] `PATCH '/concerts/:id'` - edit concert. if `logged_in && current_user.concerts.include?(@concert)`: @concert.save and redirect to `"/concerts/#{@concert.id}"`. else: redirect to `"/concerts/#{@concert.id}"`.

[ ] `DELETE '/concerts/:id'` -  if `logged_in && current_user.concerts.include?(@concert)`: delete concert. redirect to '/'

**ArtistsController**

[x] `GET '/artists'` - `'/artists/artists.erb' `

[x] `GET '/artists/new` - verify if logged_in `'/artists/create_artist.erb

[x] `POST '/artists'` - verify if logged_in. create new Artist through user. redirect to `"/artists/#{@artist.slug}"`. else: redirect to `'/artists/new'`

[x] `GET '/artists/:slug'` - Artist.find_by(slug: params[:slug]). `'/artists/show_artist.erb'`

[x] `GET '/artists/:slug/edit'` - verify if `logged_in && current_user.artists.include?(@artist)`. `'/artists/edit_artist.erb'`.

[!] `PATCH '/artists/:slug'` -  verify if logged_in, then proceed to validity check. If false: redirect to `'/'. verify if `@artist.valid? && current_user.artists.include?(@artist)`. update artist info. redirect to `'/artists/:slug'`
*Note: Just realized I'm missing a logged_in check here.*

[ ] `DELETE '/artists/:slug'` -  verify if `logged_in && current_user.artists.include?(@artist)`. delete artist and all associated concerts


Using my blog as a glorified to-do list: it's a new low! If you made it all the way to the bottom of this post, this is for you.

![You get a cookie!](http://www.clipartbest.com/cliparts/ncE/E8g/ncEE8g8Li.jpeg)
