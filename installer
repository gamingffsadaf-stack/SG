#!/bin/bash
set -e

######################################################################################
# Custom Pterodactyl Installer
# Maintained by Team : % SDF >..
# License: GPLv3
######################################################################################

LOG_PATH="/var/log/pterodactyl-installer.log"

#=======================#
# COLORS & TYPEWRITER   #
#=======================#
RED="\e[31m"
GREEN="\e[32m"
YELLOW="\e[33m"
BLUE="\e[34m"
MAGENTA="\e[35m"
CYAN="\e[36m"
WHITE="\e[37m"
BOLD="\e[1m"
RESET="\e[0m"

# টাইপওয়ার্টার ইফেক্ট ফাংশন
typewriter() {
    text="$1"
    delay="${2:-0.002}"
    for ((i=0; i<${#text}; i++)); do
        echo -ne "${text:$i:1}"
        sleep $delay
    done
    echo
}

#=======================#
# ASCII LOGO & HEADER   #
#=======================#
clear
typewriter "${CYAN}${BOLD}  _________                  __.__  __      ________    _____      _____  .___ _______    ________  ${RESET}"
typewriter "${CYAN}${BOLD} /   _____/____    ____     |__|__|/  |_   /  _____/   /  _  \\    /     \\ |   |\\      \\  /  _____/  ${RESET}"
typewriter "${CYAN}${BOLD} \\_____  \\\\__  \\\\  /    \\\\    |  |  \\\\   __\\\\ /   \\\\  ___  /  /_\\  \\\\  /  \\\\ /  \\\\|   |/   |   \\/   \\\\  ___  ${RESET}"
typewriter "${CYAN}${BOLD} /        \\/ __ \\\\|   |  \\\\   |  |  ||  |   \\\\    \\\\_\\  \\/    |    \\/    Y    \\\\   /    |    \\\\    \\\\_\\  \\\\ ${RESET}"
typewriter "${CYAN}${BOLD}/_______  (____  /___|  /\\__|  |__||__|    \\\\______  /\\____|__  /\\____|__  /___\\____|__  /\\______  / ${RESET}"
typewriter "${CYAN}${BOLD}        \\/     \\/     \\/\\______|                  \\/         \\/         \\/            \\/        \\/ ${RESET}"
typewriter "${YELLOW}-----------------------------------------------------------------------------------------------${RESET}"
typewriter "${GREEN}            ⚙️  Custom Pterodactyl Installer  |  Powered by @its_sadaf_officail${RESET}"
typewriter "${YELLOW}-----------------------------------------------------------------------------------------------${RESET}"
sleep 0.5

#=======================#
# CHECK CURL            #
#=======================#
if ! command -v curl >/dev/null 2>&1; then
  typewriter "${RED}* curl is required. Install with apt or yum/dnf.${RESET}"
  exit 1
fi

#=======================#
# DOWNLOAD LIB.SH       #
#=======================#
[ -f /tmp/lib.sh ] && rm -rf /tmp/lib.sh
curl -sSL "https://raw.githubusercontent.com/pterodactyl-installer/pterodactyl-installer/master/lib/lib.sh" -o /tmp/lib.sh
source /tmp/lib.sh

#=======================#
# EXECUTE FUNCTION      #
#=======================#
execute() {
    echo -e "\n* pterodactyl-installer $(date) \n" >>$LOG_PATH
    update_lib_source
    run_ui "$1" |& tee -a $LOG_PATH
}

#=======================#
# MENU OPTIONS          #
#=======================#
options=("Install Panel" "Install Wings" "Install Both")
actions=("panel" "wings" "panel;wings")

done=false
while [ "$done" = false ]; do
    typewriter "${BLUE}* Select an option:${RESET}"
    for i in "${!options[@]}"; do
        typewriter "[${YELLOW}$i${RESET}] ${options[$i]}"
    done

    read -p "* Enter choice [0-${#options[@]}]: " action
    if [[ ! $action =~ ^[0-9]+$ ]] || [ "$action" -ge "${#actions[@]}" ]; then
        typewriter "${RED}* Invalid choice!${RESET}"
        continue
    fi

    done=true
    IFS=";" read -r i1 i2 <<<"${actions[$action]}"
    execute "$i1"
    [ -n "$i2" ] && execute "$i2"
done

#=======================#
# CLEANUP               #
#=======================#
rm -rf /tmp/lib.sh
typewriter "${GREEN}* Installation finished!${RESET}"
