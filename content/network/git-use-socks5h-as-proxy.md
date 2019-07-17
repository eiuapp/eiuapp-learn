git使用socks为代理

https://stackoverflow.com/questions/15227130/using-a-socks-proxy-with-git-for-the-http-transport

```bash
git config --global http.proxy socks5h://127.0.0.1:1080
git *****
git config --global --unset http.proxy
```

