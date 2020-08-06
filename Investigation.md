# Chall Description;

![description](https://user-images.githubusercontent.com/47820151/89540654-3285c700-d7b2-11ea-9ffa-7f2c6ef73c04.png)

# Writeup;
  As we were given a windows memory dump file I've started to analyze it with volatility.
  So 1st we go with finding image profile using imageinfo command in volatility.
  But this takes a little while...
   ![Screenshot from 2020-08-06 06-45-55](https://user-images.githubusercontent.com/47820151/89539506-ade67900-d7b0-11ea-968e-04625af18f5c.png)
  Now knowing profile we can proceed with finding what we need, 
  The main objectives are; 1. To find when is the last time that caluclator is opened.
                           2. To find how many times Chrome was used.
## Part-1; 
To find when an app is last opened, there are multiples way to proceed with. Those are,
1. Prefetch files;
    Windows store prefetch files and these prefetch files give details about every file that is opened and details like when its last modified, what version it is, and all details.
    All we have to do is get that prefetch file which is related to calc.exe and open it using **IEF** tool or **CROWDSTRIKE CROWDRESPONSE** tool.
    But issue with this method is you should be able to extract and open .pf files properly. Unfortunately the timestamp mentioned in this process is incorrect.
    Prefetch files can be found under the **C:\Windows\Prefetch** folder.
2. Super Prefetch files;
    Also in the Prefetch directory can be some files which are related to Superfetch. One of these files is AgAppLaunch.db which contains information about the       executed files. I found this file so I used **CrowdStrike’s CrowdResponse** tool to parse it. I could successfully parse the file and get a file for Adobe Reader execution but unfortunately, this value is also incorrect.
3. MFT Parser;
    I've used mftparser and copied the output to a text file. I've tried mactime plugin also which gave me 4 outputs i.e, 4 time stamps. But all ofthese are incorrect again.
4. User assist;
    Similar to prefetch there will be a folder called userassist which has crucial data like time stamps and run count and version etc... details. I got to know there is a plugin for getting info about userassist directory.
    Command will be;    **volatility -f windows.vmem --profile=Win7SP1x64 userassist **
    From the outputs searching for calc.exe, the result is as follows.
    ![calc exe-userassist](https://user-images.githubusercontent.com/47820151/89549192-d8d6ca00-d7bc-11ea-8611-b33386bb6e3f.png)
    We can see the timestamp of when calculator is last 
updated that is **21-07-2020_18:21:35**

## Part-2;
  I have already mentioned that we can get run count using userassist plugin. So from results,I searched for Chrome and the output is as follows;
  
  
  ![chrome-userassist](https://user-images.githubusercontent.com/47820151/89549764-982b8080-d7bd-11ea-8c1c-c5737413360d.png)
  The count is 19 as mentioned.
Wrapping the time stamp and count in flag format gives; **inctf{21-07-2020_18:21:35_19}**
While doing this challenge during ctf, I have started with part2 so I am so sure that count is 19 and was able to identify which timestamp is correct and which is not.

Thanks to stuxn3t for such intresting chall.

