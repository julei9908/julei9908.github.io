# frp客户端自启

💡 重命名**frp.vbs**并复制到**C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp**

```visual-basic
set ws=WScript.CreateObject("WScript.Shell") 
ws.Run "d:\frp\frpc.exe -c d:\frp\frpc.ini",0
```