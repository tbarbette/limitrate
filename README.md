#Limit rate

THis programs allows you to limit the rate of launching a certain command. Instead of launching

> /usr/bin/program argument1 argument2

call :

> limitrate 30 "/usr/bin/program % argument2" argument1

To launch only this command at maximum once every 30 seconds. If you call multiple times the last line in 30 seconds, limitrate will concatenate all "argument1" and run the command after 30 seconds. For example to send an alert e-mail containing files (for example webcam picture) at max every 30 seconds :

```
10:24:30 : limitrate 30 "echo \"Alert!\" | mailx % destination@mail.com" "-A /path/to/file1"
--> Send a mail with file1
10:24:32 : limitrate 30 "echo \"Alert!\" | mailx % destination@mail.com" "-A /path/to/file2"
10:24:38 : limitrate 30 "echo \"Alert!\" | mailx % destination@mail.com" "-A /path/to/file3"
10:24:41 : limitrate 30 "echo \"Alert!\" | mailx % destination@mail.com" "-A /path/to/file4"
10:25:05 : limitrate 30 "echo \"Alert!\" | mailx % destination@mail.com" "-A /path/to/file5"
--> Send a mail with file 2,3,4 and 5
```

Example for motion detection daemon (Camera surveillance) : 

> on_picture_save /usr/local/bin/limitrate 15 "echo \"See attachment below.\" | mailx -s 'Motion in the saloon !' % destination@tombarbette.be" "-A %f"

When an intrusioon is detected, you'll receive a first e-mail with one image. 15 seconds later you'll receive all the new images in only one mail, and so on.
