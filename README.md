# 2.Number Theory & Cryptography
## Modular Arithmetic

1. The equation for division with remainder is given by:
```math
a = dq + r
```
where $a$ is the dividend, $d$ is the divisor, $q$ is the quotient, and $r$ is the remainder.

2. The definition of the division operation is expressed as:
```math
\text{a div d} = q
```
This means when $a$ is divided by $d$, the integer quotient is $q$.

3. The definition of the modulus operation is:
```math
\text{a mod d} = r
```
This means when $a$ is divided by $d$, the remainder is $r$. 

These equations together describe the division algorithm where $0 \leq r < d$.

**Problem:**

Evaluate the following expressions:

a. $9 \times 3$ in $\mathbb{Z}_{20}$

b. $15^{26} \mod 7$

c. $(352 \cdot 407) \mod 50$

d. $(1302^3 + 4505^2) \mod 10$

**Solution:**

Using the rules for arithmetic operations modulo $n$:

a. Arithmetic in $\mathbb{Z}_{20}$ is the same as $\mod 20$, so we get:

```math
9 \times 3 \mod 20 = 27 \mod 20 = 7
```

b. 

```math
15^{26} \mod 7 = (15 \mod 7)^{26} \mod 7 = (1)^{26} \mod 7 = 1 \mod 7 = 1
```

c. 

```math
(352 \cdot 407) \mod 50 = [(352 \mod 50) \cdot (407 \mod 50)] \mod 50
```

Calculating further:

```math
352 \mod 50 = 2, \quad 407 \mod 50 = 7
```

So,

```math
(2 \cdot 7) \mod 50 = 14 \mod 50 = 14
```

d. 

```math
(1302^3 + 4505^2) \mod 10 = [(1302^3 \mod 10) + (4505^2 \mod 10)] \mod 10
```

First, calculate:

```math
1302 \mod 10 = 2, \quad 4505 \mod 10 = 5
```

Then:

```math
[(2^3 \mod 10) + (5^2 \mod 10)] \mod 10 = [(8 \mod 10) + (25 \mod 10)] \mod 10
```

This simplifies to:

```math
[8 + 5] \mod 10 = 13 \mod 10 = 3
```
## Extended Euclidean Algorithm

1. **Set Up the Table**: 
   - Prepare a table with columns for the values of $a$, $b$, the quotient $q$, the remainder $r$, and the coefficients $x$ and $y$.

2. **Initialize the Table**:
   - The first row corresponds to the initial values of $a$ (the larger of the two integers) and $b$ (the smaller one).
   - Set the coefficients for $x$ and $y$ in the first row as $x_1 = 1$, $y_1 = 0$ and for the second row as $x_2 = 0$, $y_2 = 1$.

3. **Iterate to Fill the Table**: 
   - **Calculate the Quotient**: In each iteration, compute the quotient $q = \lfloor a_i / b_i \rfloor$.
   - **Calculate the Remainder**: Compute the remainder $r = a_i - q \cdot b_i$.
   - **Update the Coefficients**: Update the values of $x$ and $y$ using the previous values:

```math
x_{i+1} = x_{i-1} - q_i \cdot x_i
```

```math
y_{i+1} = y_{i-1} - q_i \cdot y_i
```
    
   - **Update $a$ and $b$**: Set $a_{i+1} = b_i$ and $b_{i+1} = r_i$ for the next iteration.
   - Repeat these steps until the remainder $r$ becomes 0.

4. **Interpret the Results**:
   - The last non-zero remainder $r$ is the **GCD** of the original numbers.
   - The coefficients $x$ and $y$ from the corresponding row where $b = 1$ (last non-zero $r$) are the **Bézout coefficients** for the equation:

```math
\text{gcd}(a, b) = a \cdot x + b \cdot y
```
   - The coefficient $x$ is the **inverse** of $a \ mod \ b$, which can be expressed as:

```math
x \cdot a \equiv 1 \pmod{b}
```
**Chart**

| Step | $a$  | $b$  | $q$ | $r$  | $x$  | $y$  |
|------|------|------|-----|------|------|------|
| 1    | 191  | 111  | 1   | 80   | 1    | 0    |
| 2    | 111  | 80   | 1   | 31   | 0    | 1    |
| 3    | 80   | 31   | 2   | 18   | 1    | -1   |
| 4    | 31   | 18   | 1   | 13   | -1   | 2    |
| 5    | 18   | 13   | 1   | 5    | 2    | -3   |
| 6    | 13   | 5    | 2   | 3    | -3   | 5    |
| 7    | 5    | 3    | 1   | 2    | 8    | -13  |
| 8    | 3    | 2    | 1   | 1    | -11  | 18   |
| 9    | 2    | 1    | 2   | 0    | 19   | -31  |


**Final Result**
   - The last non-zero remainder is 1, which indicates that 191 and 111 are coprime.
- The Bézout coefficients for the equation $191 \cdot x + 111 \cdot y = 1$ are $x = 19$ and $y = -31$. Thus,

```math
191 \cdot 19 + 111 \cdot (-31) = 1
```
- The coefficient $x = 19$ is the inverse of $a = 191$ modulo $b = 111$, which can be expressed as:

```math
19 \cdot 191 \equiv 1 \pmod{111}
```

## Fast Exponentiation
**Example: Compute $`3^{50} \mod 23`$**

1. **Convert the Exponent to Binary:**
   - The exponent $`50`$ in binary is $`110010_2`$. This means:

```math
50 = 2^5 + 2^4 + 2^1
```

   - This tells us that $`3^{50}`$ can be rewritten using the binary representation:

```math
3^{50} = 3^{2^5} \cdot 3^{2^4} \cdot 3^{2^1}
```

2. **Compute Powers of 3 Using Successive Squaring:**
   - We compute $`3^{2^n} \mod 23`$ for $`n = 0, 1, 2, 3, 4, 5`$. Create a table for these computations:

```math
\begin{array}{c|c}
n & 3^{2^n} \mod 23 \\ \hline
0 & 3^1 \mod 23 = 3 \\
1 & 3^2 \mod 23 = 9 \\
2 & (3^2)^2 = 3^4 \mod 23 = 81 \mod 23 = 12 \\
3 & (3^4)^2 = 3^8 \mod 23 = 144 \mod 23 = 6 \\
4 & (3^8)^2 = 3^{16} \mod 23 = 36 \mod 23 = 13 \\
5 & (3^{16})^2 = 3^{32} \mod 23 = 169 \mod 23 = 8 \\
\end{array}
```

3. **Combine the Required Powers:**
   - Using the binary expansion of $`50`$, we need:

```math
3^{50} \mod 23 = 3^{32} \cdot 3^{16} \cdot 3^2 \mod 23
```

   - Substitute from the table:

```math
3^{50} \mod 23 = 8 \cdot 13 \cdot 9 \mod 23
```

4. **Calculate the Result:**
   - Multiply step-by-step modulo $`23`$:

```math
8 \cdot 13 = 104 \quad \text{and} \quad 104 \mod 23 = 12
```

```math
12 \cdot 9 = 108 \quad \text{and} \quad 108 \mod 23 = 16
```

   Thus, $`3^{50} \mod 23 = 16`$.
   
## Unit digits
To find the last $x$ binary digits of an exponential expression $a^b$, you need to compute $a^b \mod 2^x$. This result will give you the last $x$ binary digits of $a^b$. Here's how you can do it step-by-step:

**Example**

Suppose you want to find the last 5 binary digits of $3^{13}$.

1. **Calculate $2^5$**: 
```math
2^5 = 32
```

2. **Compute $3^{13} \mod 32$**:
   - Instead of calculating $3^{13}$ directly, use modular exponentiation.

Here's a breakdown using the method of exponentiation by squaring:

```math
3^{13} \mod 32
```

- Write 13 in binary: $13 = 1101_2$. This means $13 = 2^3 + 2^2 + 2^0$.

- Compute the following:
```math
3^1 \mod 32 = 3
```
```math
3^2 \mod 32 = 9
```
```math
3^4 \mod 32 = (3^2)^2 \mod 32 = 9^2 \mod 32 = 81 \mod 32 = 17
```
```math
3^8 \mod 32 = (3^4)^2 \mod 32 = 17^2 \mod 32 = 289 \mod 32 = 1
```

Now, combine the relevant powers (those corresponding to 1s in the binary representation of 13):

```math
3^{13} \mod 32 = (3^8 \times 3^4 \times 3^1) \mod 32
```
```math
= (1 \times 17 \times 3) \mod 32
```
```math
= 51 \mod 32
```
```math
= 19
```

The last 5 binary digits of $3^{13}$ are the binary representation of 19, which is $10011_2$.

> To calculate the last $x$ digits of an exponential expression, $a^b$, you can use modular arithmetic. Specifically, you're looking for $a^b \mod 10^x$.

## RSA Encryption

**Step 1: Key Generation**
1. **Choose two distinct prime numbers**:  
   - Let $p = 61$ and $q = 53$. These are two large prime numbers.
   - **Example**: We select $p = 61$ and $q = 53$.

2. **Compute their product**:  
   - $n = p \cdot q$
   - **Example**: $n = 61 \cdot 53 = 3233$

3. **Calculate the totient (Euler's totient function)**:  
   - $\phi(n) = (p - 1)(q - 1)$
   - **Example**: $\phi(n) = (61 - 1)(53 - 1) = 60 \cdot 52 = 3120$

4. **Choose an encryption exponent**:  
   - Select an integer $e$ such that $1 < e < \phi(n)$ and $\gcd(e, \phi(n)) = 1$.
   - **Example**: Choose $e = 17$ (17 is a common choice as it is relatively prime to $3120$).

5. **Determine the decryption exponent**:  
   - Compute $d$ as the modular multiplicative inverse of $e$ modulo $\phi(n)$.
   - $d \cdot e \equiv 1 \pmod{\phi(n)}$
   - **Example**: Calculate $d$ such that $d \cdot 17 \equiv 1 \pmod{3120}$. The result is $d = 2753$.

6. **Public and Private Keys**:  
   - The **public key** is $(e, n)$.
   - The **private key** is $(d, n)$.
   - **Example**: Public key is $(17, 3233)$, and private key is $(2753, 3233)$.

**Step 2: Encryption**

1. **Convert the plaintext message into an integer**:  
   - Let the plaintext be represented as an integer $m$ such that $0 \leq m < n$.
   - **Example**: Suppose we want to encrypt the message $m = 65$.

2. **Compute the ciphertext**:  
   - The ciphertext $c$ is computed using the public key $(e, n)$:
   - $c \equiv m^e \pmod{n}$
   - **Example**: Compute $c \equiv 65^{17} \pmod{3233}$.  
     To simplify:  
     $65^{17} \pmod{3233} = 2790$  
   - So, the ciphertext $c = 2790$.

**Step 3: Decryption**

1. **Compute the plaintext from the ciphertext**:  
   - Using the private key $(d, n)$, the plaintext $m$ is recovered from the ciphertext $c$:
   - $m \equiv c^d \pmod{n}$
   - **Example**: Compute $m \equiv 2790^{2753} \pmod{3233}$.  
     To simplify:  
     $2790^{2753} \pmod{3233} = 65$  
   - The original message $m = 65$ is recovered.

### Shortcut for finding the private key
An individual has chosen the public key of $N = 187 = 11 \times 17$ and $e = 3$. What is the private key using RSA encryption?  
a) 2   b) 107   c) 15   d) 160

Given:  
$e = 3$  

Calculate $\phi$:
$\phi = (11 - 1) \times (17 - 1) = 160$

On a calculator, compute:

$\text{mod}(e \times a, \phi)$

$\text{mod}(e \times b, \phi)$

$\text{mod}(e \times c, \phi)$

$\text{mod}(e \times d, \phi)$

We want the answer to be $1$:  

$\text{mod}(3 \times 107, 160) = 1$

Thus, the answer is b) 107.

## Expansions

**Binary to Decimal**  
To convert a binary number to a decimal:

1. Write down the binary number.
2. Assign powers of 2 to each digit, starting from the rightmost digit (which gets $2^0$).
3. Multiply each binary digit by its corresponding power of 2.
4. Sum all the products to get the decimal number.

**Example:**  
Convert $1101_2$ to decimal.

Calculation:
```math
1 \times 2^3 + 1 \times 2^2 + 0 \times 2^1 + 1 \times 2^0 = 8 + 4 + 0 + 1 = 13
```

**Decimal to Binary**  
To convert a decimal number to binary:

1. Divide the decimal number by 2.
2. Record the remainder (0 or 1) — this gives the least significant bit.
3. Divide the quotient by 2 and repeat until the quotient is 0.
4. The binary number is read from bottom (least significant bit) to top (most significant bit).

**Example:**  
Convert $13_{10}$ to binary.

Steps:
- $13 \div 2 = 6$ remainder $1$
- $6 \div 2 = 3$ remainder $0$
- $3 \div 2 = 1$ remainder $1$
- $1 \div 2 = 0$ remainder $1$

Binary: $1101_2$

**Hexadecimal to Decimal**  
To convert a hexadecimal number to a decimal:

1. Write down the hexadecimal number.
2. Assign powers of 16 to each digit, starting from the rightmost digit (which gets $16^0$).
3. Convert any lettered hexadecimal digits (A-F) to their decimal equivalents (A=10, B=11, ..., F=15).
4. Multiply each digit by its corresponding power of 16.
5. Sum all the products to get the decimal number.

**Example:**  
Convert $1A3_{16}$ to decimal.

Calculation:
```math
1 \times 16^2 + 10 \times 16^1 + 3 \times 16^0 = 256 + 160 + 3 = 419
```

**Decimal to Hexadecimal**  
To convert a decimal number to hexadecimal:

1. Divide the decimal number by 16.
2. Record the remainder. If the remainder is 10-15, use the equivalent hexadecimal digit (A-F).
3. Divide the quotient by 16 and repeat until the quotient is 0.
4. The hexadecimal number is read from bottom to top.

**Example:**  
Convert $419_{10}$ to hexadecimal.

Steps:
- $419 \div 16 = 26$ remainder $3$
- $26 \div 16 = 1$ remainder $10$ (which is $A$)
- $1 \div 16 = 0$ remainder $1$

Hexadecimal: $1A3_{16}$

**Binary to Hexadecimal**  
To convert a binary number to hexadecimal:

1. Group the binary digits into sets of four, starting from the right. Add leading zeros if necessary.
2. Convert each 4-digit binary group to its hexadecimal equivalent [via decimal].

**Example:**  
Convert $110101101_2$ to hexadecimal.

Steps:
- Group: $0001\ 1010\ 1101$
- Convert: $1=1$, $1010=A$, $1101=D$

Hexadecimal: $1AD_{16}$

**Hexadecimal to Binary**  
To convert a hexadecimal number to binary:

1. Write down the hexadecimal number.
2. Convert each hexadecimal digit to its 4-digit binary equivalent [via decimal].

**Example:**  
Convert $1AD_{16}$ to binary.

Steps:
- Convert: $1=0001$, $A=1010$, $D=1101$

Binary: $000110101101_2$

# 3. Recursion & Induction
## Induction
**The statement to prove:**

We need to prove that for all $n \geq 0$,

```math
1 + 2 + 2^2 + \cdots + 2^n = 2^{n+1} - 1
```

**Step 1: Base Case ($n = 0$)**

Check if the formula holds for $n = 0$.

On the left-hand side, we have:

```math
1 = 2^{0+1} - 1 = 2^1 - 1 = 1
```

The base case is true.

**Step 2: Inductive Hypothesis**

Assume the formula holds for some arbitrary $n = k$:

```math
1 + 2 + 2^2 + \cdots + 2^k = 2^{k+1} - 1
```

**Step 3: Inductive Step**

We need to show the formula holds for $n = k + 1$:

```math
1 + 2 + 2^2 + \cdots + 2^k + 2^{k+1} = 2^{(k+1)+1} - 1 = 2^{k+2} - 1
```

Start with the inductive hypothesis:

```math
1 + 2 + 2^2 + \cdots + 2^k = 2^{k+1} - 1
```

Add $2^{k+1}$ to both sides:

```math
(1 + 2 + 2^2 + \cdots + 2^k) + 2^{k+1} = (2^{k+1} - 1) + 2^{k+1}
```

Simplify the right-hand side:

```math
2^{k+1} - 1 + 2^{k+1} = 2 \cdot 2^{k+1} - 1 = 2^{k+2} - 1
```

Thus, the formula holds for $n = k + 1$.

## Summation
General Formula for Summation over Cartesian Product

When you sum the product of all elements from two sets, you're essentially summing over the Cartesian product:

```math
\sum_{(x, y) \in A \times B} x \cdot y
```

For two sets of numbers $A = \{a_1, a_2, \ldots, a_m\}$ and $B = \{b_1, b_2, \ldots, b_n\}$, the general formula for summing the product of every combination of their elements is:

```math
\text{Sum} = \left( \sum_{a \in A} a \right) \times \left( \sum_{b \in B} b \right)
```
