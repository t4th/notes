# git

show local settings
```
git config -l --global --show-origin
```
set certificate
```
git config --list | grep http.sslcainfo
git config --global http.sslcainfo "c:/cert.crt"
git config --global http.sslverify true
```
set credentials
```
git config --global user.name "asd"
git config --global user.email asd.asd@asd.asd
```
