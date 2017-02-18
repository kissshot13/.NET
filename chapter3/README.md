## 窗体常用事件
  - Load： 窗体运行完成即为加载。
  - click：点击事件
  - doubleclick：双击事件
  - KeyDown：按键按下的时候
  - KeyPress：按下并释放的时候触发
  - keyUp：按键抬起
  - MouseClick：鼠标的点击
  - MouseDoubleClick:鼠标双击
  - MouseDown：鼠标按下
  - MouseHover：鼠标光标悬停
  - MouseMove：鼠标移动
  - MosueUp：鼠标抬起
  
  
## 窗体添加控件

 - 拖拉拽用鼠标在工具中拖拽
 - 或者生产一个控件使用`窗口名.Controls.Add(控件名)`
 
```
private void Form1_Load(object sender, EventArgs e)
        {
            this.Text = "first application";
            Button button1 = new Button();
            button1.Text = "点我啊";
            button1.BackColor = Color.Blue;
            this.Controls.Add(button1);

        }
```


 - 窗体的显示：.show()
 - 窗体的隐藏：.hide()
 

```
 private void button1_Click(object sender, EventArgs e)
        {
            Form2 frm = new Form2();
            frm.Show();
            this.Hide();
        }
```

