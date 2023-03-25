# CMake and package managers

CMake can work with several packaage managers.

## vcpkg

When vcpkg is installed in the directory `VCPKG_DIR`, you can CMake
make aware of it by passing the
```bash
$ cmake  -S .  -B build/ \
      -DCMAKE_TOOLCHAIN_FILE=$VCPKG_DIR/scripts/buildsystems/vcpkg.cmake
```

Next, build as always:
```bash
$ cmake --build build/
```
