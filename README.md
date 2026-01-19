# PXE-Helper-Scripts

bash -c "$(curl -fsSL https://raw.githubusercontent.com/lenne0815/PXE-Helper-Scripts/refs/heads/main/Iventoy.sh)"

https://github.com/ventoy/PXE/releases/download/v1.0.21/iventoy-1.0.21-linux-free.tar.gz

mkdir -p /opt/iventoy/{data,iso}
curl -fsSL "https://github.com/ventoy/PXE/releases/download/v1.0.21/iventoy-1.0.21-linux-free.tar.gz" -o "iventoy-${RELEASE}-linux-free.tar.gz"
tar -C /tmp -xzf iventoy*.tar.gz
mv /tmp/iventoy*/* /opt/iventoy/
rm -rf iventoy*.tar.gz
