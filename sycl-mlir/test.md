# 测试结果
```bash
ninja check-mlir-sycl 

Testing Time: 0.38s
Total Discovered Tests: 30
  Passed: 30 (100.00%)
```

```bash
ninja check-cgeist

Testing Time: 64.47s
Total Discovered Tests: 263
  Passed           : 252 (95.82%)
  Expectedly Failed:  11 (4.18%)
```

```bash
ninja check polygeist-opt
```