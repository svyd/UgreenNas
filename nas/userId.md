A UID (user identifier) is a number assigned by the UGREEN NAS UGOS operating system to each user on the system. This number is used to identify the user to the system and to determine which system resources the user can access. Groups in UGREEN NAS UGOS are defined by GIDs (group IDs). These values often dictate the user/group that the Docker container will be running as. So if the Docker container accesses a specific drive share, with specific permissions, then the PUID and PGID values will specify the user/group that will be used by the Docker container.

To get UID (userID) and GID (groupID):

```bas
id
```

Expected output:
```bash
uid=1000(ash) gid=1000(ash)
```
