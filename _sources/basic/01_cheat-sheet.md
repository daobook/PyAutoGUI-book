# 速查表

参考：[quickstart](https://pyautogui.readthedocs.io/en/latest/quickstart.html)

这是一个关于使用 PyAutoGUI 的快速入门引用。PyAutoGUI 是跨平台的 GUI 自动化模块，可以在 Python 2 和 3 上工作。您可以控制鼠标和键盘，以及执行基本的图像识别来自动化您的计算机上的任务。

本页示例中的所有关键字参数都是可选的。

```python
import pyautogui
```

## 通用函数

```python
>>> pyautogui.position()  # 当前鼠标x和y
(968, 56)
>>> pyautogui.size()  # 当前屏幕分辨率的宽度和高度
(1920, 1080)
>>> pyautogui.onScreen(x, y)  # 如果x和y在屏幕内，则为True。
True
```

## 失效保护

在每次 PyAutoGUI 调用后设置一个 2.5 秒的暂停：

```python
import pyautogui
pyautogui.PAUSE = 2.5
```

当故障安全模式为 `True` 时，将鼠标移动到左上角将引发 `pyautogui.FailSafeException` 可以中止程序的：

```python
import pyautogui
pyautogui.FAILSAFE = True
```

## 鼠标函数

XY 坐标在屏幕的左上角以 $0,0$ 为原点。X 向右增加，Y 向下增加。

```python
# 将鼠标移动到XY坐标上的num_second秒
pyautogui.moveTo(x, y, duration=num_seconds) 
# 相对于当前位置移动鼠标
pyautogui.moveRel(xOffset, yOffset, duration=num_seconds)
```

如果 `duration` 为 0 或未指定，则立即移动。注意:在 Mac 上拖拽不能立即完成。

```python
# 拖动鼠标到XY
pyautogui.dragTo(x, y, duration=num_seconds)
# 相对于当前位置拖动鼠标
pyautogui.dragRel(xOffset, yOffset, duration=num_seconds)
```

调用 `click()` 只是用鼠标当前位置的左键点击鼠标一次，但关键字参数可以改变这一点：

```python
pyautogui.click(x=moveToX, y=moveToY, clicks=num_of_clicks, interval=secs_between_clicks, button='left')
```

`button` 关键字参数可以是`'left'`， `'middle'`，或 `'right'`。

所有的单击都可以用 `click()` 完成，但这些函数的存在是为了可读性。关键字参数是可选的：

```python
>>> pyautogui.rightClick(x=moveToX, y=moveToY)
>>> pyautogui.middleClick(x=moveToX, y=moveToY)
>>> pyautogui.doubleClick(x=moveToX, y=moveToY)
>>> pyautogui.tripleClick(x=moveToX, y=moveToY)
```

正滚动将向上滚动，负滚动将向下滚动：

```python
pyautogui.scroll(amount_to_scroll, x=moveToX, y=moveToY)
```

单个按钮向下和向上事件可以分别调用：

```python
pyautogui.mouseDown(x=moveToX, y=moveToY, button='left')
pyautogui.mouseUp(x=moveToX, y=moveToY, button='left')
```

## 键盘函数

在调用函数时，按下的键会转到键盘光标所在的位置。

```python
# 对于输入文本，换行符是 Enter
pyautogui.typewrite('Hello world!\n', interval=secs_between_keys)
```

也可以传递一个键名列表

```python
pyautogui.typewrite(['a', 'b', 'c', 'left', 'backspace', 'enter', 'f1'], interval=secs_between_keys)
```

键名的完整列表在 `pyautogui.KEYBOARD_KEYS` 中。

像 Ctrl-S 或 Ctrl-Shift-1 这样的键盘热键可以通过向 `hotkey()` 传递一个键名列表来实现：

```python
# ctrl-c 复制
pyautogui.hotkey('ctrl', 'c')
# ctrl-v 粘贴
pyautogui.hotkey('ctrl', 'v')
```

单个按钮向下和向上事件可以分别调用：

```python
pyautogui.keyDown(key_name)
pyautogui.keyUp(key_name)
```

## 信息框函数

如果你需要暂停程序，直到用户单击 OK，或者想向用户显示一些信息，消息框函数有类似于 JavaScript 的名称：

```python
>>> pyautogui.alert('This displays some text with an OK button.')
>>> pyautogui.confirm('This displays text and has an OK and Cancel button.')
'OK'
>>> pyautogui.prompt('This lets the user type in a string and press OK.')
'This is what I typed in.'
```

如果用户单击 Cancel, `prompt()`函数将返回 `None`。

## 截图函数

PyAutoGUI 使用 Pillow/PIL 来处理与图像相关的数据。

在 Linux 上，必须运行 `sudo apt-get install scrot` 来使用截图特性。

```python
>>> pyautogui.screenshot()  # returns a Pillow/PIL Image object
<PIL.Image.Image image mode=RGB size=1920x1080 at 0x24C3EF0>
>>> pyautogui.screenshot('foo.png')  # returns a Pillow/PIL Image object, and saves it to a file
<PIL.Image.Image image mode=RGB size=1920x1080 at 0x31AA198>
```

如果你有一个想要点击的图像文件，你可以通过 `locateOnScreen()` 在屏幕上找到它。

```python
>>> pyautogui.locateOnScreen('looksLikeThis.png')  # 返回被找到的第一个位置的(左，上，宽，高)
(863, 417, 70, 13)
```

`locateAllOnScreen()` 函数将为它在屏幕上找到的所有位置返回一个生成器：

```python
>>> for i in pyautogui.locateAllOnScreen('looksLikeThis.png')
...
...
(863, 117, 70, 13)
(623, 137, 70, 13)
(853, 577, 70, 13)
(883, 617, 70, 13)
(973, 657, 70, 13)
(933, 877, 70, 13)
```

```python
>>> list(pyautogui.locateAllOnScreen('looksLikeThis.png'))
[(863, 117, 70, 13), (623, 137, 70, 13), (853, 577, 70, 13), (883, 617, 70, 13), (973, 657, 70, 13), (933, 877, 70, 13)]
```

`locateCenterOnScreen()` 函数只是返回图像在屏幕上的中间位置的XY坐标：

```python
>>> pyautogui.locateCenterOnScreen('looksLikeThis.png')  # returns center x and y
(898, 423)
```

如果在屏幕上找不到图像，这些函数将返回 `None`。

```{note}
定位功能很慢，可能需要一到两秒。
```