# DosScript

DosScript that sends out email 

testtstmail.bat

@echo off
Title CheckingFile
setlocal
set File=C:\Users\sbharadw\Desktop\Workspace\testtest\WatchedDir\ael_dib.txt
set Interval=6
for %%a in ("%File%") do set FileName=%%~nxa
set OldFileTime=X
:Loop
for %%a in ("%File%") do set NewFileTime=%%~ta
echo [%Date%][%Time%] '%FileName%' last modified: %NewFileTime%

ping.exe -n %Interval% localhost >NUL
if not "%OldFileTime%"=="%NewFileTime%" (
	set OldFileTime=%NewFileTime%
	echo This is the command line emailing
	wscript "C:\Users\sbharadw\Desktop\Workspace\testtest\vmail.vbs"
	echo done emailing
	goto :Loop
)
echo [%Date%][%Time%] '%FileName%' has not been modified for %Interval% seconds!
ECHO blat.exe ...
goto Loop


vmail.vbs
------------------



Set MyEmail=CreateObject("CDO.Message")
dim fso, file, lastUpdated
Set fso = CreateObject("Scripting.FileSystemObject")
set file = fso.GetFile("C:\Users\sbharadw\Desktop\Workspace\testtest\WatchedDir\ael_dib.txt")
lastUpdated =  file.DateLastModified


MyEmail.Subject="Example Testing only - ael_dib last changed " & lastUpdated 
MyEmail.From="ael_dib@xxxx.xxx.com"
MyEmail.To="Suman.Bharadwaj@xxxx.com"
MyEmail.TextBody="Example Testing only ael_dib last changed " & lastUpdated 
MyEmail.AddAttachment "C:\Users\sbharadw\Desktop\Workspace\testtest\WatchedDir\ael_dib.txt"

MyEmail.Configuration.Fields.Item ("http://schemas.microsoft.com/cdo/configuration/sendusing")=2

'SMTP Server
MyEmail.Configuration.Fields.Item ("http://schemas.microsoft.com/cdo/configuration/smtpserver")="LFCSMTP.xxxx.com"

'SMTP Port
MyEmail.Configuration.Fields.Item ("http://schemas.microsoft.com/cdo/configuration/smtpserverport")=25 

MyEmail.Configuration.Fields.Update
MyEmail.Send

set MyEmail=nothing

-- 
Thank you  
Best Regards
Suman
