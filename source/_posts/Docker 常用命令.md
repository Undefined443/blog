```sh
docker inspect <container> | jq '.[].Config'  # 查看 container 信息
docker inspect <container> | jq '.[].Config.Cmd'  # 查看 container 的 Command 信息
docker inspect <container> | jq '.[].Mounts'  # 查看 container 的挂载情况
```