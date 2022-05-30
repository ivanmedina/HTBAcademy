## HTB-ACADEMY (Linux Fundamentals/ Processes & Services)
## Ivan Medina
---

### Processes

##### States

- running
- waiting
- stoped
- zombie

##### Signals

- SIGUP(1): Terminal closed.
- SIGINT(2): Ctrl + C.
- SIGQUIT(3): Ctrl + D.
- SIGKILL(9): Inmediatally kill (no clean).
- SIGTERM(15): Program termination.
- SIGSTOP(19): Stop Program.
- SIGSTOP(20): Ctrl + Z.

- ``` kill <signal> <pid>```

### Services

- ``` systemctl <start|stop|restart|status> <service>```
- ``` systemctl list-units --type=service```
- ``` ps -aux | grep <service>```
- ``` journalctl -u <nameservice.service> --no-page```

### Jobs

- ```ctrl + z```: set process in background.
- ```ctrl + c```: close program.
- ```bg```: open program in background.
- ```fg <n task>```: set task n in foreground.
- ```jobs```: show programs in background.

