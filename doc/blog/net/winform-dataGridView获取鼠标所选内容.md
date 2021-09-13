## winform dataGridView获取鼠标所选内容

```c#
 dataGridView1.Rows[dataGridView1.CurrentCell.RowIndex].Cells[0].Value.ToString();
```
