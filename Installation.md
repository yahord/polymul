# Installation #
You install polymul by copying the include file `polymul.h` to a suitable location where
your compiler can find it. However, because performance depends strongly on how your compiler
will inline the recursive template functions used, it can be advantageous to play with
settings for inline limits. You can also turn off bounds checking by compiling with -DNDEBUG.
You can compile and run `benchmark.cpp` to compare the results  of different settings.

An alternative to automatic inlining is to use explicitly inlined multiplication "tables". Sometimes significant improvements (speedup by 2x) can be observed. A script, written in the Python language, is included for generating these tables.

Included in the library download are tables for polynomials with up to four variables and up to degree four.
To use these tables (contained in the file polymul\_tab.h), compile with -DPOLYMUL\_TAB.
If you need tables of higher order you can run

`python gentab.py Nvar Ndeg > polymul_tab.h`

where Nvar and Ndeg are the maximal parameters for the tables (the shipped library was generated with `gentab.py 4 4`). By default gentab.h will not generate tables for multiplies with more than 1000 terms. You can modify gentab.py to change this limit. Be aware that the number of terms in the tables grow quadratically with the number of terms in the polynomials.