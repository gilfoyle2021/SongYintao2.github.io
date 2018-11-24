
#### Nile Coding stage

##### nile执行命令:
 
 ```
 #cmd: app的启动命令，如 java -jar xxxx ；
 #param：启动的相关参数，如jvm参数。。  
 #构建image时注入
 /path/to/nile -cmd xxxx -param xxx
 ```
 
#####  Nile伪代码
 

 ```
 func main(){
 	
 	# 1.解析命令cmd
 	parseCmd()
 	
 	#2.解析用户自定义参数params
 	parseParams()
 	
 	#3.获取容器注入环境变量：appType（web、mainstay、jar、nodejs...）、appName、port、version（隔离组）、group
 	getEnvParams()
 	
 	#4.根据服务类型执行相应的命令，执行失败重试3次
 	appStatus:=true
 	try{
	 	switch(appType){
	 		case web:
	 			bootStrapWeb()
	 			
	 		case mainstay:
	 			bootStrapMainstay()
	 			......
	 	}
	 } catch(Exception e){
	 
	 	log("Error :appName bootStrap failed!!!",e)
	 	appStatus=false
	 }
	 
	 #5. 采集App信息上报zk: appType:appName:ip:port:version:appStatus
	 postAppInfo()
	 
	 #6. 打开端口8081用于marathon／kubernetes的健康监测，确保容器的状态
	 
	 listen(8081)
	 
	 #7. 守护任务：租约到期，回收容器资源,清除相关节点
	 releaseContainer()
	  	 
 }
 ```
 
  
#### zk存储结构
```
nile-----appType1------appName1------
      |         
      |
      |        
      +--appType2------
      
 ```