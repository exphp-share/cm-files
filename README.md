# Scripts for my user Environment Modules setup

On remote systems that have Environment Modules installed (that's the thing with `mod avail`, `mod load`, etc.), I prefer to install compiled libraries to a local modulefiles prefix.

# Setup

```
cp -a apps modulefils $HOME
```

(or, more likely, copy them to a location on a filesystem suitable for large, installed binaries, and then create symlinks at `$HOME/apps` and `$HOME/modulefiles`)

# How to use

Look at `$HOME/apps/notes.md`

In addition there is the `link-notes` script here which moves any file at `$HOME/apps/*/notes` to here and creates symlinks for them, so that they can be easily shared across systems.
