## winform FormBordStyle=none 及 wpf FormBordStyle=none 的鼠标点击移动问题

---

winform：

```c#
private Point mPoint;

/// <summary>
        /// 鼠标按下
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void panel1_MouseDown(object sender, MouseEventArgs e)
        {
            mPoint = new Point(e.X, e.Y);
        }

        /// <summary>
        /// 鼠标移动
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void panel1_MouseMove(object sender, MouseEventArgs e)
        {
            if (e.Button == MouseButtons.Left)
            {
                this.Location = new Point(this.Location.X + e.X - mPoint.X, this.Location.Y + e.Y - mPoint.Y);
            }
        }
 

 

 

//bool formMove = false;//窗体是否移动
//Point formPoint;//记录窗体的位置

private void Login_MouseDown(object sender, MouseEventArgs e)
{
//formPoint = new Point();

//int xOffset;
//int yOffset;
//if (e.Button == MouseButtons.Left)
//{
// xOffset = -e.X - SystemInformation.FrameBorderSize.Width;
// yOffset = -e.Y - SystemInformation.CaptionHeight - SystemInformation.FrameBorderSize.Height;
// formPoint = new Point(xOffset, yOffset);
// formMove = true;//开始移动
//}
}

private void Login_MouseMove(object sender, MouseEventArgs e)
{
//if (formMove == true)
//{
// Point mousePos = Control.MousePosition;
// mousePos.Offset(formPoint.X, formPoint.Y);
// Location = mousePos;
//}
}

private void Login_MouseUp(object sender, MouseEventArgs e)
{
//if (e.Button == MouseButtons.Left)//按下的是鼠标左键
//{
// formMove = false;//停止移动
//}
}
```

wpf的移动：

```c#
private void Login_MouseDown(object sender, MouseEventArgs e)
{
//if (e.LeftButton == MouseButtonState.Pressed)
//{
// DragMove();
//}

}
```

