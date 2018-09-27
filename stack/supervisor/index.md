***SUPERVISOR***
===============

Resources
=========
[Supervisor: A Process Control System](http://supervisord.org/)

Summary
=======
Supervisor is a client/server system that allows its users to monitor and control a number of processes on UNIX-like operating systems.

Features
========
* Convenience: start, restart, crash restart
* Accuracy:  status (up/down)
* Delegation: Can allow normal (not-su) users to stop, start, and restart processes from shell or UI
* Process groups: start or stop all

Components
----------
* supervisord: server daemon, .conf file
* supervisorctl: Command Line
* Web Server
* XML-RPC interface

Usage
-----
* supervisorctl
```
add <name> [...]

Activates any updates in config for process/group

remove <name> [...]

Removes process/group from active config
update

pid <name> [all]

Get the PID of a single child process by name (or all).

start [<gmane>, <name><name>, [all]]
Start processes (by group, name, or all)

status
stop
```
* supervisord can receive signals

Config File
-----------
* Add Program
```
[program:foo]
command=/bin/cat
```
* ENV Variable
```
[program:example]
command=/usr/bin/example --loglevel=%(ENV_LOGLEVEL)s
```
* unix_http_server: Web Server
```
[unix_http_server]
file = /tmp/supervisor.sock
chmod = 0777
chown= nobody:nogroup
username = user
password = 123
```
* inet_http_server
```
[inet_http_server]
port = 127.0.0.1:9001
username = user
password = 123
```

* supervisord
```
[supervisord]
logfile = /tmp/supervisord.log
logfile_maxbytes = 50MB
logfile_backups=10
loglevel = info
pidfile = /tmp/supervisord.pid
nodaemon = false
minfds = 1024
minprocs = 200
umask = 022
user = chrism
identifier = supervisor
directory = /tmp
nocleanup = true
childlogdir = /tmp
strip_ansi = false
environment = KEY1="value1",KEY2="value2"
```
* supervisorctl
```
[supervisorctl]
serverurl = unix:///tmp/supervisor.sock
username = chris
password = 123
prompt = mysupervisor
```
* program
```
[program:cat]
command=/bin/cat
process_name=%(program_name)s
numprocs=1
directory=/tmp
umask=022
priority=999
autostart=true
autorestart=unexpected
startsecs=10
startretries=3
exitcodes=0,2
stopsignal=TERM
stopwaitsecs=10
stopasgroup=false
killasgroup=false
user=chrism
redirect_stderr=false
stdout_logfile=/a/path
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stdout_events_enabled=false
stderr_logfile=/a/path
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
stderr_events_enabled=false
environment=A="1",B="2"
serverurl=AUTO
```
* include
```
[fcgi-program:fcgiprogramname]
command=/usr/bin/example.fcgi
socket=unix:///var/run/supervisor/%(program_name)s.sock
socket_owner=chrism
socket_mode=0700
process_name=%(program_name)s_%(process_num)02d
numprocs=5
directory=/tmp
umask=022
priority=999
autostart=true
autorestart=unexpected
startsecs=1
startretries=3
exitcodes=0,2
stopsignal=QUIT
stopasgroup=false
killasgroup=false
stopwaitsecs=10
user=chrism
redirect_stderr=true
stdout_logfile=/a/path
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_events_enabled=false
stderr_logfile=/a/path
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_events_enabled=false
environment=A="1",B="2"
serverurl=AUTO
```
* eventlistener
```
[eventlistener:theeventlistenername]
command=/bin/eventlistener
process_name=%(program_name)s_%(process_num)02d
numprocs=5
events=PROCESS_STATE
buffer_size=10
directory=/tmp
umask=022
priority=-1
autostart=true
autorestart=unexpected
startsecs=1
startretries=3
exitcodes=0,2
stopsignal=QUIT
stopwaitsecs=10
stopasgroup=false
killasgroup=false
user=chrism
redirect_stderr=false
stdout_logfile=/a/path
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_events_enabled=false
stderr_logfile=/a/path
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_events_enabled=false
environment=A="1",B="2"
serverurl=AUTO
```
* rpcinterface

Subprocesses
------------
* Don't run a self-daemonizing process that detaches from terminal
* Sometimes you have to access process from pidfile, for the daemonized process created in mysqld
```
[program:mysql]
command=/path/to/pidproxy /path/to/pidfile /path/to/mysqld_safe
```
* Subprocess Environment: Can specify variables passed to subprocesses
* Process States:
STOPPED (0),
STARTING (10),
RUNNING (20),
BACKOFF (30),
STOPPING (40),
EXITED (100),
FATAL (200),
UNKNOWN (1000)

Logging
=======
Activity Log: where supervisord logs messages about its own health, its subprocess’ state changes, any messages that result from events, and debug and informational messages.
```
2007-09-08 14:43:22,886 DEBG 127.0.0.1:Medusa (V1.11) started at Sat Sep  8 14:43:22 2007
        Hostname: kingfish
        Port:9001
2007-09-08 14:43:22,961 INFO RPC interface 'supervisor' initialized
2007-09-08 14:43:22,961 CRIT Running without any HTTP authentication checking
2007-09-08 14:43:22,962 INFO supervisord started with pid 27347
2007-09-08 14:43:23,965 INFO spawned: 'listener_00' with pid 27349
2007-09-08 14:43:23,970 INFO spawned: 'eventgen' with pid 27350
2007-09-08 14:43:23,990 INFO spawned: 'grower' with pid 27351
2007-09-08 14:43:24,059 DEBG 'listener_00' stderr output:
 /Users/chrism/projects/supervisor/supervisor2/dev-sandbox/bin/python:
 can't open file '/Users/chrism/projects/supervisor/supervisor2/src/supervisor/scripts/osx_eventgen_listener.py':
 [Errno 2] No such file or directory
2007-09-08 14:43:24,060 DEBG fd 7 closed, stopped monitoring <PEventListenerDispatcher at 19910168 for
 <Subprocess at 18892960 with name listener_00 in state STARTING> (stdout)>
2007-09-08 14:43:24,060 INFO exited: listener_00 (exit status 2; not expected)
2007-09-08 14:43:24,061 DEBG received SIGCHLD indicating a child quit
```
* Log Levels:
critical	CRIT	,
error	ERRO,
warn	WARN,
info	INFO,
debug	DEBG,
trace	TRAC,
blather	BLAT
* Child Process Logs: supervisord will capture the child process’ stdout and stderr output into temporary files. Each stream is captured to a separate file. This is known as AUTO log mode.
* Capture Mode : Capture output from one process to pipe it as input to another.

Events
======
* You do need to understand events if you want to use Supervisor as part of a process monitoring/notification framework.
* Supervisor provides a way for a specially written program (which it runs as a subprocess) called an “event listener” to subscribe to “event notifications”.
* Pub/Sub
```
[eventlistener:mylistener]
command=my_custom_listener.py
events=PROCESS_STATE,TICK_60
```
* Event Listener:  An event listener implementation is a program that is willing to accept structured input on its stdin stream and produce structured output on its stdout stream.
* Event Notification Protocol
```
# HEADER
ver:3.0 server:supervisor serial:21 pool:listener poolserial:10 eventname:PROCESS_COMMUNICATION_STDOUT len:54

# PAYLOAD
processname:foo groupname:bar pid:123
This is the data that was sent between the tags
```
* Event Listener Status: ACKNOWLEDGED, READY, BUSY
* Event Listener Notification Protocol

```

def main():
    while 1:
        # transition from ACKNOWLEDGED to READY
        write_stdout('READY\n')

        # read header line and print it to stderr
        line = sys.stdin.readline()
        write_stderr(line)

        # read event payload and print it to stderr
        headers = dict([ x.split(':') for x in line.split() ])
        data = sys.stdin.read(int(headers['len']))
        write_stderr(data)

        # transition from READY to ACKNOWLEDGED
        # output result
        write_stdout('RESULT 2\nOK')
```
* Events
```
processname:cat groupname:cat from_state:STOPPED
# Body description
processname:cat groupname:cat from_state:STOPPED tries:0
processname:cat groupname:cat from_state:STARTING pid:2766
processname:cat groupname:cat from_state:STOPPED tries:0


```