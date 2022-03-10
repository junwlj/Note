# Docker 容器与宿主机之间的文件传输

```dockerfile
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH |-
```

```dockerfile
docker cp [OPTIONS] SRC_PATH |- CONTAINER:DEST_PATH
```
- OPTIONS 说明：
  - -L: 保持原目标中的链接一直

**实例：**
```dockerfile
# 将主机/data/file.txt 拷贝到容器8cd1a9d73685的~/demo下
docker cp /data/file.txt 8cd1a9d73685:~/demo/
# 将主机/data/file.txt 拷贝到容器8cd1a9d73685的~/demo下并重命名为test
docker cp /data/file.txt 8cd1a9d73685:~/test
```
```dockerfile
# 将容器8cd1a9d73685的~/demo/目录拷贝到主机的/data下
docker cp 8cd1a9d73685:~/demo /data/
```
