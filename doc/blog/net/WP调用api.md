## [WP调用api](https://www.cnblogs.com/liyangLife/p/3754392.html)

---

```c#
private string GetText()
{
            string resultString = string.Empty;
            HttpWebRequest request =                    HttpWebRequest.CreateHttp("http://www.baidu.com");
            request.Method = "GET";
            request.BeginGetResponse((IAsyncResult result) =>
                {
                    HttpWebRequest webRequest = result.AsyncState as HttpWebRequest;
                    HttpWebResponse webResponse = (HttpWebResponse)webRequest.EndGetResponse(result);
                    Stream streamResult = webResponse.GetResponseStream();
                    StreamReader reader = new StreamReader(streamResult);

                    //获取的返回值
                    resultString = reader.ReadToEnd();
                    streamResult.Close();
                    reader.Close();
                }, request);

            return resultString;
}
```

