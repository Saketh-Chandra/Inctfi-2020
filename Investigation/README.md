# Chall Description :
![descriptionImg](/Investigation/Investigation1.png)

# Writeup :
  As we were given a windows memory dump file I've started to analyze it with volatility.
  So 1st we go with finding image profile using imageinfo command in volatility.
  But this takes a little while
   ![AnsImg](/Investigation/Investigation2.png)
  Now knowing profile we can proceed with finding what we need, 
  The main objectives are; 1. To find when is the last time that caluclator is opened.
                           2. To find how many times Chrome was used.
## Part-1 : 
To find when an app is last opened, there are multiples way to proceed with. Those are,
1. Prefetch files;
    Windows store prefetch files and these prefetch files give details about every file that is opened and details like when its last modified, what version it is, and all details.
    All we have to do is get that prefetch file which is related to calc.exe and open it using **IEF** tool or **CROWDSTRIKE CROWDRESPONSE** tool.
    But issue with this method is you should be able to extract and open .pf files properly. Unfortunately the timestamp mentioned in this process is incorrect.
2. Super Prefetch files;
    It is same as prefetch but this time we have to open 
  
