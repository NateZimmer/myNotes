# Remote reverse SSH sever risks 

For an alpha/beta release, a reverse SSH tunnel is an unparalleled golden carrot to be dangled infront of any paranoid dev forced into releasing a premature product burdened by ignorant wishful dreams of clueless management. Regrettably as with everything in life, discipline and extensive knowledge is required to even attempt to not do something helplessly ignorant. A day is eons too late for catch-up to anyone playing Sisyphus's endless security game let alone a newcomer. 

## SSH jail 

An [SSH jail](https://github.com/dannysheehan/linux-chroot-jail/blob/master/jailuser.sh) apparently is a hypothetical romantic scenario in which a client is confined to their prison. A fundamental mechanism for this security is `chroot` which is compromised by the likes of [chw00t](https://github.com/earthquake/chw00t). An example of ***documented*** pitfalls are covered [here](https://deepsec.net/docs/Slides/2015/Chw00t_How_To_Break%20Out_from_Various_Chroot_Solutions_-_Bucsay_Balazs.pdf). Recall, there are endless undocumented loopholes which are ideally confined to government sponsored endeavors which are ideally, hopefully, beyond applicable to the trivial likes of *you*. The ideal advantage of the hypothetical SSH prison is one can use this pipe for reverse tunneling as well as shell opperations such as file logging.

## Reverse tunneling with mitigated risk 

To avoid documented pitfalls, one can employ a minimal restricted reverse tunnel to a remote server. These measures could include: 

- Creation of a password-less user via: `sudo useradd $my_user` without the follow-up command of `sudo passwd $my-user`
- Disabling a shell for `$my_user` via `sudo usermod -s /bin/false $my-user`
- Disabling of port forwarding by adding the following to ones sshd config(`/etc/ssh/sshd_config`) to avoid pitfalls documented [here](https://arlimus.github.io/articles/ssh.reverse.tunnel.security/). 
```Match group my_secure_group
Match group $user_group 
  GatewayPorts no
  AllowTcpForwarding remote

```

Group creation via script: 
```
if ! getent group $user_group > /dev/null 2>&1
then
  echo "creating group"
  groupadd -r $user_group
fi 
```

And adding `$my_user` to this group via script:

```
sudo usermod -a -G $my_user $user_group
```


An [SSH key](https://github.com/NateZimmer/myNotes/blob/master/ssh_key.md) can then be created for this user. Placing public key in an `~/.ssh/authorized_key` enables ssh-key authorization. 
This allows for the external client to establish a shell-less hypothetical secure reverse ssh tunnel:

```
ssh -f -N -R [server_local_port_for_remote_debugging]:localhost:[client_ssh_port_normally_22] [$my_user]@[server_url]-p [servers_ssh_port] -i [private_key]
```

This allows one to remote into the server and SSH into the remote device via: 

```
ssh [client_user]@localhost -p [server_local_port_for_remote_debugging]
```
Undoubtly, these steps are missing important details that render the above inadeqaute  
