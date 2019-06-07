# Scripts for my user Environment Modules setup

On remote systems that have Environment Modules installed (that's the thing with `mod avail`, `mod load`, etc.), I prefer to install compiled libraries to a local modulefiles prefix.

## Setup

```
mkdir $HOME/apps $HOME/modulefiles
ln -sf $(pwd)/generate-modulefile $HOME/apps/generate-modulefile
ln -sf $(pwd)/update-links $HOME/modulefiles/update-links
```

## How to use

```
module use $HOME/modulefiles
```

Packages in here will then show up in `module avail` and can be loaded with `module load`.

### Creating new packages:

Use `$HOME/apps/<PACKAGE>/<VERSION>` as an install prefix when building stuff.  Don't build packages from master; find release commits where the version is easily specified.

There MAY be notes at `apps/<PACKAGE>/notes` with additional build info, but I'd suggest checking https://github.com/ExpHP/dotfiles/blob/master/notes/notes.md just in case.

The `apps/<PACKAGE>/<VERSION>/modulefile` files are partially generated and partially handwritten:

* If another version of this package exists in the apps directory, you can probably just copy its modulefile.
* Otherwise:
    * use `generate-modulefile` to get a head start; this script will automatically check for certain directories and add path entries accordingly
    * look at the generated modulefile and see if anything is missing (in particular consider if there are any non-pathlike env vars that need to be set)

`$HOME/modulefiles` contains a tree of symlinks, which can be automatically generated from apps. Run:

```
$HOME/modulefiles/update-links
```

If you have any old, existing local install prefixes (e.g. `$HOME/.local`), feel free add your own modulefiles for them manually (you can even use `generate-modulefile`).  Regular files and valid symlinks won't be deleted by the script (it only clears broken symlinks).

In addition there is the `link-notes` script here which moves any file at `$HOME/apps/*/notes` to here and creates symlinks for them, so that they can be easily shared across systems.

### Reminder for packages that use `cmake`

```
rm -rf build && mkdir build && cd build

cmake -DCMAKE_INSTALL_PREFIX=$HOME/apps/PACKAGE/VERSION -DCMAKE_BUILD_TYPE=Release ..
```

