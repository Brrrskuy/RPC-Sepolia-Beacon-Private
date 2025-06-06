# Install RPC Sepolia dan Beacon Sepolia GETH+Lighthouse
- Buy VPS di : [t.me/skuycloud](t.me/skuycloud)
- Trakteer buat buy Kopi : https://trakteer.id/brrrskuy/tip `<---`
---------------------------------------------
| **Requirement Minimum**         |
|-------------------------|
|  16GB RAM                |
|  6cores                
|  ~1TB+ - 2TB Storage                |

`This make snap sync not full sync , maybe 6-12 hours normaly to running until success`

`Buy VPS Storage Capity in Contabo maybe ,more 1TB+ Lebih baik`

# First time Git Clone my repo
```
https://github.com/Brrrskuy/RPC-Sepolia-Beacon-Private.git
```
# One Click Command
```
chmod +x install-rpc-private-sepolia.sh && sed -i 's/\r$//' install-rpc-private-sepolia.sh && ./install-rpc-private-sepolia.sh
```
# Check Status Command
```
chmod +x check-node-status.sh && sed -i 's/\r$//' check-node-status.sh && ./check-node-status.sh
```
# Allow Port UFW
```
sudo ufw allow ssh
sudo ufw enable
sudo ufw allow 22/tcp        # SSH
sudo ufw allow 30303/tcp     # Geth P2P
sudo ufw allow 30303/udp     # Geth P2P
sudo ufw allow 9000/tcp      # Lighthouse P2P
sudo ufw allow 9000/udp      # Lighthouse P2P
```
**Reload UFW**
```
sudo ufw reload
```
# Cara Cek Sinkronisasi & Status
**Cek versi client**
```
curl -X POST http://IP-ANDA:8545 \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":1}'
```
**Cek sync status**
```
curl -X POST http://IP-ANDA:8545 \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}'
```
```
curl -X POST http://161.97.67.172:8545 \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}'
```
Jika Result seperti ini berarti Sync mode sudah aktif dan RPC Siap digunakan
```
{"jsonrpc":"2.0","id":1,"result":false}
```
**Cek Lighthouse status**
```
curl http://IP-ANDA:5052/eth/v1/node/health
```
# Check Logs Sync 
```
docker compose logs -f geth
```
```
docker compose logs -f lighthouse
```
# Maintenance dan Update
```
docker compose pull
```
```
docker compose down
```
```
docker compose up -d
```
# Akses Sepolia,Endpoint RPC HTTP & Beacon Sepolia
**Sepolia RPC**
```
http://IP_VPS:8545
```
**Beacon Sepolia RPC**
```
http://IP_VPS:5052
```
--------------------
# Jika ingin Private RPC tidak untuk Public
**Hapus port Public**
```
sudo ufw delete allow 8545/tcp
sudo ufw delete allow 8546/tcp
sudo ufw delete allow 8551/tcp
sudo ufw delete allow 5052/tcp
sudo ufw delete allow 8545/tcp
sudo ufw delete allow 8546/tcp
sudo ufw delete allow 8551/tcp
sudo ufw delete allow 5052/tcp
```
# Buka Akses untuk IP Tertentu
```
# Geth HTTP RPC
sudo ufw allow from YOUR.IP.xxx.xxx to any port 8545 proto tcp
sudo ufw allow from YOUR.IP.xxx.xxx to any port 8545 proto tcp

# Geth WebSocket (optional)
sudo ufw allow from YOUR.IP.xxx.xxx to any port 8546 proto tcp
sudo ufw allow from YOUR.IP.xxx.xxx to any port 8546 proto tcp

# Lighthouse Beacon API
sudo ufw allow from YOUR.IP.xxx.xxx to any port 5052 proto tcp
sudo ufw allow from YOUR.IP.xxx.xxx to any port 5052 proto tcp
```
`Ubah Dengan IP mu`

# Check UFW Status
```
sudo ufw status numbered
```
Untuk hapus ufw (ganti nomor `00`)
```
sudo ufw delete 00
```
# Check lewat IP Waitlist
```
curl http://<IP>:8545
```
```
curl http://<IP>:5052
```
# Jika mau block selain IP Waitlist yang kamu buat

Jangan lupa untuk batasi UFW terlebih dahulu
```
sudo ufw deny 8545/tcp
sudo ufw deny 5022/tcp
```
Buat file daemon.json
```
sudo nano /etc/docker/daemon.json
```
Isi dengan ini , kalau sudah `CTRL+X+S` Untuk Save
```
{
  "iptables": false
}
```
Setelah itu Restart UFW
```
sudo systemctl restart docker
sudo ufw disable && sudo ufw enable
```
# Test Telnet di VPS Lain tidak waitlist
```
telnet 128.392.xxx.xxx 8545
```
`Ganti 128.392.xxx.xxx Pakai IP VPSmu`

**Jika Berhasil**
```
Trying 128.xxx.xxx.xxx
Connected to 128.xxx.xxx.xxx
```
**Jika Gagal**
```
Trying 128.xxx.xxx.xxx
telnet: Unable to connect to remote host: Connection refused
```
