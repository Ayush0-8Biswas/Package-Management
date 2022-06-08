### So, how do you install stuff on linux? If you want to install software, programs or even hacking tools. Then what you will do?

> Today we are going to cover Package Management in Linux or basically how to manage software and programs on your linux machine
>

**In Linux the software or programs you want to install like discord or ms-teams is contained in something called a package**
### A package is an archive containing:
- all the files of an application
- it contains metadata about the application, such as application name, its version, list of all the dependencies it requires to run 

### How do we install these packages?
> It's something called a package manager that is used to install these packages.

## Package Manager
> package manager is a software that automates the installation process as well as the upgrading and removing of packages from your computer.
> 
> they are made so that we don't need to install or update the packages manually.

~~### Functions of package manager~~
- They extract the archives that are downloaded from repositories as packages.
- a package manager looks for software on the repository for downloading packages that we want to install
- it also checks the authenticity of packages by verifying their checksums and digital certificates. for example in windows if we have to install an application we search the application on web, but it is not secure at all as we may click at some false link that may download harmful programs in our computer 
- package manager also manages dependencies to ensure that a package is installed with all the packages it requires to run.

> Let's move on to the examples of package managers
~~### Examples of Package managers~~
- In Debian based Linux distributions we have :
   - **DPKG** 
     - dpkg is a low level package manager. It is dumb. 
     - dpkg doesn't download packages automatically we need to download .deb files manually.
     - it shows the unmet dependencies but does not resolve the dependencies automatically.

   - **APT**
     - as its name suggests it is a high level package manager 
     - It relies on a repository to get the information about packages
     - it automatically searches and installs the packages along with their dependencies that need to be installed with the package
     

> package managers have multiple repositories where they look for the packages that we may want to install and updates for the packages that are already installed.

> Now for the arch based linux distributions
- In Arch based Linux distributions there is :
  - **PACMAN**
    - > One difference between pacman and the other package managers that we discussed is that it doesn't offer commands like update or remove.
    - > Instead, it uses single-letter arguments to get various job done
    - > You can use double-dash options as well, however the short ones are more popular
  - **YAY**
    
## Pacman
> You can find the list of all the options and their usage at [this link](https://man.archlinux.org/man/pacman.8) 
> or if you are on arch machine run the command "man pacman" to list all the options


> Now I want to discuss some important options that we need to know for using pacman
### Important options
### -S sync options 
- -S (stands for sync and is used to install packages that is available on the repository)
- -Ss (tag ss is used to search for packages in the package database)
- -Sy (tag sy is used to sync and refresh the package database from repository or sever just like AUR that is the Arch User Repository)
- -Su (tag su is used to upgrade all the packages that are out of date or for which an update is available, the dependencies are automatically resolved and will be installed/upgraded if necessary)
- -Sc (tag sc is used to clean packages from cache that are no longer installed and unused sync databases that are no longer required)

### -R remove options
- -R (remove package that is installed by pacman)
- -Rn --nosave (tag rn is used to ignore the package backup, usually -R saves a backup of removed packages in .packsave extension)
- -Rs --recursive (tag rs removes the packages with its dependencies that are no longer required by other packages)
- -Ru --unneeded (tag ru removes the targets that are not required by other packages, acts like a safety switch which prevents other packages from breaking)

### -Q query options
- by default this option operates on the installed packages database
- -Qd (tag qd is used to display packages installed as dependencies)
- -Qe (tag qe to display packages that are installed explicitly)
- -Qs (tag qs is used to search for a package in the installed packages list)
- -Qu (tag qd displays available upgrades and out of date packages)


> Let me show you some practical examples of the options that we discussed
> 
> This to be performed live
> - pacman -Syu
> - pacman -S gpm
> - pacman -Ss code
> - pacman -Ss discord
> - pacman -Qs telegram-cli
> - pacman -Rns telegram-cli

## MAKEPKG
> it is a script to automate the building of packages
> 
> u just need to have the build script (that is our PKGBUILD file) and makepkg will do the rest of the job

### Important options
- -c, --clean (using tag c cleans up left over work files and directories after successful build)
- -d. --no-deps (tag d is used to ignore the dependency check and ignore any dependencies required)
- -i, --install (tag i is used to install or upgrade the package after a successful build)
- -s, --syncdeps (tag s installs the missing dependencies using pacman "where the list of dependencies are mentioned in PKGBUILD file".)

## PKGBUILD
> It is a shell script containing the build information required by Arch Linux packages.
> 
> Packages in Arch Linux are built using the makepkg utility (just discussed).
> When makepkg is run, it searches for a PKGBUILD file in the current directory or folder and follows the instructions written 
> in the file to compile or download the files to build a package archive.
> This resulting archive contains binary files and / or installation instructions, readily installable with pacman.

## Example PKGBUILD file
```bash
# Maintainer: Ayush Biswas biswas.ayush03@kgpian.iitkgp.ac.in
pkgname=guessing-game-git # Name of the package
pkgver=1.1 # current version of package
pkgrel=1  # release number specific to the distribution
pkgdesc="A cool, lightweight and open source guessing game in the terminal."  # brief description of the package and its functionality.
arch=(any)
url="https://github.com/Ayush0-8Biswas/guessing-game.git"  # project's web site or location
license=('unknown') # specifies the licence that applies to the package
depends=() # array of packages this package depends to run
makedepends=(git python3) # array of packages this package depends to build
source=("git+$url") # array of source files required to build the package.
md5sums=('SKIP') # integrity checks of the package

build() { # compile the package
	cd guessing-game
}

package() { # install files into the root directory for the package
	cd guessing-game
	mkdir -p ${pkgdir}/usr/local/bin/
	cp guessing-game.py ${pkgdir}/usr/local/bin/guessing-game
	chmod +x ${pkgdir}/usr/local/bin/guessing-game
}
```