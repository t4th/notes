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

cJson: generate and parse json file

```json
{
	"status":	"ok",
	"stuff":	[{
			"entry0":	0,
			"entry1":	0,
			"entry2":	0,
			"entry3":	3
		}, {
			"entry0":	0,
			"entry1":	0,
			"entry2":	0,
			"entry3":	0
		}, {
			"entry0":	0,
			"entry1":	0,
			"entry2":	0,
			"entry3":	0
		}]
}
```

```c
#define MAX 5
typedef struct
{
    int carray_size;

    struct
    {
        int entry0;
        int entry1;
        int entry2;
        int entry3;
    } data[MAX];
} type_t;

static void generateJson(type_t * input, char** output)
{
    cJSON* json = cJSON_CreateObject();
    cJSON* status = cJSON_AddStringToObject(json, "status", "ok");
    cJSON* array = cJSON_CreateArray();

    cJSON_AddItemToObject(json, "stuff", array);

    for (int i = 0; i < input->carray_size; ++i)
    {
        cJSON* entry = cJSON_CreateObject();
        cJSON_AddNumberToObject(entry, "entry0", input->data[i].entry0);
        cJSON_AddNumberToObject(entry, "entry1", input->data[i].entry1);
        cJSON_AddNumberToObject(entry, "entry2", input->data[i].entry2);
        cJSON_AddNumberToObject(entry, "entry3", input->data[i].entry3);

        cJSON_AddItemToArray(array, entry);
    }

    *output = cJSON_Print(json);

    cJSON_Delete(json);
}

static void readJson(char* input, type_t* output)
{
    cJSON* info = cJSON_Parse(input);

    cJSON* status = cJSON_GetObjectItemCaseSensitive(info, "status");

    cJSON* loads_array = cJSON_GetObjectItemCaseSensitive(info, "stuff");

    if (!cJSON_IsArray(loads_array))  printf("error");

    output->carray_size = cJSON_GetArraySize(loads_array);

    for (int i = 0U; i < cJSON_GetArraySize(loads_array); i++)
    {
        cJSON* array_item = cJSON_GetArrayItem(loads_array, (int)i);

        cJSON* entry0 = cJSON_GetObjectItemCaseSensitive(array_item, "entry0");

        output->data[i].entry0 = entry0->valueint;

        cJSON* entry1 = cJSON_GetObjectItemCaseSensitive(array_item, "entry1");

        output->data[i].entry1 = entry1->valueint;

        cJSON* entry2 = cJSON_GetObjectItemCaseSensitive(array_item, "entry2");

        output->data[i].entry2 = entry2->valueint;

        cJSON* entry3 = cJSON_GetObjectItemCaseSensitive(array_item, "entry3");

        output->data[i].entry3 = entry3->valueint;
    }

    cJSON_Delete(info);
}

int main()
{
    type_t in;
    in.carray_size = 3;
    in.data[0].entry0 = 1;
    in.data[1].entry1 = 2;
    in.data[2].entry2 = 3;
    in.data[3].entry3 = 4;
    char* p;

    generateJson(&in, &p);

    type_t out;
    readJson(p, &out);

    cJSON_Delete(p);

    return 0;
}
```
