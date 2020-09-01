root@atlas03:/home/HwHiAiUser/apulis_platform/src/ClusterBootstrap# ufw status
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
2049/tcp                   DENY        Anywhere
9300/tcp                   DENY        Anywhere
3399/tcp                   ALLOW       Anywhere
3399/udp                   ALLOW       Anywhere
22/udp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
80/udp                     ALLOW       Anywhere
6443/udp                   ALLOW       Anywhere
6443/tcp                   ALLOW       Anywhere
111/tcp                    ALLOW       Anywhere
111/udp                    ALLOW       Anywhere
2049/udp                   ALLOW       Anywhere
13025/tcp                  DENY        Anywhere
13025/udp                  DENY        Anywhere
1110/udp                   ALLOW       Anywhere
1110/tcp                   ALLOW       Anywhere
2049                       DENY        Anywhere
111                        ALLOW       Anywhere
13025                      ALLOW       Anywhere
Anywhere                   ALLOW       192.168.100.23
Anywhere                   ALLOW       192.168.100.25
Anywhere                   ALLOW       192.168.100.0/24
22/tcp (v6)                ALLOW       Anywhere (v6)
2049/tcp (v6)              DENY        Anywhere (v6)
9300/tcp (v6)              DENY        Anywhere (v6)
3399/tcp (v6)              ALLOW       Anywhere (v6)
3399/udp (v6)              ALLOW       Anywhere (v6)
22/udp (v6)                ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
80/udp (v6)                ALLOW       Anywhere (v6)
6443/udp (v6)              ALLOW       Anywhere (v6)
6443/tcp (v6)              ALLOW       Anywhere (v6)
111/tcp (v6)               ALLOW       Anywhere (v6)
111/udp (v6)               ALLOW       Anywhere (v6)
2049/udp (v6)              ALLOW       Anywhere (v6)
13025/tcp (v6)             DENY        Anywhere (v6)
13025/udp (v6)             DENY        Anywhere (v6)
1110/udp (v6)              ALLOW       Anywhere (v6)
1110/tcp (v6)              ALLOW       Anywhere (v6)
2049 (v6)                  DENY        Anywhere (v6)
111 (v6)                   ALLOW       Anywhere (v6)
13025 (v6)                 ALLOW       Anywhere (v6)


# master 节点要允许 22
ufw allow 22




apulis#2019#wednesday

http://121.37.54.27/

DLWSCluster-882aca27-2801-4966-9c72-4a91af1fdf7e 
IfNotPresent