---
layout: post
title:  "YT Tutorial Dashboard: React/Redux and Rails"
date:   2017-05-01 00:13:47 +0000
---


I am excited to announce my most challenging and ambitious project for the Flatiron School's Full-Stack Web Development program: [**YouTube Tutorial Dashboard**](https://yttd.herokuapp.com/)! This project is split up into two separate repositories:

- [Ruby on Rails API Backend](https://github.com/BeejLuig/yt-tutorial-dashboard)
- [React/Redux Frontend](https://github.com/BeejLuig/react-yt-tutorial-dashboard)

![YTTD homepage](http://bjcantlupe.com/img/yttd-home.png)

**PURPOSE**

You may be familiar with tutorial websites like Lynda.com or egghead.io. Those sites provide fantastic interfaces for keeping track of your progress within a series of videos. YouTube has a wealth of tutorial videos, but there isn't a way to keep track of your progress with them. 

The purpose of YT Tutorial Dashboard is to strip away all of the distractions from YouTube and provide a clean interface to interact with tutorial playlists that you choose. 


**Signing up/Logging In**

On the back end, sign up/login and refresh authentication is done using the [jwt gem](https://github.com/jwt/ruby-jwt). On the front end, form validations are performed by [redux form](http://redux-form.com/6.6.3/) and redirects by [React Router](https://reacttraining.com/react-router/web/api/Redirect). 

![Login error](http://bjcantlupe.com/img/yttd-loginerror.png)

![Form validation](http://bjcantlupe.com/img/yttd-formvalidation.png)

**Adding a Video**

For the sake of ease, I opted not to build out a search function using the YouTube Data API. Instead, you find the playlist you are looking for through YouTube, and copy/paste the URL of the playlist, or just the playlist ID. The playlist id follows the word "list" in a YouTube URL, like this: `list=PLoYCgNOIyGADILc3iUJzygCqC8Tt3bRXt`. Playlist IDs can be found in the URL of a playlist or a video within a playlist. 

![Adding a playlist URL](http://bjcantlupe.com/img/yttd-playlistid.png)

![New playlist modal](http://bjcantlupe.com/img/yttd-newplaylistmodal.png)

**The Dashboard**

The dashboard shows a list of added playlists, represented as collapsed cards. Clicking on the card opens some basic information. 

![Dashboard](http://bjcantlupe.com/img/yttd-dashboard.png)

For more detailed information, click the "Stats" button. 

![Stats modal](http://bjcantlupe.com/img/yttd-statsmodal.png)

**Watching Videos**

Clicking the "Watch" button takes you to the "watch" view. The YouTube video embed component is from [react-youtube](https://www.npmjs.com/package/react-youtube). To the right of the embedded video is a panel with a list of videos in the active playlist. Completed videos have a green dot next to their title, incomplete videos are grey. The active video has a yellow dot. Clicking the "Done with this video" button will mark the active video as complete and navigate to the next video in the playlist. 

![Video view](http://bjcantlupe.com/img/yttd-watch.png)

**Updating Playlists**

Going back to our dashboard, if you click on the "Edit" button of a playlist card, you will see an edit modal with some options. It's pretty self-explanatory.

![Edit modal](http://bjcantlupe.com/img/yttd-editmodal.png)


**Extras**

The CSS framework is built on [Bulma](http://bulma.io/). It was a new discovery but I loved it as an alternative to Bootstrap. I think I even like it better! 

This is not my prettiest project code-wise, but it was my most ambitious, so I don't feel so bad about it. There is a ton of room for improvement in the codebase, and I'm looking forward to going back and refactoring (after a week long beer break). My ultimate takeaway from this project is that React/Redux takes a while to get going, but it is definitely an awesome way to tackle complex front-end problems. I can't wait to work on my next React/Redux project!

See a usage video for **YT Tutorial Dashboard** below. Thanks for reading!

<video controls>
  <source src="http://bjcantlupe.com/img/yttd-addingaplaylist.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>



