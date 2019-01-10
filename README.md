
var http = require('http')
var path = require('path')
var fs = require('fs')
var url = require('url')


function sampleRoot(samplePath, req, res){
    //此函数实现服务器的功能
	console.log(samplePath)
	console.log(req.url)
	var pathObj = url.parse(req.url, true)
    //将URL转换为字符串
	console.log(pathObj)

	if(pathObj.pathname === '/'){
	pathObj.pathname += 'test.html'
	}
    //如果URL后面没有输入内容，那么拼接上完整的URL
	var filePath = path.join(samplePath, pathObj.pathname)
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
	sampleRoot(path.join(__dirname, 'sample'), req, res)
})

server.listen(8090)
//运行一个接口为8090的服务器
console.log('visit http://localhost:8090')
