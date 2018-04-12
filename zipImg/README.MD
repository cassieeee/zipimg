##前端利用canvas画布功能压缩图片##
可以利用次工具把用户上传的图片在上传前在前端进行压缩。减小图片上传时的压力。

demo.html可以查看压缩具体实现方式，结果

ImageResizer({
    resizeMode:"auto",//压缩模式，总共有三种  auto,width,height auto表示自动根据最大的宽度及高度等比压缩，width表示只根据宽度来判断是否需要等比例压缩，height类似。
    dataSource:"" ,//数据源。数据源是指需要压缩的数据源，有三种类型，image图片元素，base64字符串，canvas对象，还有选择文件时候的file对象。。。
    dataSourceType:"img",//image  base64 canvas
    maxWidth:500,//允许的最大宽度
    maxHeight:500,//允许的最大高度。
    success:function(resizeImgBase64,canvas){}//压缩成功后图片的base64字符串数据,canvas对象。
});"# zipimg" 
