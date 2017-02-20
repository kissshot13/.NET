##pictureBox
 - 可以显示来自位图、图标或者元文件。以及来自增强的元文件、JPEG、GIF文件的图形。如果控件不足以显示整幅图像。则裁剪图像以适应控件的大小（sizeMode属性）
 - normal 裁剪图片
 - stretchIamge 图片调整大小适应控件
 - AutoSize 控件适应图片
 - centerImag 图片处于控件中心
 - zomm 图片调整大小适应控件。控件宽高比不变
 -  代码获取image图片`pictureBox1.Image = Image.FromFile(@"文件路径")`；
 
##ImageList控件
 - 又叫存储图像控件。用来存储图像的组合。但它不能显示图片。要显示它存储的图像需要借助第二个控件
 - 第二个控件是包含Imagelsit属性的控件。这个属性一般和ImageIndex属性一起使用。ImageList属性设置为ImageList组件的一个实例。ImageIndex就是在ImageList中的一个索引
 
- 实例


```
private void button1_Click(object sender, EventArgs e)
        {
            Image img1 = Image.FromFile(@"C:\Users\Administrator.7XKGTYE0VSX85WC\Desktop\2\DSC_0009.jpg");
            Image img2 = Image.FromFile(@"C:\Users\Administrator.7XKGTYE0VSX85WC\Desktop\2\DSC_0010.jpg");
            Image img3 = Image.FromFile(@"C:\Users\Administrator.7XKGTYE0VSX85WC\Desktop\2\DSC_0011.jpg");
            imageList1.Images.Add(img1);
            imageList1.Images.AddRange(new Image[] { img2,img3});
            //imageList1.Images有三个add image的方法
            //1.Add()；加一张图片
            //2.AddRange();加一个数组图片
            //3.AddStrip()
            Graphics ghc = Graphics.FromHwnd(this.Handle);//创建一个窗口句柄
            for(var i=0;i< imageList1.Images.Count;i++)
            {
                imageList1.Draw(ghc, new Point(50, 50), i);
                //Draw（）也有三种重载的方法
                //1.Draw（gra，左上角点，索引）
                //2.Draw（gra，水平坐标，垂直坐标，索引）
                //3.Draw（gra，水平坐标，垂直坐标，图像宽，图像长，索引）
                System.Threading.Thread.Sleep(1000);//进程停止1秒。因为放的太快了
            }
            imageList1.Images.Clear();
        }
```


