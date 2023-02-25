# Binary Primer

Representing a number in powers

```
8    4    2    1    .  1/2  1/4  1/8  1/16
2^3  2^2  2^1  2^0  .  2^-1 2^-2 2^-3 2^-4

                16  x  16
                   256
                2^8 = 256 
```

### 2's compliment
2's complement is a method used in digital electronics and computer programming to **represent negative numbers** using binary digits (bits). In this system, the most significant bit (MSB) represents the sign of the number, with 0 indicating a positive number and 1 indicating a negative number.

To obtain the 2's complement of a negative number, the absolute value of the number is first represented in binary form. Then, the bits are inverted (reversed) so that 0 becomes 1 and 1 becomes 0. Finally, 1 is added to the result to obtain the 2's complement. For example, to obtain the 2's complement of -5, we first represent the absolute value of 5 in binary form as 0101. Then, we invert the bits to obtain 1010, and add 1 to get 1011, which is the 2's complement of -5.

The advantage of using the 2's complement representation is that addition and subtraction of numbers can be performed using the same hardware and algorithms, regardless of whether the numbers are positive or negative. This makes it a widely used method in computer systems.

## Fractions

```
7.5 = 0 1 1 1 . 1 0 0 0

2.1 = 0 0 1 0 . 0 0 1 0
```

With only 16 bits, it means there is a floating point resolution of 1/16 = 0.0625. Fractional numbers will be approximated because the smallest fraction that can be expressed is 0.0625.

### Biasing

Biasing refers to a technique used to represent numbers with a fixed number of bits in a way that enables both positive and negative values to be represented. In the case of floating-point numbers, biasing is used to represent the exponent of the number in a fixed number of bits.

In a floating-point number, the exponent is represented by a certain number of bits, which can be used to represent both positive and negative exponents. However, to enable the exponent to be represented in a fixed number of bits, a bias value is added to the actual exponent before it is encoded as a binary number. The bias value is chosen so that the range of possible exponents is centered around zero.

For example, in the IEEE 754 floating-point standard for 32-bit numbers, the exponent is represented by 8 bits, and a bias value of 127 is added to the actual exponent to obtain the final binary representation. This means that the range of possible exponents is from -126 to +127, with an exponent of 0 represented by a bias value of 127.

By using biasing, the encoding of the exponent can be made more efficient, and the range of possible exponents can be made symmetrical around zero, making it easier to perform arithmetic operations on floating-point numbers.

```csharp
float value = 3.14159f;
int bias = 127;
int exponent = (int)Math.Floor(Math.Log2(Math.Abs(value)));
int biasedExponent = exponent + bias;

byte[] bytes = BitConverter.GetBytes(value);
int sign = (bytes[3] & 0x80) == 0 ? 1 : -1;
bytes[3] = (byte)((biasedExponent >> 1) & 0x7F | ((biasedExponent & 1) << 7) | (sign << 7));
float result = BitConverter.ToSingle(bytes, 0);

Console.WriteLine($"Original value: {value}");
Console.WriteLine($"Encoded value: {result}");
```

#### Approximation example

```
9.6  = 1 0 0 1 . 1 0 1 0 = 9.625
```