# Unity中使用Sprie.pdf.dll以及将Python环境集成 实现PDF文件的读取查看以及搜索特定文字高亮部分,以及图片对比功能 Word转化Pdf功能
## 准备： Unity 2022.3.55编辑器, Rider2023IDE，Plugins：Spire.Doc.dll，Spire.License.dll，Spire.Pdf.dll，Microsoft.mshtml.dll， Python3.11以及各种依赖库
最好是虚拟环境中下载所有的依赖库：
python -m venv myenv
myenv\Scripts\activate
pip install fitz Pillow numpy opencv-python scikit-image scipy imagehash
## 具体步骤：
### 1. 从官网下载Sprie.pdf，配置Python虚拟环境，下载py脚本中所有的依赖库，我这里使用的Python3.11,版本，实测3.12兼容性有问题所以回退了一个版本 
### 2. 将Python解释器和脚本放置到Unity的StreamingAssets文件夹下，创建一个PythonEnv文件夹， 里面存放Python解释器以及所需要的依赖库 这里我安装的是Python311版本，本地默认路径在C:\Users\Administrator\AppData\Local\Programs\Python\Python311， 需要将python.exe，lib dlls tcl这几个文件一同拷贝到PythonEnv目录下

### 3. 在Unity中编写一个Python启动入口，列入PythonRoot，在这个文件中 设置Python的解释器路径以及Python脚本的路径，并添加一些调试信息便于查看py的信息

        public void OnButtonClick(string scriptPath)
        {
            // 获取 Python 解释器和脚本的绝对路径
            string pythonExeFullPath = Path.Combine(Application.streamingAssetsPath, "python.exe");
            string scriptFullPath = Path.Combine(Application.streamingAssetsPath, scriptPath);

            // 设置工作目录
            string workingDirectory = Path.GetDirectoryName(pythonExeFullPath);

            // 创建进程启动信息
            ProcessStartInfo startInfo = new ProcessStartInfo
            {
                FileName = pythonExeFullPath,
                Arguments = $"\"{scriptFullPath}\"",
                WorkingDirectory = workingDirectory,
                UseShellExecute = false,
                RedirectStandardOutput = true,
                RedirectStandardError = true,
                CreateNoWindow = true
            };

            // 启动进程
            using (Process process = new Process { StartInfo = startInfo })
            {
                process.Start();
                // 读取标准输出
                string stdout = process.StandardOutput.ReadToEnd();
                // 读取标准错误
                string stderr = process.StandardError.ReadToEnd();

                process.WaitForExit();

                // 输出日志
                // Debug.Log("Standard Output:");
                Debug.Log(stdout);

                // Debug.LogError("Standard Error:");
                Debug.LogError(stderr);

                if (process.ExitCode != 0)
                {
                    Debug.LogError($"Python script exited with code {process.ExitCode}");
                }
            }
            






补充几张Unity运行时的图片
![image](https://github.com/SlienceLove/UnityPdfTools/blob/main/Src/9c3f12fa-44de-4770-a4aa-8511af36421f.png)
![image](https://github.com/user-attachments/assets/c2c623e9-ef24-4627-b4e0-2cf2ff328347)
![image](https://github.com/user-attachments/assets/8c5d28cb-2dca-4f1c-b374-d33fa0635ce3)
![Uploading image.png…]()
