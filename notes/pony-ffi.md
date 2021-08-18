
# Missing library

```
ld: library not found for -lwhatevername
```

Check that your library is in the library path. You can try
running the `ld` command ponyc prints (remove quotes around `-l"whatevername"`) and add `-L $(pwd)`. There needs to be a `.dylib` in the `pwd` for this to work.
