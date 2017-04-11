---
layout: post
title:  Bits and Bytes
author: Rahul Pandey
date:   2017-04-10
categories: bits bytes
---

Computers work with the simplest possible unit of data: a bit (short for `binary digit`). A bit has value 0 or 1. Groups of 8 bits together make up a byte. Computer memory is discussed in terms of bytes, such as a kilobyte or megabyte. Hexadecimal, or base 16, is used to represent 4 bits as a time, so a byte is represented as 2 hexadecimal digits. Since 2<sup>4</sup> is 16, each digit in hexadecimal can take on 16 possible values: 0-9 represent zero to nine, and A-F represent ten to fifteen. For example, a byte could be 0xA3, where the `0x` prefix is used to indicate hexadecimal (the value of 0xA3 in hexadecimal is 163).

Contents
===========
- [Powers of 2](#powers-of-2)
- [Bit manipulation](#bit-manipulation)

## Powers of 2

You should memorize the first few powers of 2 (see bottom of this article), and some size prefixes: 

| **Prefix** | **Official definition** | **Informal meaning** |
| kilobyte     | 10<sup>3</sup> bytes    | 2<sup>10</sup> bytes |
| megabyte     | 10<sup>6</sup> bytes    | 2<sup>20</sup> bytes |
| gigabyte     | 10<sup>9</sup> bytes    | 2<sup>30</sup> bytes |
| terabyte     | 10<sup>12</sup> bytes   | 2<sup>40</sup> bytes |
| petabyte     | 10<sup>15</sup> bytes   | 2<sup>50</sup> bytes |


Depending on who you talk to, a kilobyte is either 10<sup>3</sup> (1000) bytes, or 2<sup>10</sup> (1024) bytes. See Jeff Atwood's blog post about this as a source of discrepancy between hard drives and operating systems [1]. In whatever system used, know that each increase in memory is roughly 1000 times bigger, either 10<sup>3</sup> or 2<sup>10</sup>. 

## Bit manipulation

Common bit manipulations are AND, OR, XOR, NOT, and bit shifts [2]. These operators take in two bits and output a bit. The meanings of these are clear, except XOR, which returns a 1 if exactly one of the input bits was 1 (but not both). OR is represented by `|`, AND by `&`, NOT by `~`, and XOR by `^`. Think about why the following bit properties are true: 

| **XOR** | **AND** | **OR** |
| x ^ 0s = x | x & 0s = 0 | x `|` 0s = x
| x ^ 1s = ~x | x & 1s = x |  x `|` 1s = 1
| x ^ x = 0 | x & x = x | x `|` x = x

Some questions use these binary operators in their solution. 

<details>
<summary>You are given a list of numbers, where each number appears twice, except one number appears once. How can you find the number that appears once in linear time and constant space?</summary>
<br>
The answer is clear if we use XOR. Starting with 0, we XOR each number in the list with the accumulator (which starts at 0). All the duplicates in the list will "cancel" each other out with XOR, and the remaining number will be the singleton. This solution requires linear time and the only extra memory required is a single integer. 
</details>
<br>
<hr>
Powers of 2 to know:

| **Power** | **Value** |
| 0     | 1     |
| 1     | 2     |
| 2     | 4     |
| 3     | 8     |
| 4     | 16    |
| 5     | 32    |
| 6     | 64    |
| 7     | 128   |
| 8     | 256   |
| 9     | 512   |
| 10    | 1024  |

- [1] [Jeff Atwood blog about decmial vs binary](https://blog.codinghorror.com/gigabyte-decimal-vs-binary/)
- [2] [Mozilla bitwise operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)
