# Polymul example #

This is a small example program that multiplies two polynomials and prints a coefficient from the resulting polynomial. The `term_index` functions takes an array of exponents and returns the index of the corresponding term. This might look a bit awkward, but it's implemented like this to remind you that that this conversion is very slow.

```
#include <iostream>
#include "polymul.h"

using namespace std;

int main(void)
{
  //Two third-degree polynomials of two variables
  polynomial<double, 2, 3> p1, p2;
  //One sixth-degree polynomials of two variables
  polynomial<double, 2, 6> p3;
  // Set p1 = 7*x^2y and p2 = 5*y^3
  int exp1[2] = {2,1}, exp2[2] = {0,3};
  p1 = 0; // Set all coefficients to 0
  p1[p1.term_index(exp1)] = 7; // Set the x^2y coefficients to 7
  p2 = 0;
  p2[p2.term_index(exp2)] = 5;
  // Multiply p1 and p2, put the result in p3
  polymul(p3,p1,p2);
  // Look what the resulting x^2y^4 term is:
  int exp3[2] = {2,4};
  cout << "The x^2y^4 term has coefficient " << p3[p3.term_index(exp3)] << endl;
  cout << "All coefficients of p3:" << endl;
  for (int i=0;i<p3.size;i++)
     cout << i << " " << p3[i] << endl;
  return 0;
}
```