## Harmony code



OpenHarmony内核源代码目录结构

| 名称     | 描述                                                   |
| -------- | ------------------------------------------------------ |
| apps     | 用户态的init和shell应用程序。                          |
| arch     | 体系架构的目录，如arm等。                              |
| bsd      | freebsd相关的驱动和适配层模块代码引入，例如USB等。     |
| compat   | 内核posix接口的兼容。                                  |
| fs       | 文件系统模块，主要来源于NuttX开源项目。                |
| kernel   | 进程、内存、IPC等模块。                                |
| lib      | 内核的lib库。                                          |
| net      | 网络模块，主要来源于lwip开源项目。                     |
| platform | 支持不同的芯片平台代码，如Hi3516DV300等。              |
| security | 安全特性相关的代码，包括进程权限管理和虚拟id映射管理。 |
| syscall  | 系统调用。                                             |
| tools    | 构建工具及相关配置和代码。                             |