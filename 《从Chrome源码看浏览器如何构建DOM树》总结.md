
![Smaller icon](http://images2015.cnblogs.com/blog/818663/201701/818663-20170107232834409-1506458752.png)

#### 第一步：
DocumentLoader类中的startLoadingMainResource函数加载url返回的数据。

####  第三步
###### （5）开标签处理

对于处理例如<html>标签，处理这个标签的任务是实例化一个HTMLHtmlElement元素，然后吧它的父元素指向document。

实际操作过程：

先创建一个html结点，再将其父结点和当前结点传入到一个任务队列。同时，它会被压入一个用于存放未遇到闭标签的所有开标签。

对于上面所说的任务队列，会有函数去判断任务的类型，然后进行不同的操作，比如本次执行的是insert任务，它会去执行一个插入的函数。再插入函数中，会先去检查父元素是否支持子元素，如果不支持，则直接返回（就像video标签不支持子元素）。接着再去调具体插入。

***插入操作的idea：***

* 先设置子元素的父结点，也就是说会把html结点的父结点指向document

* 如果没有lastChild，就会将这个子元素作为firstChild。

* 但是由于上面已经有一个doctype的子结点了，也就是说已经有lastChild了，就会把这个子元素的previousSibling指向老的lastChild，老的lastChild的nextSibling指向它。

* 最后把子元素设置为当前containerNode的lastChild。这样就建立起了html结点的父子兄弟关系。

###### （6）闭标签处理
当遇到一个闭标签时，会把栈里的元素一直pop出来，知道pop到第一个和它标签名字一样的：
例如：

    <!DOCTYPE html>
       <html>
          <head>
              <meta charset="utf-8"></meta>
          </head>
          <body>
              <div>
                  <p><b>hello</b></p>
                  <p>demo</p>
              </div>
           </body>
       </html>
       
把push和pop打印出来是这样的：

 	 push "HTML" m_stackDepth = 1
	 push "HEAD" m_stackDepth = 2
 	 pop "HEAD" m_stackDepth = 1
 	 push "BODY" m_stackDepth = 2
 	 push "DIV" m_stackDepth = 3
 	 push "P" m_stackDepth = 4
 	 push "B" m_stackDepth = 5
 	 pop "B" m_stackDepth = 4
 	 pop "P" m_stackDepth = 3
 	 push "P" m_stackDepth = 4
 	 pop "P" m_stackDepth = 3
 	 pop "DIV" m_stackDepth = 2
 	 "tagName: body  |type: EndTag    |attr:              |text: "
 	 "tagName: html  |type: EndTag    |attr:              |text: "
 	 
但是如果有闭标签忘记写了，就会这样：

	push "P" m_stackDepth = 4
	push "B" m_stackDepth = 5
	"tagName: p     |type: EndTag    |attr:              |text: "
	pop "B" m_stackDepth = 4
	pop "P" m_stackDepth = 3
	push "B" m_stackDepth = 4
	push "P" m_stackDepth = 5
	pop "P" m_stackDepth = 4
	pop "B" m_stackDepth = 3
	pop "DIV" m_stackDepth = 2
	push "B" m_stackDepth = 3

###### （7）自定义标签的处理
可以把这个标签当作span标签


### 第四步 查DOM过程




