Step 1 — Set up the port proxy (run PowerShell as Admin):


$wslIp = (wsl hostname -I).Trim()
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=8080 connectaddress=$wslIp
Step 2 — Open Windows Firewall:


netsh advfirewall firewall add rule name="WSL Port 8080" dir=in action=allow protocol=TCP localport=8080
netsh advfirewall firewall add rule name="WSL Port 443" dir=in action=allow protocol=TCP localport=443
Step 3 — Get your Windows machine IP:


ipconfig | findstr "IPv4"
Look for your LAN IP, e.g. 192.168.1.50.

Step 4 — From the other machine, connect to:


http://192.168.1.50:8080


Option 3 — dnsmasq on Linux/WSL:


# Install
sudo apt install dnsmasq

# Add entry
echo "address=/shelf-focal.local/192.168.1.50" | sudo tee /etc/dnsmasq.d/local.conf

# Restart
sudo systemctl restart dnsmasq
Then point your router's DNS to this machine's IP.


### move ubuntu do other dir

```
Export Ubuntu

mkdir D:\backup
wsl --export Ubuntu D:\backup\ubuntu.tar
Unregister the same distribution to remove it from the C: drive:

wsl --unregister Ubuntu
Import Ubuntu

mkdir D:\wsl
wsl --import Ubuntu D:\wsl\ D:\backup\ubuntu.tar
```

### release reserved disk space
```
wsl --shutdown
diskpart
# open window Diskpart
select vdisk file=".\ext4.vhdx"
attach vdisk readonly
compact vdisk
detach vdisk
exit

```
