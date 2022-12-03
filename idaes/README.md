### This directory has compiled stuff for ARM64 Android as provided by [idaes-ext](https://github.com/IDAES/idaes-ext)
### Petsc doesn't require any python compatibilty & will run on its own

For `idaes-pse` to work correctly you have to first install `ipopt`& related libraries also hosted in [this git](https://github.com/defencedog/arm64-Android-Termux-Builds/tree/main/IPOPT_ARM64_ANDROID). Don't fret its simple extracting & then copying / pasting stuff!

Location to place all libraries / linkers for `idaes` to perform correctly 
```
$HOME/.idaes/bin
```
If you have similar Android architecture you can copy these `*.so` in above location. A total of x5 `.so` files are here & are required for running jupyter notebooks maintained at [idaes-examples](https://github.com/IDAES/examples-pse/tree/main/scripts)

### Petsc solver
The `share` folder which hosted many `petsc` documentation / examples is omitted to reduce archive size. This can be attained via online documentation. <br>
The `petsc` archive folder `idaes_linkers` contents to be placed in `$HOME/.idaes/bin` Other x2 folders in `$PREFIX/local` <br>
Go through `README.txt`; **Important** one `.py` file in `python3.9/site-packages...` require a patch
