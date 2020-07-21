# exec_diffing

## example test
### Dependencies
```
apt-get install clang llvm
```

### Building
```bash
$ cd cctdb/examples/brokenQuicksort
$ clang++ -g -fsanitize=address -fsanitize-coverage=trace-pc-guard brokenQuicksort.cpp -o brokenQuicksort
$ ASAN_OPTIONS=coverage=1 ./brokenQuicksort 1 6 3 9 0
  0 1 3 6 9 
  SanitizerCoverage: ./brokenQuicksort.11064.sancov: 15 PCs written
$ ASAN_OPTIONS=coverage=1 ./brokenQuicksort 1 6 5 9 0
  0 1 9 6 5 
  SanitizerCoverage: ./brokenQuicksort.11076.sancov: 16 PCs written
```

### Getting source code execution path
```bash
# Get source code level execution path and diff
$ sancov -print brokenQuicksort.11064.sancov | llvm-symbolizer -obj brokenQuicksort > no_bug.log
$ sancov -print brokenQuicksort.11076.sancov | llvm-symbolizer -obj brokenQuicksort > bug.log
```

### Diffing
```
$ vimdiff no_bug.log bug.log
```

### Result
![result](https://github.com/SuhwanSong/exec_diffing/blob/master/screenshot.png)
