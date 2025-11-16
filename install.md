
su -
apt install sudo
usermod -aG sudo touko
exit

# Logout & login again

sudo apt install vim git libinput-dev waylandpp-dev wayland-protocols wayland-utils libwlroots-0.18-dev pkg-config build-essential

# git clone dwl
# git checkout 0.7 # the version for wlroots 0.18

sudo apt install wmenu foot foot-themes

sudo apt install libpango1.0-dev libcairo2-dev 

sudo apt install libfcft-dev libpixman-1-dev

sudo apt install pipewire wireplumber pavucontro  bluetooth pipewire-pulse

systemctl --user restart wireplumber pipewire pipewire-pulse

sudo apt install libspa-0.2-bluetooth

sudo apt install distrobox

sudo apt greetd tuigreet hwinfo wdisplays

git clone https://github.com/warbacon/zunder-zsh
cd zunder-zsh
./install.sh
cd ..

sudo apt install swaybg grim slurp wl-clipboard swaylock swayidle kanshi upower gh
sudo apt install thunar gvfs-backends  gvfs-fuse

sudo apt install linux-headers-$(uname -r) rfkill

sudo apt install fonts-jetbrains-mono fonts-material-design-icons-iconfont 

# added admin group, modified sudoers to allow poweroff/reboot/halt
sudo groupadd admin
sudo usermod -aG admin touko 

# Modified files
# - /etc/sudoers
# - /etc/greetd/config.toml
# - .config dir
# - scripts copied to /usr/local/bin/
# - /etc/default/grub
# - took permissions away from a file that had a name something like /etc/grub.d/05-debian-theme
# - /etc/polkit-1/rules.d/10-udisks2-mount.rules # Polkit for mounting

sudo apt install gnome-keyring seahorse
# Created login password keyring with seahorse

sudo apt install xwayland
sudo apt install brightnessctl vbetool

# Nvidia container toolkit
# https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
sudo apt install curl gnupg2

curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt update

export NVIDIA_CONTAINER_TOOLKIT_VERSION=1.18.0-1
  sudo apt-get install -y \
      nvidia-container-toolkit=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      nvidia-container-toolkit-base=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      libnvidia-container-tools=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      libnvidia-container1=${NVIDIA_CONTAINER_TOOLKIT_VERSION}

# Required for nvidia drivers
apt install libnvoptix1
apt install libvulkan1 vulkan-tools


# Podman from source

## Deps
sudo apt-get install \
  btrfs-progs \
  gcc \
  git \
  golang-go \
  go-md2man \
  iptables \
  libassuan-dev \
  libbtrfs-dev \
  libc6-dev \
  libdevmapper-dev \
  libglib2.0-dev \
  libgpgme-dev \
  libgpg-error-dev \
  libprotobuf-dev \
  libprotobuf-c-dev \
  libseccomp-dev \
  libselinux1-dev \
  libsystemd-dev \
  make \
  netavark \
  passt \
  pkg-config \
  runc \
  uidmap


# Get sourcecode
git clone https://github.com/containers/podman/
cd podman
make BUILDTAGS="selinux seccomp" PREFIX=/usr
sudo env PATH=$PATH make install PREFIX=/usr


# Install docker
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

sudo tee /etc/apt/sources.list.d/docker.sources << 
EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update  
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx docker-compose-plugin

# Docker postinstall
sudo groupadd docker
sudo usermod -aG docker $USERA
newgrp docker


# For containers, might be used
sudo bash -c 'cat > /etc/cdi/vulkan.yaml <<EOF
# This CDI specification shares the Vulkan ICD configuration with containers.
cdiVersion: "0.5.0"
kind: "nvidia.com/device"
devices:
  - name: "all"
    containerEdits:
      mounts:
        - host: /usr/share/vulkan/icd.d/nvidia_icd.json
          container: /etc/vulkan/icd.d/nvidia_icd.json
          options:
            - "ro"
            - "nosuid"
            - "nodev"
EOF'
