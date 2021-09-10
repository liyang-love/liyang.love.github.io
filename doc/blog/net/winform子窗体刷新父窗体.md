## winform子窗体刷新父窗体

---

利用委托

父窗体：

```c
    ///弹出子窗体方法
    private void addConfig_Click(object sender, EventArgs e)
    {
      AddConfig addconfig = new AddConfig();
      addconfig.ShowUpadte += new AddConfig.UpDate(LoadData);
      addconfig.Show();
    }

///加载gridview
public void LoadData()
    {
      DataTable dt = SQLiteHelp.GetData("Select name,connString,providerName From   [connectionStrings]");
      dataGridView1.DataSource = dt;
    }
```





子窗体：

```c#
public delegate void UpDate();//委托
public event UpDate ShowUpadte;//事件

//执行刷新方法

ShowUpadte();
```

