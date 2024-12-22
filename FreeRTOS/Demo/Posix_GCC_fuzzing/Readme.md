# Fuzzing Demo

## Build

```
make PROFILE=1 SANITIZE_FUZZER=1 USER_DEMO=FUZZ_DEMO SANITIZE_ADDRESS=1
```

## Execution 

```
cd build
./fuzz_demo -max_len=1028 # May need to play around with this
```

