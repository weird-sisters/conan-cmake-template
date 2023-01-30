# Template application: Conan & CMake

Conan is a package manager for C and C++:

- Docs: [docs.conan.io][conanDocs];
- Github repository: [github/conan-io/conan][conanGithub].

[conanDocs]: https://docs.conan.io/en/latest/
[conanGithub]: https://github.com/conan-io/conan

## Topics

- [Template application: Conan \& CMake](#template-application-conan--cmake)
  - [Topics](#topics)
  - [Overview](#overview)
  - [Install Conan](#install-conan)
  - [Detect profile](#detect-profile)
    - [Show profile](#show-profile)
    - [GCC `libstdc++` vs `libstdc++11`](#gcc-libstdc-vs-libstdc11)
  - [Create new project](#create-new-project)

## Overview

One of the main distinctive features of Conan is its the ability to manage binary packages.

In order to do that it has to properly handle a diversity of architectures, compilers, standard libraries, cross-compilation strategies etc. Also it must provide a way for package authors to express how these features affect binary compatibility.

Conan can also compile missing dependencies and store the locally, alongside downloaded binaries, so it's possible to share them between projects.

## Install Conan

It requires Python 3 and the recommended way to install Conan is with `pip`:

```bash
pip install conan
```

## Detect profile

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

Sample output on MacOS:

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

## Create new project

Despite being a package manager, Conan has some features that come very handy for rapid project prototyping.

Its subcommand `new` generates project skeletons based on a template specification:

```bash
conan new basic -d name=mygame -d requires=math/1.0 -d requires=ai/1.3
```

> Be sure to the documentation: [Package scaffolding for conan new command][conanNewDocs].

[conanNewDocs]: https://docs.conan.io/en/1.53/extending/template_system/command_new.html
