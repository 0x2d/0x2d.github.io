# notes

## 设置Git远程仓库

```bash
# Server
git init --bare

# Host
git remote add sw ssh://swict@40.0.1.5/home/export/online1/mdt00/shisuan/swict/oyyc/repo/lammps.git
git push sw master

# Server
git clone file:///home/export/online1/mdt00/shisuan/swict/oyyc/repo/lammps.git
```

## 6b服务器相关命令

```bash
ssh -o TCPKeepAlive=yes -o ServerAliveInterval=30  ly@10.208.130.239
ssh -o TCPKeepAlive=yes -o ServerAliveInterval=30 -J ssh://oyyc@101.43.242.13:10179  ly@10.208.130.239

gua --unit spec1 runcpu -I -i ref -c 6b-clang-ra-lyc-flang -n 1 --rebuild 619

systemctl --user stop spec1.service
kan spec1.service
systemctl --user reset-failed

ssh -L 8001:127.0.0.1:8001 6b-pub
systemctl --user status code-server@8001
systemctl --user start code-server@8001.service
http://localhost:8001

systemctl --user start code@8010.service
http://localhost:8010

https://git.inclyc.cn/simd-team/swllvm-13.0.0-vect0919
```

## 反向代理
```bash
# Host
ssh -NR 7897:localhost:7897 cnic

# Server
export https_proxy=http://127.0.0.1:7897 http_proxy=http://127.0.0.1:7897 all_proxy=socks5://127.0.0.1:7897
```