<!-- markdownlint-disable MD033-->
<!-- 禁止MD033类型的警告 https://www.npmjs.com/package/markdownlint -->

# 接口：getCourseInfo  [返回](../README.md)
用例： [课程信息](../用例/课程信息.md)

- 功能：
    获取对应课程信息。
    
- 权限：    
    必须登录，学生或老师。
    
- API请求地址： 
    接口基本地址/v1/api/getCoursesInfo

- 请求方式 ：
    POST

- 请求参数说明:        

  请求参数为students_id或teachers_id。
    
- 返回实例：

        {         
            	"status": true,
				"data":[
					{
						"course_id":"04470580",
						"course_name":"软件系统分析与设计技术",
						"teacher":"赵卫东",
                        "declare":"软件系统分析是一门解决...",
                        "course_date":"2018-12-9"
                    }
					
				]
        }



- 返回参数说明：    
 
  |参数名称|说明|
  |:---------:|:--------------------------------------------------------|      
  |status|bool类型，true表示正确的返回，false表示有错误|
  |course_id|NUMBER(6,0)|主键|否| | | 课程编号|
  |teacher|VARCHAR2(30 BYTE)| |否| | | 开课教师|
  |course_name|VARCHAR2(50 BYTE)| |否| | | 课程名称|
  |declare|VARCHAR2(100 BYTE)| |是| | | 课程说明|
  |COURSE_DATE|DATE| |是|空| | 开课时间|


