## Iventoy PXE Server TTeck Helper Script Backup

### Step by Step Installation:

> Install the basic LXC Container:

bash -c "$(curl -fsSL https://raw.githubusercontent.com/lenne0815/PXE-Helper-Scripts/refs/heads/main/Iventoy.sh)"

> Pull latest Iventoy release:

https://github.com/ventoy/PXE/releases/download/v1.0.21/iventoy-1.0.21-linux-free.tar.gz

mkdir -p /opt/iventoy/{data,iso}

curl -fsSL "https://github.com/ventoy/PXE/releases/download/v1.0.21/iventoy-1.0.21-linux-free.tar.gz" -o "iventoy-${RELEASE}-linux-free.tar.gz"

tar -C /tmp -xzf iventoy*.tar.gz

mv /tmp/iventoy*/* /opt/iventoy/

rm -rf iventoy*.tar.gz

> Run Installation Script to get the Service up and running @ port 26000

1. Define helper functions (if they aren't already in your system)
msg_info() { echo -e "\e[34m[INFO]\e[0m $1"; }
msg_ok() { echo -e "\e[32m[OK]\e[0m $1"; }

2. Create the service file (Requires sudo)
sudo bash -c 'cat <<EOF >/etc/systemd/system/iventoy.service
[Unit]
Description=iVentoy PXE Booter
Documentation=https://www.iventoy.com
Wants=network-online.target

[Service]
Type=forking
Environment=IVENTOY_API_ALL=1
Environment=IVENTOY_AUTO_RUN=1
Environment=LIBRARY_PATH=/opt/iventoy/lib/lin64
Environment=LD_LIBRARY_PATH=/opt/iventoy/lib/lin64
ExecStart=sh ./iventoy.sh -R start
WorkingDirectory=/opt/iventoy
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF'

3. Enable and start the service
msg_info "Creating Service"
sudo systemctl daemon-reload
sudo systemctl enable -q --now iventoy
msg_ok "Created Service"

> Configure SSH for Access

nano /etc/ssh/sshd_config

Modify the following lines (remove the # comment symbol if present):

To allow root login: Change PermitRootLogin to yes.

To allow passwords: Ensure PasswordAuthentication is set to yes.

Save and exit (Ctrl+O, Enter, Ctrl+X).

> Restart the service to apply changes:

systemctl restart ssh
