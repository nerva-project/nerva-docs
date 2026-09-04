# Building NERVA From Source
Building the Nerva software yourself is the most thorough answer to "what am I running": you compile the daemon and wallet from the published source, on the machine and toolchain of your choosing. It is also how you get to run development code, test a change, or produce binaries for a platform the releases do not cover. This guide walks through the whole procedure on the three desktop platforms.

The build system is CMake behind a `make` wrapper that takes care of the common cases. Older build scripts still live under `builder/` in the repository, but they are deprecated in favor of plain `make` and will be removed in a future release; nothing on this page uses them.

# Dependencies

The table below summarizes the tools and libraries required to build. A few of the libraries are also vendored in the repository: by default the build uses whatever is installed on the system, and only falls back to the vendored copy if it cannot find one. Static builds always use the vendored sources, because distribution packages tend to ship shared objects without the static archives.

| Dep | Min. version | Debian/Ubuntu pkg | Optional | Purpose |
|---|---|---|---|---|
| GCC | 7 | `build-essential` | NO | compiler |
| CMake | 3.5 | `cmake` | NO | build system |
| pkg-config | any | `pkg-config` | NO | |
| Boost | 1.66 | `libboost-all-dev` | NO | C++ libraries |
| OpenSSL | any | `libssl-dev` | NO | hashing, TLS |
| libzmq | 4.2.0 | `libzmq3-dev` | NO | ZeroMQ |
| OpenPGM | ? | `libpgm-dev` | NO | for ZeroMQ |
| libunbound | 1.4.16 | `libunbound-dev` | NO | DNS resolver |
| libsodium | ? | `libsodium-dev` | NO | cryptography |
| libunwind | any | `libunwind8-dev` | YES | stack traces |
| liblzma | any | `liblzma-dev` | YES | for libunwind |
| libreadline | 6.3.0 | `libreadline6-dev` | YES | input editing |
| expat | 1.1 | `libexpat1-dev` | YES | XML parsing |
| GTest | 1.5 | `libgtest-dev` | YES | test suite |
| ccache | any | `ccache` | YES | compilation cache |
| Doxygen, Graphviz | any | `doxygen`, `graphviz` | YES | documentation |
| lrelease | ? | `qttools5-dev-tools` | YES | translations |
| libhidapi, libusb, libprotobuf, protoc, libudev | ? | see your package manager | YES | hardware wallet support |

On other distributions the package names are close cousins of the Debian ones: `boost` and `cmake` on Arch, `boost-devel` and `cmake` on Fedora and Void, and so on. If your distribution does not package one of the optional libraries, the build simply skips the corresponding feature rather than failing.

# Installing the dependencies

On Ubuntu and Debian, one command installs everything required:

```bash
sudo apt update && sudo apt install build-essential cmake pkg-config libboost-all-dev libssl-dev libzmq3-dev libpgm-dev libunbound-dev libsodium-dev git
```

On Windows, install [MSYS2][msys2-link] and work from its shell. Update it with `pacman -Syu`, then install the toolchain for your target, 64-bit in this example:

```bash
pacman -S mingw-w64-x86_64-toolchain make mingw-w64-x86_64-cmake mingw-w64-x86_64-boost mingw-w64-x86_64-icu mingw-w64-x86_64-openssl mingw-w64-x86_64-zeromq mingw-w64-x86_64-libsodium mingw-w64-x86_64-hidapi mingw-w64-x86_64-unbound git
```

For 32-bit targets the same line with `i686` in place of `x86_64` does the job.

On macOS, install Xcode and its command line tools first:

```bash
xcode-select --install
```

Then install Homebrew if you do not have it, and use the Brewfile from the repository to pull the dependencies:

```bash
brew update && brew bundle --file=contrib/brew/Brewfile
```

# Getting the source and building

Clone the repository with its submodules, since some vendored dependencies live in them:

```bash
git clone --recursive https://github.com/nerva-project/nerva.git
cd nerva
```

An existing clone is brought up to date the same way: `git submodule init && git submodule update`.

The simple build, on Linux and macOS:

```bash
make
```

On Windows only static builds are possible, so use one of:

```bash
make release-static-win64
make release-static-win32
```

The resulting executables land in `build/release/bin`: the `nervad` daemon, the `nerva-wallet-cli` and `nerva-wallet-rpc` wallets, and the assorted utilities. Add `-jN` to any make invocation to build with N parallel jobs, which on a modern machine is the difference between an afternoon and a coffee break.

A static build that does not depend on the libraries of the machine it was built on:

```bash
make release-static
```

And if the CPU is old enough to lack AES instructions, which shows up as the daemon crashing on startup with an illegal instruction:

```bash
make release-noaes
```

# Portable and cross-compiled builds

Dynamically and even statically linked binaries still target the processor they were built on. To produce binaries that run across a whole processor family, use the portable targets:

```bash
make release-static-linux-x86_64    # any POSIX x86_64
make release-static-linux-armv8     # any POSIX arm64
make release-static-mac-x86_64      # any macOS x86_64
make release-static-mac-armv8       # any macOS Apple Silicon
make release-static-win64           # any 64-bit Windows
```

with `linux-i686`, `linux-armv7`, `linux-armv6` and `win32` completing the set.

Cross compiling goes through the depends system and builds static binaries for another platform on the one you are sitting at:

```bash
make depends target=x86_64-w64-mingw32    # Windows binaries from Linux
make depends target=x86_64-apple-darwin   # macOS binaries from Linux
make depends target=aarch64-linux-gnu     # arm64 binaries from x86_64 Linux
```

The full list of targets and their extra apt requirements is in the `docs/BUILDING.md` file of the repository; the depends system has been exercised on Ubuntu 18.04 and 20.04, and works on their successors. Using depends from Windows Subsystem for Linux is often easier than MSYS2 if you are only after Windows binaries.

One caveat travels with static Linux builds: the produced binaries still link libc dynamically, so a binary built on a current distribution may refuse to run on an older one. Passing `-DBACKCOMPAT=ON` to CMake extends compatibility down to libc 2.17, which covers anything you are likely to encounter in the wild.

# Verifying what you built

The release process publishes hashes and signatures for official binaries, and the same discipline applies to your own. After building, record the hashes of the binaries you intend to deploy:

```bash
sha256sum build/release/bin/*
```

Genuinely paranoid builds start from a tagged release, verify the tag against the repository's history, and build in a clean environment; the multi-stage Dockerfile at the repository root is a reasonable starting point for that, since it compiles through the depends system in a defined container. For everyday use, building from `master` on a patched machine is fine, and the version string the daemon prints at startup will tell you exactly which commit you are running.

<!--Reference links -->
[msys2-link]: https://www.msys2.org
