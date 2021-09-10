## winform MD5加密

---

```c#
byte[] result = Encoding.Default.GetBytes(this.tbPass.Text.Trim());    //tbPass为输入密码的文本框
MD5 md5 = new MD5CryptoServiceProvider();
byte[] output = md5.ComputeHash(result);
this.tbMd5pass.Text = BitConverter.ToString(output).Replace("-","");  
```

