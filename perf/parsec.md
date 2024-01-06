### Notes
* PARSEC (Princeton Application Repository for Shared-Memory Computers) is a collection of parallel programs which can be used for performance studies of multiprocessor machines.
* Building the benchmark suite can take between 30-60 minutes.
* Running `parsecmgmt -a run` will verify all benchmarks have been built successfully and are executable. It is not meant to be used a performance measurement.
* Parsec includes build configuraitons which influence how to compile a program. This includes `gcc` which will build a parallel version of the suite with gcc and `gcc-serial` which will build a serial version of the suite.
* Region of interest
 * test - minimal input to verify programs are executable.
 * simdev - Small input which causes code execution compare to a typical input.
 * native - Very large input intended for large-scale experiments on real machines. Expect minutes to complete.
* test & simdev should not be used for performance analysis.

### Examples
~~~
parsecmgmt -a build -p x264 blackscholes -c gcc
~~~
Build the x264 and blackscholes tests with gcc.

~~~
parsecmgmt -a build -p netapps
~~~

Run the x264 and blackholes tests

~~~
parsecmgmt -a run -p x264 blackscholes -i simlarge
~~~


Build all network benchmarks.

### Resources
* https://github.com/connorimes/parsec-3.0