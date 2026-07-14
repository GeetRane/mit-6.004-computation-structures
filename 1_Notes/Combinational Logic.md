
# Converting truth table to Boolean exp
- isolate all the rows where output is 1
- now if one of these selected rows have (A=0, B=1, C=0) the first term is A(bar)BC(bar) + ...(rest of the terms according to the rows). 

# Multiple inputs
- AND and OR gates can be used with multiple inputs in either chain form or tree form, it is difficult to know which form is efficient it is determined using propagation delay and other factors (like there maybe such that input D will only be registered once input C is registered)
- It can also be that a single AND or OR gate is taking multiple inputs (this can be done when converting the boolean expression to logic gate design), this is because AND and OR gates are associative
- we will majorly use NOT, NAND, NOR gates since CMOS is itself inverting. AND and OR gates can be made using the NAND and NOR and NOT
- ![](Pasted%20image%2020260711114427.png|358)  - ==for non-speed sensitive parts (and size sensitive), we will use normal AND and OR, for speed sensitive parts (and non size sensitive) we will use inverting gates.== **The creators of the library decided to make the non-inverting gates small but slow by using MOSFETs with much smaller widths than used in the inverting logic gates, which were designed to be fast.** 
- AND gate is two staged (NAND and NOT) but the size is smaller than NAND because the width of NAND(in AND) is kept shorter and width of NAND (individual) is kept large. 