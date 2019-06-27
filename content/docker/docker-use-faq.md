

```
ubuntu@VM-0-12-ubuntu:~$ sudo docker pull tomtsang:ubuntu-desktop-ide
Error response from daemon: pull access denied for tomtsang, repository does not exist or may require 'docker login'
ubuntu@VM-0-12-ubuntu:~$
```

表示要登录

```
ubuntu@VM-0-12-ubuntu:~$ sudo docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: tomtsang
Password: 
Login Succeeded
ubuntu@VM-0-12-ubuntu:~$
```
