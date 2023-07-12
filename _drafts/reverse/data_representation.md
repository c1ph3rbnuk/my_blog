---
layout: page
title: Data Representation and Organization
date: 2023-01-20
author: "c1ph3rbnuk"
---

#### Numbering System 
There are different ways of representing numerical values in a computer. Most common number systems include:  

##### Decimal System  
Also known as the base 10, the decimal system is most often used in our daily lives. It uses digts 0-9 and values from right to left are multiplied with an increasing power of 10. `eg`  
The value 257, is 2 x 10<sup>2</sup> + 5 x 10<sup>1</sup> + 7 x 10<sup>0</sup> = 200 + 50 + 7 = 257  

##### Binary System 
Also known as base 2, it uses only digits 0 and 1. This is the language computers understand because it can be communicated through digital circuits in series of off(0) and on(1) pulses. These binary digits are often refered to as bits. The difference between decimal and binary is that decimal uses digits 0 through 9 while binary uses 0 and 1. Just like decimal when you count 0 -9 and you want to continue you add another digit to the left and start over to make 10, you keep increment the left digit everytime the right most digit value is 9 and when you count to 99 you add a third digit to make it 100. well in binary your highest digit is 1 not 9. So everytime you get to 1 and want to continue you keen adding extra digits to the left.  

So counting to 17 in binary will be like  
![](../assets/images/RevJourney/binary.png) ![](../assets/images/RevJourney/binary1.png)

To map a binary value to it's decimal you sum all the bits that have value 1.  
![](../assets/images/RevJourney/power2.png)
This will be `128 + 64 + 16 + 8 + 1 = 217`

The rightmost bit in binary is called the **Least Significant Bit(LSB)**, whereas the leftmost bit is called the **Most Significant Bit(MSB)**