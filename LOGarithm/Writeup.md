# Description;

![description](https://user-images.githubusercontent.com/47820151/89988683-bddcdd80-dc34-11ea-9440-cc5622b38b8a.png)

# Writeup;

We're given a .vmem and a pcap file.
Let's start with .vmem file using volatility and we can find profile using **imageinfo** plugin in volatility.


![imageinfo](https://user-images.githubusercontent.com/47820151/89989706-2bd5d480-dc36-11ea-9293-3aa577799a82.png)

We can use any of the profiles listed above in the pic.
Now let us observe what processes are running to know what is suspicious in memory dump.
We can do this using **pstree** plugin and I'm focusing on main observation we are supposed to observe. 
Well we can see there is cmd.exe in explorer.exe as there in pic below.

![pstree](https://user-images.githubusercontent.com/47820151/89989818-532ca180-dc36-11ea-8e1d-4b086cd49110.png)

Along with that there is some pythonw.exe. Lets start analysing cmd.exe
By using **cmdscan** we can know what command is executed last in terminal.

![cmddownload](https://user-images.githubusercontent.com/47820151/89989991-90912f00-dc36-11ea-9f71-8145f6f9bdff.png)

As we see we couldn't find any commands we can see "Downloads" and "Desktop" maybe a clue to proceed.
So now lets check if there are any suspicious files in Desktop or Downloads. 
We can find it using Filescan as shown above in the pic.
Well we see a .py in Downloads. Lets go see what it does.

![pycode](https://user-images.githubusercontent.com/47820151/89990549-67bd6980-dc37-11ea-8b25-bd8c3dd9debd.png)

So after understanding the code, we can understand its prime motive is to xor a message with variable **t3mp**.
And it is recieving message from specific ip address and port number.
So we have a pcap let's try to get the message from it using wireshark.

![wireshark](https://user-images.githubusercontent.com/47820151/89991483-c0413680-dc38-11ea-928b-2c84d370c41f.png)

We see there is some encoded data lets get that using exfiltration in wireshark or we can use scapy module in python. So from scapy, we have to code and extract.
Now I thought it would be xor and xoring again with "t3mp" could give flag But its not. It's base64 encoded though.
Now as "t3mp" isnt working and base64 decoding is not giving anything useful we have to search for more details.
After using **cmdline, consoles, envars...**. I encountered some data in envars which is as follows;

![envar](https://user-images.githubusercontent.com/47820151/89992739-8ffa9780-dc3a-11ea-8827-8fdaa7611ec3.png)

Now lets try xor this msg with "t3mp", but still that isn't useful.
So I tried xoring the data we got in envars with data we got in wireshark.
Well finally this gave the flag. And the flag is; **inctf{n3v3r_TrUs7_Sp4m_e_m41Ls}**
