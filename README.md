# Template application: Conan

Conan is a package manager for C and C++:

- Docs: [docs.conan.io][conanDocs];
- Github repository: [github/conan-io/conan][conanGithub].

[conanDocs]: https://docs.conan.io/en/latest/
[conanGithub]: https://github.com/conan-io/conan

## Overview

One of the main distinctive features of Conan is its the ability to manage binary packages.

In order to do that it has to properly handle a diversity of architectures, compilers, standard libraries, cross-compilation strategies etc. Also it must provide a way for package authors to express how these features affect binary compatibility.

Conan can also compile missing dependencies and store the locally, alongside downloaded binaries, so it's possible to share them between projects.

## Instal Conan

It requires Python 3 and the recommended way to install Conan is with `pip`:

```bash
pip install conan
```

## Detect profile

Conan handles the build tools to be used in particular situation with profiles:

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

### GCC `libstdc++` vs `libstdc++11`

If GCC (>=5.1) is being used, `conan detect` selects the old runtime library (`libstdc++`) for backward compatibility. So for GCC systems the following command can be issued afterward profile detection to update the runtime library:

```bash
conan profile update settings.compiler.libcxx=libstdc++11 default
```
