## HTB-ACADEMY (Linux Fundamentals/ Intro)
## Ivan Medina
---

### History 

Unix -> Ken T, Dennis Ritchie 1970
BSD -> 1970
Richard Stallman -> GNU 1983 (Kernel)
Linus Torvald -> 1991 (OS) 

### Philosophy

- Everthing is a file.
- Small, Single-purpose programs.
- Chain programs-complex tasks.
- Avoid UI, Shell for all.
- Configuration in files.

### Componments

- Bootloader
- OS Kernel
- Daemons
- OS Shell
- Graphics Server
- Windows Manager 
- Utilities

### Architecture

- Hardware
- Kernel
- System
- Utilities
- Shell

### System Info

- ``` whoami ``` : Actual user logged.
- ``` id ``` : Numeric id for users and groups.
- ``` hostname ``` : Host Name
- ``` uname ``` : System Information.
- ``` pwd ``` : Actual path in terminal.
- ``` ifconfig ``` : Network information and configurations.
- ``` ip ``` : Network information and configurations.
- ``` netstat ``` : Network information of system about actual proccesses and services.
- ``` ss ``` : Processes in sockets.
- ``` ps ``` : Processses in system.
- ``` who ``` : Who users are logged in system.
- ``` env ``` : Environment variables.
- ``` lsbk ``` : List block devices.
- ``` lsub ``` : List USB devices.
- ``` lsof ``` : List oppened files.
- ``` lspci ``` : List PCI devices.

### Packages

Packages are used for build, update and mantain software.
Other common tasks are:

- Package Download.
- Dependency Resolution.
- Common Instalation.

Some linux package manager are:

- dpkg
- apt
- aptitude
- snap
- gem
- pip
- git

The file for source configuration commonly is in ...

``` /etc/apt/sources.list ```

Search for specific package:

``` apt-cache search xxxx ```

List installed packages:

``` apt list --instaled ```