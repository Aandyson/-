# base64 #

Python内置的base64模块可以实现base64、base32、base16、base85、urlsafe_base64的编码解码，python 3.x通常输入输出都是二进制形式，2.x可以是字符串形式。

base64模块的base64编码、解码调用了binascii模块，binascii模块中的b2a_base64()函数用于base64编码，binascii模块中的a2b_base64()函数用于base64解码

	>>>import base64
	>>> s = 'hello,word!'
	
	>>> base64.b64encode(bytes(s,'ascii'))    #base64编码，编码的字符串必须是二进制形式的
	b'aGVsbG8sd29yZCE='
	
	>>> base64.b64decode(b'aGVsbG8sd29yZCE=')    #base64解码
	b'hello,word!'