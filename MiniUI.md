# MiniUI

## Base

1. mini.parse()  将html解析为miniui控件

2. mini.get(id)

3. mini.getByName(name,*parent)

4. mini.getsByName

5. mini.formatNumber(number,format)

   ```javascript
   var a=3.1415926;
   var s=mini.formatNumber(a,"0.00");
   alert(s);
   ```

   

## JSON

> 字符串转json对象

​	mini.encode(jsonobj);

> json对象转字符串

​	mini.decode(jsonstring,autoParseDate)

*  autoParseDate是否自动解析date数据

## Date

1. mini.parseDate(String) 将特定格式字符串转为Date对象

   ```javascript
   var date="2018-09-06";
   date=mini.parseDate(date);
   //date.getMonthOfDate();
   ```

1. mini.formatDate(Date,String) 将Date对象按指定格式转为字符串

   ```javascript
   var date=new Date();
   date=mini.formatDate(date,"yyyy-MM-dd");
   alert(date);
   ```

## Control

1. show() 显示控件

2. hide() 隐藏控件

   ```javascript
   mini.getByName("totalscore").hide();
   ```

## 控件

### Form 表单

1. 获取表单控件

   ```javascript
   var form=new mini.Form("#id");
   ```

2. 获取控件的值

   ```javascript
   var form=new mini.Form("#id");
   var data=form.getData();//data为json对象
   var jsonString=mini.encode(data);
   ```

3. 为控件赋值

   ```javascript
   var form=new mini.Form("#id");
   form.setData(data);//data为json对象
   ```

4. 重置表单

   ```javascript
   var form=new mini.Form("#id");
   from.reset();
   ```

5. 验证表单

   * validate 验证表单
   * isvalidate 是否全部验证通过
   * clear 清空表单
   * reset 重置表单
   * isChanged 表单是否变动
   * getFields 获取表单数组

### Button 按钮

1. 创建一个按钮

   ```javascript
   <a class="mini-button" iconCls="icon-edit" onclick="">按钮</a>
   ```

2. 按钮参数

   * href
   * target
   * iconCls 图标
   * plain 背景透明(false)
   * checked 是否选择(false)
   * checkedOnClick(单击时是否被选中 false)
   * groupName(菜单组名称)

### CheckBoxList 复选框(单选框)

1. 创建一个复选框

   ```javascript
   <div id="checkboxlist1" class="mini-checkboxlist" url="data.txt" textField="text" valueField="id" onvaluechanged="test01" value="1,2">
   ```

2. 复选框参数

   * url 内容所在地址
   * textField 需要显示的数据 对应url地址中json字符串的key值
   * valueField 字段对应的值 对应url地址中json字符串的key值
   * multiSelect 是否可以多选(true)
   * value 选中项 对应valueField所对应的值

3. 事件

   valuechanged 值发生改变

### RadioButtonList

1. 创建单选框

   ```javascript
   <div id="radiobuttonlist" class="mini-radiobuttonlist" url="data/booktype.txt" value=2 onvaluechanged="test01"></div>	
   ```

2. 单选框参数

   * url 内容所在地址
   * textField 需要显示的数据 对应url地址中json字符串的key值
   * valueField 字段对应的值 对应url地址中json字符串的key值
   * multiSelect 是否可以多选(true)
   * value 选中项 对应valueField所对应的值

3. 事件

   valuechanged 值发生改变

### Calendar 日期控件

1. 创建日期控件

   ```javascript
   <div id="calendar" class="mini-calendar"><div>
   ```

### TextBox 文本输入框

### Password 密码输入框

### TextArea 多行文本输入框

### Datepicker 日期输入框

1. 创建日期输入框

   ```javascript
   <input name="buydate" class="mini-datepicker" required="true"/></input>
   ```

   



