
First we start with what is information
1. Information is something that is transmitted from communicator to receiver
2. we say information is something that resolved uncertainty of receiver
3. information should give something new to the receiver

- Say there are N number of messages, each of the messages have some probability to happen, if an event with low probability happened it means it provided with the largest information.

- Now since we work with computers, we need to store the information in bits, we use the claude shannon formula

- Information or number of bits required to store the info = log2(1/P) where P is the probability of that info

### Encodings

- Okay so now you know the number of minimum bits required to store the info.
So how do you encode it? You use the binary system or the hexadecimal system to encode or decode the system. For binary it is very easy just use the powers of 2, for example the binary 111 is equal to 2^2 + 2^1 + 2^0 = 4+2+1 = 7
so to express any whole number in binary just use try to express in the sum of powers of 2. 
Now that we know the decimal number 7 can be expressed in 3 bits but if we have assigned it 5 bits we encode it as 00111, we fill it by adding 0. 
Now to express integers we use two bit complement method; like for example we know how to express 7 in 5 bit, it is 00111, now to express -7
1. invert it first, 11000
2. add 1 we get= 11001 here you go.

hexadecimal is just assigning fixed 4 bit to a value.

Now for variable length encoding we use the hoffmann method
you go from bottom to top
take the two lowest and merge them and represent them with N1, now you have the remaining probabilities and N1, now take the two lowest and merge them and go on. along with this try for the maximum width balanced approach.

Compression can be done by rather than working with N messages work with pairs of N messages

### Errors

Now comes the error part
Say we are transimitting the information whether the coin gives head or tails, for this information we need only 1 bit (0 for heads and 1 for tails), now if error happens, we wouldnt know if an error has happened since if 0 exchanges to 1, it is still a valid info. So we increase bit size, we take 00 for heads and 11 for tails, now if any single bit flips, we will know its an error since it won't be valid info. In general the difference in bits between transmitted and received info is hamming distance. Now for double bit error we duplicate 3 times now we know that any valid info will have odd numbers of 1s, if any received info has even number of 1s we know that error has been occured. The bit we added is called the parity bit. 

More generally, we can correct k-bit errors by ensuring a Hamming distance of at least 2⋅k+1 between valid codewords.

So for example we have different bit encodings for different values, now we want to know if there was any error, so if we want to use the even parity, we count the number of 1s in each encoding, if it is odd, add 1; if it is even add 0.

==**Hamming distance is the number of bit positions in which two binary strings of equal length differ.  It is the fundamental metric used to determine how many errors a code can detect or correct.**==
- ==**Example: The distance between `1011` and `1001` is 1 (they differ only in the third bit).**== 
    
- ==**Example: The distance between `1010` and `0101` is 4 (they differ in all positions).**==

==**To detect N errors there much be hamming distance of N+1. The minimum Hamming distance of N+1 must exist between every pair of distinct valid codewords in your encoding scheme.**==
==**To do this we have to add one or more bits, these are called parity bits**==

==**hamming distance can be established by adding parity bits whereas the even 1s or odd 1s is a very specific parity scheme to add a single parity bit.**==


# Correction Of errors
To detect N number of errors we need to have hamming distance of N+1 (can be done by adding parity bits, for specific single bit error detection, we use the rule of even 1s or odd 1s)

But to correct N number of errors we need Hamming distance of 2N+1

For example: Take the case where we encode 0 to heads and 1 to tails, if a single bit error occurs we wouldn't be able to detect it, we need hamming distance of 1+2=2 for it which can be done by
    Heads= 00
	 Tails = 11
	 Now if any single bit error occurs we will know cause encodings like 01, 10 will be invalid

Now to correct single bit errors we need hamming distance of 2x1 + 1 = 3
	So we encode Heads = 000
				Tails = 111
So how do we correct an error

Now we know that only single bit error has happened so if the received encoding was: 001, 010, 100; we will know the system was trying to transmit 000 but an error has been occured same can be done if the received encoding: 110, 101, 011; We will know that the system was trying to transmit 111 but an error has been occured.

### Understanding the 2D Grid

In this problem, each transmission represents a $3 \times 3$ data matrix flanked by a row of **column parity bits** and a column of **row parity bits**.

Let's visualize the structure of the incoming bits:

$$\begin{matrix} \text{Data Bit 1} & \text{Data Bit 2} & \text{Data Bit 3} & \mathbf{\text{Row Parity}} \\ \text{Data Bit 1} & \text{Data Bit 2} & \text{Data Bit 3} & \mathbf{\text{Row Parity}} \\ \text{Data Bit 1} & \text{Data Bit 2} & \text{Data Bit 3} & \mathbf{\text{Row Parity}} \\ \mathbf{\text{Col Parity}} & \mathbf{\text{Col Parity}} & \mathbf{\text{Col Parity}} & \end{matrix}$$

- **The Rule:** Every row and every column must have an **even number of 1s** (including its parity bit).
    
- **The Correction Mechanism:** If a single data bit flips, it will cause **exactly one row parity error** and **exactly one column parity error**. The intersection of that row and column pinpoints the broken bit!
    

### First Transmission

Plaintext

```
1 1 0  [0]  <- Row 1
0 0 0  [0]  <- Row 2
0 1 0  [1]  <- Row 3
-----------
1 0 0       <- Column Parity bits
```

- **Row Check:**
    
    - Row 1: `1100` (Two 1s = Even. Correct)
        
    - Row 2: `0000` (Zero 1s = Even. Correct)
        
    - Row 3: `0101` (Two 1s = Even. Correct)
        
- **Column Check:**
    
    - Col 1: `1, 0, 0` with parity `1` (Two 1s = Even. Correct)
        
    - Col 2: `1, 0, 1` with parity `0` (Two 1s = Even. Correct)
        
    - Col 3: `0, 0, 0` with parity `0` (Zero 1s = Even. Correct)
        

**Conclusion:** 0 errors. Read the original 3-bit binary data directly:

- First? **6** (`110` in binary)
    
- Second? **0** (`000` in binary)
    
- Third? **2** (`010` in binary)
    

### Second Transmission

Plaintext

```
1 0 1  [1]  <- Row 1 (Three 1s = ERROR!)
0 1 1  [0]  <- Row 2 (Two 1s = Correct)
0 0 1  [1]  <- Row 3 (Two 1s = Correct)
-----------
0 1 1       
^
Col 1 (One 1 = ERROR!)
```

- **Locating the Error:** Row 1 has an error, and Column 1 has an error. Their intersection is the very first bit (`Row 1, Col 1`), which is currently a `1`.
    
- **Correction:** Flip that bit from `1` to `0`.
    
- **Corrected Row 1 Data:** `001` (instead of `101`)
    

**Conclusion:** Convert the corrected rows to decimal:

- First? **1** (`001` in binary)
    
- Second? **3** (`011` in binary)
    
- Third? **1** (`001` in binary)
    

### Third Transmission

Plaintext

```
0 1 1  [1]  <- Row 1 (Three 1s = ERROR!)
1 0 0  [1]  <- Row 2 (Two 1s = Correct)
0 1 1  [0]  <- Row 3 (Two 1s = Correct)
-----------
1 0 0       
```

- **Row Check:** Row 1 has an odd number of 1s (**Error!**).
    
- **Column Check:** Let's look closely at the columns:
    
    - Col 1: `0, 1, 0` with parity `1` (Two 1s = Correct)
        
    - Col 2: `1, 0, 1` with parity `0` (Two 1s = Correct)
        
    - Col 3: `1, 0, 1` with parity `0` (Two 1s = Correct)
        

**Locating the Error:** This is a beautiful edge case! A row error fired, but _zero_ column errors fired. This means the data payload is entirely untouched—**the row parity bit itself was the one that flipped during transmission.**

Since the data bits and uncorrupted, we read them exactly as they arrived:

- First? **3** (`011` in binary)
    
- Second? **4** (`100` in binary)
    
- Third? **3** (`011` in binary)

==For complex single bit error correction we use 2D matrix to narrow down specifically which bit interchanges==




