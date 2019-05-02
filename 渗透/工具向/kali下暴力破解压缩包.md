# 使用fcrackzip破解zip保护密码
```
root in ~ λ fcrackzip --help

fcrackzip version 1.0, a fast/free zip password cracker
written by Marc Lehmann <pcg@goof.com> You can find more info on
http://www.goof.com/pcg/marc/

USAGE: fcrackzip
          [-b|--brute-force]            use brute force algorithm 暴力破解
          [-D|--dictionary]             use a dictionary 使用字典
          [-B|--benchmark]              execute a small benchmark
          [-c|--charset characterset]   use characters from charset
          [-h|--help]                   show this message
          [--version]                   show the version of this program
          [-V|--validate]               sanity-check the algortihm 理智检查 应该是和暴力破解相反的吧
          [-v|--verbose]                be more verbose
          [-p|--init-password string]   use string as initial password/file 指定字典字符串，或文件
          [-l|--length min-max]         check password with length min to max 
          [-u|--use-unzip]              use unzip to weed out wrong passwords
          [-m|--method num]             use method number "num" (see below)
          [-2|--modulo r/m]             only calculcate 1/m of the password
          file...                    the zipfiles to crack  破解的文件

methods compiled in (* = default):

 0: cpmask
 1: zip1
*2: zip2, USE_MULT_TAB

```
# 常见的命令
```
# 使用字典爆破
fcrackzip -D -p /usr/share/wordlists/rockyou.txt -u crack_this.zip
# 爆破数字
fcrackzip -b -c '1' -l 1-10 -u crack_this.zip 
# 
```