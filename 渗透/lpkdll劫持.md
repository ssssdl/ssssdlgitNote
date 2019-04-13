# dll劫持
原理：Windows执行exe的时候会调用lpk.dll这个库，默认调用exe所在目录下的lpk.dll然后就上传lpk.dll到含有会经常执行的exe所在的文件夹，然后当执行这个软件的时候就会运行上传的dll，，但是：