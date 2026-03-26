# Create keys
```
openssl req -new -x509 -newkey rsa:2048 -keyout MOK_dh470.priv -outform DER -out MOK_dh470.der -nodes -days 36500 -subj "/CN=prashant-dh470/"
```

# Change `mok_enroll_key.sh` a bit to include your own der file

# write, build and sign the sample driver file

```
# After write and build sign the file as shown below
/lib/modules/$(uname -r)/build/scripts/sign-file sha256 MOK_dh470.priv MOK_dh470.der simple_driver.ko

```

# Ensure that secure boot is disabled then run `mok_run_enroll_key.sh`.
 - This is very crucial step. 
 - `mok_run_enroll_key.sh` accepts one argument which is boot device of the system
```
 mok_enroll_key.sh <boot device like /dev/sda>`
 reboot

```

# When UEFI firmware runs after rebooting it also executes `MokEnrollKey.efi` which puts the key into MOKList

# Once `MokEnrollKey.efi` finishes UEFI firmware/grub provides you an option to enter into UEFI settings

- Enter the setting and enable secure boot. save & exit

- Let the system boot normally.

# From the command prompt. Type following commands to see if secure boot is enabled and also to see your enrolled keys
```
mokutil --sb-state
mokutil --list-enrolled

```

# Try to load the compiled driver
```
sudo insmod simple_driver.ko
```

