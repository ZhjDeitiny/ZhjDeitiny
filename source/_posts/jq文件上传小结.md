---
title: 基于jq的input file文件上传
date: 2018-8-21 10:25:03
categories: Web
---

input file文件上传很多人都遇见过，就样式问题就有一大堆的问题，默认样式实在是不敢恭维，但在此今天不讨论样式的问题，只给一个大致的思路，一个div包括一个span和一个input，分别设置两个的定位，然后给input设置opacity为0，设置好宽度充满div，在设置好span的样式即可。

众所周知，ajax不能传递文件，但是html的formdata可以。

### html代码
``` 
<p class="selected-box">
	<form class="selected-wrap" id="form1" action="Upload" method="post" enctype="multipart/form-data">
		<input type="file" class="selected-btn" id ="file" name="file">
	</form>
	<span class="selected-file">选择本地</span>
</p>
<div class="leading-in">导入</div>
```
我的代码是点击选择本地出发input file的按钮选择文件，然后点击导入出发ajax，中间还会判断是否已选择文件，没有选择的话，提示它，选择文件不对的话，也提示（我这边判断的是Execl文件，下方代码有）。其中需要注意的是必须要用form标签包裹，注意action、method和enctype属性，其中特别需要注意的是<strong style='color:red;'>enctype</strong>属性必须为<strong style='color:red;'>multipart/form-data</strong>，这样子才能将文件处理为一个二进制的文件，后台才能够进行接收。

<!--more-->

&emsp;&emsp;PS：action这个属性其实也可以不填，它是放地址的，我们在ajax中会有提交地址的;
&emsp;&emsp;&emsp;&emsp;enctype这个属性其实也可以不在标签中添加，不过在js中的属性contentType就必须设置为multipart/form-data，
* 简单来讲
>如果你的input file标签里面<strong style='color:red;'>使用了enctype="multipart/form-data"</strong>，JQ中这两个属性就要这样写<strong style='color:red;'>processData: false,  contentType: false</strong> 
>如果如果你的input file标签里面<strong style='color:red'>没有使用 enctype="multipart/form-data"</strong>，JQ就要这样写<strong style='color:red;'>processData: false，contentType: "multipart/form-data"</strong>。

### js代码
``` 
$(".leading-in").on("click",function(){
	var name = $(".selected-txt select option:selected").val();
	var flag = $(".selected-btn").val();
	var hzm =  flag.substr(flag.indexOf(".")+1);
	if(flag == ""){
		alert("请选择要上传的文件");
	}else if(hzm != "xls" && hzm != "xlsx"){
		alert("请选择Excel格式的文件");
	}else{
		var forData = new FormData();
		forData.set("importfile", $("#file")[0].files[0]);
		forData.set("systemName", name);
		
//		多个文件上传的情况，需要啊后台进行字段匹配如：上方的importfile
//		var i;
//		for (i = 0; i < $('.select-file').files.length; i++) {
//
//			forData.append('file[]', this.files[i]);
//		}
		
		$.ajax({
			url: baselocation + "/system/file/import",
			type: 'post',
			data: forData,
			cache: false,
			processData: false,
			contentType: false,
			success:function(json){
				if(json.success == "success"){
					alert("上传成功");
				}else{
					alert("上传失败");
				}
			},error:function(){
				
			}
		});
	}	
})
```

js代码中我先判断获取文件的后缀名是不是我自己想要的类型文件，然后进行相应的提示，然后就新建一个FormData对象，然后把文件添加到里面，规则名一个要和后台确定好，如果还有其他的属性的，一并放到FormData的对象中就好，毕竟一个单独的属性，没有必要单独拿出来去提交，直接放到对象里面一起提交就好。
### 小结
* 1、 <strong style='color:red'>cache</strong>：cache设为false可以禁止浏览器对该URL（以及对应的HTTP方法）的缓存。 jQuery通过为URL添加一个冗余参数来实现。
* 2、 <strong style='color:red'>contentType</strong>：jQuery中content-type默认值为application/x-www-form-urlencoded，因此传给data参数的对象会默认被转换为query string，我们不需要jQuery做这个转换，否则会破坏掉multipart/form-data的编码格式。 因此设置contentType: false来禁止jQuery的转换操作。
* 3、 <strong style='color:red'>processData</strong>：jQuery会将data对象转换为字符串来发送HTTP请求，默认情况下会用 application/x-www-form-urlencoded编码来进行转换。 我们设置contentType: false后该转换会失败，因此设置processData: false来禁止该转换过程。我们给的data就是已经用FormData编码好的数据，不需要jQuery进行字符串转换。(它与enctype的关系上方已经阐述)


</br>如果有多文件的情况，代码中也有注释，前提是一定要和后台定好参数规则，建议比如说：file[文件索引]这样的规则，后台拿到数据之后出掉一些定义好的其他属性，根据length，一个一个去取好；