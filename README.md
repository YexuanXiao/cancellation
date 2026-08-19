# Dxxxx Collaborative cancellation for coroutines

[source](https://github.com/YexuanXiao/cancellation/blob/master/proposal.bs)

[html](https://storage.nykz.org/proposals/cancellation/)

## Build

```shell
git clone https://github.com/YexuanXiao/llvm-project.git -b coro-260819 --depth=1
cd llvm-project
cmake -S llvm -B build -G Ninja \
  -DCMAKE_BUILD_TYPE=Release \
  -DLLVM_ENABLE_PROJECTS=clang \
  -DLLVM_ENABLE_RUNTIMES="libcxx;libcxxabi;libunwind" \
  -DLIBCXX_ABI_VERSION=2 \
  -DLIBCXX_INCLUDE_BENCHMARKS=ON \
  -DLLVM_USE_LINKER=lld \
  -DBUILD_SHARED_LIBS=ON
cmake --build build --target clang cxx cxxabi
```

## Run benchmarks

```shell
cd llvm-project
libcxx/utils/libcxx-lit -b build -sv \
  build/runtimes/runtimes-bins/libcxx/test/benchmarks/coroutines/coroutine_cancellation.bench.cpp \
  build/runtimes/runtimes-bins/libcxx/test/benchmarks/coroutines/coroutine_cancellation_stock.bench.cpp
cat build/runtimes/runtimes-bins/libcxx/test/benchmarks/coroutines/Output/coroutine_cancellation_stock.bench.cpp.dir/benchmark-result.txt
cat build/runtimes/runtimes-bins/libcxx/test/benchmarks/coroutines/Output/coroutine_cancellation.bench.cpp.dir/benchmark-result.txt
```

## Run new tests

```shell
cd llvm-project

./build/bin/llvm-lit ./llvm/test/Transforms/Coroutines/coro-cancel.ll
./build/bin/llvm-lit ./clang/test/SemaCXX/coro-cancel.cpp \
               ../clang/test/CodeGenCoroutines/coro-cancel.cpp
```

## Run full tests

```shell
cd llvm-project
cmake --build build --target check-clang check-cxx
```
