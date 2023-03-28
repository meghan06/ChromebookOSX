# This file is a vauge guide for flashing custom coreboot on linux. dont follow this, you might end up with a paperweight



git clone https://github.com/mrchromebox/coreboot --depth 1

Debian based distros: `sudo apt-get install -y bison build-essential curl flex git gnat libncurses5-dev m4 zlib1g-dev`

Arch based distros: `sudo pacman -S base-devel curl git gcc-ada ncurses zlib`

Redhat based distros: `sudo dnf install git make gcc-gnat flex bison xz bzip2 gcc g++ ncurses-devel wget zlib-devel patch`

`make crossgcc-i386 CPUS=$(nproc)`

make changes now 

install `libuuid-devel`

install `nasm`

`./build-uefi.sh leona`

download https://elly.rocks/tmp/coreboot-development/flashrom-alderlake chmod +x this

using that flashrom, 
`sudo ./flashrom-alderlake -p internal -w coreboot_tiano-leona-mrchromebox_yearmonthdate.rom `

if it said sucess on all checks, reboot and ez
