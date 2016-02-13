# Coefficient storage order #
Polynomial coefficients are stored ordered first by order and then lexicograpically. For example, a second degree polynomial in three variables would be stored in the order
```
1 x y z x^2 xy xz y^2 yz z^2
```
The number of terms in a polynomial with **Nvar** variables and **Ndeg** degree is binomial(Nvar+Ndeg,Ndeg).