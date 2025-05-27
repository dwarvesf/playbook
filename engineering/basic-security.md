---
title: null
description: null
date: null
redirect:
  - /1P3kIw
---

# Basic security

## Devices

Keeping laptops and phones secure is vitally important. We're a software company and many of us have access to secure systems (both our own and our customers).

The following guidelines apply to how we physically secure our laptops and mobile devices that may contain customer or user data.

### General

- [ ] Make a note of serial numbers, model information
- [ ] Two Factor Authentication and strong passwords
- [ ] Keep your operating system and applications up to date
- [ ] Lock your device when you are away from it.
- [ ] Don't leave your devices unattended in an unsecured area.
- [ ] Install a device tracking and remote data wipe tool such as Prey.

### Desktop

- [ ] [Encrypt your hard drive](https://support.apple.com/en-gb/HT204837)
- [ ] Mac users must add [a firmware password](https://support.apple.com/en-gb/HT204455)
- [ ] Non Mac users must add a BIOS password
- [ ] [Disable automatic login for OSX](https://www.intego.com/mac-security-blog/mac-security-tip-disable-automatic-login/)
- [ ] [Auto logout after five minutes inactivity](https://support.apple.com/en-gb/HT201988)
- [ ] [Require password after screensaver or sleep](https://support.apple.com/en-gb/HT204379)
- [ ] Only work from company laptops or follow BYOD policy
- [ ] [Install iCloud/Find My Mac](https://www.icloud.com/)

### Mobile

We all use personal mobile devices, so your options are either not to add any company accounts to your phone (this includes Slack, Gmail etc), or to follow the checklist below.

- [ ] Ideally disable finger print login (or at least have TouchID on).
- [ ] [Create a 6 digit passcode (or better)](http://www.cnet.com/uk/how-to/secure-your-ios-device-with-a-six-digit-passcode-on-ios-9/)
- [ ] [Turn on auto lock after 5 mins](http://www.imore.com/how-change-auto-lock-time-your-iphone-or-ipad)

### Using passwords

- [ ] Use a unique password for every account you create.
- [ ] Use a tool like [pwgen](https://github.com/jbernard/pwgen) or [1password](https://1password.com) to generate random passwords.
- [ ] Use a tool like GnuPG to encrypt passwords if you need to share them with somebody.

### Encryption

- [ ] Use a PGP signature in an email if you want somebody to trust that you wrote it.
- [ ] Use PGP to check email signatures if you want to know who wrote it.
- [ ] Use PGP to encrypt emails if you want to be sure nobody but the recipient is reading it.
- [ ] Use ultimate trust for your own keys.
- [ ] Use full trust for keys you have verified in person or via a secure video chat.
- [ ] Don't share your private key with anyone, including services like Keybase.
- [ ] Keep at least one backup of your private key and revocation certificate in a secure location, such as a thumb drive.

## Handling vulnerabilities

- [ ] Depending on what you are making, limit access to your user databases.
- [ ] In case of a hack or data breach, check previous logs for data access, ask people to change passwords. You might require an audit by external agencies depending on where you are incorporated.

## Security report

When someone finds a possible security issue in our software, we encourage them to report it to our <security@d.foundation> email address.

When an email comes in through this channel, reply quickly with confirmation (and CC <security@d.foundation> so others know that it has been handled) and the information for our PGP key, which is located at <https://d.foundation/security>.
