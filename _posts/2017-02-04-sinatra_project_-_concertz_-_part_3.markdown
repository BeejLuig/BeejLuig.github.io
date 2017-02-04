---
layout: post
title:  "Sinatra Project - Concertz - part 3"
date:   2017-02-04 18:10:28 +0000
---


*This is part 3 of a multi-part series. If you haven’t already, check out parts 1 and 2.*

After completing my Sinatra project, I was so excited to jump into learning Ruby on Rails that I completely forgot about finishing this blog series! Since it has been so long between posts, I can barely remember where I left off in the process, so I will keep this last post short and sweet. 

**[THE PROJECT](https://github.com/BeejLuig/concertz-sinatra-project)**

I was very pleased with the way this project came out. I put a little extra love into it and made sure that each page looked pretty. 

<img src="http://bjcantlupe.com/img/artist-page.png" alt="Artist Page Screenshot" style="max-width: 500px; border: 2px solid gray;" />
oohhh ahhh

To review the functionality of the app, we have: 

* A homepage that shows an index of upcoming and past concerts
* Concert detail pages with ticket price, description, and links to the performing artist
* Artist detail pages with an artist bio and links to related upcoming and past concerts
* A sign up page to create a new user. A user can log in to create new artists and concerts
* A login page for users
* A user detail page with user information and related artist/concert information.

All three models (User, Artist, Concert) have the CRUD functionality. When an `Artist` is destroyed, all of it's related `Concerts` are also destroyed. When a `User` is destroyed, so too are the related `Artists` and `Concerts`.

A user has special privileges over a "guest" in this project. If you sign up and create an artist and concert(s), you will see "edit" and "delete" buttons on their detail pages. A user can only edit or destroy artists and concerts that belong to him/her. A guest can only view artist and concert pages. 

Users must be unique. You can't sign up with a username or email that is already taken. There are a few more features, but the best way to get a feel for them is to try out the project for yourself.

Check out the repo [here](https://github.com/BeejLuig/concertz-sinatra-project) and play around with it. Feedback is welcome!

**WHAT COULD HAVE BEEN BETTER**

I admittedly slacked a bit when it came to security in this application. In my project review, the Flatiron instructor pointed out that security checks are necessary for every action, not just signing in or out. Sadly, that makes my app hackable. I decided to leave the project in it's current state so I can remember what not to do in the future.

I also failed to add informative messages to potential new users who try usernames that are already taken. I ommitted this feature in the interest of time and was of course promptly called out on it. Womp womp.

Lastly, I think I could have done a little better with the seed data. If you check out the repo and load the application, you may notice that there are no upcoming concerts. That is because I did not dynamically set the dates of the seed concerts. If you want to see an upcoming concert, you'll have to make one yourself!

**TAKEAWAYS**

I'm going to try to add a "takeaway" section to any relevant posts, since it seems people like [that sort of thing](http://blog.flatironschool.com/7-hackathon-takeaways-from-devfest-2016/). For this project, my takeaways are thus:
1. Time is of the essence, but it's not too important to skip on due diligence
2. If if feels like you should add a feature, just do it. It's easier to take it out later if deemed unnecessary
3. Put some love into your project. Even if you never think about it again, it's still your project and your name is still on it

That's all, folks! Thanks for reading.
