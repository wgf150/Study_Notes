## 1. ptp4l.c

主函数 main
```C
int main(int argc, char *argv[])
{
...
	// 创建配置（hash table）
	cfg = config_create();
...
	// 获取参数
	while (EOF != (c = getopt_long(argc, argv, "AEP246HSLf:i:p:sl:mqvh",
				       opts, &index)))	
...
	// 从文件中获取配置（配置文件存在的情况）
	c = config_read(config, cfg)
...
	// 创建时钟
	clock = clock_create(type, cfg, req_phc)
...
	// 
	while (is_running()) {
		if (clock_poll(clock))
			break;
	}
...
}
```

clock_create()

` struct clock *clock_create(enum clock_type type, struct config *config, const char *phc_device) ` 

时钟类型包括以下：
```C
enum clock_type {
	CLOCK_TYPE_ORDINARY   = 0x8000,
	CLOCK_TYPE_BOUNDARY   = 0x4000,
	CLOCK_TYPE_P2P        = 0x2000,
	CLOCK_TYPE_E2E        = 0x1000,
	CLOCK_TYPE_MANAGEMENT = 0x0800,
};
```