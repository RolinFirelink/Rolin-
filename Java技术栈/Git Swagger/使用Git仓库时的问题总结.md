想要执行命令时出现提示

$ git remote

fatal: detected dubious ownership in repository at 'D:/Rolin的学习笔记'

To add an exception for this directory, call:

git config --global --add safe.directory 'D:/Rolin的学习笔记'

Set the environment variable GIT_TEST_DEBUG_UNSAFE_DIRECTORIES=true and run

again for more information.

简单来说就是提示我们这个仓库是不安全的仓库，解决方法： 参考这篇文章 https://blog.csdn.net/yxzone/article/details/125761932