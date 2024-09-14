- 查看进程
```powershell
# 查询端口
netstat -ano | findstr 8080
# 查询根据PID查询进程名
tasklist | findstr 9000
# 根据进程PID结束进程
taskkill /T /F /PID 9000
# 根据进门名结束进程
taskkill /T /F /im schost.exe
```

- 修改RDP端口

```powershell
# 查看RDP当前端口
Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "PortNumber"

# 修改RDP端口(会自动在防火墙创建入站规则)
$portvalue = 3390

Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "PortNumber" -Value $portvalue

New-NetFirewallRule -DisplayName 'RDPPORTLatest-TCP-In' -Profile 'Public' -Direction Inbound -Action Allow -Protocol TCP -LocalPort $portvalue 
New-NetFirewallRule -DisplayName 'RDPPORTLatest-UDP-In' -Profile 'Public' -Direction Inbound -Action Allow -Protocol UDP -LocalPort $portvalue

```

- windows 性能优化

*powershell修改内存读取文件数量*
```powershell
mmagent
set-mmagent 8192

```

*修改注册表开启内存缓存*
```powershell
"\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management"

DisablePagingExecutive 1
LargeSystemCache 1
```