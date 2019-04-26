---
利用tesseract识别验证码
​---

# 0x00 问题来源 
> 问题来源于于一道ctf，http://lab1.xseclab.com/vcode7_f7947d56f22133dbc85dda4f28530268/index.php​

# 0x01 基本原理
利用python将图片化为灰度图（只有黑白两色），根据颜色将图片化为01值，然后用  pytesseract 模块识别，这个模块Windows下需要安装tesseract支持。

---
# 0x02 环境搭建
我使用的是Windows,

安装需要的库
pip install PIL
pip install pillow
pip install pytesseract
安装完导入一下查看是否成功

安装tesseract
下载：https://sourceforge.net/projects/tesseract-ocr-alt/files/​

下载那个 tesseract-ocr-setup-3.02.02.exe ，操作简单一些，直接选好路径安装据可以了

配置pytesseract
这个好像python2可以不配置，但是python3不配置的话好像调用不了image_to_string函数，具体配置过程如下：

找到如下文件：

`c:/Users/xxxx/AppData/Local/Programs/Python/Python37/Lib/site-packages/pytesseract/pytesseract.py`

大概在第25行左右：

`tesseract_cmd = 'tesseract'`

改为tesseract.exe的路径

`tesseract_cmd = 'D:/tesseract/Tesseract-OCR/tesseract.exe'`

# 0x03 使用
```
from PIL import Image
from pytesseract import image_to_string

#颜色大于140的置1 小于的置0
def initTable(threshold = 140):
    table = []
    for i in range(256):
        if i< threshold:
            table.append(0)
        else:
            table.append(1)
    return table

#打开图片
im = Image.open('vcode.png')
#减少图片的色彩，化为灰度图
im = im.convert('L')
#将灰度图二值化
binaryImage = im.point(initTable(),'1')
#显示处理过后的图片binaryImage.show()
#识别文本  这个config可以根据自己的需要设置 如下
"""
-psm N
    Set Tesseract to only run a subset of layout analysis and assume a certain form of image. The options for N are:

    0 = Orientation and script detection (OSD) only.
    1 = Automatic page segmentation with OSD.
    2 = Automatic page segmentation, but no OSD, or OCR.
    3 = Fully automatic page segmentation, but no OSD. (Default)
    4 = Assume a single column of text of variable sizes.
    5 = Assume a single uniform block of vertically aligned text.
    6 = Assume a single uniform block of text.
    7 = Treat the image as a single text line.
    8 = Treat the image as a single word.
    9 = Treat the image as a single word in a circle.
    10 = Treat the image as a single character.
"""
print(image_to_string(binaryImage,config='-psm 7'))
```

