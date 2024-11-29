```
yay -Syu kata-runtime-bin linux-kata-bin kata-containers-image-bin qemu-system-x86 virtiofsd
sudo mkdir -p /opt/kata/share/defaults/kata-containers/configuration.toml
sudo cp /usr/share/defaults/kata-containers/configuration.toml /opt/kata/share/defaults/kata-containers
```

then edit /opt/kata/share/defaults/kata-containers/configuration.toml:
```
16c16,17
< kernel = "/usr/share/kata-containers/vmlinux.container"
---
> #kernel = "/usr/share/kata-containers/vmlinux.container"
> kernel = "/usr/share/kata-containers/vmlinux-5.19.2-100"
```
after that:
```
sudo ctr run --rm -t --runtime io.containerd.kata.v2 docker.io/library/archlinux:latest example-container-name bash
```
worked for me

but not the rest, so screw that.
so remove everyting but:

`yay -Syu qemu-system-x86 virtiofsd`

instead do what https://habr.com/en/articles/840538/ says

(or use https://github.com/kata-containers/kata-containers/blob/main/utils/README.md#kata-manager)

then it works - at least the docker part...
