# C++ API #
Note that this documentation refers to the current development version in SVN.
**If there is anything you are missing in this documentation please leave a comment at the bottom of the page.**

For optimized builds you should compile with -DNDEBUG to turn off bounds checking for operator`[]`. This can have a huge impact on performance.

The whole library is currently contained in the header file **polymul.h**, and optionally **polymul\_tab.h**.

## General functions ##
### `int polylen(int Nvar, int Ndeg);` ###
Return the number of terms in a polynomial of Nvar variables and Ndeg degee (=binomial(Nvar+Ndeg,Ndeg)). If Nvar and Ndeg are known at compile time you can also use for example `polynomial<int,Nvar,Ndeg>::size`, see below.

## Polynomial container class ##
For type safety and utility functions there is a class representing polynomials:
### template<class numtype, int Nvar, int Ndeg> class polynomial; ###

Class that encapsulates an Nvar variable Ndeg degree polynomial, with binomial(Nvar+Ndeg,Ndeg) coefficients. It is safe to cast (with reinterpret\_cast) an array of numtype to a polynomial, or vice versa. It is also safe to cast a polynomial to a polynomial type of lower degree, but with the same number of variables. The polynomial class supports access to the coefficients through the `[]` indexing operator, where the index is the order of the term described in [CoefficientOrder](CoefficientOrder.md). Nvar must be greater or equal to one, while Ndeg must be a non-negative number

The scalar type numtype can be any type that supports the +,`*`,+= and `*`= operators, and which has a commuting product.

The polynomial class has the following methods:
### `polynomial::polynomial();` ###

### `polynomial::polynomial(const numtype &c0);` ###
Constructors for polynomial. If the c0 argument is given the polynomial is set to zero except the constant term which is set to c0.

### `polynomial::operator=(const numtype &c0)` ###
Set the constant coefficient to c0, all other coefficients to 0. This replaces `polynomial::zero()` from previous versions.

### `enum polynomial::size;` ###
The number of coefficients (terms). This is a compile-time constant (and not a function as in early versions).

### `numtype polynomial::eval(const numtype x[Nvar]);` ###
Evaluate the polynomial at x. This is reasonably fast but may not be optimal in terms of number of multiplications and additions. The variables **x** can be of a different type than the coefficient type.

### `void polynomial::exponents(int term, int exponents[Nvar]);` ###
Calculate the exponents of term, using a very slow method. This is intended for printing, not for fast computation.

### `int polynomial::term_index(int exponents[Nvar]);` ###
Return the index of the term with exponents. Again, this is a slow function intended for printing and debugging.

### `void polynomial::convert_to(polynomial &p)` ###
Convert this to fit into p, which can have different degree or different type of coefficients. If p has higher degree than this then the high order coefficients will be set to zero, but if p has lower degree then only the lower order coefficients will be copied.

### `template<int var> void polynomial::diff(polynomial &dp)` ###
Compute the derivative of this with respect to variable `var`, put the result in dp (a polynomial of one degree lower than this). Use like `p.diff<2>(dp);`.

## Multiplication functions ##
A few different functions are provided for multiplying polynomials. These functions are not methods of polynomial. Template arguments have been omitted below, so check polymul.h for details. You can use these functions directly on coefficient arrays by using an appropriate reinterpret\_cast.
### `void polymul(polynomial &dst, const polynomial &p1, const polynomial &p2);` ###
Multiply two polynomials with the same number of variables, p1 and p2, and put the result in dst (a polynomial of the right degree to fit the product exactly). Note that the degree of p1 and p2 does not have to be the same, but dst must have the exact right order to be able to represent the product.

### `void taylormul(polynomial &dst, const polynomial &p1, const polynomial &p2);` ###
Here dst, p1 and p2 are polynomials of the same degree. The product of p1 and p2 is put in dst, but only those terms up to the original order of the polynomials (the higher terms are not calculated). The name comes from the use of this function for calculating the product of Taylor expansions, where higher order terms are neglected.

### `void taylormul(polynomial &p1, const polynomial &p2);` ###
Like taylormul(), but computes p1 = p1\*p2 in-place, using no temporary storage. The second argument can have lower degree than the first, the result is still truncated to the degree of `p1`.

### `void polycontract(polynomial &P, const polynomial &p2, const polynomial &p3);` ###
Consider the expression `dot(p1*p2,p3)`, where `dot()` is a sum of the products of polynomial coefficients. The `polycontract()` function computes P, so that
`dot(p1*p2,p3) = dot(p1,P)`. If there are many p1's for each p2 and p3 this can improve performance considerably in certain applications.

## Utility functions ##

### `void polydfac(polynomial &p)` ###
**WARNING** This function has a off-by-one bug in version 1.0, which corrupts the memory after the polynomial p. This is fixed in svn, and is included in the newer releases.

Multiply each term in the polynomial with appropriate factorials to generate derivatives, i.e. the coefficient of the term `x^n y^m z^k` would be multiplied by n!m!k!.
NOTE: The factors are generated as enums, limiting the numeric range.

### `void polytrans(polynomial &dst, const polynomial &src, const numtype T[Nvar_dst*Nvar_src])` ###
Perform a linear change of variables, so that the `n`:th variable in `src` is replaced by a weighted sum of the variables in `dst`, taken from "column" `n` of the matrix `T`. The first `Nvar_dst` elements of T determine the transformation of variable 0 in src, the next `Nvar_dst` elements the transformation of variable 1, etc. `T` is then the inverse of the Jacobian matrix for this change of variables. The polynomials `src` and `dst` can have different number of variables - it is for example possible to pick out a lower dimensional polynomial of a certain subspace. In this case `T` is a rectangular matrix.
