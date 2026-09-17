# SIGNALS

1. Signals are the software interrupt to a process. 
2. Suppose you are running ./app on a terminal and you hit ctrl+c --> Terminal sends SIGINT signal to the process --> And the process is terminated. 
3. Unix "kill" command is one way to send signals to a process --> To kill a given process: kill -pid --> The default signal handler is executed -->
   This is defined in the local man pages.
   
## Most Common Signals

| Signal | Can be Caught? | Typical Usage |
|----------|----------|----------|
| SIGINT | Yes | Ctrl+C handling |
| SIGTERM | Yes | Graceful shutdown |
| SIGKILL | No | Force shutdown |
| SIGSTOP | No | Pause process |
| SIGCONT | Yes | Resume process |
| SIGCHLD | Yes | Child process notification |
| SIGSEGV | Yes* | Segmentation fault debugging |
| SIGUSR1 | Yes | Application-specific event |
| SIGUSR2 | Yes | Application-specific event |

## Signal flow
1. kill 1234         --> kill 1234 ( Kills the process via SIGTERM SIGNAL by default ).
2. kill -9 1234      -->  kill a process, with a specific signal.
3. kill(pid,SIGTERM) --> A signal can be sent using this kill function.  
4. sigaction()       --> There is a way to catch a signal , and decide what function should be implemented before the process exits. Use sigaction to do this
   ```
   int sigaction(int sig, const struct sigaction *act , struct sigaction *oact);
   ```
   ```
   STRUCT SIGACTION {
    void (*sa_handler)(int); // Signal handler 
    sigset_t sa_mask;        // which signal must be blocked when the current signal handler execution is happening
    int  sa_flags;           // how kernel manages the signal - modify the behaviour of the handler
   };
   ```
   sa_handler: is a signal handler function, which returns void and takes int as a parameter
   sa_mask: Mask which blocks other signals to take over the execution - sigaddset() used to add specific signals into this list - sigemptyset() used to clear the mask
   Register the signal, with an associated handler using a system call sigaction(). 
   ```
   void sigint_handler(int sig)
   {
     (void)sig;
      const char msg[] = "SIGINT\n";
      write(1,msg,sizeof(msg));
   }

   int main(void)
   {
     char s[200];
     struct sigaction sa = {
          .sa_handler = sigint_handler,
          .sa_flags = 0,
     };
     sigemptyset(&sa.sa_mask);
     if (sigaction(SIGINT, &sa, NULL) == -1)
     {
         perror("sigaction");
         exit(1);
     }
   }
   ```
## Shared state 

kill()/kill command ---> SEND signal
sigaction() ---> HANDLE signal
raise() ---> Send signal to yourself
