## winform 验证用户正确后打开新窗口时关闭登陆窗口

---



在program.cs中

```c
      Login login=new Login();
      if( login.ShowDialog()==DialogResult.Ok)//注意这里要显示模态对话框
      {
           Application.Run(new 主界面());
      }
 
     然后在登录的登录按钮事件中加上
     this.DialogResult=DialogResult.Ok;
```

