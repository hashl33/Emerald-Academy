Chapter 2 Day 3 Quest

1. In a script, initialize an array (that has length == 3) of your favourite people, represented as Strings, and log it.

![1Ch2D3](https://user-images.githubusercontent.com/100004665/156101640-f5e7a2b5-4110-4e41-ae6c-44a1fa31da40.PNG)

2. In a script, initialize a dictionary that maps the Strings Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a UInt64 that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!
![2Ch2D3](https://user-images.githubusercontent.com/100004665/156101645-28cd9dfb-aa15-4f0b-ac66-448a00ee83de.png)

3. Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).
![3Ch2D3](https://user-images.githubusercontent.com/100004665/156101653-2786bb35-dd21-49e3-b1aa-8d416d4e6665.PNG)

  Here I unwrapped the string "Twitter" from socialMediaRankings with the '!' un-wrap operator. Dictionaries, by default, return the item you requested or nil, the unwrap operator   forces a return of the type you requested.
 
 4. Using this picture below, explain...

    What the error message means:
      The error message means we are getting an optional (denoted by the '?')
    Why we're getting this error
      We get this error because we have not unwrapped the dictionary call
    How to fix it
      We fix this by adding the un-wrap operator ('!') at the end of line 3
<img width="1361" alt="wrongcode" src="https://user-images.githubusercontent.com/100004665/156102019-2b365724-07ca-469e-95aa-365d94037368.png">
