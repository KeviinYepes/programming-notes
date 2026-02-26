
# Pyautogui 

### How to install
````bash
pip install pyautogui
````
---
## What is PyAutoGui?

PyAutoGUI is a Python library that allows you to automate the mouse, keyboard, and screen interactions. It is commonly used for:

* RPA (Robotic Process Automation)
* Repetitive task automation
* GUI testing
* Desktop application bots

It works on Windows, macOS, and Linux.

## 1️⃣How to capture components on screen?
PyAutoGUI does not detect elments like Selenium (by ID or XPath) Instead, it works using ( x , y ) coordinates or image recognition

---
### Get mouse coordinates
````bash
import pyautogui
print(pyautogui.position()) #Get the coords
````
You can move the mouse and see the coordinates in real time.

You can also use:
````bash
pyautogui.displayMousePosition()
````
---

### Locate a component using an image
First, take a screenshot of the button or element (for example: button.png).
````bash
import pyautogui

location = pyautogui.locateOnScreen("button.png", confidence=8.0) #80% similarity

if location:
    print(f "Found at {location}")
````
