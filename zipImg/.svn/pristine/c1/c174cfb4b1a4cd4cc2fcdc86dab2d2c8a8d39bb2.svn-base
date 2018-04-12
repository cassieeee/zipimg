/**
 * 这是基于html5的canvas压缩图片的工具。
 */
var ImageResizer=function(opts){
    var settings={
        resizeMode:"auto",//压缩模式，总共有三种  auto,width,height auto表示自动根据最大的宽度及高度等比压缩，width表示只根据宽度来判断是否需要等比例压缩，height类似。
        dataSource:"" ,//数据源。数据源是指需要压缩的数据源，有三种类型，image图片元素，base64字符串，canvas对象，还有选择文件时候的file对象。。。
        dataSourceType:"img",//image  base64 canvas
        maxWidth:500,//允许的最大宽度
        maxHeight:500,//允许的最大高度。
        success:function(resizeImgBase64,canvas){}//压缩成功后图片的base64字符串数据。
    };

    $.extend(settings,opts);

    var innerTools={
        getBase4FromImgFile:function(file,callBack){
            var reader = new FileReader();
            reader.onload = function(e) {
                var base64Img= e.target.result;
                if(callBack){
                    callBack(base64Img);
                }
            };
            reader.readAsDataURL(file);
        },

        //处理数据源,将所有数据源都处理成为图片对象，方便处理。
        getImgFromDataSource:function(datasource,dataSourceType,callback){
            var _me=this;
            var image = new Image();
            if(dataSourceType=="img"){
                image.src=$(datasource).attr("src");
                _me.judgeOnload(callback,image);
            }
            else if(dataSourceType=="base64"){
                image.src=datasource;
                _me.judgeOnload(callback,image);
            }
            else if(dataSourceType=="canvas"){
                image.src = datasource.toDataURL("image/jpeg");
                _me.judgeOnload(callback,image);
            }
            else if(dataSourceType=="file"){
                _me.getBase4FromImgFile(datasource,function(base64str){
                    image.src=base64str;
                    _me.judgeOnload(callback,image);
                });
            }
        },

        //递归直至图片src读取完成，避免图片读取不全执行后续代码BUG
        judgeOnload:function(callback,img){
            var _me=this;
            if($(img)[0].naturalWidth == 0){
                setTimeout(function(){
                    _me.judgeOnload(callback,img);
                },500);
            }else{
                callback(img);
            }
        },

        //根据设置值计算图片的需要压缩的尺寸。
        getResizeSizeFromImg:function(img){
            var _img_info={
                w:$(img)[0].naturalWidth,
                h:$(img)[0].naturalHeight
            };
            if(_img_info.w <= settings.maxWidth && _img_info.h <= settings.maxHeight){
                return _img_info;
            }
            var _percent_scale=parseFloat(_img_info.w/_img_info.h);
            if(settings.resizeMode=="auto"){
                var _size_by_mw={
                    w:settings.maxWidth,
                    h:parseInt(settings.maxWidth/_percent_scale)
                };
                var _size_by_mh={
                    w:parseInt(settings.maxHeight*_percent_scale),
                    h:settings.maxHeight
                };
                if(_size_by_mw.h <= settings.maxHeight){
                    return _size_by_mw;
                }
                if(_size_by_mh.w <= settings.maxWidth){
                    return _size_by_mh;
                }

                return {
                    w:settings.maxWidth,
                    h:settings.maxHeight
                };
            }
            if(settings.resizeMode=="width"){
                if(_img_info.w<=settings.maxWidth){
                    return _img_info;
                }
                var _size_by_mw={
                    w:settings.maxWidth,
                    h:parseInt(settings.maxWidth/_percent_scale)
                };
                return _size_by_mw;
            }
            if(settings.resizeMode=="height"){
                if(_img_info.h<=settings.maxHeight){
                    return _img_info;
                }
                var _size_by_mh={
                    w:parseInt(settings.maxHeight*_percent_scale),
                    h:settings.maxHeight
                };
                return _size_by_mh;
            }
        },

        //--将相关图片对象画到canvas里面去。
        drawToCanvas:function(img,theW,theH,realW,realH,callback){
            var canvas = document.createElement("canvas");
            canvas.width=theW;
            canvas.height=theH;
            var ctx = canvas.getContext('2d');
            ctx.drawImage(img,
                0,
                0,
                realW,
                realH,
                0,
                0,
                theW,
                theH
            );

            //--获取base64字符串及canvas对象传给success函数。
            var base64str=canvas.toDataURL("image/png");
            if(callback){
                callback(base64str,canvas);
            }
        }
    };

    //--开始处理。
    (function(){
        innerTools.getImgFromDataSource(settings.dataSource,settings.dataSourceType,function(_tmp_img){
            //--计算尺寸。
            var _limitSizeInfo=innerTools.getResizeSizeFromImg(_tmp_img);
            var _img_info={
                w:$(_tmp_img)[0].naturalWidth,
                h:$(_tmp_img)[0].naturalHeight
            };
            innerTools.drawToCanvas(_tmp_img,_limitSizeInfo.w,_limitSizeInfo.h,_img_info.w,_img_info.h,function(base64str,canvas){
                settings.success(base64str,canvas);
            });
        });
    })();
};

function dataURLtoBlob(dataurl) {
    var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
    while(n--){
        u8arr[n] = bstr.charCodeAt(n);
    }
    return new Blob([u8arr], {type:mime});
}