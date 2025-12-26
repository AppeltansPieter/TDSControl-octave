# TDSControl - Octave

An Octave wrapper for the TDS Control library (https://github.com/AppeltansPieter/TDSControl).

## Installation

- Option 1 (**Suggested**): Download the release files compatible with your operating system and octave version. Unzip the files and add the downloaded folder to your Octave path:
```octave
addpath("/path/to/TDSControl");
```
If you want to permanently add TDSControl to your Octave path, run
```octave
savepath;
```

If your Octave version/operating system is not supported, the library can be build from source (see option 2).
- Option 2: Build the necessary `.oct` files from source and install the library to the desired location (requires CMake and the Octave binaries should be findable via path):
```bash
git clone https://github.com/AppeltansPieter/TDSControl-octave.git
cd TDSControl-octave
git submodule update --init --recursive
cmake -S . -B build -DCMAKE_INSTALL_PREFIX=/desired/path/for/TDSControl
cmake --build build --parallel 6
cmake --build --target install
```
Next open Octave and add the location to which you installed the library to your path:
```octave
addpath("/path/to/TDSControl");
```
If you want to permanently add TDSControl to your Octave path, run
```octave
savepath;
```
