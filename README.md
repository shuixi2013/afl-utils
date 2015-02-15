# afl-utils

Some utilities to automate crash sample processing and analysis for crashes
found with [american-fuzzy-lop (afl)](http://lcamtuf.coredump.cx/afl/).

#### Changelog

Release | Description
:-------:|----
0.10a | Initial release, just collect crash sample files
0.11a | Crash sample file list creation added, afl_vcrash added
0.12a | gdb+exploitable script generation added
0.13a | Auto-cleanup of invalid crash samples added
0.14a | gdb+exploitable script execution and output parsing added for easy crash classification
0.15a | Code refactoring, minor bug fixes

#### Dependencies

* Python3
* [Exploitable](https://github.com/jfoote/exploitable) (for script execution support)

#### Problems / Bugs / TODO

* `avl_vcrash` might miss *some* invalid crash samples. Identification of real crashes is
  hard and needs improvements!
* `avl_vcrash` identifies *some* crash samples as invalid that are considered valid by
  `afl-fuzz` when run with option `-C`.
* Tool outputs might get cluttered if core dumps/kernel crash messages are displayed on
  your terminal.
* gdb+exploitable script execution will be interrupted when using samples that do not lead
  to actual crashes. `afl_collect` will print the files name causing the trouble (for manual
  removal).
* The more advanced features like gdb+exploitable script generation and execution as well as
  crash sample verification *probably will* fail for targets that don't read their input from
  files (`afl-fuzz` invoked without `-f <filename>`) but from `stdin`. I didn't try this yet.

#### afl\_collect

`afl_collect` is a Python3 utility that copies all crash sample files from an afl
synchronisation directory into a single location providing easy access for
further crash analysis. The afl synchronisation directory is created when using
multiple fuzzer instances in parallel. Furthermore `afl_collect` has some more advanced
features like generating and executing `gdb` scripts that make use of
[Exploitable](https://github.com/jfoote/exploitable). The purpose of these scripts is to
automate crash sample classification.  

Usage:  

![afl_collect_usage](https://raw.githubusercontent.com/rc0r/afl-utils/master/.scrots/afl_collect_usage.png)

Sample output:

![afl_collect_sample](https://raw.githubusercontent.com/rc0r/afl-utils/master/.scrots/afl_collect_sample.png)

#### afl\_vcrash

afl\_vcrash verifies that afl-fuzz crash samples lead to crashes in the target
binary.

Usage:

![afl_vcrash_usage](https://raw.githubusercontent.com/rc0r/afl-utils/master/.scrots/afl_vcrash_usage.png)
  
