## Introduction ##

Polymul is a self-contained C++ template library for efficient multiplication of multivariate polynomials.  It is intended for low order polynomials of a few variables, but is in principle limited only by the compiler's maximum template recursion depth. Polynomials can be created over any scalar type, such as integers or floating point numbers.

In addition to normal polynomial multiplication the library can also do truncated (Taylor series) multiplication, as well as linear changes of coordinates. Polynomials can also be evaluated at arbitrary points.

Check out an [Example](Example.md) or go directly to the [API](API.md) documentation.

## Scope ##

The aim of Polymul is to do "naive" polynomial multiplication as fast as possible. It does not try to use any of the tricks, such as FFTs, that exists for turning polynomial multiplication into an Nlog(N) process. Since Polymul only deals with polynomials that have a degree that is a compile time constant, it is anyway limited to rather small polynomials. This is also the reason why the polynomial class does not overload the arithmetic operators. Since the result of a multiplication is a polynomial of higher degree than the factors (and hence a different C++ type), such arithmetic would be difficult to use in practice. Of course this library can be used as a base for implementing dynamically sized polynomials, or truncated (Taylor) arithmetic.

An implementation of Taylor series arithmetic built on polymul can be
found at [[libtaylor.googlecode.com](http://libtaylor.googlecode.com)].

## Citation ##
If you use this library in a scientific work, please cite

Polymul Library, Ulf Ekström 2009. http://polymul.googlecode.com


For questions or comments contact [Ulf Ekström](http://admol.org/uekstrom). External contributions are welcome!