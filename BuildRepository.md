


---

# Overview #

The documented and supported method to build MP4v2 uses the GNU build system (also known as the Autotools). You must first obtain the sources by either downloading and extracting the source-distribution bundle or working directly MP4v2's Subversion repository. We have build documents for both methods, but unless you are a member of the MP4v2 project, you are **strongly encouraged** to use the source-distribution method.

On other supported platforms which lack Autotools we provide an alternative method for building the software. Please see the appropriate platform section.


---

# Introduction #

This document describes the recommended process to build MP4v2 from the repository. This process is a **superset** of the process to build from a source-distribution bundle.  If you are interested in building from a source-distribution bundle then this document is not for you.


---

# Quickstart #

This chapter is for the impatient or those just looking for a quick summary of all the commands used in a typical build. You may skip this summary and jump to [Build Process](#Build_Process.md).

```
svn checkout https://mp4v2.googlecode.com/svn/releases/2.0.0 mp4v2
cd mp4v2
autoreconf -fiv
rm -fr build/
mkdir build/
cd build/
../configure
make
make install
make install-man
make dist
```


---

# Build Process #

## Checkout Sources ##

Checkout sources from the project's Subversion repository.

Sources are checked out from either the trunk, release or a branch. This document was generated from one of those, and for example purposes, we will use exactly the same URL which used to create the distribution which contains this document.

If you are a project member, then you may add the appropriate login/password information as needed.

```
svn checkout https://mp4v2.googlecode.com/svn/releases/2.0.0 mp4v2
cd mp4v2
```

It is recommended to use Subversion 1.5.0 or higher. Lower versions might work.

## Boostrap (Autotools) ##

The following command causes forces Autotools to regenerate all files and install helper scripts needed at configure-time and to regenerate all files.

```
autoreconf -fiv
```

If you are a project member and preparing for a release, it is important to note that the versions of Autotools available in your path will directly effect files added to the bundle. At the time of writing, the following versions of Autotools are recommended; in some cases a minimum is hard-coded and warnings will be issued if in violation:

  * GNU autoconf 2.61 or higher (lower versions might work)
  * GNU automake 1.10 or higher (lower versions might work)
  * GNU libtool 1.5.26 or higher (lower versions might work)

## Configure ##

The following command configures the project for a build.  It is highly recommended that you invoke configure from an empty directory.

```
rm -fr build/
mkdir build/
cd build/
../configure
```

Please see `INSTALL` for details on configure usage, and standard options. Additionally, the following custom options have been added to `configure`:
<table cellpadding='4' border='1'>

<tr><td width='15%'><code>--disable-debug</code></td><td>Do not generate debug information.  Do not direct compiler to generate debugging information. By default the compiler will generate debug information if the platform supports it.</td></tr>

<tr><td width='15%'><code>--disable-optimize</code></td><td>Do not optimize. Do not direct compiler to optimize code. By default compiler optimization is enabled if the platform supports it.</td></tr>

<tr><td width='15%'><code>--disable-fvisibility</code></td><td>Do not set default ELF symbol visibility. By default configure attempts to detect if the compiler supports this feature. However on some platforms detecting incompatibilty of this feature might not be accurate in which case this option should be given.</td></tr>

<tr><td width='15%'><code>--disable-gch</code></td><td>By default certain platforms are marked to use GCC precompiled headers. Generally this greatly decrease build times but may require more diligence for iterative development; that is to say dependencies may not properly be tracked and more frequent <code>make clean</code> may be required when headers are changed. Use this option to disable GCC precompiled headers.</td></tr>

<tr><td width='15%'><code>--disable-largefile</code></td><td>On some 32-bit platforms or configurations it might be desirable to build without largefile (LFS) support. By default configure attempts to detect formal LFS support and enables it if found.</td></tr>

<tr><td width='15%'><code>--disable-util</code></td><td>Do not build/install utilities. This is convenience option for users who desire to skip building the utilities (eg. command-line executables) which are enabled by default.</td></tr>

<tr><td width='15%'><code>--enable-bi=ARCH</code></td><td>On bi-arch capable platforms it is possible to generate 32 or 64 bit code. This is supported by adding arguments <code>-m32</code> or <code>-m64</code>, respectively, when compiling or linking. Use this option to override the platform-specific default.</td></tr>

<tr><td width='15%'><code>--enable-ub[=ARCHS]</code></td><td>On OSX systems it is possible to generate universal binaries. This is supported by adding one or more argument patterns <code>-arch ARCH</code> when compiling or linking. Use this option to either target an architecture different from the platform default, or to produce universal binaries.</td></tr>

<tr><td width='15%'><code>--enable-dependency-tracking</code></td><td>Enable automatic dependency tracking for include-files. By default this feature is disabled.</td></tr>
<blockquote></table></blockquote>

## Build ##

The following command will build MP4v2.

```
make
```

On some platforms `make` refers to a BSD-flavor of make which is not compatible with this project. Check if `gmake` is installed, and if it is, substitute `gmake` wherever you may see `make` in this document. Otherwise you will need to install GNU make package version 3.81 or higher. Lower versions might work.

## Install ##

The following command will install MP4v2.

```
make install
make install-man
```

## Create Distribution ##

The following command will create a MP4v2 source distribution. It is during this step that shipped documentation is generated.

```
make dist
```

This step in the build process introduces additional requirements to the host system. While most of the following utilities are generally available, `help2man` is used to generate man-pages; however if this command is not available the man-pages will be empty. This is acceptable for non-release builds but for full quality builds this command is required.

  * GNU help2man 1.36 or higher (lower versions should work)
  * GNU tar 1.15.1 or higher (lower versions should work)
  * GNU gzip 1.3.10 or higher (lower versions should work)
  * bzip2 1.0.4 or higher (lower versions should work)
  * Info-ZIP zip 2.32 or higher (lower versions should work)

## Build Documentation ##

This step in the build process introduces some significant requirements to the host system:

  * GNU texinfo 4.8 or higher (lower versions should work)
  * Doxygen 1.5.7 or higher (lower versions should work)

Documentation that is shipped with source-distribution is generated as part of the [Build Process](#Build_Process.md) step. This section documents builds all of the supported methods to build documentation. There are three kinds of documentation in this project; man-pages, articles and API. **Documentation must be build from the repository**.

Man-pages are automatically generated for command-line utilities by using `html2man` which invokes standard options `--help` and `--version` and gleans the information, generating a man-page in **nroff** script. Note that the utilities will first be built as they are depdendencies of man-page generation.

Artcles are usually hand-written and authored in Texinfo format which permits macro-expansions, simple formatting and conversion to several popular formats using the GNU `makeinfo`. A Texinfo [manual](http://www.gnu.org/software/texinfo) is available.

API is documented inline to C and C++ source files, usually headers using Doxygen comment-formatting. Doxygen is then used to post-process these files and generate documentation in various formats. A Doxygen manual is available at it's main [site](http://www.doxygen.org) .

The project's goal is to document as thoroughly as possible the public API in MP4v2. Since we never have enough time to document everything, we try to set some priorities in this regard. Generally the public API is the highest priority to document. Next most important is probably underlying utility code which is shared by many developers; for example **libplatform**.

If you need examples of how to document C-compatible source see `include/mp4v2/mp4v2.h` and for C++-only source see `libplatform/io/File.h` .

The following table describes the various make targets available for building docs. Note you must first have completed the [Configure](#Configure.md) step.
<table cellpadding='4' border='1'>

<tr><td width='15%'><code>make man</code></td><td>Generate man-pages for command-line utilities.</td></tr>

<tr><td width='15%'><code>make html</code></td><td>Generate articles in HTML format from <code>texi</code> files.</td></tr>

<tr><td width='15%'><code>make txt</code></td><td>Generate articles in plaintext format from <code>texi</code> files.</td></tr>

<tr><td width='15%'><code>make wiki</code></td><td>Generate articles in Google Code Wiki format from <code>texi</code> files.</td></tr>

<tr><td width='15%'><code>make xml</code></td><td>Generate articles in (Texinfo) XML format from <code>texi</code> files.</td></tr>

<tr><td width='15%'><code>make api</code></td><td>Generate API in HTML format from header files.</td></tr>

<tr><td width='15%'><code>make articles</code></td><td>Convenience; the equivalent of <code>make html txt wiki xml</code> .</td></tr>

<tr><td width='15%'><code>make doc</code></td><td>Convenience; the equivalent of <code>make man articles api</code> .</td></tr>
<blockquote></table></blockquote>

And finally all of the document targets have a corresponding **clean** target which cleans up generated files. Simply prefix as follows:

```
make manclean
make htmlclean
make txtclean
make wikiclean
make xmlclean
make apiclean
make articlesclean
make docclean
```

## Post Site and API Documentation to project website. ##

This step is for project maintainers and can be used to update Site and API documentation. The following components are updated:

  * Featured Wiki article: BuildRepository
  * Featured Wiki article: BuildSource
  * MP4v2 (trunk) docs (includes Release Notes and other articles, and API docs).

This procedure may only be run from a **nix platform and has only been tested on Mac OS X.**<table cellpadding='4' border='1'>

<tr><td width='15%'><code>make google.clean</code></td><td>Clean any local working copy of google changeset.</td></tr>

<tr><td width='15%'><code>make google.post</code></td><td>Generate required docs, sparse-checkout google tree, remove files which are no longer generated, add new files which are generated, and update existing files.</td></tr>

<tr><td width='15%'><code>svn ci -m "-refreshed GoogleCode site+api docs." google/.</code></td><td>Check-in changes. This might take several minutes, especially if your upstream bandwidth is limited.</td></tr>
<blockquote></table></blockquote>


---

# Platform Notes #

MP4v2 builds on many unix-style platforms, also commonly referred to as posix-style systems. Building on Mac OS X, Linux, FreeBSD, Solaris, Cygwin, Windows are known to work.

Similar platforms should also work. Please see the following platform specific notes for instructions on commonly used options for various platforms.

## Mac OS X ##

Building on Mac OS X is well supported as it is used by maintainers of this project. The following are the recommended specifications for this platform; but is not necessarily the only configuration that is possible:

  * Mac Intel hardware
  * Mac OS X 10.5.7
  * Xcode-3.1.2
  * gcc 4.0.1 (Apple Inc. build 5493)
  * gcc 4.2.1 (Apple Inc. build 5570)

> **Note:** It is recommended to use the platform distribution's bundled compiler for maximum C++ compatibility. If you build with a custom compiler it will likely introduce non-standard runtime requirements for your users. There are of course many valid reasons to build with unbundled compilers, but be aware that is generally unsupported and left as an exercise to the reader.

### Default Binaries ###

The preferred method to produce default binaries is to run configure without any options which will compile with debug+optimize and produce static+shared libraries and dynamic executables.

```
../configure
```

### Release Binaries ###

The preferred method to produce binaries suitable for releases, (ie. which does not contain debug information) is to pass the following to configure:

```
../configure --disable-debug
```

### Developer Binaries ###

The preferred method to produce binaries suitable for development is to pass the following to configure.  Default Binaries will work, however for the best debugging experience we recommend no optimize and no static libraries.

```
../configure --disable-optimize --disable-static
```

### Universal Binaries - all architectures ###

The preferred method to produce universal binaries for all supported architectures is to pass the following option to configure. As of this writing, architectures { i386, x86\_64, ppc, ppc64 } are built.

```
../configure --enable-ub
```

### Universal Binaries - selected architectures ###

The preferred method to produce universal binaries for selected architectures is to specify a comma-delimited list specifying the desired architecture identifiers. Passing the following option will produce universal binaries for architectures { i386, x86\_64 }.

```
../configure --enable-ub=i386,x86_64
```

## Linux ##

Building on Linux is well supported as it is used by maintainers of this project. The following are the recommended specifications for this platform; but is not necessarily the only configuration that is possible:

  * Intel 32-bit or 64-bit hardware
  * Fedora 10, gcc 4.3.2
  * gcc 3.4.0 or higher is reported to work

> **Note:** It is recommended to use the platform distribution's bundled compiler for maximum C++ compatibility. If you build with a custom compiler it will likely introduce non-standard runtime requirements for your users. There are of course many valid reasons to build with unbundled compilers, but be aware that is generally unsupported and left as an exercise to the reader.

### Default Binaries ###

The preferred method to produce default binaries is to run configure without any options which will compile with debug+optimize and produce static+shared libraries and dynamic executables.

```
../configure
```

### Release Binaries ###

The preferred method to produce binaries suitable for releases, (ie. which does not contain debug information) is to pass the following to configure:

```
../configure --disable-debug
```

### Developer Binaries ###

The preferred method to produce binaries suitable for development is to pass the following to configure.  Default Binaries will work, however for the best debugging experience we recommend no optimize and no static libraries.

```
../configure --disable-optimize --disable-static
```

### Bi-arch compilation ###

The preferred method to produce a bi-arch binary is to specify the target (eg. 32-bit) with the following option to configure. This example will produce a 32-bit binary if compiling on a platform which defaults to producing 64-bit binaries. The inverse is also possible.

```
../configure --enable-bi=32
```

> **Warning:** Currently bi-arch cross-compilation is not supported due to a bug with libtool which fails miserably during linking.

## FreeBSD ##

Building on FreeBSD is supported. The following are the recommended specifications for this platform; but is not necessarily the only configuration that is possible:

  * Intel 32-bit or 64-bit hardware
  * FreeBSD 7.0 Release, gcc 4.2.1
  * gcc 3.4.0 or higher is reported to work

> **Note:** It is recommended to use the platform distribution's bundled compiler for maximum C++ compatibility. If you build with a custom compiler it will likely introduce non-standard runtime requirements for your users. There are of course many valid reasons to build with unbundled compilers, but be aware that is generally unsupported and left as an exercise to the reader.

### Default Binaries ###

The preferred method to produce default binaries is to run configure without any options which will compile with debug+optimize and produce static+shared libraries and dynamic executables.

```
../configure
```

### Release Binaries ###

The preferred method to produce binaries suitable for releases, (ie. which does not contain debug information) is to pass the following to configure:

```
../configure --disable-debug
```

### Developer Binaries ###

The preferred method to produce binaries suitable for development is to pass the following to configure.  Default Binaries will work, however for the best debugging experience we recommend no optimize and no static libraries.

```
../configure --disable-optimize --disable-static
```

### Bi-arch compilation ###

The preferred method to produce a bi-arch binary is to specify the target (eg. 32-bit) with the following option to configure. This example will produce a 32-bit binary if compiling on a platform which defaults to producing 64-bit binaries. The inverse is also possible.

```
../configure --enable-bi=32
```

> **Warning:** Currently bi-arch cross-compilation is not supported due to a bug with libtool which fails miserably during linking.

## Solaris ##

Building on Solaris is supported. The following are the recommended specifications for this platform; but is not necessarily the only configuration that is possible:

  * Intel 32-bit or 64-bit hardware
  * Solaris 10u6, gcc 3.4.3
  * gcc 3.4.0 or higher is reported to work

> **Note:** It is recommended to use the platform distribution's bundled compiler for maximum C++ compatibility. If you build with a custom compiler it will likely introduce non-standard runtime requirements for your users. There are of course many valid reasons to build with unbundled compilers, but be aware that is generally unsupported and left as an exercise to the reader.

> **Note:** Solaris does not (yet) really bundle a compiler. The recommendation is to use the companion-disk compiler for maximum C++ runtime compatibility. It is usually found in `/usr/sfw/bin`.

### Default Binaries ###

The preferred method to produce default binaries is to run configure without any options which will compile with debug+optimize and produce static+shared libraries and dynamic executables.

```
../configure
```

### Release Binaries ###

The preferred method to produce binaries suitable for releases, (ie. which does not contain debug information) is to pass the following to configure:

```
../configure --disable-debug
```

### Developer Binaries ###

The preferred method to produce binaries suitable for development is to pass the following to configure.  Default Binaries will work, however for the best debugging experience we recommend no optimize and no static libraries.

```
../configure --disable-optimize --disable-static
```

### Bi-arch compilation ###

The preferred method to produce a bi-arch binary is to specify the target (eg. 32-bit) with the following option to configure. This example will produce a 32-bit binary if compiling on a platform which defaults to producing 64-bit binaries. The inverse is also possible.

```
../configure --enable-bi=32
```

> **Warning:** Currently bi-arch cross-compilation is not supported due to a bug with libtool which fails miserably during linking.

## Cygwin ##

Building on Cygwin is supported. The following are the recommended specifications for this platform; but is not necessarily the only configuration that is possible:

  * Intel 32-bit or 64-bit hardware
  * Cygwin, gcc 4.3.2
  * gcc 3.4.0 or higher is reported to work

> **Note:** As of this writing, Cygwin has available to it several versions of gcc; only one of which may be found and used in the path as `gcc` and `g++`. Configure will thus find what is probably the older more stable version of gcc in a typical Cygwin environment. If you desire to build with the newer gcc, it is found in the path as `gcc-4` and `g++-4` respectively and you must indicate to configure the desired versions. Defining the following variables beforing running configure should do the trick:

```
setenv CC gcc-4
setenv CXX gcc-4
../configure
```

### Default Binaries ###

The preferred method to produce default binaries is to run configure without any options which will compile with debug+optimize and produce static+shared libraries and dynamic executables.

```
../configure
```

### Release Binaries ###

The preferred method to produce binaries suitable for releases, (ie. which does not contain debug information) is to pass the following to configure:

```
../configure --disable-debug
```

### Developer Binaries ###

The preferred method to produce binaries suitable for development is to pass the following to configure.  Default Binaries will work, however for the best debugging experience we recommend no optimize and no static libraries.

```
../configure --disable-optimize --disable-static
```

## Windows ##

Native builds on Windows is supported via Microsoft's Visual Studio package. Both the commercial version and free version (express) are known to work. The following are the recommended specifications for this platform; but is not necessarily the only configuration that is possible:

  * Intel 32-bit or 64-bit hardware
  * Windows 2000 or higher, Visual Studio 9.0 (aka. Visual Studio 2008)
  * Visual Studio 9.0 Express is reported to work

Only 32-bit binaries are targeted, and win32-API is set to Windows 2000 or higher. Older versions of Windows, or win32-API are not supported.

MP4v2 has directory `vstudio9.0/` which contains the necessary solution+project files to produce a basic build of libmp4v2's DLL and several command-line executables. Enabling things such as debugging, optimization, etc, are all left as an exercise to the user.

> **Warning:** Project meta-data is stored in header `project.h`. A proper source distribution is built using autotools and generates `TOP/include/mp4v2/project.h` correctly, which is then bundled with our source distribution. This is adequate for building on the Windows platform.

> However, if you are building from the repository, be warned that there is no method to automatically generate `project.h` on Windows. Instead, we periodically checkin a copy of this file (generated using autotools) as `vstudio9.0/include/mp4v2/project.h` which may from time to time get out of date. If it is significantly out of date, you should find the latest source distribution and copy the `project.h` from there.