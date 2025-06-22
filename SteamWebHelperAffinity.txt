@echo off
setlocal EnableDelayedExpansion

set "steam_path=D:\1Steam"

:: only continue if steam is not running
tasklist | findstr /i "steam.exe" || goto :query_opensteam

exit

:query_opensteam

:: open steam

start "" "!steam_path!\steam.exe" +open steam://open/minigameslist -vgui

WMIC process where name="steam.exe" CALL setpriority "Idle"

:: allow a few seconds for steam to open minigamelist
timeout /t 10 /nobreak

:: only continue if steam is running in case steam is updating
tasklist | findstr /i "steam.exe" || exit

WMIC process where name="steamwebhelper.exe" CALL setpriority "Idle"

timeout /t 10 /nobreak

:: only continue if steamwebhelper is running in case steam is updating
:query_steamwerror
tasklist | findstr /i "steamerrorreporter64.exe" || goto :post_steamerror

timeout /t 10 /nobreak

goto :query_steamwerror

:post_steamerror

WMIC process where name="steamwebhelper.exe" CALL setpriority "Idle"

:: allow a few seconds for cmd to exit
timeout /t 5 /nobreak