---
description: How remote procedure works
icon: satellite
---

# Remote Procedure

Remote Procedure is a remote controlling system for the kernel that allows external devices connected to the network to execute commands remotely. It can be triggered on and off in the kernel settings. By default, it's turned on.

For those who use the firewall, here's the configuration used for a standard RPC installation:

* Socket: UDP
* Port: `12345` (configurable)
* Transfer: Incoming and Outgoing

Basically, it listens to any commands received from any device. The commands are listed below:

* `<Request:Shutdown>`: Remotely shuts the system down
* `<Request:Reboot>`: Remotely reboots the system
* `<Request:RebootSafe>`: Remotely reboots the system to safe mode
* `<Request:RebootMaintenance>`: Remotely reboots the system to maintenance mode
* `<Request:RebootDebug>`: Remotely reboots the system to debug mode
* `<Request:SaveScr>`: Remotely saves the screen
* `<Request:Exec>`: Remotely executes the command
* `<Request:Acknowledge>`: Remotely acknowledges the device
* `<Request:Ping>`: Remotely acknowledges the device with a notification

Some commands support arguments. When the request is made, the kernel translates it to the response as in the following form:

```
TypeConfirm, Args
```

After being translated, the RPC attempts to send the response to the target device. It then translates the confirmation to the appropriate action.
