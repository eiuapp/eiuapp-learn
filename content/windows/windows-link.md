
Windows的四种链接方式

如果要看明白，请参考这里 https://binarythink.net/2013/06/windows-link/

## env

- os: win10
- cmder: 180626 preview

## step

### 创建硬链接 并 查询

```
C:\Users\DELL (master -> origin)
λ mklink /H C:\Users\DELL\Desktop\ahk.ahk C:\Users\DELL\autohotkey.ahk
为 C:\Users\DELL\Desktop\ahk.ahk <<===>> C:\Users\DELL\autohotkey.ahk 创建了硬链接

C:\Users\DELL (master -> origin)
λ fsutil.exe hardlink list autohotkey.ahk
\Users\DELL\autohotkey.ahk
\Users\DELL\Desktop\ahk.ahk
```

如果移动了这个硬链接，系统依然可以找到的，所以不用担心。

```
C:\Users\DELL (master -> origin)
λ fsutil.exe hardlink list autohotkey.ahk
\Users\DELL\autohotkey.ahk
\Users\DELL\Desktop\ahk.ahk

C:\Users\DELL (master -> origin)
λ fsutil.exe hardlink list autohotkey.ahk
\Users\DELL\autohotkey.ahk
\Users\DELL\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\ahk.ahk
```

## ref

- https://binarythink.net/2013/06/windows-link/
- https://www.konekomoe.com/command-windows-mklink/