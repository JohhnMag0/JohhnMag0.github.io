---
title: "Vim or How do I exit this sh@#?"
date: 2022-10-10T21:34:00-10:00
tables: [tutorial, nonsense]
---

# Vim or How do I exit this sh@#?

1. Introduction 
2. A Brief History of VIM
3. VIMtutor
4. Useful Commands
5. Plug-In and .vimrc

# 1.Introduction

If you came here because of the title, I will just answer you in the spot :wq if you want to save before leaving, or :q! if you just want to exit without saving. If that was all that you need to know and you really didn't like how difficult it was to use vim I can't hold you in this article anymore. All I can say is good luck on your journey and try to use nano next time. Now if you come here with the idea of learning a little bit more about how this wonderful (but sometimes complex) tool works, you're welcome. So let's start.

# 2. A Brief History of VIM

VIM is actually a acronym for "Vi IMproved", and what a hell is VI? Well, VI was a text editor that was created in the UNIX era, which in turn was based on the ed which was a line editor that was the base to the creation of another fantastic tool the sed but I not going to talk about it (not now anyway). The thing is VIM comes from an ancient lineage of editors, and that is why there are a lot of things that you see in vim that don't look quite right or look way more complex than it need to be. But what I'm really going to try to show you is that although it seems very complicated to use this tool, each of the little pieces that make it up is very well planned, and you really need to consider that in the technological field, for something to continue to exist even after a long time is because it has managed to prove itself. So if you are having trouble to get on how to use VIM, don't give up you can do amazing things with it just give yourself time. Now that I've paid tribute to Bram Moolenaar and given you a life lesson (which you didn't ask for, by the way), let's see it in practice.

# 3. VIMtutor

One of the first things that you need to get used to if you want to work with software is reading the documentation. This is one of those things that you really should start doing because it is the best way to learn (if the documentation is good). In the case of the VIM we have the vimtutor(just paste it in your terminal), and just like that you have all that you need to get started on VIM. At first it will look like a lot, and quite frankly, I don't remember everything that VIM can do and very often need to go to my notations to remember some of those things. It's very important that you read that documentation and try some of the commands that appear, you will not know everything but with time you will see what the things are that you will be using very often. Another great source to learn something are books, and for me one of the greatest books if you want to learn about VIM or Neovim is Practical VIM by Drew Neil. It's a great source of material and will give you a good grasp on VIM.

# 4. Modes

If you followed what I said in the previous step, much of what I am going to say here you must have already seen. But revisiting never hurts. So before we get started, I need you to understand the modes that you are going to encounter, because each command has to be entered in a certain mode to work.

- Normal: When you open the file, this is the first mode you see.You can look at this mode as the movement mode, is in here that you are going to move lines, delete, copy, paste, and so on. It's also from this mode that you can go to any other mode. You can access this mode by pressing .
- INSERT; this is the mode that you are going to use to insert text. You can also do movement on this mode but in the NORMAL is way faster. You can go to this mode by pressing 'i' when in normal mode.
- VISUAL; in this mode you can select a lot of lines all at once, similar at what you do with the mouse. Here,  you can use the same movements as in the NORMAL mode, but this mode really shines in copying, pasting, and deleting lines. In normal mode, press "v" to enter "visual block," which is a mode where you can select by column, which is very useful if used in conjunction with "visual line," because it allows you to edit several lines at the same time. Another mode worth mentioning is "visual line," which, as the name implies, allows you to select several lines at the same time.
- COMMAND: In this mode, which is accessed by pressing the ':' key in Normal mode, you can enter commands such as the substitution commands I'll discuss later or the help command, for example. I'm going to leave here a image that summarizes some of the commands that you have in the keyboard. You are probably screaming and asking, "WHY THERE IS SO MANY MODES I JUST WANT TO WRITE", I know that at first it looks like a airplane cabin, and that's why I like to start with some history, because you need to understand that this was created before the mouse era, so the big thing about VIM is that you can do pretty much everything just using the keyboard. Again, have a little patience and you will get there.

# 5. Useful Commands

So now that you are starting to get use to some terminologies and commands, we are gonna do a step forward so that I can show you some commands that I think are very useful. So let's get started:

- 'f<letter>'; with this command you can jump whole words to go to a certain pattern. You can go forward or backwards using ';' or ','.
- 'x'; is the same as delete.
- '0'; is the same as home.
- '$'; is the same as end.
- '*'; selects a whole word in the file. You can search for this word using the 'n' for forward words or the 'N' for backwards words.
- ':<number>'; with this command you can go to any line that you want on the file.
- ':<number>t<number>'; with this command you copy the content of the first line number to the other line number.
- ':<numer>m<number>'; this is the same as the other command but instead of copying you are moving.
- '<CTRL-o>'; this is actually a very cool one that I don't see many people talking about, if you press this in insert mode it enters the normal mode executes an action an returns to insert.
- '<CTRL-o>'; this is actually a very cool one that I don't see many people talking about, if you press this in insert mode it enters the normal mode executes an action an returns to insert.
- '<CTRL-n>' and '<CTRL-p>'; this two commands when entered in insert mode can permits you to autocomplete a word.
- '<CTRL-x>' plus '<CTRL-l>'; this two commands need to be used one after another to work, but it basically autocompletes a entire line.
- 'k' and 'j' and 'h' and 'l'; this in NORMAL or VISUAL mode are respective the arrows up(k), down(j), left(h) and right(l).
- 'w'; goes to the start of the next word.
- 'e'; goes to the end of the next word.
- 'b'; start of the last word.
- 'y'; copies. 'yw' copies the entire word. 
- 'p' and 'P'; The two are used to paste but the 'p' paste after the line and 'P' paste before the line.
- 'd'; deletes. It's important to know that in VIM when you delete something is the same thing as cutting so if you have copy some word and deleting after that the word that is save in your buffer is the one that was deleted. 'dw' deletes a entire word.
- 'c'; deletes and enters in the insertion mode(very good to save time). One good use of this command is with the 'w', 'cw' let you delete a word an insert at the same time.
- <CTRL-v> plus <SHIFT-i>; when in block visual select all the lines that you want to edit then press <SHIFT-i> and do the edition that you want, then press <ESC> and all the lines that you had selected are gonna be edited.
- '/' and '?'; this command permits you to search for a word in the entire file. 'n' search for the word forwards and 'N' search for the word backwards. '/' starts the search from the top and '?' starts the search from the bottom.
- '//' and '??' ; executes the last search.
- '='; this is a magical command with this you can indent all your code(just beautiful).
- '.'; executes the last command.
- '%s/undone/done/g'i; this is a important command to have in mind. Because with this command you can substitute every occurence of a word that you can imagine. So let's describe what is doing '%' means all lines, you could substitute this for only the lines that you want it to execute, 's' means substitute, '/undone' is the word that you are searching, '/done' is the word that you are gonna to use to substitute the '/undone' and last the '/g' means to execute it globally otherwise would execute just once per line.
  There is another commands and things that you could learn abou vim, such as registers and macros that can really improve your life. But I not gonna talk about this stuff in this article, if you get interested in this type of thing you could go to the book that I recommended earlier it goes a lot about this kind of thing.

# 6. Plug-In and .vimrc

If you already feeling overwhelmead by all this don't worry there is always more. One of the greatests things about vim is that it's very flexible and leat's you use the tool in the way that you think suits you the best.

There's two ways of editing your VIM you can modify it's usability by adding extensions that will give to you a whole another view on what you can do, for that you can use the plug-ins, I have to admit that I'm not a very fan of the use of plug-ins myself but if you need something more robust you should definitely take a look. And when we talk about plug-ins we need to talk about neovim, it's basically what vim was to vi, a way to improve the software, and althought it has kind of a war between the users of both, I have to say that if you need something more like an IDE you should definetely take a look on Neovim, and the best thing about it is that you can have both in your machine so definetely is worth while.

And other thing that you could do to make you life easier while using vim is to have a good .vimrc, that is your configuration file and is with this file that you can install your plug-ins, change keybinds and things like that. Knowing how to configure your .vimrc is as important as to learn any keybind, because is by this configuration file that you can make your vim feels like everything that you want so keep that in mind.

One of the things about dot files and plug-ins is that they are a complete personal thing to have and I really can't tell you what you should or not use. So instead of showing you my dot file and the plug-ins that I'm currently using, I will give you some links to places that are good to start with:

[GitHub - junegunn/vim-plug: Minimalist Vim Plugin Manager](https://github.com/junegunn/vim-plug) This one is my personnal favorite in plug-in management, if you are gonna use the neovim you're probably using other extension but this one works quite well for me.

[GitHub - NvChad/NvChad: An attempt to make neovim cli functional like an IDE while being very beautiful, blazing fast startuptime ~ 14ms to 67ms](https://github.com/NvChad/NvChad) This is more a a place to start, here you are gonna find a neovim distribution that come with a lot of things pre-configured, you don't need to use this but it's nice to have a base to start with.

[Vim: help.txt](https://vimhelp.org/?) again the vim documentation is one of the best places to get anything that you need to know. And this is the case for .vimrc .

You could also get a better info about the options in vimrc by the command ':help vimrc'.

## END

So we get to the end of this article. I hope that you could get a good push in the direction of how to use VIM, and from where you should start. And as always remember :wq(stay safe and goodbye).
