import subprocess
import argparse
import re


def get_mac_address(interface):
    result = subprocess.check_output(["ifconfig", interface]).decode('utf-8')
    mac_address_search = re.search("\w\w:\w\w:\w\w:\w\w:\w\w:\w\w", result)
    if mac_address_search:
        mac = mac_address_search.group(0)
        return mac
    else:
        print(f"Oops! Could not retrieve the MAC address for interface: {interface}")


def mac_changer(interface, new_mac):
    current_mac = get_mac_address(interface)
    print(f"Hey, Welcome to the MAC Changer Program Written By Deepanshu Deep.\n"
          f"\nYour current MAC address for {interface} is {current_mac}")

    subprocess.call(["ifconfig", interface, "down"])
    subprocess.call(["ifconfig", interface, "hw", "ether", new_mac], stderr=subprocess.DEVNULL)
    subprocess.call(["ifconfig", interface, "up"])
    print_result(interface, new_mac)


def print_result(interface, new_mac):
    print(f"\n[+] Trying to change the MAC address for interface {interface} to new MAC: {new_mac}\n")
    changed_mac = get_mac_address(interface)
    if new_mac == changed_mac:
        print(f"[+] Success! The MAC address has been changed.\n"
              f"\n[+] Checking the newly changed MAC address using the 'ifconfig' command...\n"
              f"\nGreat, it worked! The MAC address for {interface} is now {changed_mac}."
              f"\nRun 'ifconfig {interface}' to double-check.")
    else:
        print(f"We could not change the MAC address for interface {interface} to {new_mac}.")


def getting_commands():
    parser = argparse.ArgumentParser(description="Welcome to the MAC changing program written by Deepanshu Deep.",
                                     epilog="""If the interface is eth0 and the MAC is 00:11:22:33:44:55,
then your command will be: -i eth0 -m 00:11:22:33:44:55
or --interface eth0 --mac 00:11:22:33:44:55.

This will change the MAC address for eth0 to 00:11:22:33:44:55.
""",
                                     formatter_class=argparse.RawDescriptionHelpFormatter)
    parser.add_argument("-i", "--interface", dest="interface", help="Interface to change its MAC address.")
    parser.add_argument("-m", "--mac", dest="new_mac", help="The new MAC address.")
    user_input = parser.parse_args()

    if not user_input.interface:
        parser.error("[-] Please specify the interface. Use --help for more information.")
    elif not user_input.new_mac:
        parser.error("[-] Please specify the new MAC address. Use --help for more information.")
    return user_input

user_input = getting_commands()

mac_changer(user_input.interface, user_input.new_mac)
