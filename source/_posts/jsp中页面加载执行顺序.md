---
title: jsp中页面加载执行顺序
date: 2018-02-23 17:16:26
tags: jsp
---

- java是在服务器端运行的代码，而javascript和html都是在浏览器端运行的代码。所以加载执行顺序是是java>js=html。

- jsp中页面从上到下执行。

- js加载的顺序也就是页面中script标签出现的顺序。script标签里面的或者是引入的外部js文件的执行顺序都是其语句出现的顺序，其中js执行的过程也是页面装载的一部分。

-  onload指示页面包含图片等文件在内的所有元素都加载完成。

---

   jquery的document.ready，或者简写的$(function)表示文档结构已经加载完成执行（不包含图片等非文字媒体文件）。
   
   
```
<html>  
<head>  
<%@ include file="/common/header.jsp" %>   
<%  
    request.setAttribute("test1", "222");   
%>  
<script>  
    //执行顺序4. 这个是整个页面加载完毕后再执行  
    $(function(){         
        /**jsp表达式赋值**/  
        alert("444"+test1);               
        alert("444"+test2);  
  
    });  
  
    //在js里，执行顺序1，这块代码先执行  
    var test1= "<%=request.getAttribute("test1")%>";  
    var test2= "<%=request.getAttribute("test2")%>";  
    alert("111" + test1);  
    alert("111" + test2);  
  
<%   
    request.setAttribute("test2", "333");  
%>  
</script>  
</head>  
<body>  
    <!-- 执行顺序2 -->  
    <p>222</p>  
</body>  
<script>   
    //执行顺序3  
    alert("333" + test1);  
    alert("333" + test2);  
</script>  
</html> 
```

