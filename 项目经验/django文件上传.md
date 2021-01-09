# django文件上传

来源：军语管理系统的完善。

[ ]ajax文件上传（不到服务器）

[ ] 链接上传（到服务器）

## ajax不传输文件到服务器：

使用模板：

```html
前端：
html
<div class="col-xs-5" style="width:35%">
    <button class="btn btn-white btn-xs" id="upload_csv" data-toggle="modal" data-target="#myModal" onclick="FuncCSVInport()">提交</button>
    <!--点击提交，同时调用myModel代码块，同时触发事件，FuncCSVInport()-->
    <!--                        <button class="btn btn-white btn-xs" id="upload_csv" data-toggle="model" data-target="#myModal">提交 </button>-->
    <form id="uploadForm" enctype="multipart/form-data">
        <input class="csv_file" id="upFile" name="csv_file" multiple=""  type="file" onchange="readCSVFile(this)"></input>
    <!--文件改变的时候调用readCSVFile函数-->
    </form>
</div>

<!--#myModal代码块：-->
                   <!--弹出确认上传弹框-->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true" >
    <div class="modal-dialog"  style="width:800px;height:auto">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                <h4 class="modal-title" id="myModalLabel">确认上传这个文件吗？</h4>
            </div>
            <div class="modal-body">
                <table id="upcontent" border="1">
                </table>
                <div style="margin-top:30px;">..............</div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-xs btn-white" data-dismiss="modal">取 消</button>
                <button type="button" class="btn  btn-xs btn-danger" id="weapon-confirm-btn" data-dismiss="modal" onclick="upload()">上传</button>
                <!--点击上传后电泳upload函数传送数据-->
            </div>
        </div><!-- /.modal-content -->
    </div><!-- /.modal -->
</div>

```

js代码块：

```javascript
let data;
#设定全局变量供下列两个函数使用
 function readCSVFile(obj) {
     data="";
     var reader = new FileReader();
     reader.readAsText(obj.files[0],'gbk');
     reader.onload = function () {
         data = this.result.split("\r\n");
         data = data.slice(0,4);//data为csv转换后的对象
         console.log(data);
    }
 }

 function FuncCSVInport() {
      $("#upcontent").empty();
      for(let i=0;i<data.length;i++){
        let tr1=document.createElement('tr');
        let td02=document.createElement('td');
        td02.innerText=i.toString();

        tr1.appendChild(td02);
        document.getElementById('upcontent').appendChild(tr1);

        let list = data[i].split(',');
        for (let j=0;j<6;j++){
            let td2=document.createElement('td');
            td2.innerText=subStrFormat(list[j],25);
            tr1.appendChild(td2);
        }
     }
 }

```

