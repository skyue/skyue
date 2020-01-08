+++
title = "windows 上修改 ipython notebook 的工作目录"
date = "2016-01-02T20:00:00+08:00"
slug = "2016010220"
+++

在 Windows 上，ipython notebook 默认的工作目录是 C 盘的用户目录，实际使用中，可能希望切换到其他目录，比如 D 盘。方法其实也非常简单，只需要在命令行下切换到工作目录，再执行`ipython notebook`启动 ipython notebook 即可。如下：

```
C:\Users\Administrator>d:

D:\>cd @me\coding

D:\@me\coding>ipython notebook
[I 20:05:48.002 NotebookApp] Serving notebooks from local directory: D:\@me\coding
[I 20:05:48.002 NotebookApp] 0 active kernels
[I 20:05:48.002 NotebookApp] The IPython Notebook is running at: http://localhost:8888/
[I 20:05:48.003 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to s
kip confirmation).
```

但每次这样也挺麻烦的，可以使用 Windows 的批处理文件进一步简化：只需要将下面的代码保存为`.bat`文件，并复制到你的工作目录，双击该文件就相当于在当前目录启动 ipython notebook 了。如果你有多个 ipython notebook 工作目录，只需要在每个目录下放一份该文件即可，非常方便。

```
rem -- start_ipython_notebook_here.bat ---
dir
ipython notebook 
pause
```

