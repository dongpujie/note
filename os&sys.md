## os



os.path



| 参数                                   | 功能                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| **os.path.abspath(path)**              | 返回路径 *path* 的绝对路径                                   |
| **os.path.basename(path)**             | 返回路径 *path* 的基本名称。即文件名                         |
| **os.path.dirname(path)**              | 返回路径 *path* 的目录名称。即 pwd                           |
| **os.path.join(path, *paths)**         | 合理地拼接一个或多个路径部分。                               |
| **os.path.split(path)**                | 将路径 *path* 拆分为一对，即 `(head, tail)`，其中，*tail* 是路径的最后一部分，而 *head* 里是除最后部分外的所有内容。 |
| **os.path.exists(path)**               | 如果路径 path 存在，返回 True；如果路径 path 不存在，返回 False。 |
| os.path.splitdrive(path)               | 将路径 *path* 拆分为一对，即 `(drive, tail)`，其中 *drive* 是挂载点或空字符串。 |
| os.path.commonpath(paths)              | 返回序列 *paths* 中每个路径名称的最长共同子路径。 如果 *paths* 同时包含绝对和相对路径名称或者如果 *paths* 为空则会引发 ValueError。此方法将返回一个有效路径。 |
| os.path.commonprefix(list)             | 接受包含多个路径的 *列表*，返回所有路径的最长公共前缀（逐字符比较）。如果 *列表* 为空，则返回空字符串 (`''`)。此函数是逐字符比较，因此可能返回无效路径。 |
| os.path.lexists(path)                  | 如果路径 path 存在，返回 True；如果路径 path 不存在，返回 False。 |
| os.path.expanduser(path)               |                                                              |
| os.path.expandvars(path)               |                                                              |
| os.path.getatime(path)                 | 返回 *path* 的最后访问时间。                                 |
| os.path.getmtime(path)                 | 返回 *path* 的最后修改时间。                                 |
| os.path.getctime(path)                 | 返回 *path* 在系统中的 ctime                                 |
| os.path.getsize(path)                  | 返回 *path* 的大小，以字节为单位。                           |
| os.path.isabs(path)                    | 如果 *path* 是一个绝对路径，则返回 `True`。                  |
| os.path.isfile(path)                   | 如果 *path* 是现有的常规文件，则返回 `True`。                |
| os.path.isdir(path)                    | 如果 *path* 是现有的目录，则返回 `True`。                    |
| os.path.islink(path)                   | 如果 *path* 指向的现有的目录条目是一个符号链接，则返回 `True`。 |
| os.path.ismount(path)                  | 如果路径 path 是 挂载点（文件系统中挂载其他文件系统的点），则返回 True。 |
| os.path.normcase(path)                 | 规范路径名称的大小写。                                       |
| os.path.normpath(path)                 | 通过折叠多余的分隔符和对上级目录的引用来标准化路径名         |
| os.path.realpath(path)                 | 返回指定文件的规范路径，消除路径中存在的任何符号链接（如果操作系统支持）。 |
| os.path.relpath(path, start=os.curdir) | 返回从当前目录或 *start* 目录（可选）到达 *path* 之间要经过的相对路径。 |
| os.path.samefile(path1, path2)         | 如果两个路径都指向相同的文件或目录，则返回 `True`。          |
| os.path.sameopenfile(fp1, fp2)         | 如果文件描述符 *fp1* 和 *fp2* 指向相同文件，则返回 `True`。  |
| os.path.samestat(stat1, stat2)         | 如果 stat 元组 *stat1* 和 *stat2* 指向相同文件，则返回 `True`。 |
| os.path.splitext(path)                 | 将路径 *path* 拆分为一对，即 `(root, ext)`，使 `root + ext == path`，其中 *ext* 为空或以英文句点开头，且最多包含一个句点。 |
| os.path.supports_unicode_filenames     | 如果（在文件系统限制下）允许将任意 Unicode 字符串用作文件名，则为 `True`。 |
|                                        |                                                              |



