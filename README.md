# mk Proto Files

This directory contains architecture-specific mkfiles and common proto
files for use with mk. Install to /usr/share/mk/ or set MKROOT environment
variable to point to this directory.

## Directory Structure

```
$MKROOT/
├── proto/
│   ├── mkfile.proto    # common variables (YACC, LEX, etc.)
│   ├── mkone           # template for building single programs
│   └── mklib           # template for building libraries
├── x86_64/
│   └── mkfile          # native x86_64 (amd64) compilation
├── 386/
│   └── mkfile          # i386 compilation (32-bit)
├── arm/
│   └── mkfile          # ARM cross-compilation (requires arm-linux-gnueabihf-gcc)
└── arm64/
    └── mkfile          # ARM64 cross-compilation (requires aarch64-linux-gnu-gcc)
```

## Usage

In your mkfile:

```
# Include architecture-specific definitions
<$MKROOT/$objtype/mkfile

# Or for a specific architecture
<$MKROOT/x86_64/mkfile

# Include common build template
<$MKROOT/proto/mkone

TARG=myprog
OFILES=main.$O util.$O
```

## Environment Variables

- `MKROOT` - Root directory for mk proto files (default: /usr/share/mk)
- `objtype` - Target architecture (x86_64, 386, arm, arm64)

## Cross-Compilation

For cross-compilation, install the appropriate toolchain:

- ARM: `apt install gcc-arm-linux-gnueabihf`
- ARM64: `apt install gcc-aarch64-linux-gnu`

Then set objtype before running mk:

```
objtype=arm mk
```

## Variables Set by Architecture mkfiles

| Variable | Description                    |
|----------|--------------------------------|
| CC       | C compiler                     |
| LD       | Linker                         |
| AS       | Assembler                      |
| AR       | Archiver                       |
| RANLIB   | Archive indexer                |
| O        | Object file extension          |
| CFLAGS   | Default compiler flags         |
| LDFLAGS  | Default linker flags           |

## This repository

`mkroot` is the tree above, as installed under `/usr/share/mk` on our machines; every project of ours builds with
`<$MKROOT/$objtype/mkfile` and one of the proto files. `mk` itself is the [mk](https://github.com/Levalicious/mk) repository.

In CI:

    - uses: Levalicious/mk@<commit>          # mk on the PATH, objtype exported
    - uses: Levalicious/mkroot@<commit>      # this tree checked out, MKROOT exported

Pin both commits: a dependant fetches the versions it pinned, nothing else.
