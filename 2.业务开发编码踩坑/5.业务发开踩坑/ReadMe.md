#业务开发踩坑
## 有股word转pdf关键字标红预览之后导致文字错版重叠 
场景:***授予书等需要替换变量、用户不想使用系统模板而是使用自己上传的word，为了给用户更好的体验，提示用户自己替换需要改变的关键字为本系统需要使用的变量（eg:{{grant_name}}等），然后直接预览给与变量样例数据看是否满足本系统需求（如是否缺失变量或者打错字等）以及用户模板需求   
坑: 预览需要替换的变量都给与标红提示成功，但是有部分字体与其他字体造成了重叠。  
解决： 经调查与linux的word转pdf命令unoconv无关""sudo unoconv -f pdf wordfile_path""，问题出在python第三方包docx上。总结就是两点：   
    - 1. 每有一段新内容就新起 add_paragraph去填充 add_run内容    
    - 2. add_run注意编码格式,毕竟都是中文

```python 
from docx import Document
from docx.shared import Inches, Pt, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH, WD_LINE_SPACING
from docx.oxml.ns import qn
import re 

demo_dict = {
        "{{company_name}}": lib.dao.company.get_company(company_id)['company_name'],
        "{{grant_name}}": u"张某",
        "{{stockholder}}": u"李某",
        "{{share_num}}": u"10000",
        # ...,
}

doc = Document()
doc.styles['Normal'].font.name = u'宋体'
doc.styles['Normal']._element.rPr.rFonts.set(qn('w:eastAsia'), u'宋体')


split_str = "|".join(["("+x+")" for x in demo_dict.keys()])
text_list = re.split(split_str, temp_text)  
text_list = [x for x in text_list if x]  # 利用re.split正则分割保留要分割的字符，列表中会存在None元素

for text in text_list:
    if text[:2] == '{{' and text[-2:] == "}}":
        replace = doc.add_paragraph()
        replace.paragraph_format.line_spacing = 1.5
        r = replace.add_run(demo_dict[text].decode('utf-8'))
        r.font.size = Pt(12)
        r.bold = True
        r.font.color.rgb = RGBColor(255, 0, 0)
    else:
        context = doc.add_paragraph()
        context.paragraph_format.line_spacing = 1.5
        r = context.add_run(text.decode('utf-8'))
        r.font.size = Pt(12)
```


## pdf电子签章找不到关键字(e签宝)
场景：需要使用e签宝（其它也类似）进行电子签名和盖章，原理就是ocr识别pdf中的关键字进行定位解析然后给与横纵坐标参数x、y进行签字和盖章重新生成pdf。  
坑：pdf人眼观察确实是有关键字的，且关键字没有重叠或者错版什么的，但是e签宝确实无法识别进行签名   
解决： python-docx第三方包在生成word时候给与字体设置"宋体"，对应的签字签章关键字给与加粗展示。   
```
doc = Document()
doc.styles['Normal'].font.name = u'宋体'
doc.styles['Normal']._element.rPr.rFonts.set(qn('w:eastAsia'), u'宋体')
```

## http长链接sse（server-sent event）的onmessage接收不到消息
场景：手机h5端提交签名以后生成图片，然后在pc端要进行一个实时显示预览，确认无误再进行pc端的确认提交以修改个人签名。   
坑：因为对于pc端和后台的交互不涉及pc给服务端发起请求，所以采用sse的方式，但是后端写好以后，前端开发讲onopen触发了但是onmessage没有触发接受到消息   
解决：自己用浏览器直接访问接口确实有不断返回数据，但是onmessage没有触发，经过调研以后发现是sse的数据格式自己加了event的add方式（默认不加的话前端对应onmessage就可以接受到消息）。   
样例demo:   
服务端
```python
import json
import time

from flask import Flask, request
from flask import Response
from flask_cors import CORS, cross_origin

app = Flask(__name__)
# CORS(app, supports_credentials=True)  # 所有路由都允许跨域

def get_message():
    """this could be any function that blocks until data is ready"""
    time.sleep(1)
    s = time.ctime(time.time())
    return json.dumps(['当前时间：' + s , 'a'], ensure_ascii=False)

@app.route('/sse_demo')
@cross_origin(supports_credentials=True)
def sse_demo():
    user_id = request.args.get('user_id', 'LJP')
    print(user_id)
    def eventStream():
        id = 0
        while True:
            id +=1
            print(id)

            # yield 'id: {}\nevent: add\ndata: {}\n\n'.format(id,get_message())
            yield 'id: {}\ndata: {}\n\n'.format(id,get_message())  # 注意此处sse的数据返回中，去掉了event: add

    return Response(eventStream(), mimetype="text/event-stream")


if __name__ == '__main__':
    app.run(host='127.0.0.1', port=5000, processes=4, threaded=True)
```

客户端
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<h1>获取服务端更新数据</h1>
<div id="start"></div>
<div id="status"></div>
<div id="result"></div>

<script>
if(typeof(EventSource)!=="undefined")
{
    document.getElementById("start").innerHTML+="启动连接sse" + "<br>";
	var source=new EventSource("http://127.0.0.1:5000/sse_demo");

	document.getElementById("start").innerHTML+="sse连接状态：" + source.readyState + "<br>";


	source.onopen = function(event) {
	    document.getElementById("start").innerHTML+="sse连接成功" + source.readyState + "<br>";
	    console.log("sse连接成功，触发onopen");
    };


	source.onmessage = function(event) {
        var data = event.data;
        var origin = event.origin;
        var lastEventId = event.lastEventId;
        console.log("sse接收消息成功，触发onmessage");
		document.getElementById("result").innerHTML =event.data + "<br>";
        // handle message
    };

}
else
{
	document.getElementById("result").innerHTML="抱歉，你的浏览器不支持 server-sent 事件...";
}
</script>

</body>
</html>

```