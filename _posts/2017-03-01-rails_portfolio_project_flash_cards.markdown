---
layout: post
title:  "Rails Portfolio Project: Flash Cards"
date:   2017-03-01 03:51:14 +0000
---


My latest project for the Flatiron School's online verified program is complete! This is my most feature-filled application to date, and it was definitely the most fun to work on. Ok, let's get into it!

**Flash Cards**

<img class="img img-responsive" alt="Flash Cards homepage" src="https://raw.githubusercontent.com/BeejLuig/BeejLuig.github.io/master/img/flash-cards-homepage.png" />

*Flash Cards* is a web application for viewing and managing sets of flash cards, dubbed "study sets." A guest can see any study set and view user profiles. A logged in user can create manage their own study sets, organize study sets in folders, and copy other user's study sets into their own collection. A user can also take advantage of the "study mode" feature. 

**Managing User Sessions with Omniauth and Devise**

I opted to utilize [Devise](https://github.com/plataformatec/devise) to manage registering new users and managing sessions. I only needed a user to login with an email and password, and the Devise defaults suited this nicely. Devise also plays well with [Omniauth](https://github.com/omniauth/omniauth) (kinda), which was nice because Omniauth was a requirement for this project. I used Omniauth to enable [Google OAuth2](https://github.com/zquestz/omniauth-google-oauth2) sign-ins. It would have been nice to authorize a single user via username/password OR Google OAuth, but it was a headache that I decided to avoid. 

<figure style="margin: 0 auto;text-align:center;">
  <video controls="" autoplay="" name="media" style="max-width: 100%;">
    <source src="http://bjcantlupe.com/img/flash-cards-omniauth.mp4" type="video/mp4">
  </video>
  <figcaption><small>Logging in with Omniauth</small></figcaption>
</figure>

Unfortunately in development mode, the client ID and secret key required to utilize Omniauth don't like to stick around for very long. I used the [Dot Env](https://github.com/bkeepers/dotenv) gem to hold my ENV variables. If you decide to use this, don't forget to add your `.env` file to `.gitignore`. You don't want strangers seeing those variables!

**Models**

There are four models for this application:

User

```ruby
class User < ApplicationRecord
  has_many :folders
  has_and_belongs_to_many :study_sets
  has_many :flash_cards, through: :study_sets, source: :flash_cards
end
```

StudySet

```ruby
class StudySet < ApplicationRecord
  has_and_belongs_to_many :studiers, class_name: "User"
  belongs_to :owner, class_name: "User"
  has_many :flash_cards
  has_and_belongs_to_many :folders

  validates :title, presence: true

  accepts_nested_attributes_for :flash_cards, allow_destroy: true
end
```

FlashCard

```ruby
class FlashCard < ApplicationRecord
  belongs_to :study_set
  validates :term, :definition, presence: true
end
```

Folder

```ruby
class Folder < ApplicationRecord
  belongs_to :user
  has_and_belongs_to_many :study_sets
  validates :name, :user_id, presence: true
end
```

Not too complicated. The most important relationship is the one between `StudySet` and `User`. If you look in the `StudySet` class, you will see this line:

`belongs_to :owner, class_name: "User"`

This is the relationship between a newly created instance of `StudySet` and the user who created it. There can be only *one* owner.

Before that line, you can see this:

`has_and_belongs_to_many :studiers, class_name: "User" `
	
Users can see and interact with any study set. At the bottom of every study set is a "Study Mode" button. Whenever a unique user presses that button on a study set, that user is added to the study set's `studiers` array. This is why `StudySet` has many `:studiers`.

<figure style="margin: 0 auto;text-align:center;">
  <img src="http://bjcantlupe.com/img/flash-cards-studymode-button.png" class="img img-responsive" alt="study mode button">
  <figcaption><small>Engage study mode!</small></figcaption>
</figure>

A user should also know about which study sets he/she has studied. Users can then easily find recent study sets in their account without copying them. They are listed under "Recently Studied."

<figure style="margin: 0 auto;text-align:center;">
  <img src="http://bjcantlupe.com/img/flash-cards-user-page.png" class="img img-responsive" alt="user profile page">
</figure>
	
**Controllers and routes**
	
Besides the automatically generated Devise controller and `ApplicationController`, there are four controllers for this app. The first is the `Users::OmniauthCallbacksController`. This controller kicks in when a user attempts to login by pressing the GoogleOAuth2 link. 
	
```ruby
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController

  def google_oauth2
    @user = User.from_omniauth(request.env["omniauth.auth"])
    if @user.persisted?
      session[:user_id] = @user.id
      sign_in_and_redirect @user, :event => :authentication #this will throw if @user is not activated
      set_flash_message(:notice, :success, :kind => "Google OAuth2") if is_navigational_format?
    else
      session["devise.google_oauth2_data"] = request.env["omniauth.auth"]
      redirect_to new_user_registration_url
    end
  end

  def failure
    redirect_to root_path
  end

end
```
	
There are two actions here. One is for a successful callback, and the second is for a failure. If you are trying to use Omniauth with Devise, take a good look at those action methods. This is necessary for Omniauth to work with Devise, and it was sort of a pain to figure out. A devise sanitizer is also necessary to permit specific keys from the callback hash. This is what I have in my `ApplicationController`:

```ruby
protected

def configure_permitted_parameters
  devise_parameter_sanitizer.permit(:account_update, keys: [:image])
end
```

This was necessary so I could pull the Google profile image from `request.env["omniauth.auth"]`.

`UsersController` has one lowly action.

```ruby
class UsersController < ApplicationController
  before_action :authenticate_user!, except: [:show]

  def show
    @user = User.find_by_id(params[:id])
    @study_sets = @user.study_sets
    @folders = @user.folders
  end
end
```

Devise takes care of the rest of the `User`-related actions!

Take note of the top line there:

`before_action :authenticate_user!, except: [:show]`. The `authenticate_user!` helper method is provided by Devise to clean up authentication logic. you can pass in an array of exceptions to allow guests to view certain pages. I have that line at the top of every controller. The only thing that changes is what is inside `except: []`.

<figure style="margin: 0 auto;text-align:center;">
  <video controls="" autoplay="" name="media" style="max-width: 100%;">
    <source src="http://bjcantlupe.com/img/flash-cards-authorization-example.mp4" type="video/mp4">
  </video>
  <figcaption><small>The <code>authenticate_user!</code> helper at work</small></figcaption>
</figure>

`FoldersController` and `StudySetsController` look pretty similar, and they are pretty long. So, instead of sharing both completely, I'm just going to explain the pattern.

As mentioned before, user authentication starts at the top, using the `authenticate_user!` helper method. For some actions, like `#create`, `#update` and `#destroy`, it is necessary to ensure that the current user is the same as the user attached to the model being manipulated. I created a couple helpers in `ApplicationController` for that.

```ruby
  def user_verified?
    params[:user_id].to_i == current_user.id
  end

  def whoops
    flash[:notice] = "Whoops! You can't go there."
    redirect_to user_path(current_user)
  end
```

The general controller logic looks like this:

```ruby
def create
 if user_verified?
  #some code 
 else 
  whoops
 end
end
```

Nice and simple! Nothing too crazy there. 

<figure style="margin: 0 auto;text-align:center;">
  <video controls="" autoplay="" name="media" style="max-width: 100%;">
    <source src="http://bjcantlupe.com/img/flash-cards-user-validation-example.mp4" type="video/mp4">
  </video>
  <figcaption><small>User verification</small></figcaption>
</figure>

Now, I want to point out a few extra actions in `StudySetsController`.

Let's start with `#sort`. In a study set show page, there is a drop-down select with an option to sort flash cards alphabetically. On change, the sort option triggers the `#sort` action, pushing the option value to the `params` hash.

```ruby
  def sort
    @study_set = StudySet.find_by_id(params[:id])
    @sort ||= params[:sort]
    if params[:sort] == "Alphabetical"
      @flash_cards = @study_set.flash_cards.sort_by {|fs| fs.term }
    else
      @flash_cards = @study_set.flash_cards
    end
    render :show
  end
```
<figure style="margin: 0 auto;text-align:center;">
  <figcaption><small>Not sorted</small></figcaption>
  <img src="http://bjcantlupe.com/img/flash-cards-sort-before.png" class="img img-responsive" alt="unsorted flash cards">
</figure>

<figure style="margin: 0 auto;text-align:center;">
  <figcaption><small>Sorted!</small></figcaption>
  <img src="http://bjcantlupe.com/img/flash-cards-sort-after.png" class="img img-responsive" alt="sorted flash cards">
</figure>

Then we have the `#copy` action. If a user is viewing a study set show page created by another user, a "Copy this" link appears in the middle of the page. When clicked, the `#copy` action is triggered. 

```ruby
  def copy
    @study_set = StudySet.find_by_id(params[:id])
    @study_set.make_copy(current_user)
    redirect_to user_path(current_user)
  end
```

`make_copy(current_user)` is a helper method I added in the `StudySet` model. It looks like this:

```ruby
  def make_copy(user)
    copy = self.dup
    self.flash_cards.each do |flash_card|
      copy.flash_cards << flash_card.dup
    end
    user.study_sets << copy
    copy.owner = user
    user.save
    copy.save
  end
```

The `#dup` method duplicates all of the attributes of an object. In the case of `StudySet`, I need to `dup` the `study_set` instance and all of the `flash_cards` belonging to `study_set`.

<figure style="margin: 0 auto;text-align:center;">
  <img src="http://bjcantlupe.com/img/flash-cards-copythis.png" class="img img-responsive" alt="copy this">
  <figcaption><small>Copy this</small></figcaption>
</figure>

Lastly, we have the `#study_mode` action. This is attached to the button I mentioned earlier. For this button, I wanted a different authentication behavior. 

```ruby
  def study_mode
    @study_set = StudySet.find_by_id(params[:id])
    if !current_user
      @flash_cards = @study_set.flash_cards
      flash[:alert] = "You must be signed in to use this feature!"
      redirect_to user_study_set_path(@study_set)
    else
      @study_set.add_studier(current_user)
      @flash_cards = @study_set.flash_cards
      @study_mode = true
      render :show
    end
  end
```

If a guest presses the button, the page will reload with a flash alert telling them to sign in. A logged-in user will successfully enter *study mode*. The `add_studiers` helper is called, and the `:show` page reloads, this time displaying an alternate view.

<figure style="margin: 0 auto;text-align:center;">
  <video controls="" autoplay="" name="media" style="max-width: 100%;">
    <source src="http://bjcantlupe.com/img/flash-cards-studymode-example.mp4" type="video/mp4">
  </video>
  <figcaption><small>Study mode engage, for real this time!</small></figcaption>
</figure>

Here is the `#add_studiers(user)` method

```ruby
  def add_studier(user)
    if !self.studiers.include?(user)
      self.studiers << user
      self.save
    end
  end
```

The method checks to make sure the current user has not already been added to this `study_set`'s `studiers` array. If `current_user` is unique, it is pushed in.

**Search feature**

Ok, so there is one more thing I want to talk about because it was a feature that I assumed would be more difficult to implement than it actually was. That feature is the *search form*. On the home page is a search form. This searches `StudySet.all` with a class-level query on the `:title` and `:description` attributes. Here is the search-form:

```html
<div class="search-bar">
  <%= form_tag(root_path, method: "get", id: "search-form") do %>
    <div class="input-group">
      <%= text_field_tag :search, params[:search], class: "form-control" %>
      <span class="input-group-btn">
        <%= submit_tag "Search", name: nil, class: "btn btn-default"%>
      </span>
    </div><!-- /input-group -->
  <%end%>
</div><!-- /.search-bar -->
```

This form basically just sends the inputted text to the params hash in `params[:search]`. Then, in `study_sets#index`:

```ruby
  def index
    @study_sets = StudySet.all
    if params[:search]
      @study_sets = StudySet.search(params[:search]).order("created_at DESC")
    else
      @study_sets = StudySet.all.order("created_at DESC")
    end
  end
```

There is a check for the existence of `params[:search]`. If there are search params, the @study_sets instance variable is set to the results of the `StudySet.search(params[:search])` query. This class-level query looks like this:

```ruby
  def self.search(search)
    where("title LIKE ? OR description LIKE ?", "%#{search}%", "%#{search}%")
  end
```

And that's it! The results of the search will be visible on the page. If the results are empty, the page will show a message and a "back" link. 

<figure style="margin: 0 auto;text-align:center;">
  <video controls="" autoplay="" name="media" style="max-width: 100%;">
    <source src="http://bjcantlupe.com/img/flash-cards-search-example.mp4" type="video/mp4">
  </video>
  <figcaption><small>Searching on the homepage</small></figcaption>
</figure>

**Views**

I'm actually not going to get into the view yet. I will be updating this application for my next project (JavaScript and jQuery). It will make more sense to write about the front-end then. In the meantime, I'll let the images and videos above speak for themselves.

Here is the full demo! I found a mistake towards the end of the video. The mistake is fixed, the video is not ;)

<iframe width="560" height="315" src="https://www.youtube.com/embed/sfPlwC9oZ6o" frameborder="0" allowfullscreen></iframe>
