# realm-helper
An easy-to-use Realm port forwarding tool. 

To use this tool, simply run:

```bash
curl -L https://raw.githubusercontent.com/RomanovCaesar/realm-helper/main/realm_helper.sh -o realm_helper.sh && chmod +x realm_helper.sh && sudo ./realm_helper.sh
```

Then you can access this tool by entering:

```bash
realm-helper
```

This script has the following functions:

1. After the script runs, a menu will be displayed. Entering numbers will allow you to perform various operations.

2. The menu options include: download and install/update Realm, add forwarding rules, view existing forwarding rules, enable/disable automatic startup, start/stop/restart the service, uninstall Realm, and exit the script.

3. The script needs to detect whether Realm is installed and running. After running the script, the first line will display the Realm installation status, the second line will display the Realm running status, and the third line onwards will display the main menu for selecting various operations.

4. When choosing to download and install or update Realm from GitHub, it will first check if Realm is installed. If Realm is not installed, it will download and install it from https://github.com/zhboner/realm/releases, depending on the host system and architecture (only Debian and Alpine systems are supported, and only x86 and aarch64 architectures are supported). If already installed, the system checks for a new version. If a new version is available, it prompts you whether to install or exit the current operation and return to the main menu. If no new version is available, it indicates that there is no new version of realm. Pressing any key returns you to the main menu.

5. When adding a forwarding rule, if realm.toml does not exist in the /root directory, it creates the file first and then writes the forwarding configuration. If realm.toml already exists, the new forwarding configuration is added after the existing content in that file. The realm configuration file uses the following template (the two enpoints here are just for demonstrating; these two example configurations will not be created when the script runs):

```toml
[log]
level = "warn"

[dns]
# ipv4_then_ipv6, ipv6_then_ipv4, ipv4_only, ipv6_only
# mode = "ipv6_then_ipv4"

[network]
no_tcp = false
use_udp = true

[[endpoints]]
listen = "0.0.0.0:123"
remote = "test.com:456"

[[endpoints]]
listen = "0.0.0.0:111"
remote = "8.8.8.8:222"

```

6. When selecting to add forwarding configuration, it will first ask for the listening IP. If not filled in, 0.0.0.0 will be used. Then, it queries for the listening port (required; otherwise, the script will exit). This port is compared with existing rules; if a duplicate is found, the script also exits. Next, it queries for the forwarding target IP (required; otherwise, the script will exit). Finally, it queries for the forwarding target port (required; otherwise, the script will exit). Once all information is obtained, the configuration is written to the configuration file.

7. When selecting to view existing forwarding rules, it reads realm.toml and outputs it line by line in the format: Listening IP: Listening Port → Forwarding Target IP: Forwarding Target Port.

8. After each operation is completed, a prompt is given to press any key to return to the main menu.
