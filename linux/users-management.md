# Users and groups management

Multiple commands to add / remove users and groups.

- Add a system user (without creating home folder)
```console
useradd -r <user>
```
- Prevent user login (lock)
```console
usermod -L <user>
```
- Add group (two options)
```
adduser --group <group>
addgroup <group>
```
- Add user to a group
```
adduser <user> <group>
```