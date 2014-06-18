# AutoIt scripting on Windows

## What is it

* [AutoIt](http://www.autoitscript.com/site/autoit/) is a tool to automate software actions that are normally only interactive, meaning: you can script/automate starting a program, clicking on buttons, pressing keys and waiting for windows to appear.


## Examples


### Cloudberry

* we used [Cloudberry Explorer for S3](http://www.cloudberrylab.com/products.aspx) for a long time for uploading files to Amazon S3. Cloudberry has a compare-and-sync functionality to only upload what is new, but one could only start it manually, there was no way to runit asa command line. So I built a script that started the program, wiated for it to appear (it starts very slow), performs a number of shortcut buttons (e.g. CTRL-S, CTRL-U) to start the sync-folder task and wait for execution to end.
* we compiled the program to a .exe so it runs on any PC (without have AutoIt installed).