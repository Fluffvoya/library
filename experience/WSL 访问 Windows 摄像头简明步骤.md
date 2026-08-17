

---

**1. Windows 端（管理员 PowerShell）**  
```powershell
winget install usbipd                      # 安装
usbipd list                                # 查看摄像头 BusID（如 2-1）
usbipd bind --busid 2-1                    # 绑定
```

**2. WSL 中直接以 `sudo` 运行程序（无需手动挂载）**  
```bash
sudo ./build/cv_learn
```

> 若提示权限不足，已通过 `sudo` 临时提权解决。

**3. 画面花屏（绿色方块）——设置格式**  
在代码中添加：
```cpp
video.set(cv::CAP_PROP_FOURCC, cv::VideoWriter::fourcc('M','J','P','G'));
```

**4. 需要镜像显示时，使用 `cv::flip`**

```cpp
cv::flip(frame, frame, 1);   // 水平翻转（照镜子效果）
```


---

搞定！🎉