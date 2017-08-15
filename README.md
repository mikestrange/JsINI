# JsINI


 function parseINIString(data){
  var regex = {
    section: /^\s*\[\s*([^]*)\s*\]\s*$/,
    param: /^\s*([\w\.\-\_]+)\s*=\s*(.*?)\s*$/,
    comment: /^\s*;.*$/
  };
  var value = {};
  var lines = data.split(/\r\n|\r|\n/);
  var section = null;
  lines.forEach(function(line){
    if(regex.comment.test(line)){
      return;
    }else if(regex.param.test(line)){
      var match = line.match(regex.param);
      if(section){
        value[section][match[1]] = match[2];
      }else{
        value[match[1]] = match[2];
      }
    }else if(regex.section.test(line)){
      var match = line.match(regex.section);
      value[match[1]] = {};
      section = match[1];
    }else if(line.length == 0 && section){
      section = null;
    };
  });
  return value;
}


var loadXml = function(){
    //解析xml代码
   
    var xmlDom = laya.net.Loader.getRes("../res/config.xml");
     console.log("数据:",xmlDom)
    var jsonDom = laya.net.Loader.getRes("../res/test.ini");
    //console.log("数据:",jsonDom)
    var data = parseINIString(jsonDom)
    for(var val in data){
        console.log("数据:key=",val, ",val=",data[val])
    }
    console.log("数据2:", data['player'])
}

function loadINI(){
    console.log("加载啊")
var res = [{url: "../res/test.ini", type: laya.net.Loader.TEXT},
{url: "../res/config.xml", type: laya.net.Loader.XML}];
Laya.loader.load(res, laya.utils.Handler.create(this, loadXml));
}

loadINI();
