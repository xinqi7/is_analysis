# 接口：getTests  [返回](../README.md)
用例： [实验信息](../用例/实验信息.md)

- 功能：
   根据课程编号返回实验信息。
    
- 权限：
    学生/老师。    
    
- API请求地址： 
    接口基本地址/v1/api/getTests

- 请求方式：
    GET

- 请求实例：

        {
            "course_id": "456879"
        }
        
- 请求参数说明:        

  |参数名称|说明|
  |:---------:|:--------------------------------------------------------|      
  |course_id|课程编号，对应唯一课程|
  
  
- 返回实例：

        { 
            "status": true,
            "info": null,
            "data": {
                "tests": [{
                    "test_id": 1，
                    "test_name": "python爬虫练习"，
                    "test_content": "使用requests爬取拉勾网"，
                    "last_time": "2018-1-9 10:00:00"

                },{
                    "test_id": 2，
                    "test_name": "python二次爬虫练习"，
                    "test_content": "使用requests爬取淘宝网"，
                    "last_time": "2018-1-10 10:00:00"
                }]   
            }    
        }

- 返回参数说明：    
 
  |参数名称|说明|
  |:---------:|:--------------------------------------------------------|      
  |status|bool类型，true表示正确的返回，false表示有错误|
  |info|返回结果说明信息|
  |data|返回主体信息JSON串|
  |tests|返回课程对应的所有实验集合，若空则返回[]|
  |test_id|实验编号|  
  |course_id|课程编号|
  |title|实验名称|
  |declare|实验内容|
  |last_time|实验截至时间|
