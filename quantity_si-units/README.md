### Python Unit Library using Rust
### Contains wheel for Python39
@ https://itt-ustutt.github.io/quantity/examples.html#pressure-of-an-ideal-gas 

```
git clone https://github.com/itt-ustutt/quantity
cd quantity

nano si-units/Cargo.toml
```
make last para like this (remove all 'features' except one below)
```
[dependencies.pyo3]
version = "0.16"
features = ["extension-module"]
```
Android specific symlinks
```
ln -s /apex/com.android.runtime/lib64/bionic/libdl.so $PREFIX/lib/libdl.so
ln -s /system/lib64/ld-android.so $PREFIX/lib/ld-android.so
```
Main build command to generate wheel
```
export CARGO_BUILD_TARGET=aarch64-linux-android && maturin build --release -m si-units/Cargo.toml
```
