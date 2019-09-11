# MiniUI入门

## 要点

1. html与js对象的关系

   ```
   html中的class对应"mini-"+js中的对象名
   ```

2. html与js属性的关系

   ```
   html中的每个属性都对应js里的set+首字母大写的属性名
   ```

3. miniUI中事件名称必须小写,事件对应的方法不能带括号

## JS操作

1. 将html标签解析为miniUI控件

   ```
   mini.parse();
   ```

2. 获取控件对象

   ```javascript
   mini.get(id);
   mini.getByName(name);
   mini.getByName(name,parent)
   ```

3. 赋值

   ```javascript
   mini.get(id).setUrl("xxx.xxx");
   ```

4. 刷新控件

   ```javascript
   mini.get(id).load
   ```

5. 格式化数字

   ```javascript
   var num=mini.formatNumber(12.666,"0.0");
   alert(num.toString());
   ```

6. 格式化日期

   ```
   var dat=mini.formatDate(new Date(),"yyyy-MM-dd HH:mm:ss");
   ```

   

## DataGrid

## Form

### TextBox(文本框)

>  <input class="mini-testbox" name="password" required="true"/>

| Name                 | Type                       | Description                                                  |
| -------------------- | -------------------------- | ------------------------------------------------------------ |
| emptyText            | String                     | 文本为空时的提示内容                                         |
| value                | String                     | 值                                                           |
| allowInput           | Boolean                    | 允许文本输入                                                 |
| selectOnFocus        | Boolean                    | 获取焦点时选中文本                                           |
| maxLength            | Number                     | 最大字符串                                                   |
| inputStyle           | String                     | 输入框样式。比如：inputStyle="text-align:right;"             |
| errorMode            | String：icon、border、none | 错误提示方式                                                 |
| validateOnChanged    | Boolean                    | 值改变时验证                                                 |
| validateOnLeave      | Boolean                    | 失去焦点时验证                                               |
| indentSpace          | Boolean                    | 是否显示占位空白                                             |
| required             | Boolean                    | 不允许为空                                                   |
| labelField           | Boolean                    | 是否显示label                                                |
| label                | String                     | label文本                                                    |
| labelStyle           | String                     | label样式                                                    |
| requiredErrorText    | String                     |                                                              |
| vtype                | String                     | 验证规则。如vtype="email"。[参考示例](http://www.miniui.com/demo/form/rules.html)。 |
| emailErrorText       | String                     |                                                              |
| urlErrorText         | String                     |                                                              |
| floatErrorText       | String                     |                                                              |
| intErrorText         | String                     |                                                              |
| dateErrorText        | String                     |                                                              |
| maxLengthErrorText   | String                     |                                                              |
| minLengthErrorText   | String                     |                                                              |
| maxErrorText         | String                     |                                                              |
| minErrorText         | String                     |                                                              |
| rangeLengthErrorText | String                     |                                                              |
| rangeErrorText       | String                     |                                                              |