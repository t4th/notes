# notes

```
setjump "push"
longjump "pop"
```
simple ipc
```
Peterson's algorithm
```

js post
```javascript
    var data0 = document.getElementById("data0").value;
    var data1 = document.getElementById("data1").value;
    var params = `inverter=${data0}&inverter-sn=${data1}`
    
    var req = new XMLHttpRequest();
    req.open("POST", "custom.cgi");
    req.onload = function()
    {
        var data = JSON.parse(req.response);
        if(data.status == "success"){
            console.log("post success");
            location.reload();
        } else {
            console.log("post error");
        }
    }
    req.onerror = function()
    {
        console.log("http post request error");
    }
    req.send(params);
```

js get
```javascript
    var req = new XMLHttpRequest();
    req.open("GET", "custom.cgi");
    req.onload = function()
    {
        var obj = JSON.parse(req.response);
        document.getElementById("data0").value = obj["data0"].value;
        document.getElementById("data1").value = obj["data1"].value;

    }
    req.onerror = function()
    {
        console.log("http get error");
    }
    req.send(null);
```
