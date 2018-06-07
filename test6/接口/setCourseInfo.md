# 接口：setCourseInfo  [返回](../README.md)
用例：[新增课程](../用例/课程信息.md) [修改课程信息](../用例/修改课程信息.md) 

- 功能：
    修改课程信息。
    
- 权限：
    老师。    
    
- API请求地址： 
    接口基本地址/v1/api/setCourseInfo

- 请求方式 ：
    POST

- 请求实例：

        {
            "course_id":"04470580",
            "course_name":"软件系统分析与设计技术",
            "teacher":"赵卫东",
            "declare":"软件系统分析是一门解决...",
            "course_date":"2018-12-9"
        }    
    
        
- 请求参数说明:        

  |参数名称|说明|
  |:---------:|:--------------------------------------------------------|      
  |course_id|NUMBER(6,0)|主键|否| | | 课程编号|
  |teacher|VARCHAR2(30 BYTE)| |否| | | 开课教师|
  |course_name|VARCHAR2(50 BYTE)| |否| | | 课程名称|
  |declare|VARCHAR2(100 BYTE)| |是| | | 课程说明|
  |COURSE_DATE|DATE| |是|空| | 开课时间|
  * 注：新建课程无需传入course_id,course_id为数据库主键，创建后无法更改，
  teacher_id由系统关联，同样无法更改。
  
- 返回实例：

        {         
            "status": true,
            "info": null,    

        }
 
- 返回参数说明： 
 
  |参数名称|说明|
  |:---------:|:--------------------------------------------------------|      
  |status|bool类型，true表示正确的返回，false表示有错误|
  |info|返回结果说明信息|

