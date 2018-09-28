***FABRIC***
===========

Resources
==========
[Pythonic remote execution](http://www.fabfile.org/)

Definition
==========
Fabric is a high level Python (2.7, 3.4+) library designed to execute shell commands remotely over SSH, yielding useful Python objects in return:

```
>>> from fabric import Connection
>>> result = Connection('web1.example.com').run('uname -s', hide=True)
>>> msg = "Ran {0.command!r} on {0.connection.host}, got stdout:\n{0.stdout}"
>>> print(msg.format(result))
Ran 'uname -s' on web1.example.com, got stdout:
```

Usage
=====

* Singular
>>> result = Connection('web1').run('hostname')

* Single across hosts
>>> result = SerialGroup('web1', 'web2').run('hostname')

* Code blocks invoking RPCs


Get Started
===========
* Connection objects open ssh object and are core of API.  When run, usually return instances of invoke.runners.Result
* Sudo:  c.run('sudo useradd mydbuser', pty=True)
* Context.sudo object
* File transfer: >>> result = Connection('web1').put('myfiles.tgz', remote='/opt/mydata/')
* Multiple Servers: Group (SerialGroup, ThreadingGroup) wraps collection of Connection objects

```
from fabric import SerialGroup as Group

@task   #decorator for fab CLI
def upload_and_unpack(c):
    if c.run('test -f /opt/mydata/myfile', warn=True).failed:
        c.put('myfiles.tgz', '/opt/mydata')
        c.run('tar -C /opt/mydata -xzvf /opt/mydata/myfiles.tgz')
for connection in Group('web1', 'web2', 'web3'):
    upload_and_unpack(connection)
```

* Authentication over PK, passphrase, password, config file
* Config file:
connect_kwargs,
forward_agent,
gateway,
load_ssh_configs,
port,
inline_ssh_env: ,
ssh_config_path,
timeouts (eg. connect),
user
* ssh_config: Connection parameters, Proxying, Authentication
* Networking:  ProxyJump, ProxyCommand