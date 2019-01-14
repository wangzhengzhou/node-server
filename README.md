
var http = require('http')//实现服务器的核心模块
var path = require('path')//实现路径相关的核心模块
var fs = require('fs')//实现读写功能的核心模块
var url = require('url')//实现url相关功能的核心模块


function sampleRoot(samplePath, req, res){
    //此函数实现服务器的功能在后面被调用
	console.log(samplePath)
	console.log(req.url)
	var pathObj = url.parse(req.url, true)
    //解析当前请求的URL得到其所有信息
	console.log(pathObj)

	if(pathObj.pathname/*我们所需要的URL地址*/ === '/'){
	pathObj.pathname += 'test.html'
	}
    //如果域名后面没有输入内容，那么拼接上完整的URL实现默认访问此页面
	var filePath/*读取文件的路径*/ = path.join(samplePath, pathObj.pathname)/*所有数据文件存储的文件夹拼接上请求获得的文件目录*/
	fs.readFile(filePath, 'binary'//将响应的内容转为二进制, function(err, fileContent){
		if(err){
			console.log('404')
			res.writeHead(404, 'not found')
			res.end('<h1>404 Not Found</h1>')
            //如果没找到文件，报错
		}else{
			console.log('ok')
			res.writeHead(200, 'OK')
			res.write(fileContent, 'binary')
			res.end()
            //找到了文件，返回200状态码及OK
		}
	})
}
console.log(path.join(__dirname, 'ststic'))
var server = http.createServer(function(req, res){
    //定义一个服务器
	sampleRoot(path.join(__dirname/*代表当前文件*/, 'sample'/*所有的数据库文件在此目录下*/)/*生成一个当前文件的绝对路径再拼接上数据库的路径*/, req, res)
})

server.listen(8090)
//运行一个接口为8090的服务器
console.log('visit http://localhost:8090')
