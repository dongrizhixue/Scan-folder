# 扫描文件夹中的文件

本例是递归扫描文件夹中文件，并将文件夹的文件信息，全部显示到listview控件中，以下是源代码

/// <summary>
/// 搜索按钮点击事件
/// 执行步骤
/// 1.从“路径”文本框中读取文件夹路径。
/// 2.从文件夹路径中搜索到所有的csv格式文件。
/// 3.显示该路径中的所有文件。
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void btnSeachcsv_Click(object sender, EventArgs e)
{
    try
    {
        //验证“路径”文本框中的路径是否存在
        string path = txtPath.Text;
        if (Directory.Exists(path))
        {
            //搜索“路径”文件夹下面的csv文件
            lvCVSList.Items.Clear();  //清除在控件中显示的csv文件夹路径
            string[] files = Directory.GetFiles(path, "*.csv", SearchOption.AllDirectories);    //从“路径”中搜索所有的csv文件
            SortedList<string, FileInfo> fileList = new SortedList<string, FileInfo>();     //声明一个字典，用于存储csv文件信息
            //验证是否在“路径”中搜索到了csv文件
            if (files.Length > 0)
            {
                //搜索到了csv文件，继续执行

                foreach (string f in files)    //把文件夹中搜索到文件信息全部存储到刚才声明的字典中
                {
                    FileInfo fi = new FileInfo(f);  //根据路径获取文件信息
                    fileList.Add(fi.LastWriteTime.Ticks.ToString() + fi.Name, fi);  //存储文件信息到字典中
                }
                //把存储到文件信息字典中的数据显示到listview中

                foreach (string item in fileList.Keys)
                {
                    ListViewItem lvItem = new ListViewItem(fileList[item].Name);  //初始化一个ListViewItem
                    lvItem.SubItems.Add(fileList[item].DirectoryName);    //加入路径信息列
                    lvItem.SubItems.Add(fileList[item].Length.ToString());   //加入当前文件大小信息列
                    lvItem.SubItems.Add(fileList[item].LastWriteTime.ToString());   //加入当前文件最新修改时间信息列
                    lvItem.SubItems.Add("--");
                    lvItem.Tag = fileList[item].FullName;
                    lvCVSList.Items.Insert(0, lvItem);   //将ListViewItem插入到ListView中
                }
            }
        }
        else
        {
            //没有搜索到csv文件，停止继续执行
            rtxtResult.AppendText("\r\n" + DateTime.Now.ToString() + " 没有找到csv文件，请重新选择目录。");
        }
    }
    catch (Exception ex)
    {
        rtxtResult.AppendText("\r\n" + DateTime.Now.ToString() + " 出现异常：" + ex.Message);
    }
}

