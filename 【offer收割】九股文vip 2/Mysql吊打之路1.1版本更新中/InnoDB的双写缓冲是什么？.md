# 👌InnoDB 的双写缓冲是什么？

# <font style="color:rgb(44, 44, 44);">题目详细答案</font>
<font style="color:rgb(44, 44, 44);">双写缓冲区（doublewrite buffer）是磁盘上的一块存储区域，双写缓冲存储区位于双写文件（doublewrite files）中，InnoDB 将页面冲刷（flush）到磁盘上的数据文件之前，会先将其写入缓冲池中的双写缓冲区。是一种用于数据完整性和恢复的关键机制。它是为了在数据库发生崩溃时保护数据不受损坏设计的。如果在页面写入过程中，出现操作系统、存储子系统或 mysqld 进程意外退出，InnoDB 可以在崩溃恢复期间从双写缓冲区中找到这些页面的无损副本。</font>

## 双写缓冲组成部分
**双写缓冲区（Doublewrite Buffer Area）**：这是内存中的一个缓冲区，通常位于 InnoDB 缓冲池（Buffer Pool）中。默认情况下，这个缓冲区的大小是 2 MB，分为 128 个页，每个页的大小为 16 KB。这个缓冲区用于暂时存放即将写入磁盘的数据页。

**双写文件（Doublewrite File）**：这是磁盘上的一个区域，通常位于 InnoDB 系统表空间（ibdata 文件）中。双写文件的大小与内存中的双写缓冲区相同，也是 2 MB。这个文件用于存放从内存中的双写缓冲区写入的数据页，确保在写入数据文件之前，数据已经安全地写入磁盘。

## 双写缓冲的工作流程
1. **写入双写缓冲区**：当 InnoDB 需要将脏页（Dirty Page）从缓冲池写入磁盘时，它首先将这些数据页写入内存中的双写缓冲区Doublewrite Buffer。这个缓冲区位于共享表空间（通常是 ibdata 文件）中。
2. **写入双写文件**：接下来，InnoDB 将双写缓冲区中的数据页批量写入磁盘上的双写文件。这一步确保数据页已经安全地写入磁盘，即使在写入过程中发生崩溃，数据页也不会损坏。
3. **写入数据文件**：一旦数据页成功写入双写文件，InnoDB 会将这些数据页写入实际的数据文件（即表空间文件）。如果在这一步发生崩溃，InnoDB 可以从双写文件中恢复数据页，确保数据的一致性和完整性。

通过这种机制，InnoDB 能够防止部分写入导致的数据页损坏，提高数据的可靠性和系统的稳定性。双写缓冲的使用对用户是透明的，无需手动配置或干预。Doublewrite Buffer 机制大大降低了数据因崩溃而损坏的风险。在发生崩溃后，InnoDB 可以使用 Doublewrite Buffer 中的页来恢复那些在崩溃过程中可能已损坏的页。

## 缺点
虽然 Doublewrite Buffer 提高了数据的安全性，但它也带来了一些性能开销

写入延迟：因为每个数据页需要写入两次（一次到 Doublewrite Buffer，一次到最终位置），这增加了 I/O 操作的数量，从而可能影响数据库的写入性能。

空间使用：Doublewrite Buffer 占用了额外的磁盘空间，尽管这通常不是主要问题，但在空间非常有限的环境中可能需要考虑。

## 配置
在 MySQL 中，可以通过 innodb_doublewrite 参数启用或禁用 Doublewrite Buffer。默认情况下，这个选项是启用的，因为其对于保护数据完整性非常重要。只有在特定情况下，当写入性能比数据完整性更为重要时，才考虑禁用它。





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/yfqmmtyp2efch5sk>