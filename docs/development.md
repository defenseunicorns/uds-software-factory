# Software Factory Development Quickstart

## Apple Silicon Mac Users

If deploying on an Apple Silicon Mac you can use colima, an open source alternative to Docker Desktop, to deploy this bundle. Colima can be installed via Homebrew by running ```brew install colima```.

To set up an appropriately configured colima VM you can run the following command:

```bash
colima start --cpu 9 --memory 28 --disk 50 --vm-type vz  --vz-rosetta --arch aarch64 --profile uds
```

Additionally, some settings need to be configured on the host to facilitate a successful deployment of Sonarqube:

```bash
colima ssh --profile uds
sudo sysctl -w vm.max_map_count=1524288
sudo sysctl -w fs.file-max=1000000
exit
```

## Linux users

Depending on your linux distrobution and how it is configured you may need to run the following steps to be able to properly deploy SWF and/or UDS Core:

```bash
sudo sysctl -w vm.max_map_count=1524288
sudo sysctl -w fs.file-max=1000000
ulimit -n 1000000
ulimit -u 8192
sudo sysctl --load
sudo swapoff -a
sudo sysctl fs.inotify.max_user_instances=8192
sudo sysctl -p
```
