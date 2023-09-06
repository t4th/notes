# git

set cert
git config --list | grep http.sslcainfo

git config --global http.sslcainfo "c:/cert.crt"