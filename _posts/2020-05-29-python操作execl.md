---
title: python 读写 execl
description: 使用python openyxl库完成对execl文件的读写
categories: python
tags: something
---

由于最近业务上有统计相关的需求，需要对上千多张表进行操作，并且整理成excel，因此使用python openyxl库来进行相关操作

> 概念：
>   * workbook： 相当于是一个文件，也就是一个excel
>   * sheet： excel 的一个工作表
>   * cell ： sheet 的一个单元格

# 具体操作

## excel文件操作

#### 创建excel文件
    
    wb = openpyxl.Workbook() 

#### 打开excel文件
    
    wb = openpyxl.load_workbook('/Users/python操作excel.xlsx')
    
#### 保存excel文件
    
    wb.save('/Users/python操作excel.xlsx')
   
 
## sheet表操作

#### 创建sheet表
    
    sheet = wb.create_sheet(title=("创建sheet".decode('utf-8')))     #插入在末尾  
    or  
    sheet = wb.create_sheet(title=("创建sheet".decode('utf-8')）, 0) #插入在最开始  
   

#### 打开sheet表
    
    sheet = wb.active 
    # 返回当前默认的WorkSheet
    
    sheet = wb["New Title"]
    # 打开title为 "New Title"的表
    

## 单元格操作

#### 写入数据

    sheet.cell(row = 1, column = 1, value = 2)
    第一行的第一格数据写入2
    
#### 合并单元格

    sheet.merge_cells(start_row=1, start_column=1, end_row=1, end_column=2)
    合并第一行的 第1格和第2格。
    
#### 修改单元格属性
openyxl通过styles模块可以设置单元格、列的属性，通过以下函数分别配置：

* Font：来设置文字的大小，颜色和下划线等
* PatternFill：填充图案和渐变色
* Border：单元格的边框
* Alignment：单元格的对齐方式等
* protection：写保护

> 传送门：[官方文档](https://openpyxl.readthedocs.io/en/default/styles.html#worksheet-additional-properties)

导入依赖
    
    from openpyxl.styles import Font, Alignment, Border,  protection # 导入代码库   


设置字体大小：

    sheet.cell(row=1, column=1).font = Font(u'宋体',size = 11,bold=True,italic=True,strike=True,color='000000')   
 
 
字体:“宋体”，size：大小, bold：加粗，italic：斜体，strike：删除线，颜色为黑色  


设置单元格填充颜色：

    sheet.cell(row=1, column=1).fill = PatternFill(fill_type = None, start_color='FFFFFF', end_color='000000')  
    sheet.cell(row=1, column=1).fill = PatternFill(fill_type='solid', fgColor="FF0000") # 设置填充为红色
     
start_color：前景色， end_color：背景色  


设置单元格居中：

    sheet.cell(row=1, column=1).align = Alignment(horizontal='left',vertical='center',wrap_text=True)    

horizontal: 水平方向
* 居中 center
* 左对齐 left
* 右对齐 right
* 分散对齐 distributed
* 跨列居中 centerContinuous
* 两端对齐 justify
* 填充 fill
* 常规 general    

vertical:  垂直方向
* 居中 center
* 靠上 top
* 靠下 bottom
* 两端对齐 justify
* 分散对齐 distributed  


wrap_text：自动换行，布尔类型的参数，这个参数还可以写作wrapText  


## 行/列
Worksheet 对象的 row_dimensions 和 column_dimensions 属性，分别控制行高和列宽。

#### 设置行高 
    sheet.row_dimensions[1].height=100 
    
#### 设置列宽   
    sheet.column_dimensions['A'].width=100  
    
    
# 元素属性

## workbook属性
* sheetnames： 返回所有WorkSheet的名字列表，类型为list
* worksheets： 返回所有WorkSheet的列表，类型为list
* active： 返回当前默认选中的WorkSheet

## workbook方法：
* get_sheet_names()： 同sheetnames
* get_active_sheet()： 同active属性
* get_sheet_by_name(name)： 根据名称获取WorkSheet
* remove(worksheet)： 删除一个WorkSheet，注意是WorkSheet对象，不是名字
* save(filename)： 保存到文件，记住有写入操作记得保存。
* copy_worksheet(worksheet)： 复制一个WorkSheet，注意是WorkSheet对象，不是名字

## sheet属性
* rows： 返回所有有效数据行，有数据时类型为generator，无数据时为tuple
* columns： 返回所有有效数据列，类型同rows
* max_column： 有效数据最大列
* max_row： 有效数据最大行
* min_column： 有效数据最小列，起始为1
* min_row： 有效数据最大行，起始为1
* values： 返回所有单元格的值的列表，类型为tuple
* title： WorkSheet的名称

## cell属性
* column： 所在列，起始为1
* row： 所在行，起始为1
* coordinate：  所在坐标，如'A1'
* parent： 所属的WorkSheet
* value： 单元格的值