# GDB baby step 1
*Flag:* picoCTF{549698}

- first I had to install gcc and gdb for my WSL. To do this, I followed a YouTube tutorial and a website, both of which I have referenced at the bottom.
  the versions after downloading are shown below:
  ```
  srishti_7@DESKTOP-DEOL9I4:~$ gcc --version
  gcc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0
  Copyright (C) 2021 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

  srishti_7@DESKTOP-DEOL9I4:~$ gdb --version
  GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
  Copyright (C) 2022 Free Software Foundation, Inc.
  License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
  This is free software: you are free to change and redistribute it.
  There is NO WARRANTY, to the extent permitted by law.
  ```
- then I opened the file with gdb:
```
srishti_7@DESKTOP-DEOL9I4:/mnt/c/Users/DELL/Downloads$ gdb debugger0_a
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_a...
(No debugging symbols found in debugger0_a)
```
- I then used a website to get all the commands I can use in gdb. from this, I found that `info functions` allows to see all the functions present in the program.
```
(gdb) info functions
All defined functions:

Non-debugging symbols:
0x0000000000001000  _init
0x0000000000001030  __cxa_finalize@plt
0x0000000000001040  _start
0x0000000000001070  deregister_tm_clones
0x00000000000010a0  register_tm_clones
0x00000000000010e0  __do_global_dtors_aux
0x0000000000001120  frame_dummy
0x0000000000001129  main
0x0000000000001140  __libc_csu_init
0x00000000000011b0  __libc_csu_fini
0x00000000000011b8  _fini
(gdb)
```
- from the challenge description it was clear that I'd have to work with the main function. to disassemble the main function, I found the command from the same list of commands I used earlier:
```
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
```
- I wasn't able to make sense of the output but since eax was given at the bottom I figured I wwas on the right path. I used a paper on disassembled code (linked in references) to understand the given output.
- I understood that `mov    $0x86342,%eax` meant that the value 0x86342 was being loaded into the eax register. therefore that was the required value
- I converted 0x86342 into hexadecimal using an online converter and got the value 549698                
![converison](https://github.com/user-attachments/assets/17630245-f0e0-4eb5-8e66-d49e0b229cb2)


Learnings: 
- what gdb is, how it is used, some of its basic commands
- instead of gdb, the `objdump -d` command is a good alternative for code disassembly
- learnt that registers are basically memory locations that can be accessed by the processor without slowing it down
- how to read assembly language (at least some of it)

Incorrect methods I tried:
- used `strings` command (did not work because it just searches for readable data and the value required was simply loaded into the eax register and not directly readable)
  
References:
- https://www.youtube.com/watch?v=ISVpxDGVCvo and https://stackoverflow.com/questions/62215963/how-to-install-gcc-and-gdb-for-wslwindows-subsytem-for-linux to install gnu debugger     
- http://www.yolinux.com/TUTORIALS/GDB-Commands.html
- https://www.sciencedirect.com/topics/computer-science/disassembled-code to read assembly language
- https://www.tutorialspoint.com/assembly_programming/assembly_registers.htm to understand registers




# Vault Door 3

*Flag:* picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}

Procedure:
- I downloaded the java file, which showed me this code:
  
  ```
  import java.util.*;
  class VaultDoor3 {
    public static void main(String args[]) {
        VaultDoor3 vaultDoor = new VaultDoor3();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
        }
    }

    // Our security monitoring team has noticed some intrusions on some of the
    // less secure doors. Dr. Evil has asked me specifically to build a stronger
    // vault door to protect his Doomsday plans. I just *know* this door will
    // keep all of those nosy agents out of our business. Mwa ha!
    //
    // -Minion #2671
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm12g94c_u_4_m7ra41");
    }
  }
- I put this code into online compilers but it wasn't working.
- I have no prior experience with java, however, it seemed very similar to C, a language I'm quite familiar with.
- So I tried writing my own code in C to mimic the operations given in the source code:
- ![image](https://github.com/user-attachments/assets/91389144-8a69-4010-8c2f-6149d9dcf228)

- I just pasted the first half of the output buffer within the curly brackets of picoCTF{} and got the flag

References: none
