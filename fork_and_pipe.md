Note về fork và pipe (chỉ dành cho Linux)

fork dùng để tạo ra một process con từ process cha. Khi gọi lệnh này, process con sẽ được copy vùng nhớ như của process cha và được thực hiện tiếp từ điểm gọi lệnh fork.
Một điểm lưu ý: Mặc dù process chả là multi-thread, nhưng sau khi thực hiện lệnh fork, process con được tạo ra chỉ là single thread và thread chính của process con lúc này giống như thread ở process cha thực hiện lệnh fork.
Do đó, lệnh fork nên/luôn luôn được gọi ở thread chính (main thread) của process cha.

pipe dùng để tạo 1 pipe để truyền dữ liệu giữa 2 process.
Cách dùng:

#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main()
{
	int pipefd[2];
	pipe(pipefd);
}

Lệnh pipe tạo ra một cặp file descriptor, 1 dùng để truyền dữ liệu (pipefd[0]) và 1 để nhận dữ liệu (pipefd[1]).
Tùy theo hướng giao tiếp giữa 2 process (truyền -> nhận; nhận <- truyền; truyền, nhận <-> nhận, truyền) mà ta có thể đóng các file descriptor tương ứng
Một lưu ý nữa, lệnh pipe luôn được dùng ngay trước lệnh fork, điều này khiến cho process con và process cha cùng chia sẻ được pipe này (do file descriptor là duy nhất trong hệ thống UNIX like).

Ví dụ về giao tiếp giữa 2 process cha và con, process cha chỉ truyền, process con chỉ nhận:

/*****************************************************************************
 Excerpt from "Linux Programmer's Guide - Chapter 6"
 (C)opyright 1994-1995, Scott Burkett
 ***************************************************************************** 
 MODULE: pipe.c
 *****************************************************************************/

#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main(void)
{
        int     fd[2], nbytes;
        pid_t   childpid;
        char    string[] = "Hello, world!\n";
        char    readbuffer[80];

        pipe(fd);
        
        if((childpid = fork()) == -1)
        {
                perror("fork");
                exit(1);
        }

        if(childpid == 0)
        {
                /* Child process closes up input side of pipe */
                close(fd[0]);

                /* Send "string" through the output side of pipe */
                write(fd[1], string, (strlen(string)+1));
                exit(0);
        }
        else
        {
                /* Parent process closes up output side of pipe */
                close(fd[1]);

                /* Read in a string from the pipe */
                nbytes = read(fd[0], readbuffer, sizeof(readbuffer));
                printf("Received string: %s", readbuffer);
        }
        
        return(0);
}