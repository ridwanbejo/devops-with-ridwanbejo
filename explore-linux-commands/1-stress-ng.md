# Stress-ng examples

1. run CPU stress test upon 4 cores within 1 minute
```
stress-ng --cpu 4 --timeout 1m
```

2. run CPU stress test upon 4 cores within 1 minute by using square root CPU stressor methods and show compute metrics
```
stress-ng --cpu 4 --timeout 1m --cpu-method sqrt --metrics
```

cpu-method must be one of:

```
all ackermann bitops callfunc cdouble cfloat clongdouble
correlate crc16 decimal32 decimal64 decimal128 dither djb2a double euler explog fft
fibonacci float fnv1a gamma gcd gray hamming hanoi hyperbolic idct int128 int64 int32
int16 int8 int128float int128double int128longdouble int128decimal32 int128decimal64
int128decimal128 int64float int64double int64longdouble int32float int32double
int32longdouble jenkin jmp ln2 longdouble loop matrixprod nsqrt omega parity phi pi
pjw prime psi queens rand rand48 rgb sdbm sieve sqrt trig union zeta
```

3. run 2 virtual memory with forked stressors 4 within 1 minute and show metrics
```
stress-ng --vm 2  --fork 4 --timeout 1m --metrics
```

4. run 1 hdd with forked stressors 4 within 1 minute and show metrics
```
stress-ng --hdd 1 --fork 4 --timeout 1m --metrics
```

5. run mix of 3 I/O stressors within 1 minute then check disk SMART metadata
```
stress-ng --iomix 3 --smart --timeout 1m
```

6. run combination (cpu, memory, disk) of stress-test within 1 minute and show metrics

```
stress-ng --cpu 4 --vm 2 --hdd 1 --fork 4 --timeout 1m --metrics
```

7. run 4 matrix operations on CPU within 1 minutes

```
stress-ng --matrix 2 --timeout 1m --metrics
```

8. run virtual memory stressros within 1 minute then verify it

```
stress-ng --vm 2 --vm-bytes 2G  --timeout 1m --verify -v
```

# References:

1. https://github.com/ColinIanKing/stress-ng
2. https://wiki.ubuntu.com/Kernel/Reference/stress-ng
3. https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/8/html/optimizing_rhel_8_for_real_time_for_low_latency_operation/assembly_stress-testing-real-time-systems-with-stress-ng_optimizing-rhel8-for-real-time-for-low-latency-operation
4. https://manpages.debian.org/testing/stress-ng/stress-ng.1.en.html
