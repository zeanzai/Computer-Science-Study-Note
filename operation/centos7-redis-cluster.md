[TOC]

# 1. 准备
## 1.1 centos
``` shell
// 查看版本
[root@studyCentOS7 ~]# cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core) 

// 查看位数
[root@studyCentOS7 ~]# getconf LONG_BIT
64
```
## 1.2 redis
[下载地址](https://redis.io/)，下载最近的stable版本：redis-4.0.6.tar.gz

# 2. 上传
使用filezilla进行上传即可。

# 3. 安装
## 3.1 编译环境
由于Redis是由ANSI C语言编写，故需要c语言的编译环境。
### 3.1.1 检查环境
```shell
[root@studyCentOS7 ~]# rpm -qa | grep gcc-c++
gcc-c++-4.8.5-16.el7.x86_64
```
检查环境之后发现已经安装过gcc-c++工具包，所以不用安装可以直接进入3.2进行编译源码。
### 3.1.2 安装
``` shell
[root@studyCentOS7 ~]# yum install gcc-c++
```

## 3.2 解压并编译源码
```shell
[root@studyCentOS7 ~]# cd /software_bak/
[root@studyCentOS7 software_bak]# tar -zxf redis-4.0.6.tar.gz 
[root@studyCentOS7 software_bak]# ll
总用量 5063000
-rw-r--r--. 1 root root    8924465 11月 22 14:43 apache-tomcat-7.0.70.tar.gz
-rw-r--r--. 1 root root    9305616 11月 22 14:43 apache-tomcat-8.0.38.tar.gz
-rw-r--r--. 1 root root    9376359 11月 22 14:44 apache-tomcat-8.5.14.tar.gz
-rw-r--r--. 1 root root 4329570304 11月 25 19:28 CentOS-7-x86_64-DVD-1511.iso
-rw-r--r--. 1 root root  181442359 11月 22 14:44 jdk-8u111-linux-x64.gz
drwxr-xr-x. 2 root root       4096 11月 29 11:11 mysql5.7.16
-rw-r--r--. 1 root root  569978880 11月 22 14:45 mysql-5.7.16-1.el7.x86_64.rpm-bundle.tar
-rw-r--r--. 1 root root   73187012 11月 29 19:20 nexus-2.14.5-02-bundle.tar.gz
drwxr-xr-x. 9 1001 1001       4096 11月 22 15:47 nginx-1.12.2
-rw-r--r--. 1 root root     981687 11月 22 15:37 nginx-1.12.2.tar.gz
drwxrwxr-x. 6 root root       4096 12月  5 01:01 redis-4.0.6
-rw-r--r--. 1 root root    1723533 12月  6 11:18 redis-4.0.6.tar.gz
[root@studyCentOS7 software_bak]# cd redis-4.0.6
[root@studyCentOS7 redis-4.0.6]# ll
总用量 288
-rw-rw-r--.  1 root root 142334 12月  5 01:01 00-RELEASENOTES
-rw-rw-r--.  1 root root     53 12月  5 01:01 BUGS
-rw-rw-r--.  1 root root   1815 12月  5 01:01 CONTRIBUTING
-rw-rw-r--.  1 root root   1487 12月  5 01:01 COPYING
drwxrwxr-x.  6 root root   4096 12月  5 01:01 deps
-rw-rw-r--.  1 root root     11 12月  5 01:01 INSTALL
-rw-rw-r--.  1 root root    151 12月  5 01:01 Makefile
-rw-rw-r--.  1 root root   4223 12月  5 01:01 MANIFESTO
-rw-rw-r--.  1 root root  20543 12月  5 01:01 README.md
-rw-rw-r--.  1 root root  57764 12月  5 01:01 redis.conf
-rwxrwxr-x.  1 root root    271 12月  5 01:01 runtest
-rwxrwxr-x.  1 root root    280 12月  5 01:01 runtest-cluster
-rwxrwxr-x.  1 root root    281 12月  5 01:01 runtest-sentinel
-rw-rw-r--.  1 root root   7606 12月  5 01:01 sentinel.conf
drwxrwxr-x.  3 root root   4096 12月  5 01:01 src
drwxrwxr-x. 10 root root   4096 12月  5 01:01 tests
drwxrwxr-x.  8 root root   4096 12月  5 01:01 utils
[root@studyCentOS7 redis-4.0.6]# make
cd src && make all
make[1]: 进入目录“/software_bak/redis-4.0.6/src”
    CC Makefile.dep
make[1]: 离开目录“/software_bak/redis-4.0.6/src”
make[1]: 进入目录“/software_bak/redis-4.0.6/src”
rm -rf redis-server redis-sentinel redis-cli redis-benchmark redis-check-rdb redis-check-aof *.o *.gcda *.gcno *.gcov redis.info lcov-html Makefile.dep dict-benchmark
(cd ../deps && make distclean)
make[2]: 进入目录“/software_bak/redis-4.0.6/deps”
(cd hiredis && make clean) > /dev/null || true
(cd linenoise && make clean) > /dev/null || true
(cd lua && make clean) > /dev/null || true
(cd jemalloc && [ -f Makefile ] && make distclean) > /dev/null || true
(rm -f .make-*)
make[2]: 离开目录“/software_bak/redis-4.0.6/deps”
(rm -f .make-*)
echo STD=-std=c99 -pedantic -DREDIS_STATIC='' >> .make-settings
echo WARN=-Wall -W -Wno-missing-field-initializers >> .make-settings
echo OPT=-O2 >> .make-settings
echo MALLOC=jemalloc >> .make-settings
echo CFLAGS= >> .make-settings
echo LDFLAGS= >> .make-settings
echo REDIS_CFLAGS= >> .make-settings
echo REDIS_LDFLAGS= >> .make-settings
echo PREV_FINAL_CFLAGS=-std=c99 -pedantic -DREDIS_STATIC='' -Wall -W -Wno-missing-field-initializers -O2 -g -ggdb   -I../deps/hiredis -I../deps/linenoise -I../deps/lua/src -DUSE_JEMALLOC -I../deps/jemalloc/include >> .make-settings
echo PREV_FINAL_LDFLAGS=  -g -ggdb -rdynamic >> .make-settings
(cd ../deps && make hiredis linenoise lua jemalloc)
make[2]: 进入目录“/software_bak/redis-4.0.6/deps”
(cd hiredis && make clean) > /dev/null || true
(cd linenoise && make clean) > /dev/null || true
(cd lua && make clean) > /dev/null || true
(cd jemalloc && [ -f Makefile ] && make distclean) > /dev/null || true
(rm -f .make-*)
(echo "" > .make-cflags)
(echo "" > .make-ldflags)
MAKE hiredis
cd hiredis && make static
make[3]: 进入目录“/software_bak/redis-4.0.6/deps/hiredis”
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  net.c
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  hiredis.c
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  sds.c
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  async.c
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  read.c
ar rcs libhiredis.a net.o hiredis.o sds.o async.o read.o
make[3]: 离开目录“/software_bak/redis-4.0.6/deps/hiredis”
MAKE linenoise
cd linenoise && make
make[3]: 进入目录“/software_bak/redis-4.0.6/deps/linenoise”
cc  -Wall -Os -g  -c linenoise.c
make[3]: 离开目录“/software_bak/redis-4.0.6/deps/linenoise”
MAKE lua
cd lua/src && make all CFLAGS="-O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC='' " MYLDFLAGS="" AR="ar rcu"
make[3]: 进入目录“/software_bak/redis-4.0.6/deps/lua/src”
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lapi.o lapi.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lcode.o lcode.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o ldebug.o ldebug.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o ldo.o ldo.c
ldo.c: 在函数‘f_parser’中:
ldo.c:496:7: 警告：未使用的变量‘c’ [-Wunused-variable]
   int c = luaZ_lookahead(p->z);
       ^
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o ldump.o ldump.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lfunc.o lfunc.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lgc.o lgc.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o llex.o llex.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lmem.o lmem.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lobject.o lobject.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lopcodes.o lopcodes.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lparser.o lparser.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lstate.o lstate.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lstring.o lstring.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o ltable.o ltable.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o ltm.o ltm.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lundump.o lundump.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lvm.o lvm.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lzio.o lzio.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o strbuf.o strbuf.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o fpconv.o fpconv.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lauxlib.o lauxlib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lbaselib.o lbaselib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o ldblib.o ldblib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o liolib.o liolib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lmathlib.o lmathlib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o loslib.o loslib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o ltablib.o ltablib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lstrlib.o lstrlib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o loadlib.o loadlib.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o linit.o linit.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lua_cjson.o lua_cjson.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lua_struct.o lua_struct.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lua_cmsgpack.o lua_cmsgpack.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lua_bit.o lua_bit.c
ar rcu liblua.a lapi.o lcode.o ldebug.o ldo.o ldump.o lfunc.o lgc.o llex.o lmem.o lobject.o lopcodes.o lparser.o lstate.o lstring.o ltable.o ltm.o lundump.o lvm.o lzio.o strbuf.o fpconv.o lauxlib.o lbaselib.o ldblib.o liolib.o lmathlib.o loslib.o ltablib.o lstrlib.o loadlib.o linit.o lua_cjson.o lua_struct.o lua_cmsgpack.o lua_bit.o    # DLL needs all object files
ranlib liblua.a
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o lua.o lua.c
cc -o lua  lua.o liblua.a -lm 
liblua.a(loslib.o)：在函数‘os_tmpname’中：
loslib.c:(.text+0x28c): 警告：the use of `tmpnam' is dangerous, better use `mkstemp'
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o luac.o luac.c
cc -O2 -Wall -DLUA_ANSI -DENABLE_CJSON_GLOBAL -DREDIS_STATIC=''    -c -o print.o print.c
cc -o luac  luac.o print.o liblua.a -lm 
make[3]: 离开目录“/software_bak/redis-4.0.6/deps/lua/src”
MAKE jemalloc
cd jemalloc && ./configure --with-lg-quantum=3 --with-jemalloc-prefix=je_ --enable-cc-silence CFLAGS="-std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops " LDFLAGS=""
checking for xsltproc... /usr/bin/xsltproc
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking how to run the C preprocessor... gcc -E
checking for grep that handles long lines and -e... /usr/bin/grep
checking for egrep... /usr/bin/grep -E
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking whether byte ordering is bigendian... no
checking size of void *... 8
checking size of int... 4
checking size of long... 8
checking size of intmax_t... 8
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking whether pause instruction is compilable... yes
checking for ar... ar
checking malloc.h usability... yes
checking malloc.h presence... yes
checking for malloc.h... yes
checking whether malloc_usable_size definition can use const argument... no
checking whether __attribute__ syntax is compilable... yes
checking whether compiler supports -fvisibility=hidden... yes
checking whether compiler supports -Werror... yes
checking whether tls_model attribute is compilable... yes
checking whether compiler supports -Werror... yes
checking whether alloc_size attribute is compilable... yes
checking whether compiler supports -Werror... yes
checking whether format(gnu_printf, ...) attribute is compilable... yes
checking whether compiler supports -Werror... yes
checking whether format(printf, ...) attribute is compilable... yes
checking for a BSD-compatible install... /usr/bin/install -c
checking for ranlib... ranlib
checking for ld... /usr/bin/ld
checking for autoconf... false
checking for memalign... yes
checking for valloc... yes
checking configured backtracing method... N/A
checking for sbrk... yes
checking whether utrace(2) is compilable... no
checking whether valgrind is compilable... no
checking whether a program using __builtin_ffsl is compilable... yes
checking LG_PAGE... 12
checking pthread.h usability... yes
checking pthread.h presence... yes
checking for pthread.h... yes
checking for pthread_create in -lpthread... yes
checking for library containing clock_gettime... none required
checking for secure_getenv... yes
checking for issetugid... no
checking for _malloc_thread_cleanup... no
checking for _pthread_mutex_init_calloc_cb... no
checking for TLS... yes
checking whether C11 atomics is compilable... no
checking whether atomic(9) is compilable... no
checking whether Darwin OSAtomic*() is compilable... no
checking whether madvise(2) is compilable... yes
checking whether to force 32-bit __sync_{add,sub}_and_fetch()... no
checking whether to force 64-bit __sync_{add,sub}_and_fetch()... no
checking for __builtin_clz... yes
checking whether Darwin OSSpin*() is compilable... no
checking whether glibc malloc hook is compilable... yes
checking whether glibc memalign hook is compilable... yes
checking whether pthreads adaptive mutexes is compilable... yes
checking for stdbool.h that conforms to C99... yes
checking for _Bool... yes
configure: creating ./config.status
config.status: creating Makefile
config.status: creating jemalloc.pc
config.status: creating doc/html.xsl
config.status: creating doc/manpages.xsl
config.status: creating doc/jemalloc.xml
config.status: creating include/jemalloc/jemalloc_macros.h
config.status: creating include/jemalloc/jemalloc_protos.h
config.status: creating include/jemalloc/jemalloc_typedefs.h
config.status: creating include/jemalloc/internal/jemalloc_internal.h
config.status: creating test/test.sh
config.status: creating test/include/test/jemalloc_test.h
config.status: creating config.stamp
config.status: creating bin/jemalloc-config
config.status: creating bin/jemalloc.sh
config.status: creating bin/jeprof
config.status: creating include/jemalloc/jemalloc_defs.h
config.status: creating include/jemalloc/internal/jemalloc_internal_defs.h
config.status: creating test/include/test/jemalloc_test_defs.h
config.status: executing include/jemalloc/internal/private_namespace.h commands
config.status: executing include/jemalloc/internal/private_unnamespace.h commands
config.status: executing include/jemalloc/internal/public_symbols.txt commands
config.status: executing include/jemalloc/internal/public_namespace.h commands
config.status: executing include/jemalloc/internal/public_unnamespace.h commands
config.status: executing include/jemalloc/internal/size_classes.h commands
config.status: executing include/jemalloc/jemalloc_protos_jet.h commands
config.status: executing include/jemalloc/jemalloc_rename.h commands
config.status: executing include/jemalloc/jemalloc_mangle.h commands
config.status: executing include/jemalloc/jemalloc_mangle_jet.h commands
config.status: executing include/jemalloc/jemalloc.h commands
===============================================================================
jemalloc version   : 4.0.3-0-ge9192eacf8935e29fc62fddc2701f7942b1cc02c
library revision   : 2

CONFIG             : --with-lg-quantum=3 --with-jemalloc-prefix=je_ --enable-cc-silence 'CFLAGS=-std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops ' LDFLAGS=
CC                 : gcc
CFLAGS             : -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -fvisibility=hidden
CPPFLAGS           :  -D_GNU_SOURCE -D_REENTRANT
LDFLAGS            : 
EXTRA_LDFLAGS      : 
LIBS               :  -lpthread
TESTLIBS           : 
RPATH_EXTRA        : 

XSLTPROC           : /usr/bin/xsltproc
XSLROOT            : 

PREFIX             : /usr/local
BINDIR             : /usr/local/bin
DATADIR            : /usr/local/share
INCLUDEDIR         : /usr/local/include
LIBDIR             : /usr/local/lib
MANDIR             : /usr/local/share/man

srcroot            : 
abs_srcroot        : /software_bak/redis-4.0.6/deps/jemalloc/
objroot            : 
abs_objroot        : /software_bak/redis-4.0.6/deps/jemalloc/

JEMALLOC_PREFIX    : je_
JEMALLOC_PRIVATE_NAMESPACE
                   : je_
install_suffix     : 
autogen            : 0
cc-silence         : 1
debug              : 0
code-coverage      : 0
stats              : 1
prof               : 0
prof-libunwind     : 0
prof-libgcc        : 0
prof-gcc           : 0
tcache             : 1
fill               : 1
utrace             : 0
valgrind           : 0
xmalloc            : 0
munmap             : 0
lazy_lock          : 0
tls                : 1
cache-oblivious    : 1
===============================================================================
cd jemalloc && make CFLAGS="-std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops " LDFLAGS="" lib/libjemalloc.a
make[3]: 进入目录“/software_bak/redis-4.0.6/deps/jemalloc”
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/jemalloc.o src/jemalloc.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/arena.o src/arena.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/atomic.o src/atomic.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/base.o src/base.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/bitmap.o src/bitmap.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/chunk.o src/chunk.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/chunk_dss.o src/chunk_dss.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/chunk_mmap.o src/chunk_mmap.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/ckh.o src/ckh.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/ctl.o src/ctl.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/extent.o src/extent.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/hash.o src/hash.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/huge.o src/huge.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/mb.o src/mb.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/mutex.o src/mutex.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/pages.o src/pages.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/prof.o src/prof.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/quarantine.o src/quarantine.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/rtree.o src/rtree.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/stats.o src/stats.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/tcache.o src/tcache.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/util.o src/util.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/tsd.o src/tsd.c
ar crus lib/libjemalloc.a src/jemalloc.o src/arena.o src/atomic.o src/base.o src/bitmap.o src/chunk.o src/chunk_dss.o src/chunk_mmap.o src/ckh.o src/ctl.o src/extent.o src/hash.o src/huge.o src/mb.o src/mutex.o src/pages.o src/prof.o src/quarantine.o src/rtree.o src/stats.o src/tcache.o src/util.o src/tsd.o
make[3]: 离开目录“/software_bak/redis-4.0.6/deps/jemalloc”
make[2]: 离开目录“/software_bak/redis-4.0.6/deps”
    CC adlist.o
    CC quicklist.o
    CC ae.o
    CC anet.o
    CC dict.o
    CC server.o
    CC sds.o
    CC zmalloc.o
    CC lzf_c.o
    CC lzf_d.o
    CC pqsort.o
    CC zipmap.o
    CC sha1.o
    CC ziplist.o
    CC release.o
    CC networking.o
    CC util.o
    CC object.o
    CC db.o
    CC replication.o
    CC rdb.o
    CC t_string.o
    CC t_list.o
    CC t_set.o
    CC t_zset.o
    CC t_hash.o
    CC config.o
    CC aof.o
    CC pubsub.o
    CC multi.o
    CC debug.o
    CC sort.o
    CC intset.o
    CC syncio.o
    CC cluster.o
    CC crc16.o
    CC endianconv.o
    CC slowlog.o
    CC scripting.o
    CC bio.o
    CC rio.o
    CC rand.o
    CC memtest.o
    CC crc64.o
    CC bitops.o
    CC sentinel.o
    CC notify.o
    CC setproctitle.o
    CC blocked.o
    CC hyperloglog.o
    CC latency.o
    CC sparkline.o
    CC redis-check-rdb.o
    CC redis-check-aof.o
    CC geo.o
    CC lazyfree.o
    CC module.o
    CC evict.o
    CC expire.o
    CC geohash.o
    CC geohash_helper.o
    CC childinfo.o
    CC defrag.o
    CC siphash.o
    CC rax.o
    LINK redis-server
    INSTALL redis-sentinel
    CC redis-cli.o
    LINK redis-cli
    CC redis-benchmark.o
    LINK redis-benchmark
    INSTALL redis-check-rdb
    INSTALL redis-check-aof

Hint: It's a good idea to run 'make test' ;)

make[1]: 离开目录“/software_bak/redis-4.0.6/src”
[root@studyCentOS7 redis-4.0.6]# 
```

## 3.3 安装
```shell
[root@studyCentOS7 redis-4.0.6]# mkdir /softwares/redis4.0.6_singleton
[root@studyCentOS7 redis-4.0.6]# make install PREFIX=/softwares/redis4.0.6_singleton
cd src && make install
make[1]: 进入目录“/software_bak/redis-4.0.6/src”
    CC Makefile.dep
make[1]: 离开目录“/software_bak/redis-4.0.6/src”
make[1]: 进入目录“/software_bak/redis-4.0.6/src”

Hint: It's a good idea to run 'make test' ;)

    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
make[1]: 离开目录“/software_bak/redis-4.0.6/src”
[root@studyCentOS7 redis-4.0.6]# cd /softwares/redis4.0.6_singleton/
[root@studyCentOS7 redis4.0.6_singleton]# ll
总用量 4
drwxr-xr-x. 2 root root 4096 12月  6 11:35 bin
[root@studyCentOS7 redis4.0.6_singleton]# cd bin/
[root@studyCentOS7 bin]# ll
总用量 21812
-rwxr-xr-x. 1 root root 2451462 12月  6 11:35 redis-benchmark
-rwxr-xr-x. 1 root root 5753703 12月  6 11:35 redis-check-aof
-rwxr-xr-x. 1 root root 5753703 12月  6 11:35 redis-check-rdb
-rwxr-xr-x. 1 root root 2616655 12月  6 11:35 redis-cli
lrwxrwxrwx. 1 root root      12 12月  6 11:35 redis-sentinel -> redis-server
-rwxr-xr-x. 1 root root 5753703 12月  6 11:35 redis-server
```

## 3.4 测试
```shell
[root@studyCentOS7 bin]# ./redis-server 
6924:C 06 Dec 11:42:20.702 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
6924:C 06 Dec 11:42:20.702 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=6924, just started
6924:C 06 Dec 11:42:20.702 # Warning: no config file specified, using the default config. In order to specify a config file use ./redis-server /path/to/redis.conf
6924:M 06 Dec 11:42:20.703 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 4.0.6 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 6924
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

6924:M 06 Dec 11:42:20.705 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
6924:M 06 Dec 11:42:20.705 # Server initialized
6924:M 06 Dec 11:42:20.705 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
6924:M 06 Dec 11:42:20.705 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
6924:M 06 Dec 11:42:20.705 * Ready to accept connections

// 另外再打开一个命令行窗口，注意：不要关闭上面上面正在运行的程序【前端启动模式】
[root@studyCentOS7 bin]# ./redis-cli 
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set test 1111 
OK
127.0.0.1:6379> get test
"1111"
```
# 4. 简单操作
## 4.1 启动
**前后端启动模式并不是指使用redis的客户端或者是第三方客户端连接redis。**
### 4.1.1 前端启动
前端启动是指在终端中启动，但是终端关闭后，redis跟着关闭。
```shell
// 在现有的终端中，进入redis的安装目录下，然后执行下面命令
[root@studyCentOS7 bin]# ./redis-server 
6924:C 06 Dec 11:42:20.702 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
6924:C 06 Dec 11:42:20.702 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=6924, just started
6924:C 06 Dec 11:42:20.702 # Warning: no config file specified, using the default config. In order to specify a config file use ./redis-server /path/to/redis.conf
6924:M 06 Dec 11:42:20.703 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 4.0.6 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 6924
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

6924:M 06 Dec 11:42:20.705 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
6924:M 06 Dec 11:42:20.705 # Server initialized
6924:M 06 Dec 11:42:20.705 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
6924:M 06 Dec 11:42:20.705 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
6924:M 06 Dec 11:42:20.705 * Ready to accept connections
6924:M 06 Dec 12:42:21.052 * 1 changes in 3600 seconds. Saving...
6924:M 06 Dec 12:42:21.678 * Background saving started by pid 7457
7457:C 06 Dec 12:42:22.655 * DB saved on disk
7457:C 06 Dec 12:42:22.655 * RDB: 6 MB of memory used by copy-on-write
6924:M 06 Dec 12:42:22.684 * Background saving terminated with success

// 再打开一个终端窗口，在新打开的终端中执行，会发现有相对应的执行进程
[root@studyCentOS7 ~]# ps -aux | grep redis
root       6924  0.0  0.7 145264  7932 pts/0    Sl+  11:42   0:13 ./redis-server *:6379
root       8888  0.0  0.0 112680   976 pts/1    S+   15:29   0:00 grep --color=auto redis

// 然后进入安装目录下，执行下面命令进行测试
[root@studyCentOS7 ~]# cd /softwares/redis4.0.6_singleton/bin/
[root@studyCentOS7 bin]# ./redis-cli 
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set test 1111 
OK
127.0.0.1:6379> get test
"1111"

// 使用ctrl+c关闭执行./redis-server的终端程序，然后在另一个打开的终端中执行下面命令，发现已经没有redis的执行进程了
// 在第一个终端中使用快捷键Ctrl+c
^C6924:signal-handler (1512545618) Received SIGINT scheduling shutdown...
6924:M 06 Dec 15:33:38.345 # User requested shutdown...
6924:M 06 Dec 15:33:38.345 * Saving the final RDB snapshot before exiting.
6924:M 06 Dec 15:33:38.348 * DB saved on disk
6924:M 06 Dec 15:33:38.348 # Redis is now ready to exit, bye bye..
// 在另一个终端中执行查询进程的命令
[root@studyCentOS7 bin]# ps -aux | grep redis
root       8924  0.0  0.0 112680   976 pts/1    R+   15:34   0:00 grep --color=auto redis
```
### 4.1.2 后端启动
后端启动模式是指，redis作为后台应用启动，不会随着终端的关闭而关闭，生产环境基本使用的都是这种启动方式。
后端启动模式需要依赖配置文件，所以先进入解压后的redis安装包下面，将redis.conf文件拷贝到安装目录下的bin下面。
```shell
[root@studyCentOS7 bin]# cd /software_bak/redis-4.0.6/
[root@studyCentOS7 redis-4.0.6]# ll
总用量 296
-rw-rw-r--.  1 root root 142334 12月  5 01:01 00-RELEASENOTES
-rw-rw-r--.  1 root root     53 12月  5 01:01 BUGS
-rw-rw-r--.  1 root root   1815 12月  5 01:01 CONTRIBUTING
-rw-rw-r--.  1 root root   1487 12月  5 01:01 COPYING
drwxrwxr-x.  6 root root   4096 12月  6 11:28 deps
-rw-rw-r--.  1 root root     11 12月  5 01:01 INSTALL
-rw-rw-r--.  1 root root    151 12月  5 01:01 Makefile
-rw-rw-r--.  1 root root   4223 12月  5 01:01 MANIFESTO
-rw-rw-r--.  1 root root  20543 12月  5 01:01 README.md
-rw-rw-r--.  1 root root  57764 12月  5 01:01 redis.conf
-rwxrwxr-x.  1 root root    271 12月  5 01:01 runtest
-rwxrwxr-x.  1 root root    280 12月  5 01:01 runtest-cluster
-rwxrwxr-x.  1 root root    281 12月  5 01:01 runtest-sentinel
-rw-rw-r--.  1 root root   7606 12月  5 01:01 sentinel.conf
drwxrwxr-x.  3 root root   8192 12月  6 11:35 src
drwxrwxr-x. 10 root root   4096 12月  5 01:01 tests
drwxrwxr-x.  8 root root   4096 12月  5 01:01 utils
[root@studyCentOS7 redis-4.0.6]# cp redis.conf /softwares/redis4.0.6_singleton/bin/
```
拷贝完成之后需要修改配置文件，修改配置参数 daemonize 的值为yes。【daemon：守护进程】，下面是修改之后的值
```shell
# A reasonable value for this option is 300 seconds, which is the new
# Redis default starting with Redis 3.2.1.
tcp-keepalive 300

################################# GENERAL #####################################

# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
daemonize yes

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
```
启动
```shell
[root@studyCentOS7 bin]# ./redis-server redis.conf 
9054:C 06 Dec 15:49:36.670 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
9054:C 06 Dec 15:49:36.670 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=9054, just started
9054:C 06 Dec 15:49:36.670 # Configuration loaded
[root@studyCentOS7 bin]# ps -aux | grep redis
root       9055  0.1  0.7 145264  7564 ?        Ssl  15:49   0:00 ./redis-server 127.0.0.1:6379
root       9068  0.0  0.0 112680   976 pts/0    S+   15:49   0:00 grep --color=auto redis
```
## 4.2 关闭
关闭比较简单，不管是前端启动模式还是后端启动模式都可以使用下面两种方式。
```shell
// 在客户端中执行shutdown命令
127.0.0.1:6379> shutdown // 关闭服务器即可

// 第二种：使用终端命令直接关闭，使用 -p 加端口号即可
[root@studyCentOS7 bin]# ./redis-cli –p 6379
```

## 4.3 redis的相关命令
```shell
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set a 10
OK
127.0.0.1:6379> get a
"10"
127.0.0.1:6379> incr a  // a+1
(integer) 11
127.0.0.1:6379> get a
"11"
127.0.0.1:6379> decr a  // a-1
(integer) 10
127.0.0.1:6379> keys *  // 获取所有的key值
1) "a"
127.0.0.1:6379> del a   // 删除key为a的值
(integer) 1
127.0.0.1:6379> keys *
(empty list or set)
```

## 4.4 设置开机自启
```shell
# chkconfig: 2345 10 90  
# description: Start and Stop redis   
  
PATH=/usr/local/bin:/sbin:/usr/bin:/bin   
export PATH 
REDISPORT=6379  
EXEC=/softwares/redis-4.0.6/bin/redis-server   
REDIS_CLI=/softwares/redis-4.0.6/bin/redis-cli   
 
PIDFILE=/var/run/redis_6379.pid  
CONF="/softwares/redis-4.0.6/bin/redis.conf"  
AUTH="1234"  

case "$1" in   
        start)   
                if [ -f $PIDFILE ]   
                then   
                        echo "$PIDFILE exists, process is already running or crashed."  
                else  
                        echo "Starting Redis server..."  
                        $EXEC $CONF   
                fi   
                if [ "$?"="0" ]   
                then   
                        echo "Redis is running..."  
                fi   
                ;;   
        stop)   
                if [ ! -f $PIDFILE ]   
                then   
                        echo "$PIDFILE exists, process is not running."  
                else  
                        PID=$(cat $PIDFILE)   
                        echo "Stopping..."  
                       $REDIS_CLI -p $REDISPORT  SHUTDOWN    
                        sleep 2  
                       while [ -x $PIDFILE ]   
                       do  
                                echo "Waiting for Redis to shutdown..."  
                               sleep 1  
                        done   
                        echo "Redis stopped"  
                fi   
                ;;   
        restart|force-reload)   
                ${0} stop   
                ${0} start   
                ;;   
        *)   
               echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2  
                exit 1  
esac
```

# 5. 集群环境搭建
## 5.1 集群概述
### 5.1.1 集群知识
创建Redis集群，每个主节点都有一个从节点进行备份，让主节点发生错误时，从节点自动连接，实现集群的高可用。
### 5.1.2 集群架构
redis的架构图如下：
![架构图](http://wxyleanote.oss-cn-shenzhen.aliyuncs.com/redis/redis架构图.jpg)

redis-cluster把所有的物理节点映射到[0-16383]slot上,cluster负责维护node<->slot<->value的映射。Redis 集群中内置了 16384 个哈希槽，当需要在 Redis集群中放置一个 key-value 时，redis 先对 key 使用 crc16 算法算出一个结果，然后把结果对16384（2的14次方）求余数，这样每个key都会对应一个编号在0-16383之间的哈希槽，redis会根据节点数量大致均等的将哈希槽映射到不同的节点。此外，由于集群环境中只有2的14次方（16384）个slot（哈希槽），所以，**集群环境中最多只能有16384台主机，此时，每个主机上面只有一个slot。**
### 5.1.3 集群容错机制
如下图：
![容错图](http://wxyleanote.oss-cn-shenzhen.aliyuncs.com/redis/redis容错图.jpg)

 1. master A在连接masterB时，发现masterB没有响应，则masterA就会向集群中广播，然后开始投票。领着投票过程是集群中所有master参与，如果半数以上master节点与masterB节点通信出现(cluster-node-timeout)错误，则认为master B节点挂掉。集群中节点之间的通信是通过ping-pong协议通信的。
 2. 什么时候整个集群不可用(cluster_state:fail)? 
    a: 如果集群任意master挂掉,且当前master没有slave。集群进入fail状态，也可以理解成集群的slot映射[0-16383]不完成时进入fail状态。ps:redis-3.0.0.rc1加入cluster-require-full-coverage参数，默认关闭，打开集群兼容部分失败。

    b: 如果集群超过半数以上master挂掉，无论是否有slave集群进入fail状态。ps:当集群不可用时，所有对集群的操作做都不可用，收到((error) CLUSTERDOWN The cluster is down)错误。

## 5.2 伪集群搭建
由于使用的是window环境下虚拟机的运行环境，如果要进行测试集群环境的话，至少需要六台虚拟机，故采用伪集群式进行搭建。这样的话，就只使用一台虚拟机，然后建立六台Redis实例即可。
为什么至少六个实例？因为集群环境中进行容错机制测试时，需要半数投票通过，所以至少需要三个实例，而每一个实例又有一个备份实例，所以，至少需要有六个实例。
我们要求搭建一个伪集群环境，其中有六个Redis实例，占用的端口是7001~7006。

### 5.2.1 环境准备
**在搭建集群之前，需要将运行着的单例的redis关闭。**
在创建完实例时，由于需要使用/redis-3.0.0/src/redis-trib.rb脚本进行创建集群，而/redis-3.0.0/src/redis-trib.rb脚本的运行，又需要使用一个叫做redis-3.0.0.gem的ruby包，所以需要安装Ruby的运行环境（此过程可以类比Java，一个工具包的运行依赖另外一个工具包，而另外一个工具包需要有Java的运行环境）。
**此过程也可放到六个实例启动之后、创建集群之前进行。**
因为redis4需要的ruby版本要大于等于2.2.2，又因为yum源中没有大于2.2.2的版本，所以需要自己下载，使用2.2.7的版本。[下载地址](https://cache.ruby-lang.org/pub/ruby/2.2/ruby-2.2.7.tar.gz)
```shell
// 查看是否有ruby库，如果有，需要使用rpm -e XXXX命令进行移除
[root@studyCentOS7 bin]# rpm -qa | grep ruby
[root@studyCentOS7 bin]# 

// 使用filezilla软件，将下载好的ruby源码包上传到centos上面

// 安装ruby
[root@studyCentOS7 ruby-2.2.7]# ./configure --prefix=/softwares/ruby-2.2.7
[root@studyCentOS7 ruby-2.2.7]# make && make install

// 需要先删除旧的快捷方式重新创建
[root@studyCentOS7 ruby-2.2.7]# ln -s /softwares/ruby-2.2.7/bin/ruby /usr/bin/ruby

// 查看版本
[root@studyCentOS7 ruby-2.2.7]# ruby -v
ruby 2.2.7p470 (2017-03-28 revision 58194) [x86_64-linux]

// 使用gem安装redis依赖
[root@studyCentOS7 ruby-2.2.7]# cd /softwares/ruby-2.2.7/
[root@studyCentOS7 bin]# ./gem install redis
Fetching: redis-4.0.1.gem (100%)
Successfully installed redis-4.0.1
Parsing documentation for redis-4.0.1
Installing ri documentation for redis-4.0.1
Done installing documentation for redis after 0 seconds
1 gem installed
```
### 5.2.2 创建实例
如果虚拟机上面本来就有一个Redis实例则需要在安装目录下面建立redis-cluster目录，用来安装六个实例，原有实例不需要加入到集群中。
如果虚拟机上面没有Redis实例，则需要在安装过程中直接指定安装目录为redis_cluster。
redis-cluster目录并不是特定或默认安装目录，是便于管理的一个文件夹。
我们假设虚拟机上面已经有一个存在的redis实例，端口为默认端口6379。集群安装完成之后进入安装目录，然后在原有实例安装目录下面建立集群的安装目录redis_cluster。
```shell
[root@studyCentOS7 softwares]# ll
总用量 8
drwxr-xr-x. 9 root root 4096 11月 22 14:46 apache-tomcat-7.0.70
drwxr-xr-x. 8   10  143 4096 9月  23 2016 jdk1.8.0_111
drwxr-xr-x. 7 root root   60 11月 29 10:17 nginx-1.12.2
drwxr-xr-x. 2 root root    6 12月  6 11:34 redis4.0.6_cluster
drwxr-xr-x. 3 root root   16 12月  6 11:35 redis4.0.6_singleton
drwxr-xr-x. 6 root root   52 12月  6 17:21 ruby-2.2.7
[root@studyCentOS7 softwares]# cp -r redis4.0.6_singleton/bin/ ./redis4.0.6_cluster/7001
```
### 5.2.3 修改配置文件
如果此实例非别处拷贝而来，则需要从源码包中拷贝配置文件（redis.conf）到redis01中，并且还需要修改一个参数daemonize=yes（如第4章节安装过程）。
由于直接是从原有实例基础上面拷贝过来的，所以无需配置daemonize参数。直接进行下述配置即可：
```shell
// 删除原有快照文件【如果不是从原有实例中拷贝过来的，则不要要此步骤】
[root@studyCentOS7 7001]# rm -f dump.rdb 

// 修改“端口”和“是否是集群”两个参数，保存关闭
[root@study 7001]# vi redis.conf
port 7001               // 修改端口号：7001
cluster-enabled yes     // 打开注释

// 拷贝剩余五个实例
[root@studyCentOS7 redis4.0.6_cluster]# cp -r 7001/ ./7002
[root@studyCentOS7 redis4.0.6_cluster]# cp -r 7001/ ./7003
[root@studyCentOS7 redis4.0.6_cluster]# cp -r 7001/ ./7004
[root@studyCentOS7 redis4.0.6_cluster]# cp -r 7001/ ./7005
[root@studyCentOS7 redis4.0.6_cluster]# cp -r 7001/ ./7006

// 修改端口号
[root@study 7002]# vi 7002/redis.conf
port 7002 
[root@study 7003]# vi 7003/redis.conf  
port 7003
[root@study 7004]# vi 7004/redis.conf  
port 7004
[root@study 7005]# vi 7005/redis.conf  
port 7005
[root@study 7006]# vi 7006/redis.conf
port 7006

// 查看端口号
[root@studyCentOS7 redis4.0.6_cluster]# find . -name '700*' | cat 700*/redis.conf | grep -a0b0 'port 700*'     
3661:port 7001
--
61424:port 7002
--
119187:port 7003
--
176950:port 7004
--
234713:port 7005
--
292476:port 7006

// 查看是否开启集群
[root@studyCentOS7 redis4.0.6_cluster]# find . -name '700*' | cat 700*/redis.conf | grep -a0b0 'cluster-enabled *'   
35756:cluster-enabled yes
--
93519:cluster-enabled yes
--
151282:cluster-enabled yes
--
209045:cluster-enabled yes
--
266808:cluster-enabled yes
--
324571:cluster-enabled yes
```
### 5.2.4 启动
创建集群前，需要启动六个实例。
```shell
[root@studyCentOS7 redis4.0.6_cluster]# vi startcluster.sh
# 启动7001
cd 7001
./redis-server redis.conf

# 启动7002
cd ../7002
./redis-server redis.conf

# 启动7003
cd ../7003
./redis-server redis.conf

# 启动7004
cd ../7004
./redis-server redis.conf

# 启动7005
cd ../7005
./redis-server redis.conf

# 启动7006
cd ../7006
./redis-server redis.conf

// 添加可执行属性
[root@studyCentOS7 redis4.0.6_cluster]# chmod +x startcluster.sh 

// 运行启动
[root@studyCentOS7 redis4.0.6_cluster]# ./startcluster.sh 
24384:C 06 Dec 17:47:25.733 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
24384:C 06 Dec 17:47:25.733 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=24384, just started
24384:C 06 Dec 17:47:25.733 # Configuration loaded
24386:C 06 Dec 17:47:25.759 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
24386:C 06 Dec 17:47:25.759 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=24386, just started
24386:C 06 Dec 17:47:25.759 # Configuration loaded
24388:C 06 Dec 17:47:25.788 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
24388:C 06 Dec 17:47:25.789 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=24388, just started
24388:C 06 Dec 17:47:25.789 # Configuration loaded
24396:C 06 Dec 17:47:25.818 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
24396:C 06 Dec 17:47:25.819 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=24396, just started
24396:C 06 Dec 17:47:25.819 # Configuration loaded
24401:C 06 Dec 17:47:25.844 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
24401:C 06 Dec 17:47:25.844 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=24401, just started
24401:C 06 Dec 17:47:25.844 # Configuration loaded
24406:C 06 Dec 17:47:25.868 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
24406:C 06 Dec 17:47:25.868 # Redis version=4.0.6, bits=64, commit=00000000, modified=0, pid=24406, just started
24406:C 06 Dec 17:47:25.868 # Configuration loaded

// 查看是否全部启动
[root@studyCentOS7 redis4.0.6_cluster]# ps aux |grep redis
root      24385  0.1  0.9 147312  9604 ?        Ssl  17:47   0:00 ./redis-server 127.0.0.1:7001 [cluster]
root      24387  0.0  0.9 147312  9604 ?        Ssl  17:47   0:00 ./redis-server 127.0.0.1:7002 [cluster]
root      24392  0.0  0.9 147312  9608 ?        Ssl  17:47   0:00 ./redis-server 127.0.0.1:7003 [cluster]
root      24397  0.0  0.7 147312  7568 ?        Ssl  17:47   0:00 ./redis-server 127.0.0.1:7004 [cluster]
root      24402  0.0  0.6 147312  6048 ?        Ssl  17:47   0:00 ./redis-server 127.0.0.1:7005 [cluster]
root      24407  0.0  0.6 147312  6048 ?        Ssl  17:47   0:00 ./redis-server 127.0.0.1:7006 [cluster]
```

### 5.2.5 创建集群
```shell
// 将源码包中的ruby脚本拷贝到集群安装目录下，便于使用
[root@studyCentOS7 redis4.0.6_cluster]# cp /software_bak/redis-4.0.6/src/*.rb ./

// 执行脚本，创建集群
[root@studyCentOS7 redis4.0.6_cluster]# ./redis-trib.rb create --replicas 1 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005  127.0.0.1:7006
>>> Creating cluster
>>> Performing hash slots allocation on 6 nodes...
Using 3 masters:
127.0.0.1:7001
127.0.0.1:7002
127.0.0.1:7003
Adding replica 127.0.0.1:7004 to 127.0.0.1:7001
Adding replica 127.0.0.1:7005 to 127.0.0.1:7002
Adding replica 127.0.0.1:7006 to 127.0.0.1:7003
M: 0bc83a5cf4168d450058d8166f9f1348d561892d 127.0.0.1:7001
   slots:0-5460 (5461 slots) master
M: 501b4931270fe9a17244d731eeaaec80d54c9e5e 127.0.0.1:7002
   slots:5461-10922 (5462 slots) master
M: b2dc6908386de2c3f86a3dd40c71dc5f8609d935 127.0.0.1:7003
   slots:10923-16383 (5461 slots) master
S: f6474eaca28888d8b1d53fe299cb15ae5b93220d 127.0.0.1:7004
   replicates 0bc83a5cf4168d450058d8166f9f1348d561892d
S: 9755cac30b2365b7e74a4423e274cd807ea165ef 127.0.0.1:7005
   replicates 501b4931270fe9a17244d731eeaaec80d54c9e5e
S: 25e73bbabe8550bd5fc0853eda1c00addf835f05 127.0.0.1:7006
   replicates b2dc6908386de2c3f86a3dd40c71dc5f8609d935
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join...
>>> Performing Cluster Check (using node 127.0.0.1:7001)
M: 0bc83a5cf4168d450058d8166f9f1348d561892d 127.0.0.1:7001
   slots:0-5460 (5461 slots) master
   1 additional replica(s)
S: 25e73bbabe8550bd5fc0853eda1c00addf835f05 127.0.0.1:7006
   slots: (0 slots) slave
   replicates b2dc6908386de2c3f86a3dd40c71dc5f8609d935
M: b2dc6908386de2c3f86a3dd40c71dc5f8609d935 127.0.0.1:7003
   slots:10923-16383 (5461 slots) master
   1 additional replica(s)
S: f6474eaca28888d8b1d53fe299cb15ae5b93220d 127.0.0.1:7004
   slots: (0 slots) slave
   replicates 0bc83a5cf4168d450058d8166f9f1348d561892d
S: 9755cac30b2365b7e74a4423e274cd807ea165ef 127.0.0.1:7005
   slots: (0 slots) slave
   replicates 501b4931270fe9a17244d731eeaaec80d54c9e5e
M: 501b4931270fe9a17244d731eeaaec80d54c9e5e 127.0.0.1:7002
   slots:5461-10922 (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
[root@studyCentOS7 redis4.0.6_cluster]# 
```
执行创建集群的脚本中：--replicas 1代表的是单个备份。
到此为止，整个伪集群环境已经搭建完毕。

### 5.2.6 测试
测试时，连接哪一个节点（不分主备）都可以。所以使用下面命令进行连接。
```shell
[root@studyCentOS7 redis4.0.6_cluster]# cd 7001
[root@studyCentOS7 7001]# ./redis-cli -h 127.0.0.1 -p 7001 -c
127.0.0.1:7001> set a 10
-> Redirected to slot [15495] located at 127.0.0.1:7003
OK
127.0.0.1:7003> set hello nihao
-> Redirected to slot [866] located at 127.0.0.1:7001
OK
127.0.0.1:7001> set wxy gyl
-> Redirected to slot [13270] located at 127.0.0.1:7003
OK
127.0.0.1:7003> set beidian posun
-> Redirected to slot [7658] located at 127.0.0.1:7002
OK
127.0.0.1:7002> 
```
注意在连接命令中，一定要加上-c参数。不加-c参数也可以，但是运行的时候就不是集群了，而是单个实例。
### 5.2.7 关闭
```shell
[root@studyCentOS7 redis4.0.6_cluster]#  vi shutdown.sh
7001/redis-cli -p 7001 shutdown
7001/redis-cli -p 7002 shutdown
7001/redis-cli -p 7003 shutdown
7001/redis-cli -p 7004 shutdown
7001/redis-cli -p 7005 shutdown
7001/redis-cli -p 7006 shutdown

// 修改脚本文件的权限
[root@studyCentOS7 redis4.0.6_cluster]# chmod +x shutdown.sh 

// 测试关闭
[root@studyCentOS7 redis4.0.6_cluster]# ./shutdown.sh 
[root@studyCentOS7 redis4.0.6_cluster]#  ps aux | grep redis
root      46236  0.0  0.0 112680   976 pts/0    S+   20:00   0:00 grep --color=auto redis
[root@studyCentOS7 redis4.0.6_cluster]# 
```
## 5.3 真集群搭建
真集群环境的搭建过程跟伪集群的搭建过程一样，只不过是在使用ruby脚本创建集群时，需要指定的是主机地址和端口。只需要修改命令还要注意相关文件的配置即可。
```shell
./redis-trib.rb create --replicas 1 192.168.100.X1:7001 192.168.100. X2:7002 192.168.100. X3:7003 192.168.100. X4:7004 192.168.100. X5:7005  192.168.100. X6:7006
```

# 6. 客户端连接redis
## 6.1 redis-cli
命令行式客户端，最常用的客户端。可以使用不同主机上面的客户端连接不同的Redis实例。可以类比于Mysql客户端。
## 6.2 redis Manager desktop
图形化客户端，只能连接单机版的Redis，不能连接集群。如果连接不上可以检查一下原因：
1.	是否开启Redis服务器
	.	是否开启防火墙中的6379端口
	.	修改配置文件中的bind参数，不要绑定IP登录
可类比MySQL相关操作。

## 6.3 Jedis
1.	加入Jedis的依赖包
	.	使用Jedis连接测试，或者使用JedisPool连接测试。
此处不再赘述。
**注意：在测试集群环境时，需要修改防火墙配置文件，开启7001~7006端口。**







