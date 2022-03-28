Title: jquery-ajax
Published: 03/20/2022
Tags:
    - ajax
    - jquery
---

XMLHttpRequest object
************************

the most fundamental building block of Ajax is the XMLHttpRequest object

```
    function get() {
        let req = new XMLHttpRequest();
        req.onreadystatechange = function () {
        console.log(this);
        };
        req.open("GET", URL);
        req.send();
    }
```

post request

```shell
function postRequest() {
        let postdata = getFromInput();
        let req = new XMLHttpRequest();
        // When the entire request fails it is probably a network error
        req.onerror = function () {
          
        };

        req.onreadystatechange = function () {
            if (this.readyState === XMLHttpRequest.DONE && this.status === 201) {
              var data = JSON.parse(this.response);
            }
            else if (this.readyState === XMLHttpRequest.DONE && this.status >= 400) {
            // Check for error
            
            }
        };

        req.open("POST", URL);
        req.setRequestHeader("Content-Type","application/json");
        req.send(JSON.stringify(postdata));
      }
```

Fetch API
*******************
The very basic fetch api looks like below

```
    functionget(){
        fetch(URL)
        .then(response=>response.json())
        .then(data=>console.log(data));
    }
```

post using fetch api

```
    function insertData() {
        
        let postdata = getFromInput();

        let options = {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(postdata)
        };

        fetch(URL, options)
          .then(response => response.json();)
          .then(data => {
            console.log(data)
          })
          .catch(error => console.log(error));
      }
```

Jquery Ajax with ASP.net core
**********************************

**contentType**
  contentType (default: 'application/x-www-form-urlencoded; charset=UTF-8')

  Type: String

  When sending data to the server, use this content type. Default is "application/x-www-form-urlencoded; charset=UTF-8", which is fine for most cases. If you explicitly pass in a content-type to $.ajax(), then it'll always be sent to the server (even if no data is sent). If no charset is specified, data will be transmitted to the server using the server's default charset; you must decode this appropriately on the server side.

**data type**

  dataType (default: Intelligent Guess (xml, json, script, or html))

  Type: String

  The type of data that you're expecting back from the server. If none is specified, jQuery will try to infer it based on the MIME type of the response (an XML MIME type will yield XML, in 1.4 JSON will yield a JavaScript object, in 1.4 script will execute the script, and anything else will be returned as a string).

**Ajax Get Example**

```
$.ajax({
        url: URL,
        dataType: 'JSON',
        contentType: "application/json",
        type: "GET",
        data: { name: name },
        success: function (response) {
            console.log(response);
        },
        error: function (ex) {
            console.error(ex);
        }
    });
```

**asp.net backend**

```
  [HttpGet]
  public IActionResult GetInfoByName(string name)
  {
    try
    {
      .....
      return Json(new { data = response, result = CommonAjaxResponse(MessageType.success, "", ResponseCode.success) });
    }
    catch (Exception ex) when (ex is Exception)
    {
      ....
      return Json(new { result = CommonAjaxResponse(MessageType.error, ex.Message, ResponseCode.error) });
    }
  }
```
# Ajax Post Example

```
$.ajax({
        type: "POST",
        url: url,
        data: JSON.stringify(obj),
        dataType: 'JSON',
        contentType: "application/json",
        success: function (response) {
            console.log(response)
        },
        error: function (request, status, error) {
            console.error(error);
        }
      });
```


```
  [HttpPost]
      
  public JsonResult SaveData([FromBody] DataModelDto data)
  {
      try
      {
        ................
        ................
        return Json(new { result = CommonAjaxResponse(result.MessageType, result.Message, result.ResponseCode), data = dataItem });
      }
      catch (Exception exception)
      {
        return Json(new { result = CommonAjaxResponse(MessageType.error, exception.Message, ResponseCode.error) });
      }
  }
```

```
var obj = $("form#formid").serialize();
$.ajax({
        url: '/Controller/Update',
        type: 'post',
        dataType: 'json',
        data: obj,
        success: function (response) {
          console.log(response);
        },
        error: function (xhr) {
          console.error(xhr)
        }
    });
```

```
[HttpPost]
public IActionResult Update(UpdateModelDto model)
{
    try
    {
      Response result = ......................
      ........................................
      return Json(new { result = CommonAjaxResponse(result.MessageType, result.Message, result.ResponseCode) });
    }
    catch (Exception ex) when (ex is { } exception)
    {
      return Json(new { result = CommonAjaxResponse(MessageType.error, ex.Message, ResponseCode.error) });
    }
}
```
```
var obj = {};
obj.FromDate = $("#txtFromDate").val();
obj.ToDate = $("#txtToDate").val();
$.ajax({
        type: "POST",
        url: "/Controller_Name/LoadData",
        data: obj,
        dataType: 'JSON',
        contentType: "application/x-www-form-urlencoded; charset=UTF-8",
        success: function (response) {
            LoadAssignStatSuccessData(response.data);
        },
        error: function (xhr) {
            ShowMessage('error', xhr.innerText);
        }
  });
```


```
[HttpPost]
[CustomAuthorize(IsCheck = false)]
public IActionResult LoadData(DataViewModel model)
{
  try
  {
    DateTime fromDate = Convert.ToDateTime(model.FromDate);
    DateTime toDate = Convert.ToDateTime(model.ToDate);
    .................
    return Json(new { data = StatusEntityList, result = CommonAjaxResponse(MessageType.success, "", ResponseCode.success) });
  }

  catch (Exception exception) when (exception is Exception ex)
  {
    return Json(new { result = CommonAjaxResponse(MessageType.error, ex.Message, ResponseCode.error) });
  }
}
```


```
$('#vendorTable').on('click', '.btnEditDetails', function () {
        $('#btnSaveVendor').show();
        $('.modal-body').load(`/Vendor/GetEditVendorDetails?id=${$(this).data('id')}`, function (response, status, xhr) {
            if (status == "error") {
                var msg = "Sorry but there was an error: ";
                ShowMessage("error", msg);
            }
            else {
                $('#modalVendor').modal('show');
            }
        });
        
    });
```

```
public PartialViewResult GetEditVendorDetails(int id)
{
    if (id <= 0)
    {
        return PartialView("_error");
    }
    try
    {
        var vendor = _vendorService.GetVendorById(id);
        if (vendor == null || vendor.Id == 0)
        {
            return PartialView("_error");
        }
        return PartialView("_EditVendorPartials", vendor);
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "{ProcessName} {Channel} Message: " + ex.Message, ControllerContext.ActionDescriptor.ControllerName, "web");
        return PartialView("_error");
    }
}
```

