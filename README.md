# This is the issue tracker for jotta-cli 

- (http://docs.jottacloud.com/jottacloud-command-line-tool)

Please create issues for bugs or feature requests in this repository.

## Stable vs. unstable 

### jotta-cli stable (NB still in beta!)

Jotta-cli is available on several platforms. There is more information about this [here](http://docs.jottacloud.com/jottacloud-command-line-tool) including instructions to install and update jotta-cli. Those instructions are all for the stable branch of jotta-cli.

Builds for stable https://repo.jotta.us/archives/

*Note: Jotta-cli stable is still considered a beta release for now.*

### jotta-cli unstable
As of 12th October 2020, in preparation for releasing sync for jotta-cli, we also have an unstable branch of jotta-cli for more adventurous users. We will push changes and new features to this branch more frequently than to stable. 

The main difference is that we will not document all changes to the unstable branch as we do with stable on https://docs.jottacloud.com and that some features might not be as complete as when they hit the stable branch. 

Builds for unstable are located at https://repo.jotta.us/archives-unstable/

## Compatibility

In most cases switching between the stable and unstable branch should be unproblematic. However, in the case where unstable contains changes to (for instance) the internal data model there will always be a patch from stable to unstable. But there might not be a way of patching back from unstable. In that case you will have to reset (see below), login and start over. Switching from unstable to the next major stable release should always work.

### Resetting

Resetting means deleting the contents of jotta-cli's appdata folder. Typically this will be `/var/lib/jottad` or `~/.jottad`
You can find the appdata-folder by executing `jotta-cli version`
```
> jotta-cli version
-------------------------------------------
(...)
jottad appdata    : /Users/kim/.jottad
(...)

# stop jottad
> rm -rf /Users/kim/.jottad/*
# restart jottad
> jotta-cli login
(...)
```

### Using unstable with brew

Make sure brew is updated and install the jotta-cli-unstable recipe:
```
brew update && brew install jotta-cli-unstable
```
If you already have jotta-cli stable installed you need to uninstall this (only removes binaries, not userdata)
```
brew uninstall jotta-cli && brew install jotta-cli-unstable
```

### Using unstable with apt/deb

Add gpg key, add unstable repository, update apt and install from unstable repository:
```
sudo apt-get install curl apt-transport-https ca-certificates
sudo curl -fsSL https://repo.jotta.cloud/public.asc -o /usr/share/keyrings/jotta.gpg
echo "deb [signed-by=/usr/share/keyrings/jotta.gpg] https://repo.jotta.cloud/debian unstable main" | sudo tee /etc/apt/sources.list.d/jotta-cli.list
sudo apt-get update && apt install jotta-cli
```

### Using unstable with rpm

Change from stable to unstable in repository config, install with yum and restart jotta-cli:
```
sed -i 's#https://repo.jotta.us/redhat#https://repo.jotta.us/redhat-unstable#g' /etc/yum.repos.d/JottaCLI.repo
yum install jotta-cli
systemctl restart jotta-cli (or alternative init used)
```

### Using unstable with tar
Download tarball of your correct arch from https://repo.jotta.us/archives-unstable, replace the jottad/jotta-cli binaries and restart jottad:
```
wget https://repo.jotta.us/archives-unstable/os/arch/archive.tar
tar -czvf archive.tar.gz /tmp
mv /tmp/jotta-cli /usr/local/bin/jotta-cli
mv /tmp/jottad /usr/local/bin/jotta-cli
```

### Using jottad on linux
The package supplies init script for running jotta-cli in systemd user slice.
For each user in the system that should run jotta-cli, enable it with: 
```
run_jottad
```

The daemon should now be started with user login.
