# ChromeOS Policy Editor

This is a Python program which is able to modify the device policies on ChromeOS, based on [lilac](https://github.com/MercuryWorkshop/lilac).

## Installation:
You must have Python 3 installed on Linux, with support for virtual environments and `pip`. 

To get started, clone this repository:
```
git clone https://github.com/ading2210/policyedit.git
cd policyedit
```

Run the following commands:
```
python3 -m venv .venv
source .venv/bin/activate
pip3 install -r requirements.txt
```

## Usage:
```
usage: main.py [-h] {view,patch} ...

positional arguments:
  {view,patch}
    view        Read the device settings without modifying anything.
    patch       Patch an existing device policy file.

options:
  -h, --help    show this help message and exit
```

### On Real ChromeOS:
1. Make sure you are in developer mode and have rootfs verification off.
2. Add `--disable-policy-key-verification` to the very bottom of `/etc/chrome_dev.conf`.
3. Edit `/etc/lsb-release` to change the value of `CHROMEOS_RELEASE_TRACK` to `testimage-channel`.
4. Run `main.py` with the correct arguments, specifying any policy files that are in `/var/lib/devicesettings/`.
5. Copy the public key to `/var/lib/devicesettings/owner.key`.
6. Overwrite the original policy files with the patched versions.

### On Linux-ChromiumOS:
1. Locate the user data directory (this defaults to `~/.config/chromium`) or explicitly set it with `--user-data-dir=DATA_DIR`.
2. Run `main.py`, specifying the policy file at `DATA_DIR/stub_device_policy`.
3. Overwrite the original policy with the patched version.
4. Copy the public key to `DATA_DIR/stub_owner.key`.

### JSON Format
For ease of use, you can specify `--policy-json` when patching policies to import multiple policies at once in readable JSON format. This format is exactly the same as the JSON format on `chrome://policy` (you can try it by using the *Export to JSON* or *Copy as JSON* buttons). Here's an example:
```
{
   "chromeMetadata": {
      "application": "Google Chrome",
      "platform": "14816.131.0 (Official Build) stable-channel kefka",
      "revision": "bcb0e65ad2f1e2bedb2d41e4cc15ec825ae3b7fd-refs/branch-heads/5060@{#1253}",
      "version": "103.0.5060.132 (Official Build) (64-bit)"
   },
   "chromePolicies": {
      "AdsSettingForIntrusiveAdsSites": {
         "level": "mandatory",
         "scope": "user",
         "source": "cloud",
         "value": 2
      },
      "AllowDeletingBrowserHistory": {
         "level": "mandatory",
         "scope": "user",
         "source": "cloud",
         "value": false
      },
      "AllowDinosaurEasterEgg": {
         "level": "mandatory",
         "scope": "user",
         "source": "sourceEnterpriseDefault",
         "value": false
      },
   }
}
```

## Copyright:

This repository is licensed under the GNU GPL v3.

[License](LICENSE)
