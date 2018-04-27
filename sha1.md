# sha1 #

	def sha1_encode(data):
	    # 调用sha1算法，用一个变量接受
	    sha1 = hashlib.sha1()
	    # 调用uodate对传来的data进行数据加密,encode utf-8的编码才能用uodate
	    sha1.update(data.encode('utf-8'))
	    # 经过特殊处理之后以字符串形式返回
	    return sha1.hexdigest()
	
	result2 = sha1_encode(data)
	print("sha1加密:", result2)


