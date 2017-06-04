---
layout: post
title:  Become a Wizard with Google Apps Script
date:   2017-06-03
---


If you are like me, you like to use productivity apps like [IFTTT](https://ifttt.com/discover) and [Automate](https://llamalab.com/automate/)*, and you are constantly looking for new ways to improve your day-to-day efficiency. It's also likely that you use Google Apps (Gmail, Sheets, Docs, Calendar, etc) for personal or work use. Combining automation apps with Google can be a powerful thing, but wouldn't it be cooler if you could implement your own ideas without them? I say yes.

This is an introduction to [Google Apps Script](https://developers.google.com/apps-script/). Apps Script is a cloud-based platform in JavaScript. If you can write JavaScript, you can write Google Apps Script! One thing to be careful of though: as of now, Apps Script doesn't support ECMAScript 6. That means no block scoped variables or arrow functions for you! 

In this post we are going to build a to-do list flow combining Gmail, Google Sheets and Google Calendar. The idea is this: you send yourself an email with "to do" or "todo" in the subject, and a new task in the body. A spreadsheet will be periodically checking for emails in your inbox with that subject line, and when it finds one (or more), it will add a new row to the sheet with the given task. At the beginng of every day, a new calendar event will be added to your default calendar with the entire list of to-dos in the description. That's a bunch of integration. The good news is it will only take about 25 lines of code! 

**Sorry iPhone peeps, Automate is for Android only*


**Getting Started**

Google Apps Script almost always live inside a spreadsheet. We'll get started by creating a new sheet and calling it *Todo*. Then, at the top of our toolbar, we are going to navigate to `Tools > Script editor...`

You should see something like this:

![Apps Script editor](http://bjcantlupe.com/img/apps-script-text-editor.png)

Nice, we're in! We will be building out two functions, the first of which will take care of scanning our Gmail inbox and updating the spreadsheet. 

Add this to the script editor:

```javascript
function addToDo() {
	
}
```

**Accessing Google Apps**

Before we can do anything else, we have to actually access the spreadsheet we are in, as well as the Gmail inbox.

```javascript
function addToDo() {
	var threads = GmailApp.getPriorityInboxThreads(0, 10);
	var sheet = SpreadsheetApp.getActiveSheet();
}
```
As you can see, the various Google App services are accessed by classes with the naming pattern of `AppName + "App"`. What would it look like if we wanted to access the Calendar App? You guessed it: `CalendarApp`!

The `getActiveSheet` method is used to pull the current sheet open in the UI. For demo purposes, this will be just fine. If you really want to use this script for your daily to-dos, you may want to consider amending that second line to `SpreadsheetApp.openByUrl('your_spreadsheet_url_here').getActiveSheet();`

We are using the `getPriorityInboxThreads` method to pull the 10 most recent emails from our inbox. There is a `getInboxThreads` method, but it will include a bunch of emails we won't need to check.

> **Quick Tip**: the [Apps Script docs](https://developers.google.com/apps-script/reference/calendar/) are great, keep them handy for detailed information about these class methods!

**Iterating over Gmail Threads**


Now, we are going to loop through all of those threads and check the subjects of the first message. Presumably, there will only ever be one message in the 'thread' of an email we send to ourselves. We can do that like this:

```javascript
function addToDo() {
  var threads = GmailApp.getPriorityInboxThreads(0, 50);
  var sheet = SpreadsheetApp.getActiveSheet();
  
  for(var i = 0; i < threads.length; i++) {
    var subject = threads[i].getFirstMessageSubject().toLowerCase().trim();
    Logger.log(subject);
  }
}
```

Dropping the subject to lower case and trimming the trailing whitespace is a good way to make sure we are catching all of the emails we want. 

**Logger.log("Logging logs with Logger")**

For now, we are just logging the subjects to the console. Apps Script has it's own class for logging, `Logger`, so you will have to use `Logger.log` instead of `console.log`. If you are accustomed to using `console.log` when debugging with JavaScript, this will get confusing.

At this point, we should probably run our function to see if it's working. If you haven't already, save your code. You will be prompted to name your project. Name it whatever, it doesn't matter. Now, hit the "play" triangle next to the bug icon. You will be prompted to give permission to the script to access your sheets and your email. This is a nice feature to ensure that you never open up a sheet with malicious code that automatically runs in your browser.

Did it work? You'll know if it didn't because a big red error message will pop up at the top. Now, check the logs to see if your email subjects are listed in the log console.

> **Quick Tip**: You can access the log console with `cmd+enter` on Mac. It's probably `ctr+enter` on PC.

**Getting to the Meat**

Now we can actually *do stuff* to get this automation flow going. First, we are going to add a check to see if the subjects match "todo" or "to do".

```javascript
function addToDo() {
  var threads = GmailApp.getPriorityInboxThreads(0, 50);
  var sheet = SpreadsheetApp.getActiveSheet();
  
  for(var i = 0; i < threads.length; i++) {
    var subject = threads[i].getFirstMessageSubject().toLowerCase().trim()
    if(subject === "to do" || subject === "todo") {
      // TODO add email body to spreadsheet row
      // TODO trash email
    }
  }
}
```
Now, lets append a new spreadsheet row for the text in the email's body.

```javascript
function addToDo() {
  var threads = GmailApp.getPriorityInboxThreads(0, 50);
  var sheet = SpreadsheetApp.getActiveSheet();
  
  for(var i = 0; i < threads.length; i++) {
    var subject = threads[i].getFirstMessageSubject().toLowerCase().trim()
    if(subject === "to do" || subject === "todo") {
      var body = threads[i].getMessages()[0].getPlainBody();
      sheet.appendRow([body]);
      // TODO trash email
    }
  }
}
```
To get the body of our first message of each thread, we have to `getMessages`, which returns an array. We access the first message with bracket notation and call `getPlainBody` for the body text. There is a `getBody` method, but that returns HTML. 

If you have an auto-generated signature like me, you can split the plain body string using the first part of your signature, then return the first element of *that* array.

This is what it would look like for me:

`var body = threads[i].getMessages()[0].getPlainBody().split("-----")[0]`

It's a little messy, but it gets the job done. Yay method chaining!

The `appendRow` method takes an array as an argument. Since we are only populating the row with one thing, we just put the body in brackets, like so: 
`sheet.appendRow([body])`

Run your function again. Did our spreadsheet update? Probably not. But why? Because we need to actually send an email with the subject line to match of course! Email yourself with the subject "todo" or "to do" and put a task in the body, then run the script again.

> **Quick Tip**: As it is, the script will only update one row per email. If you want to add multiple tasks in each email, think about ways you can parse the text with commas or line breaks.

We need to trash our email after updating the spreadsheet. If we don't, we'll get duplicates every time the sheet runs.

```javascript
function addToDo() {
  var threads = GmailApp.getPriorityInboxThreads(0, 50);
  var sheet = SpreadsheetApp.getActiveSheet();
  
  for(var i = 0; i < threads.length; i++) {
    var subject = threads[i].getFirstMessageSubject().toLowerCase().trim()
    if(subject === "to do" || subject === "todo") {
      var body = threads[i].getMessages()[0].getPlainBody().split("-----")[0];
      sheet.appendRow([body]);
      threads[i].moveToTrash(); //Easy peasy
    }
  }
}
```

Run the script one last time. Your spreadsheet should have a new row *and* your Gmail inbox should be should be clear of "todo" emails. You may have to refresh the Gmail page to see the changes there.

**Setting a Calendar Event**

Alright! That first function wasn't too painful. This next one will be even easier. Let's set up a daily to-do event.

```javascript
function setDailyTodo() {
	//TODO: everything
}
```

Just like the last function, we are going to access the current spreadsheet. We are also going to access our default calendar.

```javascript
function setDailyTodo() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var calendar = CalendarApp.getDefaultCalendar();
}
```

Now, we need to get the range of cells with data in them, and join that range into a single string to use as the description of the calendar event.

```javascript
function setDailyTodo() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var calendar = CalendarApp.getDefaultCalendar();
  
  var range = sheet.getDataRange().getValues();
  var description = range.join()
}
```

>**Quick Tip**: Whenever you access a range of cells in a Google Sheet, you are getting a `Range` object. In order to see the actual values, you need to use `getValues`.

Now, all that's left to do is create the calendar event! The `createAllDayEvent` method can accept three arguments: a title (String), date (Date), and options (Object). We are going to get today's date for the date and set the description in the options object, like so: 

`calendar.createAllDayEvent("Todo", new Date(), { description: description})`

And that's it! Your final function should look like this:

```javascript
function setDailyTodo() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var calendar = CalendarApp.getDefaultCalendar();
  
  var range = sheet.getDataRange().getValues();
  var description = range.join()
 
  calendar.createAllDayEvent("Todo", new Date(), { description: description})
}
```

Run the function by selecting it in the drop down and pressing the run button. You should have a new calendar event for today!

**Putting the Scripts to Use**

It's great that we have these magical functions, but how useful are they if we need to press the "run" button every time we want them to work? Not very. Fortunately, we can set up trigger events to run these scripts on a timer. In your toolbar, navigate to `Edit > Current project's triggers`. A modal should pop up saying "No triggers set up. Click here to add one now." Click.

We are going to set up two time-driven triggers. For `addToDo`, lets set up a trigger every minute. That way, our spreadsheet will update within one minute of sending an email with a new to-do. For `setDailyTodo`, set a day timer from "Midnight to 1am" or whenever you want that calendar event to be created. Your settings should look like this:

![Trigger settings](http://bjcantlupe.com/img/apps-script-trigger-settings.png)

> **Quick Tip**: Google Apps Script has limits on the number of operations it can do each day. You can check out a list of limits [here](https://docs.google.com/macros/dashboard). If you time-driven triggers for sheets elsewhere, you may want to consider setting up triggers for every 10-15 minutes instead of one minute, or delete unnecessary scripts.

Save your changes, and that should be it! Try emailing yourself another to-do. Your sheet should be updated within a minute, and you'll have a new calendar event with all of your to-dos in the description tomorrow!

Congratulations, you are now a Google Apps Script Wizard

![wizard socks](https://media.giphy.com/media/l3V0bty5vtSAWQpJS/giphy.gif)

***

Thanks for reading! If you have questions or comments, you can find me on [Twitter](https://goo.gl/7hy3Ny), [LinkedIn](https://goo.gl/W8zNLE), or just [email me](mailto:bjcantlupe@gmail.com).
