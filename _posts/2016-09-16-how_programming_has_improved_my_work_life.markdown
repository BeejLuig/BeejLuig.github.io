---
layout: post
title:  "How Programming Has Improved My Work Life"
date:   2016-09-16 14:07:06 +0000
---


In the past few years, I have developed a fetish for automation. My Android phone is loaded with automation apps like [Automate](http://llamalab.com/automate/) and [Tasker](http://tasker.dinglisch.net/), and I have recently begun playing around with [Google Apps Script](https://www.google.com/script/start/). Basically, I'm hoping that one day I will be to the point where I can check my phone and computer a couple times a day just to see my creations doing all of the grunt work for me. Recently, this passion has started trickling into my daily work life.

I work in a university library full time and no, but not as a librarian. My title is "circulation assistant," which basically means I do everything that the librarians don't have time (or just don't want) to do. I like the job, but some of the responsibilities are a little tedious. Fortunately, the new skills I am acquiring as a programmer are giving me the opportunity to take on some more interesting projects, like maintaining the library website (which is a hot mess). I am becoming sort of a hot commodity in my department, and because of it I am afforded a little more freedom at work than what my job title suggests.

## `tumblr.rb`, the CLI I use every day

Besides getting to do work that is above my paygrade and actually what *want* to do, I have been able to, at times, find ways to apply the things I am learning directly to every day problems. One example is actually my first CLI, `tumblr.rb`. It's a humble little program and admittedly not the cleanest in the world, but it has turned my least favorite task of the day into one of the things I look forward to the most!

Long story short, every day I have to post 5-10 images on our library Tumblr account, and there are specific guidelines about captioning. It needs to looks something like this:

> *This is the Title for a Book: Subtitle Here* 
>
>  by Joe Schmo 
>
>  Call # 1234.56a 

The library catalog records that we get this information from looks like this:

> This is the title for a book : subtitle here / Joe Schmo.

The call number is listed elsewhere. Obviously, these formatting differences are not ripe for vanilla copy/pasting. They are, however, perfect for parsing! How great would it be to copy the library catalog info as is and have it magically return the proper formatting? Pretty great. 

I'm not going to do a step-by-step of this program. Instead, I'm going to list all of the features I wanted to add, and point to them using comments within the code. 

**Features**
* Ask for user input
* Receive user input as such "This is the title for a book : subtitle here / Joe Schmo."
* Capitalize titles according to Library of Congress rules, capitalize the first word after a colon ":"
* Split the title and author at the forward slash " / "
* account for titles with no author
* account for editors, curators, etc. in formatting
* display the formatted information in a way that can be copy/pasted into Tumblr captions

I wrote my program in Ruby, in a file called `tumblr.rb` and placed it in my user folder. So every morning, I open up Terminal and type `ruby tumblr.rb`. Let's take a look and see what is going on in the background:

```
def tumblr
  no_caps = ['a', 'an', 'the', 'at', 'by', 'for', 'in', 'of', 'on', 'to', 'up', 'and', 'as', 'but', 'or', 'nor', 'from']  # array of words that should not be capitalized in a title

  puts "Copy and paste title/author information" # prompts for user input

  title_author = gets.chomp # gets user input

  if title_author == "end" || title_author == "exit" # closes the program if input is "end" or "exit"
    abort("Ended Tumblr session")
  end

  if title_author.include?(" / ")    # split at the slash if title and author are both present
    title_author = title_author.split(" / ")
    title = title_author[0]
    author = title_author[1]
  else
    title = title_author # otherwise, only format the title
  end

  title_array = [] 
  title_array = title.split(" ")
  title_array.each do |word| #formats the title by checking each word against the no_caps array
    index = title_array.index(word)
    if no_caps.include?(word) 
      word
    else
      word.capitalize!
    end

    if title_array[index-1] == ":" #capitalizes the first word in a subtitle
      word.capitalize!
    end
  end
  title = title_array.join(" ")
  title.sub! ' : ', ': ' #takes the unnecessary space away from the colon between title and subtitle


  puts "" #presents the title and author info
  puts "#{title}"
  if author != ""
    if author.end_with?('.')
      author.delete!('.')
    end
    if author[0] == author[0].downcase
        puts "#{author}"
    else
        puts "by #{author}"
    end
  end

  puts "Call # " # call number isn't included in the input, so it just sets up the formatting
  puts ""
  tumblr #starts the tumblr method over
end

tumblr #calls the tumblr method!
```

I am about to use this program right now, so lets see it in REAL LIFE ACTION

<div style="width: 100%; text-align: center;">
  <video width="582" height="368" controls>
    <source src="/img/tumblrrb.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</div>

Ah, my new favorite part of the day: watching my programs do the grunt work for me.
