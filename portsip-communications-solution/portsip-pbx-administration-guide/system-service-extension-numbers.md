# System Service Extension Numbers

### Reserved System Extension Numbers

PortSIP PBX assigns default extension numbers to built-in system services. These numbers are reserved for system use and cannot be assigned to user extensions.

#### Default System Extensions

| Service                   | Default Extension | Where to Change It                                  |
| ------------------------- | ----------------: | --------------------------------------------------- |
| Call Announcement Service |               555 | **Advanced Services > Automatic Callback**          |
| Call Park Service         |               666 | **Advanced Services > Call Park > Global Settings** |
| Voicemail Service         |               777 | **Advanced > Voicemail**                            |
| Call Monitor Service      |               888 | **Contact Center > Monitor Service**                |

#### Important Notes

* These extension numbers are reserved by default and are unavailable when creating user extensions.
* If you change a system extension number, make sure the new number does not conflict with any existing user extension, ring group, call queue, IVR, or other system service.
* After changing a system extension number, inform users and administrators who rely on the previous number.
