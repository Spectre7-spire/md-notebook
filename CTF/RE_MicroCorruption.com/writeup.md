# MicroCorruption.com
Author: Sri Hari Karthick N

----
[Link to the site](https://www.microcorruption.com/), create an account and start the tutorial challenge.

[Their MSP430 modified manual](manual.pdf)

### Debugger Console - Inputs
---
The following are the allowed inputs that are useful for the debug console.

1. help - Shows the help message.
2. c - continues execution till an error or breakpoint is encountered.
3. reset - resets the cpu and sets the pointer to the start.
4. break "address/fn name" - sets a breakpoint at the start of the address/ function name, remove it by clicking or using unbreak "name".
5. step ["value"] - go forward one address if no value, or go forward value addresses. Value is in **hex**.
6. out - executes till the next ret instruction is encountered.
7. let "reg"="value" - sets the value for the register.
8. read "addr" "no.of.bytes" - displays the value at address for the number of bytes.
9. solve - No debugging, final solution entering for completing the problem.

### Challenge 0 - Tutorial
---
Running it through without any modifications the first time, it shows that function named check_password() is called after we input the password, which is copied to r15. We will analyse check_password() now.

#### check_password()
Copies whatever value @r15 to r14, increments r15 and r12, checks if r14 is null, repeated until so. Once r14 does become null, r12 is checked whether has the value of 9.

This means the password should be of length 8 (with null character becomes 9), in order for r12 to become 9.

---
Answer: Any password value with a length of 8 (exactly) will break this.

### Challenge 1 - New Orleans
---
The first actual challenge of this wargame site. Also easy to crack. Running through the program once, we can see that the code calls the function set_password() before getting our input. It seems interesting. check_password() this time just checks if the entered input is the same as the generated password, so we will focus on set_password().

#### set_password()
This function literally sets the password as 3b613a2e67517a in hex, (cause null values cannot be entered as text), in r14 which is then used to compare. Straight forward.

---
Answer: 3b613a2e67517a is the hex input to break.

---

### Challenge 2 - Sydney
---
They removed the set_password() function and made it harder for us to bypass. No worries, the problem just falls back on get_password(). Lets analyse it.

#### check_password()
It compares the following: The first two bytes of the input as 0x724d, next two 0x2124, next two 0x3e2e, next two 0x2960, failing any one set fails the check. But wait! The CPU processes values as Little Endian format, meaning 4d is stored first before 72 in memory for the first set.

###### def: Little Endian
The least significant byte (the "little end") of the data is placed at the byte with the lowest address.

---
Answer: 4d7224212e3e6029 is the hex input to break.

### Challenge 3 - Hanoi
---
