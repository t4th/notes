# bat

## pack and copy

```
rem remove old archive
del C:\repo.7z

rem create new archive
"C:\Program Files\7-Zip\7z.exe" a -t7z C:\repo.7z C:\repo\

rem copy archive
xcopy /Y C:\repo.7z \\network\dirname$\
```

## unpack

```
rem remove old dir
rmdir \\network\dirname$\repo /s /q

rem extract archive
"C:\Program Files\7-Zip\7z.exe" x \\network\dirname$\repo.7z
```
