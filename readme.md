# Python scripts for processing USPTO inventor and patent data

The following collection of scripts performs pre- and
post-processing on patent data as part of the patent
inventor disambiguation process.


Join the [Google discussion
group](https://groups.google.com/forum/?fromgroups=#!forum/disambiguation),
let us know how you're using the code.

Follow development, subscribe to
[RSS
feed](https://github.com/funginstitute/patentprocessor/commits/master.atom).

## Processing patents

There are two ways to get started:

* Run `preprocess.sh`. This will run the preprocessor on any files
  contained in the PATENTROOT directory (defaults to the root directory)

* run `parse.py` directly to customize which directories are processed and
  which regex is used to process the files. Run `parse.py -h` to see the
  relevant command-line options. Follow with `clean.py` then
`consolidate.py` to obtain a full set of tables.


See "Configuring the Preprocessing Environment" below.


## Contributing to the Patent Processor Project

Contributions are welcome, for source code development, testing
(including validation and verification), uses cases, etc. We're
targeting general PEP-compliance, so even an issue noting where we could
do better is appreciated.

### Contributing code

Pull requests are especially welcome. Here are a few pointers which will make everything easier:

* Small, tightly constrained commits.
* New files should be in their own commit, and committed before they are used in subsequent commits.
* Commits should tell a story in a logical sequence. It should be possible to understand the gist
  of the development just from reading the commits (hard, but worthwhile goal).
* The ideal commit:
    * Unit (or similar) test for a single functionality.
    * Implementation to pass the unit test.
    * Documentation (the "why") of the function/method in the appropriate location (platform dependent).
    * 0 or 1 use of the new functionality in production.
    * Further uses of functionality should go in future commits.
* Formatting updates, code cleanup and renaming should go into independent commits.
* Submit only code which is covered by *working* unit tests.
* Testing scripts, including unit tests, integration tests and functional tests go in the `test` directory.
* Code which does work goes in the lib directory.
* Code which provides a workflow (i.e., processing patents or building necessary
  infrastructure) goes in the top level directory. In the future, much of this code may
  be put into a `bin` directory.
* Test code should follow the pattern `test/test_libfile.py`. This pattern may change in
  the future, whence this documentation will change at that time.

**You must rebase before issuing a pull request: `git pull --rebase <upstream> master`**.

### Coding style

Start with [PEP8](http://www.python.org/dev/peps/pep-0008/). A very
large number of extremely intelligent software engineers working at the
wealthiest corporations on the planet have more or less agreed on a
standard set of conventions allowing J. Random Coder (that's you and I)
to read and write Python like the Big Boys and Girls (PyLadies!).
It *highly* unlikely we can improve on these guidelines.

That said, rules are rules and exists to broken once in a while.
So, optimize for readability.  Specifically:

* Use vowels, not secret shorthand 1337 cmptr cd fr nmng vrbls.
* Line length to 80 characters, no more.


## Configuring the Preprocessing Environment

The python-based preprocessor is tested on Ubuntu 12.04 and MacOSX 10.6.
Any flavor of unixen with the following installed should work:

* Python 2.7.3
* scipy package for Python
* sqlite3 --> Note: you need version 3.7.15.1 or higher 
* More? Please [file an
  issue](https://github.com/funginstitute/patentprocessor/issues) if you find another dependency.

In order to properly configure the preprocessing environment, the end user must
manually perform the following:

* Download the relevant XML files which need to be processed. These can be
  placed in any directory, but `parse.py` assumes the root directory `/`.

* Set the PATENTROOT environment variable (assuming a UNIX environment). This
  can be customized later using command line options, but for the purposes of
  running unit tests, it is necessary to actually export the environment
  variable, e.g.

  `export PATENTROOT = /path/to/xml/files`

* You *do not* need to have the `.sqlite3` files already in the project root 
  directory, as they will be automatically generated by `parse.py`.

### Configuring for testing

Currently, testing requires having the environment configured as above,
and having some of the processing results. That is, testing the
"cleaning" phase requires having files from the parse phase.

This is sub-optimal for testing.

In the future, the small test xml file in the fixtures directory can be
used to construct mock data such that testing the cleaning phase can
proceed independently of any other phase.


## Disambiguating inventor names

Inventor disambiguation is performed using order restricted Bayesian
inference, [implemented in c++ and hosted on
github](https://github.com/funginstitute/disambiguator).
The application compiles and runs on both Ubuntu Linux 12.04 and
MacOS Snow Leopard (OSX 10.6). An external library for solving a
quadratic program is required, which is free for academic use.



## Postprocessing

After disambiguating, the resulting unique inventors are listed by number in an output file
(e.g., `final.txt`). This output file then processed to tie the inventor number to the
inventor name in the input database. The program which performs this linking is in the
`c++` disambiguation code. After the linking is done, verification proceeds.

### Verification

Verifying the results of the disambiguation is performed using the `benchmark.py` script,
which compares the linked results database with a (specially formatted) CSV file containing
a list of inventor-patent instances which have been verified by direct communication with
each inventor.

----

[Contributors](https://github.com/doolin/patentprocessor/graphs/contributors)


