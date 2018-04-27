# md5 #

Python2.x中有md5模块，此模块调用了hashlib模块，python3.x已中将md5取掉，直接通过调用hashlib模块来进行md5。Python2.x可以直接使用unicode字符，但3.x中必须使用二进制字节串。

	>>> import hashlib
	>>> m = hashlib.md5()
	>>> m.update(b'hello,word!')
	>>> m.hexdigest()
	
	'9702d6722a0901398efd4ecb3a20423f'

## md5加密不可逆转 ##

> 注意：每调用一次update(s)，相当于给md5对象m增加了s。对一个新的需md5加密的内容，需要新建一个md5对象。 
Hashlib模块还可以进行sha1、sha224、sha256、sha384、sha512等hash算法。Sha384、sha512在32位的平台上处理较慢。

## 个人例子 ##

	def md5_encode(data):
	    # 调用md5算法,用一个变量接收
	    m = hashlib.md5()
	    # 调用update对传来的data进行数据加密,encode utf-8的编码后才能用update
	    m.update(data.encode('utf-8'))
	    # hexdigest()转换为二进制数的十六进制表
	    return m.hexdigest()  # 经过特殊处理之后以字符串形式返回
	
	data = "i love pythoon"
	
	result1 = md5_encode(data)
	
	print("md5加密:", result1)
	
	md5加密: a6f3ba84c70f394fc4e7d87bc467cd4e
