# totem-yads
ZMK Config and Build for Totem and YADS

Inspired by a [post](https://www.reddit.com/r/ErgoMechKeyboards/comments/1qc5729/zmk_local_build_script_build_your_firmware_locally/) and a [script](https://github.com/carlosedp/zmk-sofle/blob/main/build_local.sh) from @carlosedp I decided to get my [TOTEM](https://github.com/GEIGEIGEIST/TOTEM) keyboard up to date with ZMK and get a local build process in place becasue I want to add some features to the dongle.

This repo builds the firmware for the TOTEM and the [ZMK Dongle Screen YADS (Yet another Dongle Screen)](https://github.com/janpfischer/zmk-dongle-screen) with the latest ZMK and Zephyr 4.1.  It includes my ridiculous keymap as well.

## build_zmk_local.sh

I've updated the script to work in more situations.  The biggest change for me was getting it to work on rootless Podman on Fedora 43.  For Podman to be able to access the project directory you need to give permissions using ACLs for SELinux.  The command is

``` shell
sudo chcon -Rt container_file_t ./
```

This allows rootless Podman containers to write to local directories that are mounted to a container.  In this case, we're giving permission for the container to access this project directory.  The script will pop up a warning if it doesn't find the correct ACLs in place when using Podman.

## Building locally

ZMK is great, but the Github Action build takes too long so building locally is very helpful when working on new features or iterrating on your keymap.  You can just copy the script from this project and use it in yours or you can copy the repo and update the `config` directory and the `build.yaml` for your project.  Then you can just run the build process.

``` shell
./build_zmk_local.yaml build
```

### Other commands

``` shell
Usage: ./build_zmk_local.yaml [flags] <command>

Commands:
  init             Initialize the repository (west init + update)
  update           Update the repository (west update)
  list             List all available build targets from build.yaml
  build_dongle     Build central dongle firmware
  build_left       Build peripheral left firmware
  build_right      Build peripheral right firmware
  build_central_left Build central left (no dongle) firmware
  build_reset      Build settings reset firmware
  build [name]     Build all firmware or specific target by name
  clean [target]   Clean build directory (all or specific target)
  clean_all        Clean all west dependencies and build artifacts
  gitignore        Generate/update .gitignore from config/west.yml
  copy [dest]      Copy build artifacts to directory (default: ./artifacts)
  help             Show this help message
```
