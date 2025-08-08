要将你的Python程序打包成可执行应用，推荐使用 `PyInstaller`，它支持Windows、macOS和Linux系统，且对PyQt应用有良好的兼容性。以下是详细步骤：


### **步骤1：安装PyInstaller**
打开命令行，执行以下命令安装：
```bash
pip install pyinstaller
```


### **步骤2：准备打包文件**
确保你的程序文件（例如 `main.py`）能正常运行，且所有依赖库已安装（`pandas`、`PyQt6`、`matplotlib`、`seaborn`、`openpyxl` 等）。


### **步骤3：执行打包命令**
在命令行中切换到程序所在目录，执行以下命令：

#### **基础打包（单文件模式）**
```bash
pyinstaller --onefile --windowed --name "可视化分析工具" main.py
```

#### **参数说明**：
- `--onefile`：打包成单个可执行文件（方便分发）。
- `--windowed`：隐藏控制台窗口（GUI程序必备，避免运行时弹出黑框）。
- `--name "可视化分析工具"`：指定生成的可执行文件名称。
- `main.py`：你的程序入口文件。


### **步骤4：处理可能的问题**
1. **中文显示问题**  
   若图表中的中文显示异常，需确保代码中已设置字体（你的代码中已包含相关设置）：
   ```python
   plt.rcParams["font.family"] = ["SimHei", "Microsoft YaHei", "Arial Unicode MS"]
   plt.rcParams['axes.unicode_minus'] = False
   ```

2. **依赖库缺失**  
   若打包后运行提示缺少某库（如 `openpyxl`），可手动指定包含：
   ```bash
   pyinstaller --onefile --windowed --name "混凝土价格分析工具" --hidden-import openpyxl main.py
   ```

3. **文件路径问题**  
   若程序中使用了相对路径读取文件，打包后需确保文件与可执行文件在同一目录，或使用绝对路径。


### **步骤5：获取可执行文件**
打包完成后，在程序目录会生成：
- `dist` 文件夹：包含最终的可执行文件（如 `混凝土价格分析工具.exe`）。
- `build` 文件夹：临时文件，可删除。
- `*.spec` 文件：打包配置文件，如需高级设置可修改此文件。


### **可选：高级配置（通过.spec文件）**
如果基础命令打包失败，可通过 `.spec` 文件精细化配置：
1. 生成.spec文件：
   ```bash
   pyinstaller --name "混凝土价格分析工具" main.py
   ```
2. 编辑生成的 `混凝土价格分析工具.spec`，添加缺失的依赖或资源文件：
   ```python
   a = Analysis(
       ['main.py'],
       hiddenimports=['openpyxl', 'pandas._libs.tslibs.np_datetime'],  # 补充缺失的依赖
       datas=[],  # 如需添加额外资源文件，格式为 (源路径, 目标路径)
   )
   ```
3. 使用.spec文件打包：
   ```bash
   pyinstaller 混凝土价格分析工具.spec
   ```


### **最终效果**
在 `dist` 文件夹中找到生成的可执行文件，双击即可运行，无需安装Python环境。

如果遇到特定系统的问题（如macOS的权限问题或Linux的库依赖），可以进一步针对性调整。
