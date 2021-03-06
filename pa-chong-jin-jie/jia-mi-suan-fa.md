# 计算机存储单位

#### 位 比特 字节
存储信息的最小单位：位是计算机存储信息的最小单位，称之为位（bit），音译比特，二进制的一个“0”或一个“1”叫一位。
最小的存储单位：字节是计算机存储容量基本单位是字节（Byte），音译为拜特，8个二进制位组成1个字节，一个标准英文字母占一个字节位置，一个标准汉字占二个字节位置。
也就是我们所说的1字节等于8位（比特）

# 常见加密算法

常见的加密算法可以分成三类，对称加密算法，非对称加密算法和Hash算法。
## 对称加密

对称加密算法用来对敏感数据等信息进行加密，常用的算法包括：
DES（Data Encryption Standard）：数据加密标准，速度较快，适用于加密大量数据的场合。
3DES（Triple DES）：是基于DES，对一块数据用三个不同的密钥进行三次加密，强度更高。
AES（Advanced Encryption Standard）：高级加密标准，是下一代的加密算法标准，速度快，安全级别高；

DES:
全称为Data Encryption Standard，即数据加密标准，是一种使用密钥加密的块算法
入口参数有三个：Key、Data、Mode
Key为7个字节共56位，是DES算法的工作密钥；
Data为8个字节64位，是要被加密或被解密的数据；
Mode为DES的工作方式,有两种:加密或解密

3DES（即Triple DES）是DES向AES过渡的加密算法，
使用两个密钥，执行三次DES算法，
加密的过程是加密-解密-加密
解密的过程是解密-加密-解密

AES：
高级加密标准（英语：Advanced Encryption Standard，缩写：AES），这个标准用来替代原先的DES
AES的区块长度固定为128 比特，密钥长度则可以是128，192或256比特 （16、24和32字节）
大致步骤如下：
1、密钥扩展（KeyExpansion），
2、初始轮（Initial Round），
3、重复轮（Rounds），每一轮又包括：SubBytes、ShiftRows、MixColumns、AddRoundKey，
4、最终轮（Final Round），最终轮没有MixColumns。

## 非对称加密
常见的非对称加密算法如下：
RSA：由 RSA 公司发明，是一个支持变长密钥的公共密钥算法，需要加密的文件块的长度也是可变的；
DSA（Digital Signature Algorithm）：数字签名算法，是一种标准的 DSS（数字签名标准）；
ECC（Elliptic Curves Cryptography）：椭圆曲线密码编码学。
RSA:
 公钥加密算法，一种非对称密码算法
 公钥加密，私钥解密

 3个参数：
 rsa_n， rsa_e，message
 rsa_n, rsa_e  用于生成公钥
 message： 需要加密的消息
 
 
 ## base64
 
'''
    base64：
    是网络上最常见的用于传输8Bit字节码的编码方式之一，
    Base64就是一种基于64个可打印字符来表示二进制数据的方法，用于在HTTP环境下传递较长的标识信息
    后一两位可能有“=”
    防君子不防小人，很容易解密
    输出为 A-Z、a-z、0-9和"+"、"/" 字符组成的字符串
    很多时候字符串尾部为 1个或2个  "="
    把3个字节的二进制拼接， 24位， 按6位分割，变成4个字节，每个字节小于64
    最后留下1个字节的时候，会在尾部添加 2个 '='
    最后留下2个字节的时候，会在尾部添加 1个 '='
'''
 
 ## binascii 模块
 binascii 模块包含许多在二进制和各种ASCII编码二进制表示之间转换的方法。通常，您不会直接使用这些函数，而是使用包装模块，如 uu，base64 或 binhex。 binascii 模块包含以C编写的低级函数，用于更高的速度，由更高级别的模块使用。
 
binascii 模块定义以下功能：
binascii.b2a_hex(data)
返回二进制 data 的十六进制表示。 data 的每个字节都转换为相应的2位十六进制表示。因此返回的字节对象的长度是 data 的长度的两倍。
binascii.a2b_hex(hexstr)
返回由十六进制字符串 hexstr 表示的二进制数据。这个函数是 b2a_hex() 的逆。 hexstr 必须包含偶数个十六进制数字（可以是大写或小写），否则会引发 Error 异常。
binascii.b2a_base64(data, *, newline=True)
在base64编码中将二进制数据转换为一行ASCII字符。返回值是转换后的行，如果 newline 为true，则包括换行符。
binascii.a2b_base64(string)
将一个base64数据块转换回二进制并返回二进制数据。一次可以传递多于一行。



 
 
 
