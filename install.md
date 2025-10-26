
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

sudo apt install swaybg grim slurp wl-clipboard swaylock swayidle kanshi upower

sudo apt install linux-headers-$(uname -r)

sudo apt install fonts-jetbrains-mono

# not completed setup
sudo apt install gnome-keyring seahorse
