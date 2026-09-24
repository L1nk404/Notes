## 10.1.1 The Cisco IOS Command Line Interface
The Cisco IOS (Internetwork Operation System) command line interface (CLI) is a text-based program that enables entering and executing Cisco IOS commands to configure, monitor, and maintain Cisco devices. The Cisco CLI can be used with either in-band or out-of-band management tasks.
CLI commands are used to alter the configuration of the device and to display the current status of processes on the router. For experienced users, the CLI offers many time-saving features for creating both simple and complex configurations. **Almost all Cisco networking devices use a similar CLI**. When the router has completed the power-up sequence and the `Router>` prompt appears, the CLI can be used to enter Cisco IOS commands, as shown in the command output.

```cisco
Router con0 is now avaiable

press RETURN to get started!

Router> enable
Router# config terminal
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)# hostname R1
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)#
```

Technicians familiar with the IOS commands and operation of the CLI find it easy to monitor and configure a variety of different networking devices because the same basic commands are used for configuring a switch and a router. The CLI has an extensive help system that assists users in setting up and monitoring devices.
## Primary Command Modes
All network devices require an OS and devices require an OS and they can be configured using CLI or GUI . Using the CLI may provide the network administrator with more precise control and flexibility than using the GUI. This topic discusses using CLI to navigate the Cisco IOS.
As a security feature, the Cisco IOS software separates management access into the following two command modes:
- **User EXEC Mode** - This mode has limited capabilities but is useful for basic operations. It allows only a limited number of basic monitoring commands but does not allow the execution of any commands that might change the configuration of the device. The user EXEC mode is identified by the CLI promp that ends with the `>` symbol
- **Privileged EXEC Mode** - To execute configuration commands, a network administrator must access privileged EXEC mode. Higher configurations modes, like global configuration mode, can only be reached from privileged EXEC mode. The privileged EXEC mode can be identified by the prompt ending with the `#` symbol.

| Command Mode         | Description                                                                                                                                       | Default Device Prompt  |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| User EXEC Mode       | - Mode allows access to only a limited number of basic monitoring commands.<br>- It is often referred to as "view-only" mode.                     | `Switch>`<br>`Router>` |
| Privileged EXEC mode | - Mode allow access to all commands and features<br>- The user can use any monitoring commands and execute configuration and management commands. | `Switch#`<br>`Router#` |

