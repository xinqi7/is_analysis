# 实验2：图书管理系统用例建模
## 1. 图书管理系统的用例关系图
### 1.1 用例图PlantUML源码如下

``` usecase
@startuml test2-1
left to right direction
skinparam packageStyle rectangle
actor 读者 as a
actor 图书管理员 as b
actor 系统管理员 as c
rectangle 图书管理系统用例图{

    a --> (查询借阅信息)
    a --> (预定图书)
    (预定图书)<..(取消预订):<<include>>
    a --> (借书)
    (借书)<..(续借):<<include>>
    a --> (还书)
    (还书)<..(交罚金):<<extend>>
    a --> (注册)
    a --> (修改个人信息)
    a --> (查询图书信息)
    a --> (登录)

    b --> (修改个人信息)
    b --> (查询图书信息)
    b --> (登录)
    b --> (添加图书)
    b --> (修改（删除）图书)
    b --> (解除预定)
    b --> (允许还书)
    (允许还书)<..(收罚金):<<extend>>
    b --> (允许借书)
    (允许借书)<..(允许续借):<<include>>

    (发布新公告) <-- c
    (管理图书管理员) <-- c
    (管理读者) <-- c
    (维护系统) <-- c
    (登录) <-- c
    (添加图书) <-- c
    (修改（删除）图书) <-- c
}
@enduml 
```
### 1.2用例图如下
![](images/1.png '描述')
## 2.参与者说明
### 2.1图书馆管理员
主要职责是：维护书目、借出图书、归还图书、维护读者信息
### 2.2 读者
主要职责是：取消预定、查询借阅情况、预定图书、查询书目、个人信息查询
## 3.用例规约表



### 3.1维护书目

<table>
<tr>
<th>用例名称</th>
<td>维护书目</td>
</tr>
<tr>
<th>参与者</th>
<td>图书管理员</td>
</tr>
<tr>
<th>前置条件</th>
<td>图书管理员已被识别和授权</td>
</tr>
<tr>
<th>后置条件</th>
<td>根据所需维护书目的实际库存来更新数据库中记录的库存量</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.图书管理员检查所维护书目的实际库存量；<br>
2.图书管理员在系统中查询图书书目；<br>
<br><br>
5.图书管理员比对数量是否一致并更新系统信息<br></td>
<td>
<br><br>3.系统验证图书管理员身份；，<br>
4.系统返回该书目信息；<br><br>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;3a.非法管理员<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示并拒绝进入系统<br>
</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1.如果实际库存量低于系统数量，更新系统数量后应通知相关人员补充书目<br>
</td>
</table>



### 3.2借出图书
#### 源码：
>
``` usecase
@startuml test2-2
 start 
 :登录; 
 :列出所有图书; 
 :选择借出图书; 
if(是否有借出图书？) then (yes) 
 :显示该图书信息; 
else (no) 
 :提示不存在此图书; 
stop 
 endif 
 :借出图书; 
 :填写借书单; 
 :确认库存; 
if(是否有库存？) then (yes) 
 :确认借书单; 
else (no) 
 :提示此图书没有库存; 
stop 
 endif 
 :更新借书单; 
 :更新图书库存; 
stop 
 @enduml 
```
#### 流程图：
![](images/2.png '描述')
<table>
<tr>
<th>用例名称</th>
<td>借出图书</td>
</tr>
<tr>
<th>参与者</th>
<td>图书管理员（主要参与者）、读者（次要参与者）</td>
</tr>
<tr>
<th>前置条件</th>
<td>图书管理员已被识别和授权</td>
</tr>
<tr>
<th>后置条件</th>
<td>存储借书记录，更新库存数量，所借图书状态为借出</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.图书管理员将读者借书卡提供给系统；<br>
<br>
3.图书管理员将读者所借图书输入系统；<br>
<br><br>
6.重复3~5，知道图书管理员确认全部图书登记完毕<br><br></td>
<td><br>2.系统验证读者身份和借书条件；，<br><br>
4.系统记录借书信息，并且修改图书的状态和此种图书的可借数量；<br>
5.系统累加读者的借书数量；<br><br>
7.系统打印借书清单，交易成功完成</td>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;2a.非法读者<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示错误并拒绝接受输入<br>
&nbsp;&nbsp;2b.读者借书数已达限额<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示错误并拒绝接受输入<br>
&nbsp;&nbsp;5a.读者借书数已达限额<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示，并要求结束输入<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.图书管理员确认借书完成<br>
&nbsp;&nbsp;5b.读者有该书的预定记录<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.删除该书的预定信息<br>
</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1.借书时间为每日8.00~22.00（周一~周六）<br>
&nbsp;&nbsp;2.每人最多同时借阅5本图书
</td>
</table>



### 3.3归还图书

<table>
<tr>
<th>用例名称</th>
<td>归还图书</td>
</tr>
<tr>
<th>参与者</th>
<td>图书管理员（主要参与者）、读者（次要参与者）</td>
</tr>
<tr>
<th>前置条件</th>
<td>图书管理员已被识别和授权</td>
</tr>
<tr>
<th>后置条件</th>
<td>修改借书记录，更新库存数量，修改图书状态为可借</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.图书管理员将图书提供给系统；<br>
<br><br><br>
5.图书管理员重复步骤1，知道退出；<br>
</td>
<td><br>2.系统根据借书记录验证图书信息；，<br>
3.系统提供借阅该书的读者信息；<br>
4.系统修改借书记录，更新该书的状态以及此种书的可借数量；<br>
</td>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1a.图书不完整<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.图书管理员拒绝回收，并罚款，同时维护书目<br>
</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1.回收图书时检查图书是否完整，不完整将收取罚金<br>
&nbsp;&nbsp;2.还书超过规定日期将收取额外费用
</td>
</table>



### 3.4维护读者信息

<table>
<tr>
<th>用例名称</th>
<td>维护读者信息</td>
</tr>
<tr>
<th>参与者</th>
<td>图书管理员（主要参与者）、读者（次要参与者）</td>
</tr>
<tr>
<th>前置条件</th>
<td>图书管理员已被识别和授权</td>
</tr>
<tr>
<th>后置条件</th>
<td>更改或补充读者信息</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.图书管理员将读者个人信息提供给系统；<br>
<br><br>
4.重复步骤1</td>
<td><br>2.系统验证图书管理员身份；<br>
3.系统更新读者信息；<br><br>
</td>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;2a.非法管理员<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示并拒绝进入系统<br>
</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;无<br>
</td>
</table>



### 3.5取消预定

<table>
<tr>
<th>用例名称</th>
<td>取消预定</td>
</tr>
<tr>
<th>参与者</th>
<td>读者</td>
</tr>
<tr>
<th>前置条件</th>
<td>读者已被识别和授权</td>
</tr>
<tr>
<th>后置条件</th>
<td>更新读者信息，设置图书状态为未预定</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.图书管理员将读者借书卡提供给系统；<br>
<br>
3.图书管理员将读者取消预定图书输入系统；<br>
<br>
5.图书管理员确认全部图书登记完毕<br><br></td>
<td><br>2.系统验证读者身份；，<br><br>
4.系统删除预定信息，并且修改图书的状态和此种图书的可借数量；<br>
<br></td>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;4a.无预定信息<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示错误并拒绝接受输入
</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1.每人最多同时借阅5本图书
</td>
</table>



### 3.6查询借阅情况


<table>
<tr>
<th>用例名称</th>
<td>查询借阅情况</td>
</tr>
<tr>
<th>参与者</th>
<td>读者</td>
</tr>
<tr>
<th>前置条件</th>
<td>读者已被识别和授权</td>
</tr>
<tr>
<th>后置条件</th>
<td>系统显示读者借阅情况</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.读者登录到系统；<br>
<br><br>
4.重复步骤1；<br>
<br></td>
<td><br>2.系统验证读者身份；，<br>
3.系统反馈读者的借阅情况；<br><br>
</td>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;2a.非法读者<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示错误并拒绝访问<br>
</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1.读者的借阅情况只能系统修改
</td>
</table>



### 3.7预定图书
#### 源码：
>
``` usecase
@startuml test2-3
 start
 :登录;
 :列出所有图书;
 :选择预定图书;
if(是否有预定图书？) then (yes)
 :显示该图书信息;
else (no)
 :提示不存在此图书;
stop
 endif
 :预定图书;
 :填写预定单;
if(图书是否全部借出？) then (yes)
 :图书已全部借出，不能进行预定;
stop
 else (no)
 :确认预定信息;
endif
 :保存预定单;
stop
 @enduml
```
#### 流程图：
![](images/3.png '描述')
<table>
<tr>
<th>用例名称</th>
<td>预定图书</td>
</tr>
<tr>
<th>参与者</th>
<td>读者</td>
</tr>
<tr>
<th>前置条件</th>
<td>读者已被识别和授权</td>
</tr>
<tr>
<th>后置条件</th>
<td>更新读者信息，设置图书状态为预定</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.图书管理员将读者借书卡提供给系统；<br>
<br>
3.图书管理员将读者预定图书输入系统；<br>
<br>
5.图书管理员确认全部图书登记完毕<br><br></td>
<td><br>2.系统验证读者身份和借书条件；，<br><br>
4.系统记录预定信息，并且修改图书的状态和此种图书的可借数量；<br>
<br></td>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;2a.非法读者<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示错误并拒绝接受输入<br>
&nbsp;&nbsp;2b.读者借书数已达限额<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示错误并拒绝接受输入<br>

</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1.每人最多同时借阅5本图书
</td>
</table>

### 3.8查询书目
#### 源码：
>
``` usecase
@startuml test2-4
 start   
 :登录;   
 :给出查询图书按钮;   
 :选择查询图书;   
 :列出所有图书信息;   
 :输入所需要查询的图书;   
if(是否有该图书？) then (yes)   
 :显示该图书信息;   
else (no)   
 :提示不存在该图书;   
stop   
 endif   
 :选择查看书目信息;   
if(是否有书目信息？) then (yes)   
 :显示该图书书目信息;   
else (no)   
 :提示该图书没有书目信息;   
endif   
 stop   
 @enduml 
```  
#### 流程图：
![](images/4.png '描述')
<table>
<tr>
<th>用例名称</th>
<td>查询书目</td>
</tr>
<tr>
<th>参与者</th>
<td>图书管理员、读者</td>
</tr>
<tr>
<th>前置条件</th>
<td>进入系统</td>
</tr>
<tr>
<th>后置条件</th>
<td>返回查询信息</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.进入系统；<br>
2.提交需要查询书目的信息；<br>
<br>
4.退出系统<br></td>
<td><br><br>3.系统根据查询信息返回书目信息；，<br><br>
</td>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;3a.无此书籍<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示无该书籍<br>
</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1.读者可以向管理员提供图书馆没有的书籍，待审核成功后可购入<br>
&nbsp;&nbsp;2.管理员应注意读者们的需求，添加图书
</td>
</table>

### 3.9个人信息查询
#### 源码：
>
``` usecase
@startuml test2-5
 |教务处|    
start    
 @startuml     
 start    
 :点击个人信息按钮;    
 :显示对应的个人信息;    
stop    
 @enduml    
 stop    
 @enduml    
```
#### 流程图：
![](images/5.png '描述')
<h4>规约表:</h4>
<table>
<tr>
<th>用例名称</th>
<td>个人信息查询</td>
</tr>
<tr>
<th>参与者</th>
<td>读者</td>
</tr>
<tr>
<th>前置条件</th>
<td>读者已被识别和授权</td>
</tr>
<tr>
<th>后置条件</th>
<td>系统显示读者个人信息</td>
</tr>
<tr>
<th colspan=2>主事件流</th>
</tr>
<tr>
<th>参与者动作</th>
<th>系统行为</th>
</tr>
<tr>
<td>1.读者登录到系统；<br>
<br><br>
4.重复步骤1；<br>
<br></td>
<td><br>2.系统验证读者身份；，<br>
3.系统反馈读者的个人信息；<br><br>
</td>
</tr>
<tr>
<th colspan=2>备选事件流</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;2a.非法读者<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.系统提示错误并拒绝访问<br>
</td>
<tr>
<tr>
<th colspan=2>业务规则</th>
</tr>
<tr>
<td colspan=2>
&nbsp;&nbsp;1.读者的信息只有提交给管理员修改
</td>
</table>

