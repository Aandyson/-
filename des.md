# des #

pyDes.des(key,[mode],[IV],[pad],[padmode])
参数的意思分别如下:

> key - 加密密钥.长度为8位,必选
> 
> mode -  加密方式.ECB(默认),CBC(安全性好于前者)

> IV - 初始字节数(长度为8位),如果你选择的加密方式为CBC就必须有这个参数,否则可以没有

> pad - 加密时,将该字符添加到数据块的结尾;解密时,将删除从最后一个的往前8位

> padmode - PAD_NORMAL\PAD_PKCS5,当选择前者时必须设置pad



	def des_encode(data):
	    # 设置加密的规范
	    k = des("xqtest66", padmode=PAD_PKCS5)
	    # k = des("xqtest66",CBC,"goodluck",pad = "hahahehe",padmode = PAD_NORMAL)
	
	    # encrypt来加密我的数据,然后进行base64编码
	    encodeStr = base64.b64encode(k.encrypt(data))
	    return encodeStr
	
	data = "i love pythoon"	

	def des_decode(data):
	    k = des("xqtest66", padmode=PAD_PKCS5)
	    decodestr = k.decrypt(base64.b64decode(data))
	    return decodestr

	result3 = des_encode(data)
	jiemi3 = des_decode(result3)
	print("des加密:", result3)
	print("des解密", jiemi3)