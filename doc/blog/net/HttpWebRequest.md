## HttpWebRequest

---

```c#
HttpWebRequest request;
HttpWebResponse response;
ASCIIEncoding encoding = new ASCIIEncoding();
request = WebRequest.Create(postUrl) as HttpWebRequest;
byte[] b = encoding.GetBytes(postUrl);
request.UserAgent = "Mozilla/4.0";
request.Method = "PUT";
request.ContentLength = b.Length;
using (Stream stream = request.GetRequestStream())
{
stream.Write(b, 0, b.Length);
}
try
{
//获取服务器返回的资源
response = request.GetResponse() as HttpWebResponse;
using (StreamReader reader = new StreamReader(response.GetResponseStream(), Encoding.UTF8))
{
string result = reader.ReadToEnd();
if (result.Trim().Length == 0)
{
textBox2.Text = "执行成功,但未检测到任何返回信息！";
}
else
{
textBox2.Text = result;
}
}
}
catch (WebException ex)
{
textBox2.Text = ex.ToString();
}

 

 

HttpWebRequest request;
HttpWebResponse response;
request = WebRequest.Create(postUrl) as HttpWebRequest;
request.Method = "GET";
request.UserAgent = "Mozilla/4.0";
request.KeepAlive = true;
response = (HttpWebResponse)request.GetResponse();
StreamReader reader = new StreamReader(response.GetResponseStream(), Encoding.UTF8);
string result = reader.ReadToEnd();
if (result.Trim().Length == 0)
{
textBox2.Text = "执行成功,但未检测到任何返回信息！";
}
else
{
textBox2.Text = result;
}
```

