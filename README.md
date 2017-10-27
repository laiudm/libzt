# libzt
*Embed ZeroTier directly into your app*

My local copy, with original repository [here](https://github.com/zerotier/libzt.git)

***



<a href="https://www.zerotier.com/?pk_campaign=github_libzt"><img src="https://raw.githubusercontent.com/zerotier/ZeroTierOne/master/artwork/ZeroTierIcon.png" width="128" height="128" align="left" hspace="20" vspace="9"></a>

A library version of the popular [ZeroTier One](https://github.com/zerotier/ZeroTierOne), **libzt** makes it easy to securely connect devices, servers, cloud VMs, containers, and apps everywhere and manage them at scale. Now you can bake this ability directly into your app or service using your preferred language or framework. We provide a BSD socket-like API supporting `SOCK_STREAM`, `SOCK_DGRAM`, and `SOCK_RAW` to make the integration simple. There's also no longer any need for system-wide virtual interfaces. This connection is exclusive to your app and fully encrypted via the [Salsa20](https://en.wikipedia.org/wiki/Salsa20) cipher.

<hr>

[![irc](https://img.shields.io/badge/IRC-%23zerotier%20on%20freenode-orange.svg)](https://webchat.freenode.net/?channels=zerotier)

Pre-Built Binaries Here: [zerotier.com/download.shtml](https://zerotier.com/download.shtml?pk_campaign=github_libzt).

*** 

### Example

```
#include "libzt.h"

char *str = "welcome to the machine"; // test msg 
char *nwid = "c7cd7c9e1b0f52a2";      // network to join
char *path = "zt1";                   // path where this node's keys and configs will be stored
char *ip = "10.8.8.42";               // host on ZeroTier network
int port = 8080;                      // resource's port

struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_addr.s_addr = inet_addr(ip);
addr.sin_port = hton(port);	

zts_simple_start(path, nwid);
int fd = zts_socket(AF_INET, SOCK_STREAM, 0);
zts_connect(fd, (const struct sockaddr *)addr, sizeof(addr));
zts_write(fd, str, strlen(str));
zts_close(fd);
```

Bindings for various [languages](examples)

For an example using only the [Virtual Layer 2](https://www.zerotier.com/manual.shtml#2_2?pk_campaign=github_libzt), see [test/layer2.cpp](test/layer2.cpp)

***

### Building (linux, macos, bsd, win, ios)

 ```
 git submodule init
 git submodule update
 make static_lib
 make tests
 ```
 
 All targets will output to `build/`. Complete instructions [here](BUILDING.md)

***

### Testing and Debugging
 - See [TESTING.md](TESTING.md)


### Contributing

Please make pull requests against the `dev` branch. The `master` branch is release, and `edge` is for unstable and work in progress changes and is not likely to work.

### License
 - **Commercial**, with BSD license, build using `lwIP` network stack with `STACK_LWIP=1`, then contact us directly via `contact@zerotier.com` to discuss commercial licensing.
 - **Personal**, with GPL license, build using `picoTCP` network stack with `STACK_PICO=1`, everything is free. Have fun.

 Regardless of which network stack you build with, the socket API will remain the same.