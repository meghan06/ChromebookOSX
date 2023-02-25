# Security Policy

## Change PlatformInfo

ChromebookOSX does not provide EFI folders due to the risk it creates where multiple people would share the same serial numbers defined in `config.plist`. It is highly recommended to generate your own serial numbers to protect your device from potential privacy leaks.

- Download [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) and select option 1 to download MacSerial and next option 3. Then, type `MacBookAir8,1` (reccomended) or `MacBook10,1` (Only if you run into issues.) to generate new serials.

### For Clover Users
- Note: ZERO (0) support for Clover users.
- Go to `/EFI/CLOVER/` and open `config.plist`
- In `SMBIOS`, add `BoardSerialNumber` with value `Board Serial` you got from [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS), add `SerialNumber` with value `Serial` you got from [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS), and add `SmUUID` with value `SmUUID` you got from [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- Restart

### For OC Users
- Go to `/EFI/OC/` and open `config.plist`
- In `PlatformInfo - Generic`, change `MLB` value to `Board Serial` you got from [GenSMBIOS], change `SystemSerialNumber` value to `Serial` you got from [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS), and change `SystemUUID` value to `SmUUID` you got from [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- Restart


## UEFI Security (OC Only)

- Please read [Security and FileVault | OpenCore Post-Install](https://dortania.github.io/OpenCore-Post-Install/universal/security.html) and [OpenCore Configuration](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf) **UEFI Secure Boot** section. It includes security instructions about Apple Secure Boot, secure DMG loading, APFS drivers, signing all third-party drivers, enabling BIOS Secure Boot function, and more.


## Reporting a Vulnerability

Report security vulnerabilities in the [Issue Page](https://github.com/meghan06/ChromebookOSX/issues) if you find any.

We will respond to security vulnerabilities as soon as possible. Please note that releases of this repository are in general collections of hackintosh kernel extensions and bootloaders. We may not be capable of fixing code-level security vulnerabilities unless the authors of responsible kernel extensions/bootloaders assist us in fixing them.


## Reference

Thanks to [dortania](https://github.com/dortania) for providing [Fixing iMessage and other services with OpenCore](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html).
