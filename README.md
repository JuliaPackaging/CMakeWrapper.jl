# CMakeWrapper

![CI](https://github.com/aminya/CMakeWrapper.jl/workflows/CI/badge.svg)
[![codecov.io](http://codecov.io/github/JuliaPackaging/CMakeWrapper.jl/coverage.svg?branch=master)](http://codecov.io/github/JuliaPackaging/CMakeWrapper.jl?branch=master)

This package provides a [BinDeps.jl](https://github.com/JuliaLang/BinDeps.jl)-compatible `CMakeProcess` class for automatically building CMake dependencies.

A modern version of CMake is installed using the [CMake.jl](https://github.com/JuliaPackaging/CMake.jl) package; you can use
that package instead if you just want to run `cmake` by itself without using BinDeps.

# Installation

    julia> Pkg.add("CMakeWrapper")

# Usage

You can declare a `CMakeProcess` similarly to the way you would use the `Autotools` provider in BinDeps.jl. In your `deps/build.jl` file, this would look like:

    provides(Sources,
        URI(source_url),
        dependency_name)

    provides(BuildProcess, CMakeProcess(),
             dependency_name)

where `source_url` and `dependency_name` are set elsewhere in your `build.jl`.

You can also pass raw cmake options directly with the `cmake_args` flag:

    provides(BuildProcess, CMakeProcess(cmake_args=["-DCMAKE_BUILD_TYPE=Debug"]),
             dependency_name)

If the high-level provider doesn't work for you, you can also use the lower-level `CMakeBuild`, analogous to the `AutotoolsDependency` in BinDeps.jl:

    CMakeBuild(srcdir=source_dir,  # where the CMakeLists.txt resides in your source
               builddir=build_dir,  # where the cmake build outputs should go
               prefix=install_prefix,  # desired install prefix
               libtarget=[library_name],  # name of the library being built
               installed_libpath=[path_to_intalled_library],  # expected installed library path
               cmake_args=[],  # additional cmake arguments
               targetname="install")  # build target to run (default: "install")
