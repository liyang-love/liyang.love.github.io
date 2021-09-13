## SQL创建数据库、增删查改

```c#
//连接SQLite数据库源码（C#）
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;
using System.Data.SQLite;

namespace SQLite数据库连接
{

    public partial class Form1 : Form
    {
        public Form1()
        {
            //InitializeComponent();
        }
        //函数：访问数据库，将数据返回到表格中
        private DataTable GetReaderSchema(string tableName, SQLiteConnection connection)
        {
            DataTable schemaTable = null;
            IDbCommand cmd = new SQLiteCommand();
            cmd.CommandText = string.Format("select * from [{0}]", tableName);
            cmd.Connection = connection;
            using (IDataReader reader = cmd.ExecuteReader(CommandBehavior.KeyInfo | CommandBehavior.SchemaOnly))
            {
                schemaTable = reader.GetSchemaTable();
            }
            return schemaTable;
        }
        //连接数据库并获取数据

        private void button1_Click(object sender, EventArgs e)
        {
            string datasource = "d:/1.sqlite";
            //string connectionString = "Data Source=" + datasource + ";Pooling=true;FailIfMissing=false;read only=true";  
            SQLiteConnection.CreateFile(datasource);
            //连接数据库
            SQLiteConnection conn = new SQLiteConnection();
            SQLiteConnectionStringBuilder connstr = new SQLiteConnectionStringBuilder();
            connstr.DataSource = datasource;
            //connstr.Password = "admin";//设置密码，SQLite ADO.NET实现了数据库密码保护
            conn.ConnectionString = connstr.ToString();
            conn.Open();
            //创建表
            SQLiteCommand cmd = new SQLiteCommand();
            //string sql = "CREATE TABLE test(username varchar(20),password varchar(20))";  
            string sql = "CREATE TABLE test(username int,password varchar(20))";
            cmd.CommandText = sql;
            cmd.Connection = conn;
            cmd.ExecuteNonQuery();
            //插入数据
            int x = 14710;
            string y = "aabb";
            //sql = "INSERT INTO test VALUES(" + x + ", ' " + y + " ')";
            //三种方法都行
            //sql = "INSERT INTO test VALUES('14710,'b')";  
            sql = string.Format("INSERT   INTO   test   VALUES   ({0}, '{1} ') ", x, y);
            //这个写法更清晰
            cmd.CommandText = sql;
            cmd.ExecuteNonQuery();
            DataTable schemaTable = conn.GetSchema("TABLES");
            //this.dataGridView1.DataSource = schemaTable;  
        }
        //获取字段
        private void button2_Click(object sender, EventArgs e)
        {
            string datasource = "d:/1.sqlite";
            //连接数据库
            SQLiteConnection conn = new SQLiteConnection();
            SQLiteConnectionStringBuilder connstr = new SQLiteConnectionStringBuilder();
            connstr.DataSource = datasource;
            //connstr.Password = "admin";
            //设置密码，SQLite ADO.NET实现了数据库密码保护
            conn.ConnectionString = connstr.ToString();
            conn.Open();
            DataTable table = conn.GetSchema("TABLES");
            if (table != null && table.Rows.Count > 0)
            {
                string tableName = table.Rows[0]["TABLE_NAME"].ToString();
                DataTable schemaTable = GetReaderSchema(tableName, conn);
                //this.dataGridView1.DataSource = schemaTable;
            }
            MessageBox.Show("GOOD!");
        }
    }
}
```
