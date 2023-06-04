## This file is a vauge guide for flashing custom coreboot on linux. dont follow this, you might end up with a paperweight



1. `git clone https://github.com/mrchromebox/coreboot --depth 1`

2. Debian based distros: `sudo apt-get install -y bison build-essential curl flex git gnat libncurses5-dev m4 zlib1g-dev`

Arch based distros: `sudo pacman -S base-devel curl git gcc-ada ncurses zlib`

Redhat based distros: `sudo dnf install git make gcc-gnat flex bison xz bzip2 gcc g++ ncurses-devel wget zlib-devel patch`

3. `make crossgcc-i386 CPUS=$(nproc)`

4. make your changes now.

5. install `libuuid-devel`

6. install `nasm`

6. `./build-uefi.sh leona`

7. download https://elly.rocks/tmp/coreboot-development/flashrom-alderlake chmod +x this

8. using that flashrom, 
`sudo ./flashrom-alderlake -p internal -w coreboot_tiano-leona-mrchromebox_yearmonthdate.rom `

9. if it said sucess on all checks, reboot and ez
