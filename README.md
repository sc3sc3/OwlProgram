# OwlProgram
Dynamically loaded OWL program.
See http://hoxtonowl.com for more details of the Open Ware Laboratory.

# Instructions

## Prerequisites
* gcc arm compiler (to make patch binary) [[1](#ref1)]
* FirmwareSender (to make sysex and run) [[2](#ref2)]
* emcc (to make web) [3](#ref3)
* faust2owl (to compile FAUST patches) [[4](#ref4)]
* Enzien Audio account (to compile PD patches) [[5](#ref5)]

* On Windows, you'll need a MAKE utility [[6](#ref6)].  You'll also need to open common.mk and point TOOLROOT to your gcc installation directory, using a path string without spaces (such as using 8.3 filenames).

## Make targets
* make patch: build patch binary
* make sysex: package binary as sysex
* make run: upload patch to attached OWL
* make store: upload and save to attached OWL
* make web: build Javascript patch
* make clean: remove intermediary and target files
* make realclean: remove all (library+patch) intermediary and target files
* make size: show binary size metrics and large object summary
* make map: build map file (Build/patch.map)
* make as: build assembly file (Build/patch.s)
* make help: print target information

Make sure to do a `make clean` before compiling a new patch, or add `clean` to your make target, otherwise the previous patch name will be retained.

## Make options
* PATCHNAME: specify name of patch, e.g. SimpleDelay
* PATCHCLASS: name of patch class, e.g. SimpleDelayPatch
* PATCHFILE: name of main patch file, e.g. SimpleDelayPatch.hpp
* PATCHIN: number of input channels, default 2
* PATCHOUT: number of output channels, default 2
* SLOT: user program slot to store patch in, default 0
* TARGET: changes the output prefix, default 'patch'

If you follow the convention of SimpleDelay then you don't have to specify `PATCHCLASS` and `PATCHFILE`, they will be deduced from `PATCHNAME`.

Note that when storing user programs, the current OWL firmware has four user defined patches, numbered 37 to 40. These correspond to slot number 0 to 3.

## Building C++ patches
First copy all patch files to `PatchSource` folder, then issue the appropriate make command.

Example: Compile and run the TestTone patch, defined in file `PatchSource/TestTonePatch.hpp` as class `TestTonePatch`:
`make PATCHNAME=TestTone run`

Example: Compile and run in browser
`make PATCHNAME=TestTone web`
Then open `Build/web/patch.html`

## Building FAUST patches
To compile and run a FAUST patch
* copy .dsp file and dependencies into `PatchSource`, e.g. `LowShelf.dsp`
* `make FAUST=LowShelf run`

Note: assign OWL parameters with slider metadata: `[OWL:A]`, `[OWL:B]` et c. For example:
```gain = vslider("gain[OWL:C]", 1,0,1,0.1);```. Assign push button with e.g. ```gate = button("gate[OWL:Push]");```

## Building Pure Data patches
Requires an account with Enzien Audio [5]

To compile and run a PD patch, with C code generated by Enzien Audio:
* make a Heavy project called `owl` and compile your PD patch to C
* download and unzip the Heavy C code into `PatchSource`
* `make PATCHNAME=Heavy run`

Or, using the Heavy uploader (recommended):
* ensure you have a Heavy project called `owl`
* put your PD patch file (e.g. `Foo.pd`) into `PatchSource`
* `make HEAVY=Foo run`
* enter username and credentials when prompted (first time only)

Note: assign OWL parameters with PD receivers called `Channel-A`, `Channel-B`, et c.

## Building Max Gen patches
Requires the `.cpp` and `.h` files of a Gen patch generated by Max Gen.

To compile and run a Gen patch called `Foo`:
* copy `Foo.cpp` and `Foo.h` into `PatchSource`
* `make GEN=Foo run`

Note: use OWL parameters in Gen with parameter names: `A`, `B`, `C`, `D`, `Exp`, and `Push`.

# References
<a name="ref1">[1]</a> https://developer.arm.com/open-source/gnu-toolchain/gnu-rm

<a name="ref2">[2]</a> https://github.com/pingdynasty/FirmwareSender

<a name="ref3">[3]</a> http://emscripten.org

<a name="ref4">[4]</a> http://faust.grame.fr/

<a name="ref5">[5]</a> https://enzienaudio.com

<a name="ref6">[6]</a> http://sourceforge.net/projects/mingw/
