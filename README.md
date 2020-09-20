# Super Mario 64 Port - Traducció al Català
#### *Info in Catalan, to read the English version please scroll down.*

*Text en traducció, disculpeu les molèsties.*

La traducció catalana de Super Mario 64 ha estat creada per a portar el joc a un nou idioma emprant el Port de la Decompilació de SM64, per a que sigui jugable a tots els sistemes que es vulgui.
Aquest projecte **no** està relacionat, de cap manera, amb els equips responsables del Projecte de Decompilació ni del Port.

Els continguts, informació i instruccions del Port de SM64 estan detallats a continuació:

- Aquest repositori conté l'íntegra decompilació de Super Mario &4 (J), (U) i (E) amb excepcions menors al subsistema d'àudio.
- Les nomenclatures i documentació del codi font i de les estructures de dades estan encara en procés.
- Els esforços per a decompilar la ROM Shindou avança constantment a una construcció parella.
- A part de la Nintendo 64, també pot córrer nativament a Windows i Linux.

Aquest repositori no inclou totes les llibreries necessàries per a compilar el joc.
Es requereix una còpia prèvia del joc per a extreure'n les llibreries.

## Construint executables nadius

### Linux

1. Instal·lar els prerequisits (Ubuntu): `sudo apt install -y git build-essential pkg-config libusb-1.0-0-dev libsdl2-dev`.
2. Clona el respositori: `git clone https://github.com/sm64-port/sm64-port.git`, which will create a directory `sm64-port` and then **enter** it `cd sm64-port`.
3. Col·loca una ROM de Super Mario 64 anomenada `baserom.<VERSION>.z64` al directori arrel del repositori, on `VERSION` pot ser `us`, `jp`, o `eu`.
4. Empra `make` per a construir-la. Qualifica la versió a partir de `make VERSION=<VERSION>`. Afegeix `-j4` per a millorar la velocitat de construcció (basat en el nombre de nuclis del processador disponibles).
5. El binari de l'executable es trobarà a `build/<VERSION>_pc/sm64.<VERSION>.f3dex2e`.

### Windows

1. Instal·la i actualitza MSYS2, seguint tots els passos llistats a https://www.msys2.org/.
2. Des del menú d'inici, executa MSYS2 MinGW i instal·la els paquets requerits depenent del teu sistema ( **NO** executis "MSYS2 MSYS"):
  * 64-bit: Executa "MSYS2 MinGW 64-bit" i instal·la: `pacman -S git make python3 mingw-w64-x86_64-gcc`
  * 32-bit (també funciona en sistemes de 64-bits): Executa "MSYS2 MinGW 32-bit" i instal·la: `pacman -S git make python3 mingw-w64-i686-gcc`
  * **NO** instal·lis per error el paquet anomenat simplement `gcc`.
3. El terminal d'MSYS2 té un _current working directory_ (directori actual de treball) que inicialment és `C:\msys64\home\<username>` (directori d'usuari). A la finestra de comandes veuràs el directori de treball actual de color groc. `~` és un àlies per a directori d'usuari. Pots canviar el directori actual de treball a `Documents` usant `cd /c/Users/<username>/Documents`.
4. Clona el repositori: `git clone https://github.com/sm64-port/sm64-port.git`, el qual crearà el directori `sm64-port`, llavors **insereix** `cd sm64-port`.
5. Col·loca una ROM de *Super Mario 64* anomenada `baserom.<VERSION>.z64` al directori arrel per a l'extracció de les llibreries, on `VERSION` pot ser `us`, `jp`, o `eu`.
6. Empra `make` per a construir-la. Qualifica la versió a partir de `make VERSION=<VERSION>`. Afegeix `-j4` per a millorar la velocitat de construcció (basat en el nombre de nuclis del processador disponibles).
7. El binari de l'executable es trobarà a `build/<VERSION>_pc/sm64.<VERSION>.f3dex2e.exe` dins del repositori.

#### Troubleshooting (no traduït)

1. If you get `make: gcc: command not found` or `make: gcc: No such file or directory` although the packages did successfully install, you probably launched the wrong MSYS2. Read the instructions again. The terminal prompt should contain "MINGW32" or "MINGW64" in purple text, and **NOT** "MSYS".
2. If you get `Failed to open baserom.us.z64!` you failed to place the baserom in the repository. You can write `ls` to list the files in the current working directory. If you are in the `sm64-port` directory, make sure you see it here.
3. If you get `make: *** No targets specified and no makefile found. Stop.`, you are not in the correct directory. Make sure the yellow text in the terminal ends with `sm64-port`. Use `cd <dir>` to enter the correct directory. If you write `ls` you should see all the project files, including `Makefile` if everything is correct.
4. If you get any error, be sure MSYS2 packages are up to date by executing `pacman -Syu` and `pacman -Su`. If the MSYS2 window closes immediately after opening it, restart your computer.
5. When you execute `gcc -v`, be sure you see `Target: i686-w64-mingw32` or `Target: x86_64-w64-mingw32`. If you see `Target: x86_64-pc-msys`, you either opened the wrong MSYS start menu entry or installed the incorrect gcc package.

### Debugging

El codi pot ser debugat emprant `gdb`. A Linux instal·la el paquet `gdb`i executa `gdb <executable>`. A MSYS2 instal·la-ho executant `pacman -S winpty gdb` i executant `winpty gdb <executable>`. El programa `winpty` s'assegura que el teclat funciona adequadament al terminal. Considera també canviar el criteri d'execució (flag) `-mwindows` per `-mconsole` tant per a poder veure el "stdout/stderr" com per a poder prèmer CTRL+C per a interrompre el programa. Al "Makefile", assegura't que compiles les fonts emprant `-g` enlloc de `-O2` per a incloure símbols de debugging. Cerca un tutorial online per a utilitzar gdb.

## ROM building

També és possible construir ROMs de Nintendo 64 amb aquest repositori. Dirigeix-te a https://github.com/n64decomp/sm64 per instruccions.

## Project Structure (no traduït)

```
sm64
├── actors: object behaviors, geo layout, and display lists
├── asm: handwritten assembly code, rom header
│   └── non_matchings: asm for non-matching sections
├── assets: animation and demo data
│   ├── anims: animation data
│   └── demos: demo data
├── bin: C files for ordering display lists and textures
├── build: output directory
├── data: behavior scripts, misc. data
├── doxygen: documentation infrastructure
├── enhancements: example source modifications
├── include: header files
├── levels: level scripts, geo layout, and display lists
├── lib: SDK library code
├── rsp: audio and Fast3D RSP assembly code
├── sound: sequences, sound samples, and sound banks
├── src: C source code for game
│   ├── audio: audio code
│   ├── buffers: stacks, heaps, and task buffers
│   ├── engine: script processing engines and utils
│   ├── game: behaviors and rest of game source
│   ├── goddard: Mario intro screen
│   ├── menu: title screen and file, act, and debug level selection menus
│   └── pc: port code, audio and video renderer
├── text: dialog, level names, act names
├── textures: skybox and generic texture data
└── tools: build tools
```

## Contributing

Les "Pull requests" són benvingudes. Per canvis majors, obriu primer si us plau obriu primer una "issue" per a discutir que voldriueu canviar.

Executa `clang-format` al vostre codi per a assegurar-vos que compleix els estàndards de codi del projecte.

Discord oficial del projecte: https://discord.gg/7bcNTPK


# Super Mario 64 Port - Catalan Translation
#### *Informació en anglès, per a llegir la versió catalana si us plau pujeu a munt.*

The Super Mario 64 Catalan translation is created to bring the game to a new language using the SM64 Decompilation Port, so it is playable on all systems wanted.
This project is **not** related, by any means, to the original Decompilation Project nor the Port teams.

The contents, info and instructions of the SM64 Port are detailed below:

- This repo contains a full decompilation of Super Mario 64 (J), (U), and (E) with minor exceptions in the audio subsystem.
- Naming and documentation of the source code and data structures are in progress.
- Efforts to decompile the Shindou ROM steadily advance toward a matching build.
- Beyond Nintendo 64, it can also target Linux and Windows natively.

This repo does not include all assets necessary for compiling the game.
A prior copy of the game is required to extract the assets.

## Building native executables

### Linux

1. Install prerequisites (Ubuntu): `sudo apt install -y git build-essential pkg-config libusb-1.0-0-dev libsdl2-dev`.
2. Clone the repo: `git clone https://github.com/sm64-port/sm64-port.git`, which will create a directory `sm64-port` and then **enter** it `cd sm64-port`.
3. Place a Super Mario 64 ROM called `baserom.<VERSION>.z64` into the repository's root directory for asset extraction, where `VERSION` can be `us`, `jp`, or `eu`.
4. Run `make` to build. Qualify the version through `make VERSION=<VERSION>`. Add `-j4` to improve build speed (hardware dependent based on the amount of CPU cores available).
5. The executable binary will be located at `build/<VERSION>_pc/sm64.<VERSION>.f3dex2e`.

### Windows

1. Install and update MSYS2, following all the directions listed on https://www.msys2.org/.
2. From the start menu, launch MSYS2 MinGW and install required packages depending on your machine (do **NOT** launch "MSYS2 MSYS"):
  * 64-bit: Launch "MSYS2 MinGW 64-bit" and install: `pacman -S git make python3 mingw-w64-x86_64-gcc`
  * 32-bit (will also work on 64-bit machines): Launch "MSYS2 MinGW 32-bit" and install: `pacman -S git make python3 mingw-w64-i686-gcc`
  * Do **NOT** by mistake install the package called simply `gcc`.
3. The MSYS2 terminal has a _current working directory_ that initially is `C:\msys64\home\<username>` (home directory). At the prompt, you will see the current working directory in yellow. `~` is an alias for the home directory. You can change the current working directory to `My Documents` by entering `cd /c/Users/<username>/Documents`.
4. Clone the repo: `git clone https://github.com/sm64-port/sm64-port.git`, which will create a directory `sm64-port` and then **enter** it `cd sm64-port`.
5. Place a *Super Mario 64* ROM called `baserom.<VERSION>.z64` into the repository's root directory for asset extraction, where `VERSION` can be `us`, `jp`, or `eu`.
6. Run `make` to build. Qualify the version through `make VERSION=<VERSION>`. Add `-j4` to improve build speed (hardware dependent based on the amount of CPU cores available).
7. The executable binary will be located at `build/<VERSION>_pc/sm64.<VERSION>.f3dex2e.exe` inside the repository.

#### Troubleshooting

1. If you get `make: gcc: command not found` or `make: gcc: No such file or directory` although the packages did successfully install, you probably launched the wrong MSYS2. Read the instructions again. The terminal prompt should contain "MINGW32" or "MINGW64" in purple text, and **NOT** "MSYS".
2. If you get `Failed to open baserom.us.z64!` you failed to place the baserom in the repository. You can write `ls` to list the files in the current working directory. If you are in the `sm64-port` directory, make sure you see it here.
3. If you get `make: *** No targets specified and no makefile found. Stop.`, you are not in the correct directory. Make sure the yellow text in the terminal ends with `sm64-port`. Use `cd <dir>` to enter the correct directory. If you write `ls` you should see all the project files, including `Makefile` if everything is correct.
4. If you get any error, be sure MSYS2 packages are up to date by executing `pacman -Syu` and `pacman -Su`. If the MSYS2 window closes immediately after opening it, restart your computer.
5. When you execute `gcc -v`, be sure you see `Target: i686-w64-mingw32` or `Target: x86_64-w64-mingw32`. If you see `Target: x86_64-pc-msys`, you either opened the wrong MSYS start menu entry or installed the incorrect gcc package.

### Debugging

The code can be debugged using `gdb`. On Linux install the `gdb` package and execute `gdb <executable>`. On MSYS2 install by executing `pacman -S winpty gdb` and execute `winpty gdb <executable>`. The `winpty` program makes sure the keyboard works correctly in the terminal. Also consider changing the `-mwindows` compile flag to `-mconsole` to be able to see stdout/stderr as well as be able to press Ctrl+C to interrupt the program. In the Makefile, make sure you compile the sources using `-g` rather than `-O2` to include debugging symbols. See any online tutorial for how to use gdb.

## ROM building

It is possible to build N64 ROMs as well with this repository. See https://github.com/n64decomp/sm64 for instructions.

## Project Structure

```
sm64
├── actors: object behaviors, geo layout, and display lists
├── asm: handwritten assembly code, rom header
│   └── non_matchings: asm for non-matching sections
├── assets: animation and demo data
│   ├── anims: animation data
│   └── demos: demo data
├── bin: C files for ordering display lists and textures
├── build: output directory
├── data: behavior scripts, misc. data
├── doxygen: documentation infrastructure
├── enhancements: example source modifications
├── include: header files
├── levels: level scripts, geo layout, and display lists
├── lib: SDK library code
├── rsp: audio and Fast3D RSP assembly code
├── sound: sequences, sound samples, and sound banks
├── src: C source code for game
│   ├── audio: audio code
│   ├── buffers: stacks, heaps, and task buffers
│   ├── engine: script processing engines and utils
│   ├── game: behaviors and rest of game source
│   ├── goddard: Mario intro screen
│   ├── menu: title screen and file, act, and debug level selection menus
│   └── pc: port code, audio and video renderer
├── text: dialog, level names, act names
├── textures: skybox and generic texture data
└── tools: build tools
```

## Contributing

Pull requests are welcome. For major changes, please open an issue first to
discuss what you would like to change.

Run `clang-format` on your code to ensure it meets the project's coding standards.

Official project Discord: https://discord.gg/7bcNTPK
