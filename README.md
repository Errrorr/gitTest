# Monitor
This is a monitor program.

This monitor will monitor each server, including
- BS
- AS
- DS
- SN

In order to monitor PPHook system, this monitor also run 2 devices to
- Try online
- Try connect

When servers occur any problem, monitor will send the notification to developers.

The logs of Monitor saved in log folder, and each log file will keep `7` days.

## Notes
The monitor uses `wget` to keep alive with DB of AS and `screen` to open servers.

Install wget
````sh
> sudo apt-get install wget
````

Install screen
````sh
> sudo apt-get install screen
````

## Settings
The `config.yml` is the setting of this Monitor. It can set whether enable or disable the different kind of monitor.

Enable BS, DS, SN, or AS

    monitor:
        ...
      - identity: "BS", "DS", "SN", or "AS"
        enable:   true
        ...

Enable sysD2D

    sysD2D:
      enable: true

## Build and Run
Use the monitor to execute servers
````go
> go build
> sudo screen -S mo sudo ./monitor
````
Use the monitor to monitor independent servers (SN as example)
````sh
> cd ../server
> sudo screen -S SN -d -m sudo ./PPHookSN sysfilePath
````
````go
> go build
> sudo screen -S mo sudo ./monitor
````
After screen, detach screen `Ctrl+A+D`.

Attach monitor `sudo screen -r mo`.

Attach servers (SN as example) `sudo screen -r SN`

## Screen tutorial
Screen can open another terminal, and doesn't affect running process after detach, wget, ssh, etc.

Screen cmd | Description
-----------|------------------------
-S         | Rename screen
-d         | Detach screen
-m         | Force open new screen
-r         | Reattach existed screen
-ls        | List existed all screen

Example
````C
// Open new screen, named mo, to execute the program ./monitor
> sudo screen -S mo sudo ./monitor
// Open new screen, named SN, to execute the program ./PPHookSN, and then detach
> sudo screen -S SN -d -m sudo ./PPHookSN sysfilePath
````

After entering screen, use hot key to enter command
Hot key                       | Description
------------------------------|------------------------
Ctrl + A + D                  | detach current screen
Ctrl + A + esc                | Enter copy mode, can use arrow key to move cursor and wheel to roll page
Ctrl + A + :sessionname [name]| Rename current screen
