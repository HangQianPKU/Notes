## 命令
香侬，计算节点到GPU节点的端口转发：
```
ssh -N -f -L 8888:192.168.88.211:8888 hangqian@192.168.88.211
```

本地到计算节点的端口转发：
```
ssh -N -f -L localhost:8888:localhost:8888 -p 11240 hangqian@s1.shannonyun.com
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIyNjQ1NTY3MiwtNDc3NDg3MjA0XX0=
-->