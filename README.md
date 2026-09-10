# Windows-A-i-O-System-Optimizer-and-Cleaner
Windows A-i-O System Optimizer and Cleaner
-

@echo off
:: Check for Administrator privileges and auto-elevate
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"

if '%errorlevel%' NEQ '0' (
    echo Requesting administrative privileges...
    goto UACPrompt
) else ( goto gotAdmin )

:UACPrompt
    echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"
    echo UAC.ShellExecute "%~s0", "", "", "runas", 1 >> "%temp%\getadmin.vbs"
    "%temp%\getadmin.vbs"
    del "%temp%\getadmin.vbs"
    exit /B

:gotAdmin
    pushd "%~dp0"
    cd /d "%~dp0"
    
    :: ==========================================
@echo off
title Windows A-i-O System Optimizer and Cleaner
color 0A

echo ===================================================
echo Running Administrator Check...
echo ===================================================
net session >nul 2>&1
if %errorLevel% neq 0 (
    echo Failure: Please run this script as Administrator!
    pause
    exit
)

echo ===================================================
echo 1. Cleaning Windows Temporary Files...
echo ===================================================
del /f /s /q /q "%systemdrive%\Temp\*.*" 2>nul
rmdir /s /q "%systemdrive%\Temp" 2>nul
mkdir "%systemdrive%\Temp" 2>nul
del /f /s /q /q "%windir%\Temp\*.*" 2>nul
rmdir /s /q "%windir%\Temp" 2>nul
mkdir "%windir%\Temp" 2>nul
del /f /s /q /q "%windir%\Prefetch\*.*" 2>nul
del /f /s /q /q "%userprofile%\AppData\Local\Temp\*.*" 2>nul
rmdir /s /q "%userprofile%\AppData\Local\Temp" 2>nul
mkdir "%userprofile%\AppData\Local\Temp" 2>nul

echo ===================================================
echo 2. Emptying the Recycle Bin...
echo ===================================================
rd /s /q c:\$Recycle.Bin 2>nul

echo ===================================================
echo 3. Flushing DNS Cache...
echo ===================================================
ipconfig /flushdns

echo ===================================================
echo 4. Running System File Checker (SFC)...
echo ===================================================
sfc /scannow

echo ===================================================
echo 5. Running DISM Health Restoration...
echo ===================================================
dism /online /cleanup-image /restorehealth

echo ===================================================
echo All tasks completed successfully!
echo ===================================================
pause
    :: ==========================================
    echo Running system optimizations...
    
    :: Example 1: Clear temporary files
    del /s /f /q %temp%\*.*
    del /s /f /q C:\Windows\Temp\*.*
    
    :: Example 2: Run Disk Cleanup / System File Checker (uncomment if needed)
    :: sfc /scannow
    
    echo Optimization complete!
    pause
