# ha-mikrotik (ALPHA)
High availability code for Mikrotik routers

# Warning
Please do not test this on production routers. This should be tested in a lab setup with complete out of band serial access.
This was developed on the CCR1009-8g-1s-1s+ and is in use in our production environment. Proceed at your own risk, the code can potentionally wipe out 
all of your configuration and files on your device.

# Hardware originally developed for
Pair of CCR1009-8g-1s-1s+
RouterOS v6.33.5
Routerboard firmware 3.27
Bootstrapped from complete erased routers and then config built up once HA installed.

# Installing
1. Source a pair of matching routers, ideally CCR1009-8g-1s-1s+.
2. Install RouterOS v6.33.5 and make sure the Routerboard firmware is up date.
3. Ensure you have serial connections to both devices.
4. Reset both routers using the command:
`/file remove [find]; /system reset-configuration keep-users=no no-defaults=yes skip-backup=yes`
5. Connect an ethernet cable between ether8 and ether8.
6. On one router, configure a basic network interface on ether1 with an IP address of your choosing. Just enough to be able to copy a file.
7. Upload HA_init.rsc and import it:
`/import HA_init.rsc`
8. Install HA (note to replace the fields of macA, macB, and password. I suggest a sha1 hex hash for the password.
`$HAInstall interface="ether8" macA="[MAC_OF_A_ETHER8]" macB="[MAC_OF_B_ETHER_8]" password="[A RANDOM PASSWORD OF YOUR CHOOSING]"`
9. Follow the instructions given by $HAInstall to bootstrap the secondary. I use the MAC telnet that is suggested at the top but any other method is sufficient.
10. Once router B is bootstrapped, it will reboot itself into a basic networking mode. It needs to be pushed the current configuration.
`$HASyncStandby`
11. B will now reboot and when it returns, it should be in standby mode. A should be the active router. You can now reboot A and B will takeover.
