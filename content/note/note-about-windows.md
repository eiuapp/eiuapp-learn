
## env
- os: win10

### windows startup folder


Your personal startup folder should be C:\Users\<user name>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup. The All Users startup folder should be C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup. You can create the folders if they aren't there. Enable viewing of hidden folders to see them.

```
C:\Users\$username\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup
```

### 在windows中获取某个进程的具体执行路径

在windows下要获取具体的路径可以使用powershell ，例如我们要查看chrome的执行路径，可以：

- 在cmd中这样写：

```
> powershell "get-process chrome | select-object path"  
```

- 在powershell中这样写：

```
> get-process chrome | select-object path
```

这里展示一个 bash 的执行路径：

```
C:\Users\DELL [master ≡ +1 ~4 -0 !]
λ  get-process bash | select-object path

Path
----
C:\Windows\system32\bash.exe
D:\Program Files\Git\usr\bin\bash.exe

C:\Users\DELL [master ≡ +1 ~4 -0 !]
```



## ref
- https://answers.microsoft.com/en-us/windows/forum/all/how-to-get-startup-folder-in-start-all-programs/d3f5486a-16c0-4e69-8446-c50dd35163f1
- https://chainhou.iteye.com/blog/1872294
