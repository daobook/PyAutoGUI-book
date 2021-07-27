# 入门

安装：`pip install pyautogui`。

PyAutoGUI 有几个特性：

- 移动鼠标并在其他应用程序的窗口中单击或输入。
- 向应用程序发送击键（keystrokes，例如，填写表单）。
- 截取屏幕截图，并给出图像（例如，按钮或复选框），在屏幕上找到它。
- 定位应用程序窗口，并移动、调整大小、最大化、最小化或关闭它（目前仅限 Windows）。
- 在 GUI 自动化脚本运行时显示用于用户交互的消息框。

关于 PyAutoGUI 能够做什么的一个简单例子是一个机器人自动玩游戏 [Sushi Go Round 的 YouTube 视频](https://www.youtube.com/watch?v=lfk_T6VKhTE)。机器人会观察游戏的应用程序窗口，搜索寿司订单的图像。当它找到一个时，它就点击寿司配料的按钮来完成订单。它还会点击台面收集所有完成的盘子。机器人可以完成所有7天的游戏，尽管许多人类玩家获得比机器人更高的分数。

## 例子

```python
import pyautogui

# 获取主显示器的大小。
screenWidth, screenHeight = pyautogui.size()
# 获取鼠标的 XY 位置。
currentMouseX, currentMouseY = pyautogui.position()
print(currentMouseX, currentMouseY)
# 将鼠标移动到XY坐标
pyautogui.moveTo(100, 150)
# 点击鼠标
pyautogui.click()
# 将鼠标移动到XY坐标并单击它
pyautogui.click(100, 200)
# 找到 button.png 出现在屏幕上的位置并单击它。
pyautogui.click('button.png')
# 从当前位置向下移动鼠标10像素。
pyautogui.move(0, 10)
# 双击鼠标
pyautogui.doubleClick()
# 使用 tweening/easing 函数移动鼠标超过2秒。
pyautogui.moveTo(500, 500, duration=2, tween=pyautogui.easeInOutQuad)
# 在每个键之间输入四分之一秒的暂停
pyautogui.write('Hello world!', interval=0.25)
# 按“Esc”键。所有键名都在 pyautogui.KEY_NAMES 中。
pyautogui.press('esc')
# 按住 Shift 键不放
pyautogui.keyDown('shift')
# 按左箭头键 4 次。
pyautogui.press(['left', 'left', 'left', 'left'])
# 释放 Shift 键。
pyautogui.keyUp('shift')
# 按 Ctrl-C 组合热键。
pyautogui.hotkey('ctrl', 'c')
# 出现一个警告框，并暂停程序，直到单击 OK。
pyautogui.alert('这是要显示的消息')
```

下面的例子是在 MS Paint（或任何图形绘图程序）中将鼠标拖动到一个方形的螺旋形状：

```python
distance = 200
while distance > 0:
    pyautogui.drag(distance, 0, duration=0.5)   # move right
    distance -= 5
    pyautogui.drag(0, distance, duration=0.5)   # move down
    pyautogui.drag(-distance, 0, duration=0.5)  # move left
    distance -= 5
    pyautogui.drag(0, -distance, duration=0.5)  # move up
```

与直接生成图像文件的脚本相比，使用 PyAutoGUI 的好处是可以使用 MS Paint 提供的画笔工具。