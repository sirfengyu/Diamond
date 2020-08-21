1.进入system视图，创建PFC模板
dcb pfc server
priority 4
priority 4 deadlock-detect time 10 deadlock-recovery time 10
priority 4 turn-off threshold 5
commit
quit
2.对端口进行配置，以下行端口interface 100ge 1/0/1举例:
interface 100ge 1/0/1
undo flow-control
dcb pfc enable server mode manual
dcb pfc buffer 4 xoff dynamic 6
trust dscp
commit
quit