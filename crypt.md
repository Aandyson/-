# crypt #

crypt 模块(只用于 Unix/Linux,windows平台上没有此模块)实现了单向的 DES 加密, Unix/Linx系统使用这个加密算法来储存密码，这个模块真正也就只在检查这样的密码时有用。


	>>> import crypt
	>>> import random
	>>> import string
	>>> chars = string.digits + string.letters
	>>> chars
	'0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
	>>> def getsalt(chars):
	...     return random.choice(chars) + random.choice(chars)
	... 
	>>> salt = getsalt(chars)
	>>> salt
	'sb'
	>>> msg = crypt.crypt('hello,world!',salt)
	>>> msg
	'sb0xvR6UbZsqw'

