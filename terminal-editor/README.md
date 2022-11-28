### RUST can be installed from Termux default repo (not needed by these builds)
### Two editors are included
- [x] https://github.com/jmacdonald/amp
- [x] https://github.com/zee-editor/zee
- [ ] Very buggy https://github.com/zyedidia/micro

Check dependencies & make sure no library is missing
```
amp:
        libz.so.1 => /data/data/com.termux/files/usr/lib/libz.so.1
        libdl.so => /data/data/com.termux/files/usr/lib/libdl.so
        libc.so => /system/lib64/libc.so
        ld-android.so => /data/data/com.termux/files/usr/lib/ld-android.so
zee:
        libdl.so => /data/data/com.termux/files/usr/lib/libdl.so
        libssl.so.3 => /data/data/com.termux/files/usr/lib/libssl.so.3
        libcrypto.so.3 => /data/data/com.termux/files/usr/lib/libcrypto.so.3
        libz.so.1 => /data/data/com.termux/files/usr/lib/libz.so.1
        libc.so => /system/lib64/libc.so
        libm.so => /data/data/com.termux/files/usr/lib/libm.so
        ld-android.so => /data/data/com.termux/files/usr/lib/ld-android.so
```
Copy these two `$PREFIX/local/bin`
