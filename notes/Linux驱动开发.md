# ARM Linux驱动开发

三大类驱动：

- **字符设备驱动**：最多，点灯、I2C、SPI、LCD、音频
- **块设备驱动**：存储器，EMMC、NAND、SD卡、U盘（以存储块为基础）
- **网络设备驱动**：网络

（一个设备可以属于多种设备驱动类型，USB WIFI：字符设备+网络设备驱动）



## 40 字符设备驱动开发

**字符设备**：按照字节流进行读写操作的设备，读写数据是分先后顺序的。

驱动加载成功在`/dev`目录下生成相应文件，**驱动**运行在**内核空间**，而**应用程序**运行在**用户空间**。

---->”**系统调用**“实现从用户空间陷入到内核空间，以实现对底层驱动的操作

**应用程序->C 库->内核->底层驱动**



每一个系统调用在驱动中都有与之对应的一个驱动函数，内核文件`include/linux/fs.h`中Linux内核驱动操作函数集合：结构体`file_operations`，字符设备驱动开发主要就是实现该结构体中的函数。

```c
struct file_operations{
	struct module *owner;/*拥有该结构体的模块的指针，一般设置为THIS_MODULE*/
    loff_t (*llseek) (struct file *, loff_t, int);/*修改文件当前的读写位置*/
    ssize_t (*read) (struct file *, char _user *, size_t, loff_t *);/*读取设备文件*/
    ssize_t (*write) (struct file *, const char _user *, size_t, loff_t *);/*向设备文件写入（发送）数据*/
    ...
    int (*open) (struct inode *, struct file *);/*打开设备文件*/
    int (*release) (struct inode *, struct file *);/*释放（关闭）设备文件，与应用程序的close函数对应*/
	...
}
```



### 40.2 字符设备开发步骤

Linux驱动开发重点是其**驱动框架**























