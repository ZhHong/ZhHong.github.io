### node express http & https
* *express:^4.14.0 node v7.6.0*
* openssl生成证书
```shell
>>openssl genrsa 1024 > ./private.pem               #生成私钥
>>openssl req -new -key ./private.pem -out csr.pem  #生成签名文件
>>openssl x509 -req -days 365 -in csr.pem -signkey ./priva
te.pem -out ./file.crt                              #通过私钥文件和CSR证书签名生成证书文件
```
* app.js
```javascript
var https_service = require('./https_service)
https_service.start()
```
* https_service.js
```javascript
var app = require('express')();
var fs = require('fs');
var http = require('http');
var https = require('https');
var privateKey  = fs.readFileSync('./certificate/private.pem', 'utf8');
var certificate = fs.readFileSync('./certificate/file.crt', 'utf8');
var credentials = {key: privateKey, cert: certificate};

var httpServer = http.createServer(app);
var httpsServer = https.createServer(credentials, app);
var PORT = 18080;
var SSLPORT = 18081;

exports.start = function(){
    httpServer.listen(PORT, function() {
        console.log('HTTP Server is running on: http://localhost:%s', PORT);
    });
    httpsServer.listen(SSLPORT, function() {
        console.log('HTTPS Server is running on: https://localhost:%s', SSLPORT);
    });
}

// Welcome
app.get('/', function(req, res) {
    if(req.protocol === 'https') {
        res.status(200).send('Welcome to Safety!');
    }
    else {
        res.status(200).send('Welcome!');
    }
});
// test hello
app.get('/hello',function(req,res){
    var args = req.query;
    if(req.protocol === 'https'){
        res.status(200).send({hello:'Hello',status:'0',msg:'Success',type:'https',query:args})
    }else{
        res.status(200).send({hello:'Hello',status:'0',msg:'Success',type:'http',query:args})        
    }
});
// test good
app.get('/good',function(req,res){
    var args = req.query
    if(req.protocol === 'https'){
        res.status(200).send({good:'good',status:'0',msg:'Success',type:'https',query:args})
    }else{
        res.status(200).send({good:'good',status:'0',msg:'Success',type:'http',query:args})        
    }
})
```

* *仅仅测试，如果想要证书被认证，请自行前往相关网站*

### node DiffieHellman 密钥交换 HMAC AES

* *node v7.6.0*
* DiffieHellman 密钥交换
```javascript
//生成公钥和私钥
exports.dh64_gen_key= function(){
    var blob =crypto.getDiffieHellman('modp5');
    blob.generateKeys();
    return blob;
}

//生成密钥
exports.dh64_secret= function(blob,public_key){
    try{
        var secret = blob.computeSecret(public_key,'base64','base64');
        return secret;
    }catch(err){
        return null;
    }
}
```
* hmac
```javascript
exports.hmac64 = function(content,secret){
    var hmac =crypto.createHmac('sha1',secret)
    var sign =hmac.update(content,'utf8').digest().toString('base64');
    return sign;
}
```
* aes
```javascript
exports.aes_encrypt = function(plantext,key){
    var algorithm ={aes256:'aes-256-cbc'};
    var en_key = key.MD5(32);
    var iv = key.MD5();
    var cipher = crypto.createCipheriv(algorithm.aes256,en_key,iv);
    cipher.setAutoPadding(true);
    var ciph = cipher.update(plantext, 'utf8', 'base64');
    ciph += cipher.final('base64');
    return ciph;
}

exports.aes_decrypt = function(encrypt_text,key){
    try{
        var algorithm ={aes256:'aes-256-cbc'};
        var en_key = key.MD5(32);
        var iv = key.MD5();
        var decipher = crypto.createDecipheriv(algorithm.aes256,en_key,iv);
        decipher.setAutoPadding(true);
        var txt = decipher.update(encrypt_text, 'base64', 'utf8');
        txt += decipher.final('utf8');
        return txt;
    }catch(err){
        console.log("AES DECRYPTO ERROR:",err.stack);
        return null;
    }
}
```
* *[工具方法&测试](https://github.com/ZhHong/zh_example/tree/master/node)*