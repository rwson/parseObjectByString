# parseObjectByString

根据相关字符串取得javascript对象中的属性并且计算出结果

在很多javascript的MVVM框架中,用的最多的就是指令,通过在html上指定指令对应的属性值为一个字符串时,完成数据的双向绑定

比如在angularjs(1.x)中的部分指令

    ng-bind="text"  
    
    ng-repeat="item in array"
    
    ng-hide="item.length == 0"
    
    //  ...等等

想想其中原理,个人觉得可以通过把字符串拆分成多块,然后尝试给该字符串前面加上"this."来修改其作用对象,最后把拆分并且经过处理的字符串组合在一起,通过

    new Fucntion("return ...").call(obj);
    
来求得计算结果并返回

举个🌰：

    //  定义一个对象,下面有这么些属性和值

        var obj = {
        "attrNum": 1,
        "attrNum2": 3,
        "attrBool": true,
        "attrStr": "str",
        "attrArr": [1, 2, 3],
        "attrObj": {
            "key": "val",
            "key2": "val2"
        }
    };
    
    //  然后定义一些如下的字符串
    
    var str1 = "attrNum";
    var str2 = "attrNum2";
    var str3 = "attrNum + attrNum2";
    var str4 = "attrNum2 == attrNum";
    var str5 = "attrNum == attrNum2 ? 'true': 'false'";
    var str6 = "attrNum2 != 2 ? true : false";

    var str7 = "!attrBool";

    var str8 = "attrStr != 'str'";
    var str9 = "attrStr.length";

    var str10 = "attrArr";
    var str11 = "attrArr.length";
    var str12 = "attrArr['length']";
    var str13 = "attrArr.length == attrNum2";
    var str14 = "attrArr['length'] != attrNum2";

    var str15 = "attrObj.key";
    var str16 = "attrObj['key2']";
    var str17 = "attrObj['key2'] == attrObj.key";

    var str18 = "attrNum + attrNum2";
    var str19 = "attrNum + attrNum2 * 3";
    var str20 = "(attrNum + attrNum2) * 3";
    var str21 = "attrNum + attrNum2 > 3 ? 'true': 'false'";
    var str22 = "(attrNum + attrArr['length']) < 3 ? true: false";
    
    //  然后执行下面的for循环
    for (var i = 1; i <= 25; i++) {
    	var variable = window["str" + i];
    	if(variable !== undefined) {
	        console.group("str" + i + "'s execute result:");
	        console.log("orignal expression is: " + window["str" + i]);
	        console.log(exec(window["str" + i], obj));
	        console.groupEnd();
    	}
    }
    
进入浏览器控制台,将会看到下面的运行结果
    
![1-7](1.png)

![8-14](2.png)

![15-22](3.png)
