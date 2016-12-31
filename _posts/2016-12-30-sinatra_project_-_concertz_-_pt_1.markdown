---
layout: post
title:  "Sinatra Project - Concertz - Pt. 1"
date:   2016-12-30 21:36:46 +0000
---


In the last few weeks I have been working in the Sinatra and ActiveRecord sections of the Learn program. At the conclusion of this section is a MVC (model, view, controller) web application project using Sinatra and ActiveRecord. For this project I am going to chronicle my process from beginning to end in a multi-part blog miniseries. It's gonna be great!

**In the beginning, there was an outline**

The toughest part of any new project is figuring out *what to do*. I flip-flopped for quite a while before settling on an elegantly simple idea: a concert list. The project is called *Concertz*, and it will be a web app that shows concert listings for artists. The homepage will display a list of concerts, possibly organized by date and location. If logged in, a user can create and edit artists as well as show listings. 

My MVC outline for this project is below. The outline will be updated as the project goes on.

# Concertz Outline

**MODELS**

**User**

  has_many :artists 
  
  has_many :concerts #not sure if this is necessary

  instance variables
  
    email
    
    password

**Artist**  

  belongs_to :user  
  
  has_many :concerts  

  instance variables - 
  
    name

  instance methods -  
  
    slug

**Concert**

  belongs_to :user
  
  belongs_to :artist

  instance variables - 
  
    date
    
    time
    
    location
    
    ticket_price

**VIEWS**

`index.erb` - Show upcoming and past concerts

`layout.erb` - layout template. Includes login/signup buttons if not logged in. If logged in, concerts sign out button.

**Users**

`login.erb` - Login Page, includes form with email/password for login

`signup.erb` - Sign Up Page, includes form with email/password for 
sign up

`/users/show.erb` - show user information. Includes artists and concerts. Artists headers link to artist page. Include "create artists" button

**Artists**

`/artists/index.erb` - show listing of all artists. 

`/artists/show.erb` -  Show artist info, list concerts belonging to artist. if current_user.id == @artist.user.id, include edit button and delete button and form to add concerts. 

`/artists/edit.erb` - show form to edit artist

**Concerts**

`/concerts/show.erb` - show info about a particular concert. If logged_in, include edit and delete buttons

`/concerts/edit.erb` - show form to edit a particular concert


**CONTROLLERS**

**ApplicationController**

`GET '/'` - Verify if logged_in. index.erb

helper methods

  current_user
  
  logged_in?

**UsersController**

`GET '/users/login'` - Verify if logged_in. If true: login.erb. else: redirect to '/'.

`POST '/users/login'` - authenticates login. if true: add user_id to session and redirect to '/'. if false: redirect to '/login'

`GET '/users/signup'` - signup.erb

`POST '/users/signup'` - authenticate signup. if true: create user, add user_id to session and redirect to '/'. if false: redirect to '/signup'.

`GET '/users/:id'` - verify current_user.id matches params[:id]. If true: users/show.erb . else: redirect to '/'.

**ConcertsController**

`GET '/concerts'` - redirect to '/'

`GET '/concerts/:id'` - 'concerts/show.erb'

`POST '/concerts'` - create new concert instance. redirect to '/concerts/:id'

`PATCH '/concerts/:id'` - edit concert. redirect to '/concerts/:id'

`DELETE '/concerts/:id'` - delete concert. redirect to '/'

**ArtistsController**

`GET '/artists'` - verify if logged_in '/artists/index.erb' 

`POST '/artists'` - create new Artist through user. redirect to '/artists/:slug'

`GET '/artists/:slug'` - verify if logged_in. Artist.find_by(slug: params[:slug]). '/artists/show.erb'

`GET '/artists/:slug/edit'` - verify if logged_in. /artists/edit.erb.

`PATCH '/artists/:slug'` - update artist info. redirect to '/artists/:slug'

`DELETE '/artists/:slug'` - delete artist

