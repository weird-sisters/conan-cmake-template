# Template application: Conan & CMake

Conan is a package manager for C and C++:

- Docs: [docs.conan.io][conanDocs];
- Github repository: [github/conan-io/conan][conanGithub].

CMake is a set of tools to help build, test and package software:

- Site: [cmake.org][cmakeSite]

[conanDocs]: https://docs.conan.io/en/latest/
[conanGithub]: https://github.com/conan-io/conan
[cmakeSite]: https://cmake.org/

This repository presents a project structure and development workflow based on Conan and CMake. It is expected to work on any supported platform.

## Topics

- [Template application: Conan \& CMake](#template-application-conan--cmake)
  - [Topics](#topics)
  - [Conan overview](#conan-overview)
  - [Install Conan](#install-conan)
  - [Detect Conan's default profile](#detect-conans-default-profile)
    - [Show profile](#show-profile)
    - [GCC `libstdc++` vs `libstdc++11`](#gcc-libstdc-vs-libstdc11)
  - [CMake overview](#cmake-overview)
  - [Install CMake](#install-cmake)
  - [Conan \& CMake](#conan--cmake)
    - [Create new project](#create-new-project)
    - [Build and execute](#build-and-execute)

## Conan overview

One of the main distinctive features of Conan is its the ability to manage binary packages.

In order to do that it has to properly handle a diversity of architectures, compilers, standard libraries, cross-compilation strategies etc. Also it must provide a way for package authors to express how these features affect binary compatibility.

Conan can also compile missing dependencies and store them locally, alongside downloaded binaries, so it's possible to share them between projects.

## Install Conan

It requires Python 3 and the recommended way to install Conan is with `pip`:

```bash
pip install conan
```

## Detect Conan's default profile

Conan handles the build tools to be used in particular situations with profiles:

```bash
conan profile list
```

Right after installation there are no profiles yet:

```output
No profiles defined
````

It's possible to create a default profile by detecting the toolchains available:

```bash
conan profile new default --detect
```

Sample output on MacOS:

```output
Found apple-clang 12.0
Profile created with detected settings: /Users/USER/.conan/profiles/default```
```

### Show profile

Be sure of what Conan detected; eventually some customizations are in order:

```bash
conan profile show default
```

Sample output on a MacOS host:

```output
Configuration for profile default:

[settings]
os=Macos
os_build=Macos
arch=x86_64
arch_build=x86_64
compiler=apple-clang
compiler.version=12.0
compiler.libcxx=libc++
build_type=Release
[options]
[conf]
[build_requires]
[env]
```

### GCC `libstdc++` vs `libstdc++11`

If GCC (>=5.1) is being used, `conan detect` selects the old runtime library (`libstdc++`) for backward compatibility. So for GCC systems the following command can be issued afterward profile detection to update the runtime library:

```bash
conan profile update settings.compiler.libcxx=libstdc++11 default
```

## CMake overview

CMake is designed to support cross-platform C and C++ projects building, testing and packaging. It can handle the most challenging builds in a uniform manner with the help of well written build instructions and a proper configuration. Even this configuration step is usually performed automatically.

CMake is not properly a build system, but it can detect and handle build systems present in the host. Once it succeeds in this detection step, it generates the required project files to actually build the project.

CMake presents itself as a command line tool: `cmake`. To be used it must be installed and accessible:

```bash
cmake --version
```

For a MacOS host this command can output:

```output
cmake version 3.24.3

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

## Install CMake

If CMake is not already present it's possible to download an installer from the official site.

- Download CMake: [cmake.org/download][cmakeDownload].

[cmakeDownload]: https://cmake.org/download/

## Conan & CMake

Although CMake supports dependency management, it is not designed to be a dependency manager. The workflow proposed here combines Conan and CMake to setup cross-platform projects featuring:

- Build and test automation;
- Automatic dependency fetching:
- Versioning, packaging and publishing.

### Create new project

Despite being a package manager, Conan has some features that come very handy for rapid project prototyping.

> Be sure to the documentation: [Package scaffolding for conan new command][conanNewDocs].

Its subcommand `new` generates project skeletons based on a template specification:

```bash
mkdir hello
cd hello
conan new hello/0.1 --template=cmake_exe
```

The project generated by the above `new` invocation can be compared with the one found in the [hello-sample](./hello-sample) directory, which was generated the exact same way.

[conanNewDocs]: https://docs.conan.io/en/1.53/extending/template_system/command_new.html

### Build and execute

Inside the `hello` directory the following command can be used to create a package for the `hello` application and to perform a based check on the generated package:

```bash
conan create -pr:b=default . demo/testing
```
