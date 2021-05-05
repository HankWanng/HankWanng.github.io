# python多线程使用win32com

## 报错信息

-  报错AttributeError: ＜unknown＞.Open
- pywintypes.com_error: -2147221021，'操作无法使用

## 原因

多任务时，打开了Windows的application未及时关闭导致其他任务无法调用该应用

## 解决方案

1. 在多线程里面使用win32com调用com组件的时候，需要用pythoncom.CoInitialize初始化一下
2. win32com.client.Dispatch() 改成 win32com.client.DispatchEx()
3. 关闭application
4. 最后还需要用pythoncom.CoUninitialize释放资源

> 解释：Dispatch()是多线程，在执行时，会先检查有没有已经开了的word软件资源，如果有，就会直接用那个word软件资源，这样就导致了资源抢夺最终进程堵塞； 而DispatchEx()是独立线程，在执行时，无论当前有没有已经开了的word软件资源，它都会自己根据文档再开一次word软件，也就是说，上一个进程进行到哪里与它无关，所以不会出现资源抢夺的问题。

## 实例代码

```python
#coding:utf-8
from win32com.client import DispatchEx
import pythoncom
 
def test(doc_full_path):
    table_count = 0
    pythoncom.CoInitialize()
    try:
        word = DispatchEx('word.application')
        word.visible = False
        doc = word.documents.Open(doc_full_path)
 
        #获取该文档的表格数
        table_count = doc.tables.count
    
        #关闭
        doc.Close()
    except Exception, e:
        print e.message
    finally:
        #对com操作，一定要确保退出word应用
        if word:
            word.Quit()
        
        #释放资源
        pythoncom.CoUninitialize()
        return table_count
```

