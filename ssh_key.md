### SSH key workflow

The following will generate a `myKey` file which is the private key that you keep on the client. Note it has no pass phrase and will also create a `myKey.pub`.

```
ssh-keygen -t rsa -b 4096 -f myKey -q -N ""
```

Next, transfer append/copy the plain-text contents of `myKey.pub` to the `authorized_keys` file on the server located at `/home/[user]/.ssh`. To use this key w/ SSH, see the following 

```
ssh [server_user]@[server_ip] -p [server_port] -i myKey
```

> Note, key file permissions must be as follows:

```
sudo chmod 600 myKey
```

To transfer the key to the server, one can use the following command:

```
cat myKey.pub | (ssh [server_user]@[server_url] -p [server_port] "cat >> ~/.ssh/authorized_keys")
```

### Remote tunnel 

Reference: https://www.howtoforge.com/reverse-ssh-tunneling 

```
ssh -f -N -R [random port]:localhost:22 [server_user]@[server_url] -p [server_port] -i myKey
```

- `-f -N` Executes in background 

### Chain socks proxy 

Reference: https://superuser.com/questions/836194/how-to-chain-socks-proxies 

```
ssh -tt -v -L[your_client_proxy_port]:localhost:[tunnel_port] [user_b]@[url_b] ssh -t -D [tunnel_port] [user_c]@[url_c]
```

> Note, firefox allows for proxying to a localhost:[your_client_proxy_port] . It also allows for DNS through the proxy unlike the standard windows 


