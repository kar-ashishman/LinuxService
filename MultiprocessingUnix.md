# FORK

fork creates a new process and copies the code below the fork line in the new process <br>
fork returns the process id, which is 0 for child and non zero for main process

```
#include <unistd.h>
#include <stdio.h>
int main() {
  int id = fork();
  if(id == 0) printf("Hello from child process\n");
  else printf("Hello from main process - %d", id);
}
```
produces
```
Hello from child process
Hello from main process - 3328
```

# WAITING FOR PROCESSES TO GET OVER

Lets create two processes. 1 to print 1 - 5 and 2nd to print 6 - 10. But sequencially 1 - 5 and 6 - 10 <br>
Look at the following program 
```
#include <stdio.h>
#include <unistd.h>
int main()
{
    int id = 0, n;
    id = fork();
    if(id == 0) n = 1;
    else n = 6;
    for(int i = n; i < n+5; i++) {
        printf("%d ", i);
        fflush(stdout);
    }
    return 0;
}
```
produces
`6 7 8 9 10 1 2 3 4 5` (mostly) <br>
The way to bring sequence is wait for the child process to finish execution.

```
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
int main()
{
    int id = 0, n;
    id = fork();
    if(id == 0) n = 1;
    else n = 6;
    int res;
    if(id) wait(&res); // the argument can be a NULL as well
    for(int i = n; i < n+5; i++) {
        printf("%d ", i);
        fflush(stdout);
    }
    return 0;
}
```
produces
`1 2 3 4 5 6 7 8 9 10` <br>
wait() return -1 if there is no child to wait for <br>
or it returns the pid for which the wait got completed.

# SOME USEFUL APIS

* get Process id `getpid()`
* get Parent Process id `getppid()`
* wait for child to finish execution `int wait(int * or NULL)`, returns the child process pid after it returns or -1 if no child to wait for.

# SOME IMPORTANT CONCEPTS

* If a child is spawned and before it returns, the parent is dead, then its ppid is not the earlier parent pid. Its dynamically assigned by the OS.

# VISUALIZING FORK

lets visualize this program
```
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
int main()
{
    int id = 0;
    id = fork();
    id = fork();
    printf("%d\n", id);
    int res = 0;
    while(res != -1)
        res = wait(NULL);
    return 0;
}
// Note: Each process waits for the wait() call to return a -1 which means there are no child to wait for.
```
produces 
```
483
0
485
0
```

```mermaid
flowchart TD
A[Proc] --> B[Proc] --> C[Proc] --> D[Child Proc <br> Prints 0 and returns]
B --> E[Parent Process ending<br>Prints non zero and returns]
A --> F[Proc] --> G[Child process <br>Prints 0 and returns]
A --> H[Parent process<br>Prints non zero and Returns]
```
Another way of visualization
```
int main()
{
    int id1, id2;
    id1 = fork();
    id2 = fork();   
    if(id1 == 0) { // child {
        if(id2 == 0) printf("Child of the child\n");
        else printf("Child\n");
    }
    else {
        if (id2 == 0) printf("Child of the main\n");
        else printf("Main");
    }
    int res = 0;
    while(res != -1) res = wait(NULL);
    return 0;
}
```
Produces 
```
Child
Child of the child
Child of the main
Main
```

## ERROR CHECKS

best way to wait is `while(wait(NULL) != -1 || errno != ECHILD)` <br>
include `<errno.h>` for getting `errno` and `ECHILD`
`fork() can return -1` if forking is unsuccessful


# INTERPROCESS COMMUNICATION `PIPES`

A pipe is an in-memory file with a read and write end <br>
```
inf fd[2];
pipe(fd); // Returns -1 if pipe crreation fails
       =========================
Reading end fd[0]       Writing end fd[1]      
       =========================
```
Exercise - Create a child and send some data from the child to the main process

```
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main()
{
    int fd[2], id;
    if(pipe(fd) == -1) {
        printf("Error creating pipe");
        return 1;
    } id = fork();
    if(id == -1) {
        printf("Error forking");
        return 2;
    } if(id == 0) {
        int x = 100;
        close(fd[0]); // child doesnt read from the pipe. so read terminal can be closed
        // write to the pipe a value of 100
        if(write(fd[1], &x, sizeof(int)) == -1) return 3;
        close(fd[1]); // close the write terminal after writing
    } else {
        // wait for child to process in Main process
        while(wait(NULL) != -1);
        int x;
        close(fd[1]); // main process doesnt write from the pipe. so write terminal can be closed
        if(read(fd[0], &x, sizeof(int)) == -1) return 4;
        close(fd[0]); // close read terminal after read
        printf("Read from pipe %d\n", x);
    } return 0;
}
```

# INTERPROCESS COMMUNICATION `fifo or namedpipes`

`pipe` is not an actual file, they are in memory construct that acts as files. where we can read and write using file descriptors <br>
`fifo` is an actual file that is stored. <br>
Special rule for a FIFO is - a FIFO always hangs at read if anyother process or thread has not opened the FIFO for writing. (viceversa) <br>
FIFO can be created by using mkfifo("<name>", <permission e.g. 0777>) or by mkfifo command e.g. `mkfifo -m 644 pipe1` <br>
open for WRITING a FIFO using c syntax `int fd = open("<fifo full path>", O_WRONLY)` or using `cat <fifopath>` <br>
open for READING a FIFO using c syntax `int fd =  open("<fifo full path>", O_RDONLY)` or using `cat <fifopaht>` <br>
open function call can fail and return a -1 <br>
To use FIFOs in C programs these headers are needed
```
#include <types.h>
#include <sys/stat.h>
#include <errno.h>
```






