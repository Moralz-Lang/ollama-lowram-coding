📄 setup/install_ollama.sh
#!/bin/bash
sudo pacman -S ollama
sudo systemctl enable --now ollama

📄 setup/zram_setup.sh
#!/bin/bash
sudo pacman -S zram-generator
sudo systemctl daemon-reexec
sudo systemctl start systemd-zram-setup@zram0
swapon --show
