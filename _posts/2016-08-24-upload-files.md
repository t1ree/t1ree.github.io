---
layout: post
title: "异步上传文件"
categories: AJAX
---

# 异步上传文件

参考：

> [在web应用中使用文件|MDN](https://developer.mozilla.org/zh-CN/docs/Using_files_from_web_applications)



```javascript
<?php
if (isset($_FILES['myFile'])) {
    // Example：
    move_uploaded_file($_FILES['myFile']['tmp_name'], 
        "uploads/" . $_FILES['myFile']['name']);
    exit;
}
?><!DOCTYPE html>
<html>
<head>
    <title>dnd binary upload</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <script type="text/javascript">
        function sendFile(file) {
            var uri = "/index.php";
            var xhr = new XMLHttpRequest();
            var fd = new FormData();
            
            xhr.open("POST", uri, true);
            xhr.onreadystatechange = function() {
                if (xhr.readyState == 4 && xhr.status == 200) {
                    // Handle response.
                    alert(xhr.responseText); // handle response.
                }
            };
            fd.append('myFile', file);
            // Initiate a multipart/form-data upload
            xhr.send(fd);
        }

        window.onload = function() {
            var dropzone = document.getElementById("dropzone");
            dropzone.ondragover = 
              dropzone.ondragenter = function(event) {
                event.stopPropagation();
                event.preventDefault();
            }
    
            dropzone.ondrop = function(event) {
                event.stopPropagation();
                event.preventDefault();

                var filesArray = event.dataTransfer.files;
                for (var i=0; i<filesArray.length; i++) {
                    sendFile(filesArray[i]);
                }
            }
    </script>
</head>
<body>
    <div>
        <div id="dropzone" 
          style="margin：30px; width：500px; height：300px; border：1px dotted grey;">
            Drag & drop your file here...
        </div>
    </div>
</body>
</html>
```



这篇文章在浏览器里好几天了，里面的内容非常重要，赶紧扑上来。



​	