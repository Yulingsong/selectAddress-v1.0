# selectAddress-v1.0
web移动端地址选择控件最简单最粗糙版本；

&#160;&#160;&#160;&#160;好久都没有写东西了，现在干的活都是重复和简单的活，能总结出来的东西还是不太多。

&#160;&#160;&#160;&#160;目前做电商平台，其中用到选择地址的控件，不过不是自己写的，最近花时间重新自己做一个，目前就先吧功能样式，功能完成第一版，之后再进行优化和升级，最终做成组件或者是插件形式，调用只需要几行代码去完成，不过这些都是后话了，目前显示完成基本的样子和功能。这一篇文章就当做写东西的一个笔记，记录一个过程。

&#160;&#160;&#160;&#160;先放上出来的效果
![成果gif](https://upload-images.jianshu.io/upload_images/1062695-499ce8d2bb413753.gif?imageMogr2/auto-orient/strip)

&#160;&#160;&#160;&#160;看效果图挺简单的，因为懒。就用了jquery加js，方法什么的也没考虑性能啥的，就初步先做一个出来。后面再慢慢一步步优化。

&#160;&#160;&#160;&#160;不愿意往下看的有兴趣的可以直接看这里 [githubd selectAddress-v1.0地址](https://github.com/Yulingsong/selectAddress-v1.0)

&#160;&#160;&#160;&#160;接下来我们进入正题！有什么问题可以留言跟我讨论，这个功能不是很复杂，只是作为粗糙版就没有考虑很多。

# 第一步：构思功能样子，先有概念！

&#160;&#160;&#160;&#160;首先至少得有想法，做成什么样子，这个很重要，要是会点设计，可以自己先做个大概的图或者原型，既然之后要做成组件，首先页面上就得有个按钮，然后弹出选择地址的弹框，页面上得有一个显示选择完地址的标签。
&#160;&#160;&#160;&#160;弹框出来之后，得有几个部分，一个是顶部标题，叉叉按钮用来隐藏，接下来是每级选择完的地址显示在中间，然后就是下面的每级地址列表，并且列表中若有选中的地址则标红，这些都是最基本的。
&#160;&#160;&#160;&#160;我这边做的地址目前只有四级地址，不会有更深层次的地址，所以只有四列。构思好了，大概样子就是下图。
![未弹出选择框](https://upload-images.jianshu.io/upload_images/1062695-a13343faebfa4e38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![弹出选择框](https://upload-images.jianshu.io/upload_images/1062695-32de268d9640bb78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 第二步：构思完之后就是构造页面

&#160;&#160;&#160;&#160;构造页面算是最简单的部分了，就是写html，css了。写成上图的样子。

&#160;&#160;&#160;&#160;因为四级地址列表考虑到多次切换留有缓存的问题，没有做成每切换一次就请求一次，而是做成四个列表，通过隐藏显示的方式来更方便的操作。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=0,minimum-scale=1, maximum-scale=1"/>
    <link rel="stylesheet" href="address.css">
    <script src="jquery-3.2.1.min.js"></script>
</head>
<body>
<div class="btn" onclick="show()">点击选择地址</div>

<div class="show_address" id="product_address_show" onclick="show()">北京市,北京市,朝阳区,朝外街道</div>

<div id="yls_address_choose" style="display: none">
    <div class="yls_address_bg" onclick="hide()"></div>
    <div class="yls_address_main">
        <div class="yls_address_pop_top">
            <div class="yls_address_pop_title">请选择地址</div>
            <div class="yls_address_pop_cancel" onclick="hide()"></div>
        </div>
        <div class="yls_address_pop_main">
            <div class="yls_address_product">
                <div class="yls_address_select" id="yls_address_select">
                    <div class="yls_address_top_address jdshop_alignment_middle">
                        <div id="yls_top_address_1">请选择</div>
                        <div id="yls_top_address_2"></div>
                        <div id="yls_top_address_3"></div>
                        <div id="yls_top_address_4"></div>
                    </div>
                    <div class="yls_address" id="yls_address_1"></div>
                    <div class="yls_address" id="yls_address_2"></div>
                    <div class="yls_address" id="yls_address_3"></div>
                    <div class="yls_address" id="yls_address_4"></div>
                </div>
            </div>
        </div>
    </div>
</div>
</body>
</html>

```

&#160;&#160;&#160;&#160;这个html还是一目了然的，很简单，下面是css样式。

```
body, html {
    -webkit-user-select: none;
    user-select: none
}
html {
    -webkit-text-size-adjust: 100%
}
body {
    line-height: 1.6;
    background-color: #f5f5f9;
    color: #4a4a4a;
    font-size: 14px;
    font-family: Arial, '微软雅黑', Helvetica Neue, Helvetica, sans-serif;
    -webkit-overflow-scrolling: touch;
    overflow-scrolling: touch
}
* {
    margin: 0;
    padding: 0
}
a img {
    border: 0
}
a {
    text-decoration: none;
    -webkit-tap-highlight-color: transparent;
    -webkit-appearance: none
}
@font-face {
    font-weight: 400;
    font-style: normal;
    font-size: 14px;
    font-family: Arial, '微软雅黑', Helvetica Neue, Helvetica, sans-serif
}
input, textarea {
    border: 0;
    outline: 0;
    -webkit-appearance: none;
    -webkit-tap-highlight-color: transparent;
    font-size: inherit;
    color: inherit
}

/*点击选择按钮*/
.btn{
    margin: 0 auto;
    width: calc(50%);
    height: 30px;
    margin-top: 100px;
    background-color: lightcyan;
    line-height: 30px;
    text-align: center;
    font-size: 14px;
}
.alignment_middle{
    -webkit-box-align: center;
    -webkit-align-items: center;
    align-items: center;
    -webkit-box-pack: start;
    -webkit-justify-content: flex-start;
    justify-content: flex-start;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
}
/*显示文字*/
.show_address{
    margin: 0 auto;
    margin-top: 10px;
    width: 80%;
    height: 30px;
    background-color: #fff;
    text-align: center;
    line-height: 30px;
    font-size: 14px;
}

/*背景*/
.yls_address_bg {
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: #000;
    z-index: 500;
    opacity: .3;
    -webkit-transition: opacity .3s;
    transition: opacity .3s;
    touch-action: none
}
/*主弹框*/
.yls_address_main {
    position: fixed;
    left: 0;
    width: 100%;
    bottom: 0;
    background: #fff;
    border-radius: 10px 10px 0 0;
    box-shadow: 0 0 3px #e9e9e9;
    -webkit-transform: translate3d(0, 120%, 0);
    transform: translate3d(0, 120%, 0);
    -webkit-transition: -webkit-transform .3s;
    transition: -webkit-transform .3s;
    transition: transform .3s;
    transition: transform .3s, -webkit-transform .3s;
    z-index: 1001;
    transform: translate3d(0, 0, 0);
}
.yls_address_pop_top {
    text-align: center;
    height: 50px;
    margin-bottom: 5px;
}
.yls_address_pop_title {
    height: 100%;
    line-height: 50px;
}
.yls_address_pop_cancel {
    position: absolute;
    right: 0;
    top: 0;
    width: 50px;
    height: 50px;
}
.yls_address_pop_cancel::before, .yls_address_pop_cancel::after {
    content: '';
    width: 16px;
    height: 1px;
    background: #000;
    display: block;
    position: absolute;
    right: 10px;
    top: 25px;
}
.yls_address_pop_cancel::before {
    transform: rotate(45deg); /*进行旋转*/
}
.yls_address_pop_cancel::after {
    transform: rotate(-45deg);
}
.yls_address_pop_main {

}
.yls_address_product{

}
.yls_address_select{
    height: calc(60vh);
    width: 100%;
    position: relative;
    overflow: hidden;
}
.yls_address_top_address{
    font-size: 12px;
    height: 35px;
    overflow: hidden;
    border-bottom: 1px solid #ddd;
}
.yls_address_top_address>div{
    padding: 5px 5px;
    margin: 0 5px;
    white-space: nowrap;
}
.yls_address_top_address>div.show{
    color: #c91623;
    border-bottom: #c91623 1px solid;
}
.yls_address{
    position: absolute;
    left: 0;
    top: 45px;
    overflow: auto;
    width: 100%;
    height: calc(60vh - 35px);
    -webkit-transform: translate3d(-100%,0,0);
    transform: translate3d(-100%,0,0);
    -webkit-transition: -webkit-transform .3s .2s;
    transition: -webkit-transform .3s .2s;
    transition: transform .3s .2s;
    transition: transform .3s .2s,-webkit-transform .3s .2s;
}
.yls_address p{
    padding: 8px 10px;
    font-size: 14px;
}
.yls_address p.p_show {
    position: relative;
    color: #c91623;
}
.yls_address.show {
    -webkit-transform: translate3d(0,0,0);
    transform: translate3d(0,0,0);
}

```

&#160;&#160;&#160;&#160;css上半部分是一些基本的初始化浏览器样式，下面部分就是地址选择的样式，目前还没加入动态效果，之后优化部分考虑做进去。不过这部分的样式完全可以自己按照自己的喜好做，想做成什么样子就做成什么样子的。

# 第三步 ：逻辑部分

&#160;&#160;&#160;&#160;这个部分算是最重要的，不过在此重申一遍，我做的只是粗糙的版本，逻辑部分是没有经过任何优化的，这个大家可以按照自己优化的方式进行优化，后期我会做成插件形式再写一篇。😂希望看到这篇文章的大家不要嘲笑我写的粗糙，毕竟我写这个也只是给新手看看，不要嫌弃。

### 1.做基本的变量声明工作

```
  /*赋初始值*/
    var startId = [1804,1805,1829,1831];
    var startName = ['北京市','北京市','朝阳区','朝外街道'];

    //最终获取到的值
    var returnStartId = [];
    var returnStartName = [];
```

### 2.做弹出弹框的弹出隐藏方法

```
/*显示地址选择，隐藏地址选择*/
    function show() {
        document.getElementById("yls_address_choose").style.display="";//显示
        var url = 'http://*********/address/areas?areaId=';//这边写你请求接口的url
        initAddress(url,startId,startName);//初始化工作的方法
    }

    /*隐藏地址选择*/
    function hide() {
    //点击隐藏的时候循环将列表所有的show样式去除，重新添加到最后一列上
        for(var i = 1;i < 5;i++){
            var ad = 'yls_top_address_' + i;
            var adid = 'yls_address_' + i;
            $("#"+ad).removeClass('show');
            $("#"+adid).removeClass('show');
            $("#"+ad).show();
            $("#"+adid).show();
        }
        document.getElementById("yls_address_choose").style.display="none";//显示
        //点击隐藏了，就把最终值还原成初始值
        returnStartId = startId;
        returnStartName = startName;
    }
```

### 3.做弹出弹框的初始化工作

```
 //初始化方法
    function initAddress(url,startId,startName) {
        //将我们最后需要获取的值初始化成初始值
        returnStartId = startId;
        returnStartName = startName;
        //初始化请求areaid=0的数据，我用的接口areaid=0的时候是请求省级地址
        request(url,0,1);
    }

     //初始化请求数据部分，
    //thisNum 为areaid变化数值 url是请求url, addressNum是每个address是第几个
    function request(url,thisNum,addressNum){
        var addressUrl = url+ thisNum;
        $.ajax({
            type: 'get',
            url: addressUrl,
            dataType: "json",
            success: function (res) {
                //无常规错误，根绝data数据做具体的业务判断显示
                console.log(res.result);
                //初始化的渲染html
                setHtml(res.result,thisNum,addressNum);
                //循环请求下级地址
                if(addressNum < startId.length){
                    request(url,startId[addressNum-1],addressNum+1);
                    addressNum += 1;
                }
            },
            error: function erryFunction(err) {
                //网络请求错误
                console.log(err);
                console.log('读取失败')
            }
        });
    }

    //初始化渲染页面
    function setHtml(data,thisNum,addressNum) {
        var addressId = 'yls_address_' + addressNum;
        var topAddress = 'yls_top_address_' + addressNum;
        if(addressNum == startId.length){
            $("#"+addressId).addClass('show');
            $("#"+topAddress).addClass('show');
        }
        $("#"+topAddress).text(startName[addressNum-1]);

        //初始化之后选中的地址顶部tab进行添加点击方法，每次点击会做什么事情
        document.getElementById(topAddress).addEventListener("click",function () {
            var a = parseInt(topAddress.substr(topAddress.length-1,1));
            console.log(a);
            for(var i = 1;i < 5;i++){
                var ad = 'yls_top_address_' + i;
                var adid = 'yls_address_' + i;
                if(i != a){
                    $("#"+ad).removeClass('show');
                    $("#"+adid).removeClass('show');
                    if(i > a){
                        $("#"+ad).hide();
                    }
                }else{
                    $("#"+ad).addClass('show');
                    $("#"+adid).addClass('show');
                }
            }

            //tab上选中地址点击的时候，最终选择地址数组数据也要相应的变化
            returnStartId = returnStartId.slice(0,addressNum);
            returnStartName = returnStartName.slice(0,addressNum);
           
        },false);
        $("#"+addressId).empty();//清空当前列表的html
        var html = '';
        //拼接html，为地址列表中每一个地址添加点击方法
        if(data){
            for(var i = 0;i < data.length;i++){
                if(startName[addressNum-1] == data[i].name){
                    html += '<p class="p_show" id="add_'+data[i].areaId+'" onclick="chooseDetail('+data[i].areaId+','+addressNum+',&quot;'+data[i].name+'&quot;)">'+data[i].name+'</p>';
                }else{
                    html += '<p id="add_'+data[i].areaId+'" onclick="chooseDetail('+data[i].areaId+','+addressNum+',&quot;'+data[i].name+'&quot;)">'+data[i].name+'</p>';
                }
            }
        }
        //添加html到列表上渲染页面
        $("#"+addressId).append(html);
    }

```

### 4.每次点选地址之后的具体逻辑

```
     //点击列表中详细地址的方法
    function chooseDetail(areaId,addressNum,name) {
        var url = 'http://******/address/areas?areaId=';
        var adid = 'add_'+areaId;
        var adid1 = 'yls_address_' + addressNum;
        //选中的数据加上标红显示
        $("#"+adid1+'>p').removeClass('p_show');
        $("#"+adid).addClass('p_show');

        //点击具体地址之后，将最终选择地址数组中相应位置的数据替换掉
        returnStartId[addressNum-1] = areaId;
        returnStartName[addressNum-1] = name;
        
        //判断是否是第四级地址，如果是第四级地址，把获取到的数据渲染到页面上，并且隐藏弹框
        //如果是第四级地址，发送请求获取下一级的数据进行渲染页面
        if(addressNum == 4){
            var topAddress1 = 'yls_top_address_' + addressNum;
            $("#"+topAddress1).text(name);
            $("#product_address_show").text(returnStartName);
            hide();
        }else{
            newrequest(url,areaId,addressNum,name);
        }
    }


    //点击列表里具体地址，发送新的请求获取下一级数据
    //thisNum 为areaid变化数值 url是请求url, addressNum是每个address是第几个，name是你选择的地址具体名称
    function newrequest(url,thisNum,addressNum,name){
        var addressUrl = url+ thisNum;
        $.ajax({
            type: 'get',
            url: addressUrl,
            dataType: "json",
            success: function (res) {
                //无常规错误，根绝data数据做具体的业务判断显示
                console.log(res.result);
                //获取到数据之后渲染下一级列表
                setHtml2(res.result,thisNum,addressNum,name);
            },
            error: function erryFunction(err) {
                //网络请求错误
                console.log(err);
                console.log('读取失败')
            }
        });

    }

    //选择详细地址请求获取数据之后渲染方法
    function setHtml2(data,thisNum,addressNum,name) {
        //获取到相应的div
        var adid1 = 'yls_address_' + addressNum;
        var adid2 = 'yls_address_' + (addressNum+1);
        var topAddress1 = 'yls_top_address_' + addressNum;
        var topAddress2 = 'yls_top_address_' + (addressNum+1);

        //清空相应的下一级div和下一级tab上选中的地址，填入新的数据
        $("#"+topAddress2).empty();
        $("#"+adid2).empty();
        $("#"+topAddress2).addClass('show');
        $("#"+topAddress2).show();
        $("#"+topAddress1).removeClass('show');
        $("#"+adid2).addClass('show');
        $("#"+adid1).removeClass('show');
        $("#"+topAddress2).text('请选择');
        $("#"+topAddress1).text(name);
        var html = '';
        if(data){
            //拼接html，并且添加上新的详细地址选择方法
            for(var i = 0;i < data.length;i++){
                html += '<p id="add_'+data[i].areaId+'" onclick="chooseDetail('+data[i].areaId+','+(addressNum+1)+',&quot;'+data[i].name+'&quot;)">'+data[i].name+'</p>';
            }
        }
        $("#"+adid2).append(html);
    }

```

&#160;&#160;&#160;&#160;以上三步就完成了地址选择粗糙版的构造，其中用来请求数据的方法写了两个，拼接html渲染页面的方法也写了两个，这些是有点冗余的，这个部分之后可以合成一个。
&#160;&#160;&#160;&#160;目前优化的构思是有，不过还没来得及实施，得找时间做一下，优化的思路是其中的方法先进行相应的优化，最后集成成一个对象，在页面调用的时候只要把初始化数据传入，最后加上一个回调方法，将选择完成的地址返回回来，这样整个页面也呢个简洁很多，调用方法也简单了，使用起来的话会方便很多。
&#160;&#160;&#160;&#160;因为目前我写的vue的单页面应用，用到keepAlive，所以现有的控件总是出问题，等这两天优化完进行测试下，到时候在写一篇简单点的文章发布出来。
&#160;&#160;&#160;&#160;能看到这里的，大家要是有什么问题的可以留言跟我讨论讨论，有啥好的想法也可以跟我沟通，大家一起学习。

以上















