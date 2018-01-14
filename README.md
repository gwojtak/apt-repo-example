# apt-repo-example

This example demonstrates how to use a github repo as an apt
repository.  The repo is configured on a local system and then
pushed to github, and with proper configuration on the Ubuntu/
Debian system, it can then be used directly as an apt repo.

## Procedure

A quick overview of the process. The steps below illustrate setting up 
the repository locally

```bash
# apt-get -y install reprepro gpg git apt-transport-https
# mkdir -p repo_root/{conf,incoming}
# cat <<EOF >>repo_root/conf/distributions
Origin: Parts Unknown
Label: Magical Github Apt Repo
Suite: stable
Codename: xenial
Architecture: amd64
EOF
# cd repo_root
# reprepro -S utils includedeb xenial /path/to/deb/package.deb
# cd dists/xenial
# gpg --armor -b --output Release.gpg Release
# cd main/binary-amd64
# gpg --armor -b --output Release.gpg Release
# cd ../../../..
# git init .
# git add .
# git commit -m "initial commit"
# git remote add origin https://github.com/gwojtak/apt-repo
# git push -u origin master
```
These commands will create the apt repository, then do a git commit
and push out to the github repository.  Note that this assumes the 
gpg is already created.  Once that is complete, systems
must be configured to use the apt repo stored in github.
```bash
# cat <<EOF >>/etc/apt/sources.list.d/github.list
deb https://raw.githubusercontent.com/gwojtak/apt-repo/master xenial main
EOF
# apt-get update
# apt-get install package-from-new-repo
```
