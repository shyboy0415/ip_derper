原版只支持X86，增加对ARM架构的支持，大家放心食用。
一健部署代码
`docker run --name derper --restart always -p 34567:34567 -p 3478:3478/udp -v /var/run/tailscale/tailscaled.sock:/var/run/tailscale/tailscaled.sock -e DERP_ADDR=:34567 -e DERP_CERTS=/app/certs -e DERP_VERIFY_CLIENTS=true shyboy0415/ip_derper:latest`
