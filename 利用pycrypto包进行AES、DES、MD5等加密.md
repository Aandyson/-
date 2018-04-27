# 利用pycrypto包进行AES、DES、MD5等加密 #

第三方Crypto包提供了较全面的加密算法，包括Cipher、Hash、Protocol、PublicKey、Singature、Util几个子模块，其中Cipher模块中有常用的AES、DES加密算法，Hash模块中有MD5、MD4、SHA等算法。下面介绍AES及DES的加密解密算法，python版本为2.7.9。

### 5.1 AES加密解密 ###

# coding=utf-8

	from Crypto.Cipher import AES
	from Crypto import Random
	import binascii
	
	key = '1234567890!@#$%^'   #秘钥，必须是16、24或32字节长度
	iv = Random.new().read(16) #随机向量，必须是16字节长度
	
	cipher1 = AES.new(key,AES.MODE_CFB,iv)  #密文生成器,MODE_CFB为加密模式
	
	encrypt_msg =  iv + cipher1.encrypt('我是明文')  #附加上iv值是为了在解密时找到在加密时用到的随机iv
	print '加密后的值为：',binascii.b2a_hex(encrypt_msg)   #将二进制密文转换为16机制显示
	
	
	cipher2 = AES.new(key,AES.MODE_CFB,iv) #解密时必须重新创建新的密文生成器
	decrypt_msg = cipher2.decrypt(encrypt_msg[16:]) #后十六位是真正的密文
	print '解密后的值为：',decrypt_msg.decode('utf-8')
	
	
	运行后的结果为：
	>>> ================================ RESTART ================================
	>>> 
	加密后的值为： 502d279e1cba9ef6744ad4ce5a12dbf9389c99731bfab1349e35b528
	解密后的值为： 我是明文


### 5.2 DES3加密解密 ###


	# coding=utf-8
	
	from Crypto.Cipher import DES3
	from Crypto import Random
	import binascii
	
	key = '1234567890!@#$%^'
	iv = Random.new().read(8)  #iv值必须是8位
	cipher1 = DES3.new(key,DES3.MODE_OFB,iv)  #密文生成器，采用MODE_OFB加密模式
	encrypt_msg =  iv + cipher1.encrypt('我是明文必须是八')
	#附加上iv值是为了在解密时找到在加密时用到的随机iv,加密的密文必须是八字节的整数倍，最后部分
	#不足八字节的，需要补位
	print '加密后的值为：',binascii.b2a_hex(encrypt_msg)   #将二进制密文转换为16进制显示
	
	
	cipher2 = DES3.new(key,DES3.MODE_OFB,iv) #解密时必须重新创建新的密文生成器
	decrypt_msg = cipher2.decrypt(encrypt_msg[8:]) #后八位是真正的密文
	print '解密后的值为：',decrypt_msg
	
	
	运行后的结果为：
	>>> ================================ RESTART ================================
	>>> 
	加密后的值为： 8caf464c66ec652e5305d33ff4814a3a4f8423b404ae6a48f4a1c411ecddf932
	解密后的值为： 我是明文必须是八


### 这个例子没看懂 ###


	from Crypto.Cipher import AES
	from Crypto import Random
	import binascii
	# 主要引入一些必要的模块，Crypto.Cipher模块中的AES模块以及Crypto中的Random模块，binascii模块为查看二进制下的数据提供了各种方法。
	
	
	# 实现调用AES加密API的函数
	def AES_File(fs):
	    # 用于指定AES加密的初始密钥，根据AES规范，可以是16字节、24字节和32字节长，这里我们使用了一个16字节的初始密钥，其实完全可以由用户输入的口令+salt获得；
	    key = b'1234567890!@#$%^'  # 16-bytes password
	    # 用于生成iv，这里使用了Crypto模块中的Random模块，读取其16字节的数据作为iv的值，AES的分块大小固定为16字节；
	    iv = Random.new().read(AES.block_size)
	    print("IV:", iv)
	    # 用于生成了加密时需要的实际密码，主要使用了AES.new(key, AES.MODE_CBC,iv)
	    # 函数，key和iv由前两步生成，这步可以指定加密模式，这里选择的是CBC模式；
	    cipher = AES.new(key, AES.MODE_CBC, iv)
	    # 主要用于判断数据长度是否为16字节块的整数倍，
	    # 从而进行适当的Padding，这里的关键是利用'%'运算判断是否是16字节的整数倍，然后在尾部追加(16-x)个填充字符；
	    print('if fs is a multiple of 16...')
	    # if fs is a multiple of 16
	    x = len(fs) % 16
	    print('fs的长度是： ', len(fs))
	    print('The num to padded is : ', x)
	    if x != 0:
	        fs_pad = fs + '0' * (16 - x)  # It shoud be 16-x not
	        print('fs_pad is : ', fs_pad)
	        print(len(fs_pad))
	        print(len(fs_pad) % 16)
	    # 结束
	    # 使用生成的cipher对象的encrypt方法加密文件流，得到密文文件流msg，注意这里与iv进行了一次异或，另一个需要注意的是encrypt方法的输入和输出都是一个字符串；
	    msg = iv + cipher.encrypt(fs_pad)
	    print("32行：", msg)
	    # 主要是为了调试的需要，导入binascii模块查看密文文件流的前10个字节，每个二进制字节以十六进制的形式表示；
	    print('File after AES is like...', binascii.b2a_hex(msg[:10]))
	    return msg
	
	
	# Create a Test Src File and Get FileSteam
	# 生成一个明文文件，获取其明文数据流，这里可以直接从打开的文件中读取，不过为了测试的需要，这里临时写入一个文件，因此复制数据流的时候记得重置指针到文件开头，即fs.seek(0, 0)；
	fs = open('test', 'w+')
	fs.write('啊，我爱你，我的祖国！')
	fs.write('凌晨三时开始进攻！')
	fs.seek(0, 0)
	fs_msg = fs.read()
	print("44行：", fs_msg)
	fs.close()
	
	
	# Crypt Src FileStream
	# 主要是将获取的明文数据流调用AES加密得到密文数据流，然后关闭文件；
	fc = open('fc', 'wb')
	fc_msg = AES_File(fs_msg)
	fc.writelines(fc_msg)
	fc.close()
	
	raw_input('Enter for Exit...')
