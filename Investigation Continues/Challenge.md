# Description;

![inv cont description](https://user-images.githubusercontent.com/47820151/91049340-eddf9580-e5d1-11ea-990b-853498d00525.png)


# Writeup;

Current challenge is the continution of Investigation Challenge. So skipping some basic steps and directly moving to the main part.
(tl;dr: If you want the basic steps or Investigation challenge writeup please check out file Investigationin my repos inctfi-2020. I have made a brief description over there.)
The objectives oof this challenge as per description suggests are;
1. We are asked to find when is the last time Adam entered incorrect password.
2. We have to find when is the last time the file 1.jpg opened.
3, We have to find when is the last time when chrome is launched through task bar.

I'm writing this writeup in the order I got the flags.

## Part-3;
  Last time when chrome is launched through taskbar.
  To find that I have used userassist to check if there is any info related to taskbar/..../google chrome or similar.
  And my guess was right. I got the timestamp of it and it is as follows.
  
  ![taskbar-chrome](https://user-images.githubusercontent.com/47820151/89609520-f6dc1300-d82c-11ea-9f57-0ae75542e03a.png)

  So from the pic we see the timestamp of last opened through taskbar is **2020-07-21 17:37:18**.

## Part-2;
  Generally, 1.jpg is a file. So I thought it can be in filescan. But it's not there. So We see that it might be deleted or Inserted into some other file.
  Now I have only option left and that is to check mftparser. Then I made a text file and copying output of mftparser.
  For minimizing my burden, I have tried **strings** command with searching for 1.jpg as follows;
  
  ![Screenshot from 2020-08-06 21-17-08](https://user-images.githubusercontent.com/47820151/89609875-f09a6680-d82d-11ea-8217-cd9c1c8d6f4f.png)
  
  So we can be sure that we found 1.jpg somewhere. Mind that the output file which has contents of mftparser is 16MB.
  Make sure that you have good ram or good text opener with many features and requires less ram.
  Searching for 1.jpg, we can find it is not exactly the file. But it is stored in 1.lnk. I was sure that this is the correct time stamp because I didnot find any other 1.jpgs.
 
  ![WhatsApp Image 2020-08-02 at 05 51 48](https://user-images.githubusercontent.com/47820151/89612107-d6fc1d80-d833-11ea-8c17-32f8c1da8d04.jpeg)

  From the snap, we see that timestamp is **2020-07-21 18:38:33** 
  
## Part-1;
  This is the tough part of the chall. And I had no idea how to deal with this. So after so much googilng and learning new things I got to know that we can use event logs to get those timestamps.
  But evtlogs plugin only works for windowsXP and windows2003 profiles only :(
  Again after googling and reading various volatility plugins got to know that we have to do registry analysis.
  There are 2 ways which are extracting ntuser.dat or SAM registries and we have to open it using some registry exlorer or can be decoded from hex which is similar to decoding hex in userassist.
  I tried extracting SAM registry for knowing where exactly it is we can use **Hivelist** plugin. From the output obtained we can see both as follows;

  ![Screenshot from 2020-08-06 22-49-05](https://user-images.githubusercontent.com/47820151/89613490-37d92500-d837-11ea-9169-bafab2bfac23.png)

  Now we have to extract any of those registries with **Dumpregistry** plugin. I have extracted SAM registry.
  Then opened it with Registry explorer and inspecting on users carefully gives us the timestamp **2020-07-22 09:05:11**.
  By the time I extracted this registry the ctf has ended and it really hurts when we miss something big when we lack time of 5 minutes. :(
  
Wrapping the flags we got as mentioned gives us;

  Flag1; 22-07-2020_09:05:11
  Flag2; 21-07-2020_18:38:33
  Flag3; 21-07-2020_17:37:18
  
  Joining these 3 flags, we get **inctf{22-07-2020_09:05:11_21-07-2020_18:38:33_21-07-2020_17:37:18}** .
  
But I enjoyed the challenge and learnt many things about analyzing registries and related info. Thanks to stuxn3t. 


