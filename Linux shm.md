
## 接口描述

### 1. shmget()
创建/获取共享内存
```C
#include <sys/ipc.h>
#include <sys/shm.h>

int shmget(key_t key, size_t size, int shmflg);
```
#### 参数：
* key_t key：整型，多进程访问共享内存的钥匙，需要避免冲突
	通常用ftok函数生成key，减少冲突可能，例
	```C
	#define PATH "/userdata/app"
	#define ID 0x11
	
	key_t key = ftok(PATH, ID);
	```
	IPC_PRIVATE，特殊的key类型，会无视shmflg中除了权限mode外的位，创建个新的内存块（通常用于亲缘进程间通信，因为无法通过key联系到shm）
* size_t size：申请共享内存块大小（B）
	实际操作系统申请共享内存，会以内存也为单位（PIGE_SIZE，通常为4096B）
* int shmflg：权限标志
	通常由 前置位mode + 9bits权限mode组成
	常用mode：
	* 无参数，会默认去找与key匹配的内存块，并判断是否符合权限
	* IPC_CREAT：创建新内存块
	* IPC_EXCL：和IPC_CREAT一起使用（IPC_CREAT | IPC_EXCL），判断内存块如果已存在，返回失败
	权限描述与`open()`中的mode相同，分别描述owner、group、others的读、写、执行权限。（目前shm不会使用到执行权限）
#### 返回值：
内存标识符，调用失败时，返回-1；
可以使用`errno`函数，获取具体错误原因。

### 2. shmat 
关联共享内存
```C
#include <sys/types.h>
#include <sys/shm.h>

void *shmat(int shmid, const void *shmaddr, int shmflg);
```
#### 参数：
* int shmid：目标共享内存ID（shmget返回值）
* const void \*shmaddr：指定共享内存起始地址
	* 通常为NULL，由操作系统自行分配
	* 不为NULL，shmflg配置SHM_RND，shmaddr会从指定值向下取整保证页对齐
	* 不为NUL，且shmflg未设置，shmaddr必须保证页对齐
* int shmflg：描述操作权限
	* SHM_RDONLY：只读权限，需要保证用户对块具有读权限
	* 0（不指定）：默认为读写权限

#### 返回值：
关联到共享内存块的地址，调用失败时，返回(void \*)-1；
可以使用`errno`函数，获取具体错误原因。
### 3. shmdt
取消关联共享内存
```C
#include <sys/types.h>
#include <sys/shm.h>

int shmdt(const void *shmaddr);
```