# kataos_devcontainer

VSCode devcontainer settings for building KataOS.

## Usage

1. Host Terminal

   ```sh
   git clone git@github.com:junkawa/kataos_devcontainer.git
   mkdir <your_kataos_build_path>
   cp -a kataos_devcontainer/.devcontainer <your_kataos_build_path>/
   cd <your_kataos_build_path>
   code . # open vscode
   ```

2. VSCode

   - Command Palette: `Dev Containers: Reopen in Container`
   - Terminal on Container

     ```sh
     repo init -u https://github.com/AmbiML/sparrow-manifest -m camkes-manifest.xml
     repo sync -j$(nproc)
     sh scripts/build-sparrow.sh aarch64
     cd build-aarch64 && ./simulate -M raspi3b
     ```
