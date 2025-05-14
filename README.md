```
NAME : PREETHIKA N
REG NO : 212223040130
```

# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```
#include <stdio.h>
#include <stdlib.h>
typedef struct {
int x;
int y;
int is_infinity; // Boolean to indicate if this point is the "point at infinity"
} Point;
// Parameters for the elliptic curve: y^2 = x^3 + ax + b over F_p
const int a = 2; // Coefficient a
const int b = 3; // Coefficient b
const int p = 17; // Prime number for the finite field F_p
// Function to calculate modular inverse of a number (a^-1 mod p)
int mod_inverse(int k, int p) {
k = k % p;
for (int x = 1; x < p; x++) {
if ((k * x) % p == 1) {
return x;
}
}
return -1;
}
// Function to perform modular addition
int mod(int value, int mod) {
int result = value % mod;
return (result < 0) ? result + mod : result;
}
// Function to add two points on the elliptic curve
Point point_addition(Point P, Point Q) {
if (P.is_infinity) return Q;
if (Q.is_infinity) return P;
Point result;
result.is_infinity = 0;
int lambda;
if (P.x == Q.x && P.y == Q.y) {
// Point doubling case
lambda = (3 * P.x * P.x + a) * mod_inverse(2 * P.y, p);
} else {
// Point addition case
lambda = (Q.y - P.y) * mod_inverse(Q.x - P.x, p);
}
lambda = mod(lambda, p);
result.x = mod(lambda * lambda - P.x - Q.x, p);
result.y = mod(lambda * (P.x - result.x) - P.y, p);
return result;
}
// Function to perform scalar multiplication on a point
Point scalar_multiplication(Point P, int n) {
Point result;
result.is_infinity = 1; // Point at infinity (neutral element for addition)
Point addend = P;
while (n > 0) {
if (n & 1) {
result = point_addition(result, addend);
}
addend = point_addition(addend, addend);
n >>= 1;
}
return result;
}
// Main function to demonstrate ECC operations
int main() {
Point G = {5, 1, 0}; // Base point on the curve
int n = 7; // Scalar to multiply with the point
printf("Base point G: (%d, %d)\n", G.x, G.y);
Point R = scalar_multiplication(G, n);
printf("Result of %d * G: (%d, %d)\n", n, R.x, R.y);
return 0;
}
```
## Output:
![Screenshot 2025-05-14 230529](https://github.com/user-attachments/assets/5b9b9261-86c9-4fef-bc2a-5dbb9a23b361)

## Result:
The program is executed successfully

