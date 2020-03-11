---
title: The C++ Programming Language
tags: textbook
notebook: C++
---

<!-- TOC -->

- [The C++ Programming Language](#the-c-programming-language)
  - [Ch2. The Basic](#ch2-the-basic)
    - [Ch 2.2.3 Constants](#ch-223-constants)
    - [Ch 2.4.1 Seperate Compilation](#ch-241-seperate-compilation)
    - [Ch 2.4.3.3  Static Assertions](#ch-2433-static-assertions)
  - [Ch3. Abstraction Mechanisms](#ch3-abstraction-mechanisms)
    - [Ch 3.2.1.1 An Arithmetic Type](#ch-3211-an-arithmetic-type)
    - [Abstract Types](#abstract-types)

<!-- /TOC -->

# The C++ Programming Language

## Ch2. The Basic
### Ch 2.2.3 Constants
`constexpr`: meaning roughly ‘‘to be evaluated at compile time’’，
常數表達式 (constant expression)。一堆常數可以在編譯時期經過固定確定運算得到確切值的表達式。

今天寫了一個 sq 函式想計算一個數平方。當你傳入一個常數，那麼邏輯上，這個函式的返回值也該是個編譯時期就知道的常數。
```cpp
int sq(int N) { // function for square
  return N * N;
}
//error, 無法編譯過
const int N = 123;
const int SQ_N = sq(N);
// correct
constexpr int N = 123;
constexpr int SQ_N = sq(N);
```
- for loops that traverse a sequence in the simplest way:
```cpp
void print() {
  int v[] = {0,1,2,3,4,5,6,7,8,9};
  for (auto i=0; i!=10; ++i) //standard for statement
    // ...
  for (auto x : v) //for each x in v
    // ...
  for (auto x : {10,21,32,43,54,65})
    // ...
}
```

### Ch 2.4.1 Seperate Compilation
C++ supports a notion of separate compilation where user code sees only declarations of types and functions used. The deﬁnitions of those types and functions are in separate source ﬁles and compiled separately.

![Alt text](class.png)

Strictly speaking, using separate compilation isn’t a language issue; it is an issue of how best to take advantage of a particular language implementation.

### Ch 2.4.3.3  Static Assertions
static_assert(A,S) prints S as a compiler error message if A is not true.

## Ch3. Abstraction Mechanisms
### Ch 3.2.1.1 An Arithmetic Type
```cpp
class complex {
  double re, im;  // representation: two doubles
 public:
  complex(double r, double i) :re{r}, im{i} {} // construct complex from two scalars
  complex(double r) :re{r}, im{0} {} // construct complex from one scalar
  complex() :re{0}, im{0} {} //default complex: {0,0}
  double real() const {return re;}
  void real(double d) {re=d;}
  double imag() const {return im;}
  void imag(double d) {im=d;}
  complex& operator+=(complex z) {re+=z.re , im+=z.im; return ∗ this;} //add to re and im and return the result
  complex& operator−=(complex z) {re−=z.re , im−=z.im; return ∗ this;}
  complex& operator ∗ =(complex);  // deﬁned out-of-class somewhere
  complex& operator/=(complex);  // deﬁned out-of-class somewhere
};
```
`complex(double r, double i) :re{r}, im{i} {}`: This construct is called a **Member Initializer** List in C++.
So `re` gets set to `r`. And `im` gets set to `i`.


### Abstract Types
virtual means "may be redeﬁned  later  in  a  class  derived from  this  one."