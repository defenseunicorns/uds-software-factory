# Software Factory Development Quickstart

## Apple Silicon Mac Users

If deploying on an Apple Silicon Mac you can use colima, an open source alternative to Docker Desktop, to deploy this bundle. Colima can be installed via Homebrew by running ```brew install colima```.

To set up an appropriately configured colima VM you can run the following command:

```bash
colima start --cpu 8 --memory 28 --disk 50 --vm-type vz  --vz-rosetta --arch aarch64 --profile uds
```
