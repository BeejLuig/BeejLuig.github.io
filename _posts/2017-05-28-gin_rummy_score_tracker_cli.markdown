---
layout: post
title:  Gin Rummy Score Tracker CLI
date:   2017-05-28 23:13:08 +0000
---


I can't think of a better way to spend a long holiday weekend than writing a one-page [CLI app](https://en.wikipedia.org/wiki/Command-line_interface) to keep track of how badly my grandmother is beating me at [Gin Rummy](https://en.wikipedia.org/wiki/Gin_rummy). Rummy is Grandma's card game of choice, and she takes it quite seriously. Why not give it the official feel of having an objective compu-referee? 

![referee](https://media.giphy.com/media/l0Iy9rv9r4nNyKKwU/giphy.gif)

I wrote this program in Ruby, so naturally we're going to see an OO pattern. I define four classes, `Game`, `GameController`, `Player`, and `GinRummy`. 

**`class Game`**

Instance variables: `@players`, `@round`, `@counter`

Instance methods:

- `#next_round`  
	- keeps track of the next round by updating `@counter` 
- `#show_scores`
	- displays the `@round` number and scores from each `@player`, sorted by score (high to low)
- `#won?`
	- returns a boolean indicating if the game was won (if any player score is greater than 500)
- `#winner`
	- returns the winner (whoever has the highest score at the end of the game)

You may be wondering "what if there is a tie?" Maybe your grandma ties you out of pity. Mine does not.

![Tic tac tie](https://media.giphy.com/media/d8BMUC8enESvS/giphy.gif)

**`class Player`**

Class variable: `@@all`

Instance variables: `@name`, `@score`

Methods:
	
- `.all` 
	- class method that returns all player instances

- `#add_score(score)` 
	 - adds score argument to the player's total


Easy peasy.

**`class GameController`**

This is where the game interaction resides. The only instance variable is `@game`.

There are two class methods: 

- `.new_game` 
	- Prompts for a number of players, then the names of each player. The first player named starts as dealer. Rounds are played until the game is won.
- `.play_round`
	- At the beginning of each round, the dealer is named. Once the round ends, enter the score of each player. A round summary is printed and the next round starts (or a winner is announced). 

`class GinRummy`

There is one class method, `.call`, that calls `GameController.new_game`. This class would probably be more relevant in a multi-file app. Still, we like sticking with good OO patterns, *don't we*?

At the bottom of the file is `GinRummy.call`. That way, if the `gin-rummy` file is in our root directory, we can run `ruby gin-rummy` in the command line and get this:

```bash
~$ ruby gin-rummy
--------------------------
Gin Rummy Score Calculator
--------------------------
How many players will there be?
```

Sweet!

The logic for this score tracker could really be applied to any game with a cumulative score. With some small adjustments, you could have a mini golf or bowling score keeper! Although, it may not be very practical to bring your laptop to the course/alley...If you have a better idea, let me know about it on [Twitter](https://twitter.com/BeejLuig)!

Feel free to use this little app to keep track of your own Gin Rummy games or as a template to build another score keeper CLI. Here is the [source code](https://gist.github.com/BeejLuig/8dfea53ac75c7ff4433da819672b68e2). 
