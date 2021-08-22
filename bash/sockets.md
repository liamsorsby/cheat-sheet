# Keep your SSH connections open by using sockets

### Configuration

Make sure you configure your sockets to be just long enough. You don't want to keep your terminal connected to your server when you log off.be

Add this to your ~/.ssh/config file, replacing the User with your username and tweaking the ControlPersist with the number of seconds you desire.
```bash
# Sets default user for every ssh connection
# Sets keep alive to help keep connections open
Host *
ControlMaster auto
ControlPath ~/.ssh/sockets/%r@%h-%p
ControlPersist 600
ServerAliveInterval 60
User myusername
```

Make sure to create the sockets directory:
```bash
mkdir -p ~/.ssh/sockets/
```

If you have a gateway server, you can add the following to your ~/.ssh/config file.
This example assumes your servers hostnames all start with abc, the IP's all start 10.1. and your gateway starts with gateway
```bash
Host gw
  Hostname gateway.host.com

Host abc* 10.1.* !gateway*
  ProxyCommand ssh gw -q 'nc %h %p'
```


### Navigation
[Back Home](../README.md)
