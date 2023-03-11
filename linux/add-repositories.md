# Add repositories to Ubuntu Server (_without apt-key_)


## Example using Mono project [🔗](https://www.mono-project.com)

The current problem is that `apt-key` to manage keys seems to be deprecated.

The idea is to add a gpg key to `/etc/apt/keyrings` and then modify the source list of the repository to use that new key.

Some projects like Mono (at the time of this writting), don't have the new form of importing keys.

1. Make sure you have gnupg and ca-certificates installed.
```bash
sudo apt install gnupg ca-certificates
```

2. Install the key using gpg

old apt-key command
```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
```
new command
```bash
curl -fsSL http://keyserver.ubuntu.com/pks/lookup\?op\=get\&search\=0x3fa7e0328081bff6a14da29aa6a19b38d3d831ef | sudo gpg --dearmor -o /etc/apt/keyrings/mono.gpg
```
Note: we are bypassing the apt-key add getting the GPG key directly from the ubuntu keyserver with curl and passing it to gpg.

This would add a mono.gpg file to the keyrings.

3. Then to add the source to apt we need to modify the command and add the signed-by directive
```bash
echo "deb [signed-by=/etc/apt/keyrings/mono.gpg] https://download.mono-project.com/repo/ubuntu stable-focal main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list
```

That's it, now `sudo apt update` should also fetch from the new repositories.