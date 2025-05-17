Following these steps will install waydroid but I dont have time to add any Bangla Instruction 
Feel free to fork it.

- [ ] Add bangla instruction

```bash
sudo apt update
sudo apt install curl ca-certificates
curl https://repo.waydro.id | sudo bash
sudo systemctl enable --now waydroid-container
sudo waydroid init -s GAPPS
waydroid show-full-ui
```
* Fix firewall 
* Add google cirtification 