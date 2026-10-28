# Linux-IPC-Semaphores
Ex05-Linux IPC-Semaphores

# AIM:
To Write a C program that implements a producer-consumer system with two processes using Semaphores.

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - Sempahores

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## Write a C program that implements a producer-consumer system with two processes using Semaphores.
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
#include <sys/sem.h>
#include <sys/ipc.h>

int main() {
    int semid;
    struct sembuf sb;

    semid = semget(IPC_PRIVATE, 1, 0666 | IPC_CREAT);

    if (semid == -1) {
        perror("semget");
        exit(1);
    }

    semctl(semid, 0, SETVAL, 0);

    pid_t pid = fork();

    if (pid == 0) {
        printf("Consumer: Waiting for item...\n");

        sb.sem_num = 0;
        sb.sem_op = -1;
        sb.sem_flg = 0;
        semop(semid, &sb, 1);

        printf("Consumer: Item received\n");
        exit(0);
    }
    else if (pid > 0) {
        printf("Producer: Producing item...\n");
        sleep(2);

        printf("Producer: Item produced\n");

        sb.sem_num = 0;
        sb.sem_op = 1;
        sb.sem_flg = 0;
        semop(semid, &sb, 1);

        wait(NULL);
        semctl(semid, 0, IPC_RMID);
    }
    else {
        perror("fork");
        semctl(semid, 0, IPC_RMID);
        exit(1);
    }

    return 0;
}
```
## OUTPUT
$ ./sem.o 

<img width="688" height="130" alt="image" src="https://github.com/user-attachments/assets/a0bdefcb-baa3-478b-b710-c2f73f9dad49" />

$ ipcs

<img width="747" height="426" alt="image" src="https://github.com/user-attachments/assets/0c3e2e3e-3859-4b40-817d-fa36bea242cf" />

# RESULT:
The program is executed successfully.
