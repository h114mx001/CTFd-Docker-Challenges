# Secure your Docker Daemon with TLS 

## Configuration 

After finish installing the plugin into your CTFd platform, you can follow these steps: 

1. Run the script [`secure_docker_daemon.sh`](secure-docker-daemon.sh) to generate the certificates and keys. 
   + The default location of your certificates and keys is `~/.docker/`. 
   + `$PASSWORD` is the password you wish to use when generate the certificates and keys.
   + `$HOST` is the hostname of your server. You can use your domain name or IP address here. 

2. Update the configuration file at `/etc/docker/daemon.json` as below: 

```json 
{
        "hosts": ["tcp://0.0.0.0:2376", "unix://var/run/docker.sock"], // 2376 is the default port for Docker Daemon with TLS 
        "tls": true,
        "tlscacert": "/home/<your_username>/.docker/ca.pem", 
        "tlscert": "/home/<your_username>/.docker/server-cert.pem",
        "tlskey": "/home/<your_username>/.docker/server-key.pem",  
        "tlsverify": true
}

```

3. Add a file `/etc/systemd/system/docker.service.d/override.conf`: 

```conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd
```

4. Reload the `systemd` daemon & restart the Docker service:

```bash
sudo systemctl daemon-reload 
sudo systemctl restart docker.service
```

For more information, please refer to these documentations from Docker:
+ [Use TLS (HTTPS) to protect the Docker daemon socket](https://docs.docker.com/engine/security/protect-access/#use-tls-https-to-protect-the-docker-daemon-socket).
+ [Configure the daemon with systemd](https://docs.docker.com/config/daemon/systemd/)