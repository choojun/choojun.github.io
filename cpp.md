# C/C++

###  Replacement for header
The replacement for direct.h and io.h. You also need to avoid duble includes, by removing them before build the C/C++ project.

```
#ifdef _WIN32
#include <winsock2.h>
#include <windows.h>
#include <process.h> /* _beginthread, _endthread */
#include <time.h>
#include <stdio.h>
#include <io.h>
#include <conio.h>
#include <ctype.h>
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <direct.h>
#include <stdlib.h>
#endif
```

```
#ifdef __linux__
#include <time.h>
#include <syscall.h>
#include <iostream.h>
#include <sched.h>
#include <pthread.h>
#include </usr/include/linux/threads.h>
#include <thread_db.h>
#include <memory.h>
#include <stdio.h> /* getch,kbhit*/
#include <termios.h>
#include <unistd.h>
#include <sys/io.h>
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include <netdb.h>
#include <errno.h>
#include <sys/utsname.h>
#include <sys/ioctl.h>
#include <sgtty.h>
#include <string.h>
#include <dirent.h>
#include <linux/stat.h>
#include <unistd.h>
#endif
```
