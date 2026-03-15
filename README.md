原版只支持X86，增加对ARM架构的支持，大家放心食用，在OpenWrt上配合ddns-go可以搭建基于家庭宽带公网IPv6的自建Tailcale的derper节点。

一健部署代码
```bash
docker run --name derper --restart always -p 34567:34567 -p 3478:3478/udp -v /var/run/tailscale/tailscaled.sock:/var/run/tailscale/tailscaled.sock -e DERP_ADDR=:34567 -e DERP_CERTS=/app/certs -e DERP_VERIFY_CLIENTS=true shyboy0415/ip_derper:latest
```
或者
```bash
docker run --name derper --restart always -p 34567:34567 -p 3478:3478/udp -v /var/run/tailscale/tailscaled.sock:/var/run/tailscale/tailscaled.sock -e DERP_ADDR=:34567 -e DERP_CERTS=/app/certs -e DERP_VERIFY_CLIENTS=true ghcr.io/shyboy0415/ip_derper:latest
```
Docker compose 文件编写
```bash
services:
  derper:
    image: shyboy0415/ip_derper:latest
    container_name: derper
    restart: always
    ports:
      - "34567:34567" # 这里的12345请改成你自己想要的10000以上的高位端口
      - "3478:3478/udp" # 3478 为stun端口，如果不冲突请勿修改
    volumes:
      - /var/run/tailscale/tailscaled.sock:/var/run/tailscale/tailscaled.sock # 映射本地 tailscale 客户端验证连接，用来验证是否被偷
    environment:
      - DERP_ADDR=:34567 # 此处需要与上面的同步修改
      - DERP_CERTS=/app/certs
      - DERP_VERIFY_CLIENTS=true # 启动客户端验证，这是防偷的最重要的参数
```
