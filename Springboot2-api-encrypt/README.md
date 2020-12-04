## 接口请求前后端RSA+AES加密（含前端代码）

### 加密过程如下：
**假设现在客户端是A，服务端是B，现在A要去B请求接口**

一、准备工作：
+ 1、A和B都要产生一对用于加密的RSA公私钥（AB各自生成自己的公私钥）
+ 2、A的私钥保密，A的公钥告诉B；B的私钥保密，B的公钥告诉A。(AB互换公钥)

二、发信息
+ 3、A要给B发送信息时，随机生成AES密钥（每次发信息都生成一个新的）
+ 4、A用刚生成的AES密钥加密需要发送的信息
+ 5、A用B的公钥加密刚生成的AES密钥，放到header的key参数里，并将这个AES加密后的信息发给B
+ 6、B收到这个消息后，取出header里面key参数的值，然后使用自己的RSA私钥解密得到AES密钥
+ 7、B解开AES密钥之后，从request取出AES加密后的信息，然后使用AES解密得到A发过来的明文信息。

B收到这个消息后。其他人收到这个报文都无法解密，因为只有B才有B的RSA私钥。才能得到AES的密钥
每次发信息都重复上面的步骤即可，aes 每次都是变的。

### 为什么用aes加密呢？
因为aes 比rsa 加解密速度更快，但是安全比rsa低。

### 博客介绍
[接口请求前后端RSA+AES加密（含前端代码）](https://rstyro.github.io/blog/2020/10/22/Springboot2接口加解密全过程详解(含前端代码)/)