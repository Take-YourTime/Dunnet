# Part 1. Introduction

There is some similarity between the files and directories of UNIX and the
items and rooms in a text adventure game. In a text adventure you can enter
rooms wherein you may find objects; in UNIX you can go into a directory and
find files there. In a text game you can examine or pick up the objects in
the room; in UNIX you can display or move files.

So, to create an interesting programming assignment, we will recreate the
behavior of a text adventure game called **Dunnet**.

It is weird, but Dunnet comes as part of the UNIX's emacs editor, which is the
editor I recommend you to use for programming in the UNIX environment. (I know,
yes: it is very weird for a text editor to come with games, but emacs, in fact,
does.) You don't actually need to play the original game in order for you to
do the assignment. But if you think it would help your understanding, then
you can play it by typing "emacs -batch -l dunnet" on the command line.

The reason you don't need to actually play the game is that I will now show you
a demonstration of how the game runs (which is to say: your implementation must
match these behaviors exactly.)

Note:

```text
*  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  
* This file is only demonstrating the desired behavior.                    
* A second posting tomorrow will give details on the implementation rules. 
* The reason to give this part to you is because there is a lot to read.
* (Actually, just playing the game -- up to the point of logging into the computer -- is a good way to get a sense of these rules.)  
*  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  *  
```

# Part 2. A Walkthrough of the Game (Up to Entering the Computer Room)

So we want to see how the game runs. Let's see the start of the game output:

```text
% emacs -batch -l dunnet
Dead end
You are at a dead end of a dirt(泥土) road.
The road goes to the east.
In the distance you can see that it will eventually fork off. The trees here are very tall royal palms(棕櫚樹), and they are spaced equidistant from each other.
There is a shovel(剷子) here.
>
```

From the above, we see that we are at the end of a road. There is also a shovel
here. We have also been told that the road goes east. This means that **we can
follow the road, by typing "e"**. Let's try that:

```text
>e
E/W Dirt road
You are on the continuation of a dirt road. 
There are more trees on both sides of you. 
The road continues to the east and west.
There is a large boulder(巨石) here.
>
```

From the above, we see that we can keep going east, or we can go back west.
Let's see what happens if we try going in a wrong direction. Then, let's go
back west:

```text
>n
You can't go that way.
>w
Dead end
There is a shovel here.
>l
Dead end
You are at a dead end of a dirt road. 
The road goes to the east.
In the distance you can see that it will eventually fork off. The trees here are very tall royal palms, and they are spaced equidistant from each other.
There is a shovel here.
>
```

Looking at the above text, we find that the description of the place is much
shorter if you have already visited it. **But the above also shows that we can
see the full text by typing "l"** (which is short for "look").

Now let's see how we can interact with the objects in this location. There is
a message telling us about a shovel. **Let's examine**`(檢查)` **it, by typing "x"**:

```text
>x shovel
It is a normal shovel with a price tag attached that says $19.99.
>
```

But this is not the only object we can look at. The description said there were
palm trees. Let's see all the different ways that we can examine them:

```text
>x trees
They are palm trees with a bountiful supply of coconuts(椰子) in them.
>x tree
They are palm trees with a bountiful supply of coconuts in them.
>x palms
I don't know what that is.
>x palm
They are palm trees with a bountiful supply of coconuts in them.
>x coconuts
I see nothing special about that.
>x coconut
I see nothing special about that.
```

It seems to me to be a bug in the game for it to recognize palm but not palms,
while recognizing both tree and trees. **Well, maybe it is a bug -- but that
doesn't matter. We will implement the same behavior as the game**.

The next topic to discuss is our inventory`(物品清单)` of carried items. **We can look at it (using "i") and we can pick things up and put them in our inventory (using "get")**:

```text
>i
You currently have:
A lamp(燈)
>get coconut
You cannot take that.
>dig
You have nothing with which to dig.
>get shovel
Taken.
>dig
Digging here reveals nothing.
>get shovel
I do not see that here.
>get hovel
I don't know what that is.
>i
You currently have:
A lamp
A shovel
>
```

From the above, we see that we start the game carrying a lamp. We also see that some items can be picked up (like the shovel), but other items cannot (like the coconut). And we see that you need the shovel in order to dig.

Now, since we picked up the shovel, it should no longer be on the ground.
Let's see:

```text
>l
Dead end
You are at a dead end of a dirt road.  The road goes to the east.
In the distance you can see that it will eventually fork off.  The
trees here are very tall royal palms, and they are spaced equidistant
from each other.
>
```

Correct. The shovel is no longer there.

Let's now look at that place to the east again:

```text
>e
E/W Dirt road
There is a large boulder here.
>x boulder
It is just a boulder.  It cannot be moved.
>get boulder
You cannot take that.
>l
E/W Dirt road
You are on the continuation of a dirt road.  There are more trees on both sides of you.  The road continues to the east and west.
There is a large boulder here.
>x trees
They are palm trees with a bountiful supply of coconuts in them.
>
```

We see that this place has the same trees as the place we came from. It doesn't have a shovel, but does have a boulder.  
**This means that this directory will hold files with the names "trees", "tree", "palm" (but not "palms"), "coconut", "coconuts", and "boulder".**

Let's move on:

```text
>e
Fork
You are at a fork of two passages, one to the northeast, and one to the southeast.  The ground here seems very soft. You can also go back west.
>ne
NE/SW road
You are on a northeast/southwest road.
>ne
Building front
You are at the end of the road.  There is a building in front of you to the northeast, and the road leads back to the southwest.
>
```

The path has led to a building. Let's go in:

```text
>ne
You don't have a key that can open this door.
>
```

Oh. We don't have the key. Where could it be?  
Well, there was that southeast branch.  
Let's try that path:

```text
>sw
NE/SW road
>sw
Fork
>se
SE/NW road
You are on a southeast/northwest road.
There is some food here.
>x food
It looks like some kind of meat.  Smells pretty bad.
>get food
Taken.
>
```

We picked up some food. But not a key. The road does continue on however:

```text
>se
Bear hangout(熊的出沒地)
You are standing at the end of a road.  A passage(小路) leads back to the northwest.
There is a ferocious bear here!
>x bear
It looks like a grizzly to me.
>
```

How are we going to get past the bear? Well we have a piece of meat...
But before we feed the bear, let's look for the key:

```text
>x key
I don't see that here.
>drop food
Done.
The bear takes the food and runs away with it. He left something behind.
>l
Bear hangout
You are standing at the end of a road.  A passage leads back to the northwest.
There is a shiny brass(黃銅) key here.
>
```

From the above, we see that the key only appeared when the bear left.  
Let's
look around:

```text
>x bear
I don't see that here.
>x key
I see nothing special about that.
>get key
Taken.
>
```

From the above, we see that the bear really has left. We also now have the key.
We can now go and open the door.

```text
>nw
SE/NW road
>nw
Fork 
>ne
NE/SW road
>ne
Building front
>l
Building front
You are at the end of the road.  There is a building in front of you to the northeast, and the road leads back to the southwest.
>
```

We now have the key to get through this door. But let's first prove that
the key must be in the inventory to work:

```text
>sw
NE/SW road
>ne
Building front
>drop key
Done.
>ne
You don't have a key that can open this door.
>i
You currently have:
A lamp
A shovel
>
```

OK, so let's use the key to get into the house:

```text
>sw
NE/SW road
>ne
Building front
There is a shiny brass key here.
>get key
Taken.
>i
You currently have:
A lamp
A shovel
A brass key
>ne
Old Building hallway
You are in the hallway of an old building.  There are rooms to the east
and west, and doors leading out to the north and south.
>
```

We were able to get through the door because we had the key in our inventory.
We now are in the house, with doors on all sides. We also again notice that
rooms that are revisited display only a single line of its room description.

So now we are in a hallway with doors. Let's see what is behind those doors:

```text
>l
Old Building hallway
You are in the hallway of an old building.  There are rooms to the east and west, and doors leading out to the north and south.
>n
You don't have a key that can open this door.
>e
Mailroom
You are in a mailroom.  There are many bins where the mail is usually kept.
The exit is to the west.
>w
Old Building hallway
>w
Computer room
You are in a computer room.  It seems like most of the equipment has been removed.  There is a VAX 11/780 in front of you, however, with
one of the cabinets wide open.
A sign on the front of the machine says: "This VAX is named ‘pokey’.  To type on the console, use the ‘type’ command."
The exit is to the east.
The panel lights are steady and motionless.
>type
You type on the keyboard, but your characters do not even echo.
>e
Old Building hallway
>type
There is nothing here on which you could type.
>e
Mailroom
>w
Old Building hallway
>w
Computer room
The panel lights are steady and motionless.
>
```

So now we know.
The east door goes to a mailroom.
The west door goes to a computer room.
The north door is locked (and the player will not get through it until much later in the game, so we don't need to worry about what's behind this door.)

As for the computer room, we see that a message about lights displays every time you enter the room.
Now, that was also the behavior of objects like the boulder and the key (but not of objects like the coconuts.) In the case of the lights, however, the lights are not an object, as we'll see when we look around the room:

```text
>x computer
I see nothing special about that.
>x vax
I see nothing special about that.
>x cabinet
I see nothing special about that.
>x lights
I don't know what that is.
>x panel
I don't know what that is.
>x steady
I don't know what that is.
>
```

So, yes, the panel lights are not an object, but the computer is. In fact, there are three names for that object: computer, vax, and cabinet. (Vax is the brand name of a very old computer, and cabinet refers to computer's frame, which is so large as to be called a cabinet, because the computer is so old.)

One thing we notice is that the panel lights(面板燈) are motionless. So the computer is not working. In fact, figuring out how to fix this computer is one of the puzzles in the game, which you will be implementing.

# Part 3. A Walkthrough of the Game's Puzzles

To list those puzzles all out, here are the puzzles you will need to implement:

1. Q: There is a door that is locked, but where is the key?

```text
A: Give the food to the bear (which we already did in the above walkthrough).
```

1. Q: How do we fix the computer with steady lights?

```text
A: It is missing its CPU card, which has been buried somewhere.
```

1. Q: Once you fix the computer, what is the login and password?

```text
A: The answer will be found in the mailroom.
```

1. Q: What is the password to a second computer, gamma?

```text
A: Once you log into pokey, you'll use the computer to find the password.
```

1. Q:  Where is the bracelet(手鐲)?

```text
A: The bracelet is in a hidden area. You must find it.
```

OK, then. So let's get back to the walkthrough of the game and try to solve these puzzles. We've already solved the puzzle to get the key and get in the house. Let's now solve the puzzle to fix the computer:

```text
>e
Old Building hallway
>s
Building front
>sw
NE/SW road
>sw
Fork
>l
Fork
You are at a fork of two passages, one to the northeast, and one to the southeast.  The ground here seems very soft. You can also go back west.
>x ground
I don't know what that is.
>dig
I think you found something.
>
```

The message about the ground being soft was a hint to dig. We were only allowed to dig because we had picked up the shovel. Once we dug, it said that we found something. But we'll have to "look" to see what thing we have found:

```text
>l
Fork
You are at a fork of two passages, one to the northeast, and one to the southeast.  The ground here seems very soft. You can also go back west.
There is a CPU card here.
>

```

It says that we found a CPU card. Let's look at it. But before we do that, we must note that this object has three valid names:

```text
>x card
The CPU board has a VAX chip on it.  It seems to have 2 Megabytes of RAM onboard.
>x cpu
The CPU board has a VAX chip on it.  It seems to have 2 Megabytes of RAM onboard.
>x board
The CPU board has a VAX chip on it.  It seems to have 2 Megabytes of RAM onboard.
>
```

OK. So now we have found the card. Let's use it to fix the computer:

```text
>l
Fork
You are at a fork of two passages, one to the northeast, and one to the southeast.  The ground here seems very soft. You can also go back west.
There is a CPU card here.
>get card
Taken.
>ne
NE/SW road
>ne
Building front
>ne
Old Building hallway
>w
Computer room
The panel lights are steady and motionless.
>put card in vax
As you put the CPU board in the computer, it immediately springs to life.
The lights start flashing, and the fans seem to startup.
>
```

Now we have fixed the computer. And the light message has changed. See:

```text
>e
Old Building hallway
>w
Computer room
The panel lights are flashing in a seemingly organized pattern.
>
```

OK. How to use the fixed computer? We just have to follow the instructions:

```text
>l
Computer room
You are in a computer room.  It seems like most of the equipment has been removed.  There is a VAX 11/780 in front of you, however, with one of the cabinets wide open.  A sign on the front of the machine says: This VAX is named ‘pokey’.  To type on the console, use the ‘type’ command.  The exit is to the east.
The panel lights are flashing in a seemingly organized pattern.
>type
```

```text
UNIX System V, Release 2.2 (pokey)
```

```text
login:
```

OK, so we have a working computer. But we need to solve the password puzzle.
First just type anything for the login and password. I'll just type "quit":

```text
login: quit
password: quit
login incorrect
```

```text
UNIX System V, Release 2.2 (pokey)
```

```text
login: quit
password: quit
login incorrect
```

```text
UNIX System V, Release 2.2 (pokey)
```

```text
login: quit
password: quit
login incorrect
>
```

Error three times!  
So now we are kicked off of the computer and can continue to explore, in order
to solve the password puzzle. Let's do that:

```text
>e
Old Building hallway
>e
Mailroom
>l
Mailroom
You are in a mailroom.  There are many bins where the mail is usually kept.  The exit is to the west.
>

```

Let's look at those bins:

```text
>x bins
All of the bins are empty.  Looking closely you can see that there are names written at the bottom of each bin, but most of them are faded away so that you cannot read them.  You can only make out three names:
                   Jeffrey Collier
                   Robert Toukmond
                   Thomas Stock
```

```text
>get bin
You cannot take that.
>
```

This is a mailroom that has employee names on bins. One of the names is Robert Toukmond. The computer you fixed was apparently his computer. His login name can be guessed -- it is "toukmond". But what about his password?  
Well, fortunately for us, Mr. Toukmond was very bad at choosing a password. A little guesswork determines that his password was "robert".

```text
>w
Old Building hallway
>w
Computer room
The panel lights are flashing in a seemingly organized pattern.
>type
```

```text
UNIX System V, Release 2.2 (pokey)
```

```text
login: toukmond
password: robert
```

```text
Welcome to Unix
```

```text
Please clean up your directories.  The file system is getting full.
Our tcp/ip link to gamma is a little flaky, but seems to work.
The current version of ftp can only send files from your home directory, and deletes them after they are sent!  Be careful.
```

```text
Note: Restricted bourne shell in use.
```

```text
$
```

OK. So we solved that puzzle of logging in. To leave, we type exit:

```text
exit
```

```text
You step back from the console.
```

```text
>e
Old Building hallway
>w
Computer room
The panel lights are flashing in a seemingly organized pattern.
>type
$ exit
```

```text
You step back from the console.
```

```text
>type
$
```

We see that we do not need to login again, we just say "type" again, and
we are still logged in.

Well, now that we are in, let's have a look around the computer:

```text
$ ls
total 467
drwxr-xr-x  3 toukmond restricted      512 Jan 1 1970 .
drwxr-xr-x  3 root     staff          2048 Jan 1 1970 ..
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ls
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ftp
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 echo
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 exit
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 cd
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 pwd
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 rlogin
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ssh
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 uncompress
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 cat
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 paper.o.Z
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 lamp.o
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 shovel.o
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 key.o
```

Here, we notice that each of the ".o" files corresponds to an object you are carrying. (It is a pun, because .o files are called objects.)

In the above output it says we are carrying a lamp, shovel, and key.
Let's see if we can understand what we are seeing:

```text
exit
```

```text
You step back from the console.
```

```text
>i
You currently have:
A lamp
A shovel
A brass key
>drop shovel
Done.
>e
Old Building hallway
>w
Computer room
The panel lights are flashing in a seemingly organized pattern.
There is a shovel here.
>type
$ ls
total 467
drwxr-xr-x  3 toukmond restricted      512 Jan 1 1970 .
drwxr-xr-x  3 root     staff          2048 Jan 1 1970 ..
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ls
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ftp
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 echo
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 exit
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 cd
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 pwd
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 rlogin
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ssh
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 uncompress
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 cat
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 paper.o.Z
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 lamp.o
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 key.o
$

```

From the above, we see that, as they suspected, the shovel.o file is gone from your home directory. So that is weird. This computer seems to show
what is in your inventory.

But what is this paper.o.Z in your home directory? This is the next puzzle.

// TOREAD

The next puzzle is to read the piece of paper. We first note that "paper.o.Z" sounds like a compressed file, based on the ".Z".

How to uncompress it?

```text
$ gunzip paper.o.Z
gunzip: not found.
$ unzip paper.o.Z
unzip: not found.
$
```

Well those didn't work. Let's try to look at it directly:

```text
$ cat paper.o.Z
File not found.
$ cat lamp.o
Ascii files only.
$
```

I think this is actually a bug in the Dunnet game. If lamp.o is a found
file, then why not paper.o.Z? If your game says "Ascii files only", rather
than "File not found", then that is OK. But how to uncompress it? Looking
more carefully at the output from ls, we see that there is no file named
gunzip, but there is a file named uncompress. Let's try that:

```text
$ uncompress paper.o.Z
$ ls
total 467
drwxr-xr-x  3 toukmond restricted      512 Jan 1 1970 .
drwxr-xr-x  3 root     staff          2048 Jan 1 1970 ..
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ls
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ftp
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 echo
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 exit
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 cd
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 pwd
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 rlogin
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 ssh
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 uncompress
-rwxr-xr-x  1 toukmond restricted    10423 Jan 1 1970 cat
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 lamp.o
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 key.o
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 paper.o
$ cat paper.o
Ascii files only.
$ exit
```

```text
You step back from the console.
```

```text
>i
You currently have:
A lamp
A brass key
A slip of paper
>x paper
The paper says: Don't forget to type ‘help’ for help.  Also, remember
this word: ‘worms’
>
```

In the above, we find that the paper object contains a password ("worms"),
which is needed later in the game (but we won't implement that part).

The final puzzle is to find the bracelet by looking around the file system.
The player types "cd" and "ls" and sees:

```text
>type
$ pwd
/usr/toukmond/
$ cd ..
$ ls
total 4
drwxr-xr-x  3 root     staff           512 Jan 1 1970 .
drwxr-xr-x  3 root     staff          2048 Jan 1 1970 ..
drwxr-xr-x  3 toukmond restricted      512 Jan 1 1970 toukmond
```

So the player continues looking

```text
$ cd ..
$ ls
total 4
drwxr-xr-x  3 root     staff           512 Jan 1 1970 .
drwxr-xr-x  3 root     staff          2048 Jan 1 1970 ..
drwxr-xr-x  3 root     staff          2048 Jan 1 1970 usr
drwxr-xr-x  3 root     staff          2048 Jan 1 1970 rooms
```

So, what are these rooms? The player types:

```text
$ cd rooms
$ ls
total 16
drwxr-xr-x  3 root     staff      512 Jan 1 1970 .
drwxr-xr-x  3 root     staff     2048 Jan 1 1970 ..
drwxr-xr-x  3 root     staff      512 Jan 1 1970 computer-room
drwxr-xr-x  3 root     staff      512 Jan 1 1970 mailroom
drwxr-xr-x  3 root     staff      512 Jan 1 1970 old-building-hallway
drwxr-xr-x  3 root     staff      512 Jan 1 1970 building-front
drwxr-xr-x  3 root     staff      512 Jan 1 1970 ne-sw-road
drwxr-xr-x  3 root     staff      512 Jan 1 1970 bear-hangout
drwxr-xr-x  3 root     staff      512 Jan 1 1970 se-nw-road
drwxr-xr-x  3 root     staff      512 Jan 1 1970 fork
drwxr-xr-x  3 root     staff      512 Jan 1 1970 e-w-dirt-road
drwxr-xr-x  3 root     staff      512 Jan 1 1970 dead-end
drwxr-xr-x  3 root     staff      512 Jan 1 1970 hidden-area
```

Could this be a list of the places in the game? The player will recognize
these directory names as being very descriptive of the places that had to
be visited in the game to get to this point. Perhaps the player tests the
theory, by looking at the room where they are right now (since they are
typing on the computer, that would be the computer-room.)

```text
$ cd computer-room
$ ls
total 4
drwxr-xr-x  3 root     staff     512 Jan 1 1970 .
drwxr-xr-x  3 root     staff    2048 Jan 1 1970 ..
-rwxr-xr-x  3 root     staff    2048 Jan 1 1970 description
-rwxr-xr-x  3 root     staff       0 Jan 1 1970 shovel.o
$
```

The player notices the shovel.o file and recalls how they had dropped the
shovel in this room earlier in the walkthrough. The player also notices
that objects like the computer do not show up. (The rule is: only objects
that you can pick up will show up.)

The player also notices in this directory a description file, and wants to
know about it:

```text
$ cat description
You are in a computer room.  It seems like most of the equipment has
been removed.  There is a VAX 11/780 in front of you, however, with
one of the cabinets wide open.  A sign on the front of the machine
says: This VAX is named 'pokey'.  To type on the console, use the
'type' command.  The exit is to the east.
$

```

The player recognizes that this is the very same text that they had seen
when they first had entered the computer room. Now that they understand how
these directories work, they are ready to solve the puzzle. They look around
all of the room directories, and they notice one with an odd name. They
don't remember ever visiting a room that could be described as a "hidden
area." So they want to find out more:

```text
$ cd ..
$ cd /rooms/hidden-area
$ ls
total 4
drwxr-xr-x  3 root     staff           512 Jan 1 1970 .
drwxr-xr-x  3 root     staff          2048 Jan 1 1970 ..
-rwxr-xr-x  3 root     staff          2048 Jan 1 1970 description
-rwxr-xr-x  1 toukmond restricted        0 Jan 1 1970 bracelet.o
```

There is a bracelet! How to get it? You try:

```text
   $ cp bracelet.o ~
   cp: command not found.
```

What kind of UNIX is this? It doesn't have cp? Well, this is a VAX computer
from the early days of programming. But, to be fair, what this version of
UNIX lacks in terms of basic functionality, it makes up for in its ability
to warp reality.

Actually, the reason that commands like cp and mv are not supported is so
that you have to solve the puzzle of "where is the bracelet." (And another
reason these commands are not supported is that it is just a game and the
programmers didn't actually implement UNIX. They pretended to have UNIX using
their game prompt, just as we will pretend to have the game using our UNIX
prompt.)

So, where is the bracelet? The player types:

```text
$ cat description
You are in a well-hidden area off to the side of a road.  Back to the
northeast through the brush you can see the bear hangout.
$

```

Oh, so it is southwest of the "bear hangout" -- and you know where that is,
because you had to go there to get the key to solve puzzle #1. So you go
back there:

```text
exit
```

```text
You step back from the console.
```

```text
>e
Old Building hallway
>s
Building front
>sw
NE/SW road
>sw
Fork
>se
SE/NW road
>se
Bear hangout
>look
Bear hangout
You are standing at the end of a road.  A passage leads back to the
northwest.
```

See? There was no indication of a hidden area. But now that we know that it
is there, we can:

```text
>sw
Hidden area
There is an emerald bracelet here.
>l
Hidden area
You are in a well-hidden area off to the side of a road.  Back to the
northeast through the brush you can see the bear hangout.
There is an emerald bracelet here.
>
```

I had to type "l" to see the description. But this is a bug of Dunnet, in my
view. So it is OK if your implementation displays the full description the
first time you enter the hidden area.

Well anyway, let's get the bracelet.

```text
>get bracelet
Taken.
>i
You currently have:
A lamp
A brass key
A slip of paper
A bracelet
>
```

And, it should be noted, logging back onto the computer will show this change.
The bracelet.o file will be in your home directory, not in the hidden-area.

Let's now consider the "get all" syntax:

```text
>drop bracelet
Done.
>get all
A bracelet: Taken.
>drop bracelet
Done.
>drop lamp
Done.
>get all
A bracelet: Taken.
A lamp: Taken.
>get all
Nothing to take.
>
```

Let's see some error messages:

```text
>drop all
I don't know what that is.
>drop fbgfbmokgf
I don't know what that is.
>drop boulder
You don't have that.
>drop
You must supply an object.
>get boulder
I do not see that here.
>get
You must supply an object.
>get al
I don't know what that is.
>get bracelet
I do not see that here.
>drop bracelet
Done.
>get bracelet
Taken.
>
```

Here are a few more errors:

```text
>nort
I don't understand that.
>north
You can't go that way.
>
```

You do not need to implement "look", because you just use "l", and only for
looking at rooms (ie, not for looking at objects).
You don't need to implement "examine", because you use "x", and not for rooms.
You don't need to implement any commands that have not been demoed above.
You do need to get the same exact output behavior as discussed above.

# Part 4. A Map

```text
                                     None
                                       A
                                       |n         
                             w         |            e
              computer-room <- old-building-hallway -> mailroom
                                      /
                                     /ne(s in reverse)
                                    /
                             building-front
                                  /
                                 /ne
                                /
                           ne-sw-road
                              /
                             /ne
         e                e /
dead-end -> e-w-dirt-road -> fork
                            \
                             \se
                              \
                           se-nw-road
                               \
                                \se
                                 \
                             bear-hangout
                                 /
                                /sw
                               /
                          hidden-area
```

Above are the connections. Note, the reverse links also exist. For example, if
A has a sw connection to B, then B has a ne connection to A. The exception to
this is the "hallway": it goes south to the building-front, but you had to go
northeast from the building-front to get to the hallway.
