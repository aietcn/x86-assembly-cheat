# Linker scripts

1. [hello_world.ld](hello_world.ld)

## Introduction

This directory users IA-32.

Linker scripts determine how exactly `ld` is going to transform object files into the executable.

In particular, how the sections are going to me mashed up into segments.

The default linker script operation is determined by the default linker scripts.

Those are hardcoded into the `ld` executable.

`make install` also generates an informational copy of the scripts at:

    x86_64-unknown-linux-gnu/lib/ldscripts

which some distributions provide. But those are only informational. The usual location is:

    /usr/lib/ldscripts

The actual script being used is determined by the `ld` options, and can be viewed with:

    ld --verbose

which spits out the entire script, but not it's name. You can then match this output to the scripts under `/usr/lib/ldscripts`.

Minimal example: <http://stackoverflow.com/questions/7182409/how-to-correctly-use-a-simple-linker-script> TODO make into a repo.

One case where linker scripts are needed is when building more "exotic" executables, e.g. a boot sector or a multiboot file. E.g.: <http://wiki.osdev.org/Bare_Bones>

## T

Replace the linker script with a custom one:

    ld -T script a.o

You will often use an existing script as the basis for this.

## Ttext-segment

Change the address of some predefined segments:

    -Ttext-segment=0x80000

The default linker script reads:

    . = SEGMENT_START("text-segment", 0x400000)

which chooses takes the value passed, or 4M by default.

## SHORT

Used to insert raw bytes directly into the output.

Application: insert boot sector magic bytes: https://github.com/cirosantilli/x86-bare-metal-examples/blob/2b79ac21df801fbf4619d009411be6b9cd10e6e0/a.ld#L14

## PROVIDE

Provide new symbols to the program.

http://stackoverflow.com/questions/1765969/where-are-the-symbols-etext-edata-and-end-defined/30533316#30533316