Wanna run some vms locally because it's fun and segregating at the same time?

Kata containers - my take - is small images that are spawned by some vm provider
(qemu-kvm in this case) that then consumes a container and sets that up inside the vm,
and puts you there like a regular container.

Not sure how much container shenaningas works out of the box (networks, iohardware, volumes)

Not sure how much vm shenaningas works out of the box (networks, iohardware, volumes)

# TLDR (hopefully up to date)
`yay -Syu qemu-system-x86 virtiofsd`
instead do what https://habr.com/en/articles/840538/ says

(or use https://github.com/kata-containers/kata-containers/blob/main/utils/README.md#kata-manager)

then hopefully both
```
sudo ctr run --rm -t --runtime io.containerd.kata.v2 docker.io/library/archlinux:latest example-container-name bash
```
and
```
sudo docker run --runtime io.containerd.run.kata.v2 -it ubuntu bash
```
works

# Travel the same footsteps?
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
