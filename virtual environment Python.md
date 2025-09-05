# 🐍 Virtual Environment in Python

A **virtual environment** in Python is an isolated directory that contains a specific Python interpreter and its own set of installed packages and libraries.  
It provides a **self-contained environment** for individual Python projects, preventing conflicts between projects that might need different package versions or dependencies.

---

## ⚙️ How to Create a Virtual Environment

**General Structure**:
```bash
virtualenv -p python[version] [name]
````
***Original***
````bash
virtualenv -p python3 env
````
## ▶️ How to Activate the Environment

***Windows (route):***
````bash
 \env\scripts\activate
````
## ⏹️ How to Deactivate the Environment
***command***
````bash
 deactivate
````

## 📦 Export Requirements``
***Generate a requirements.txt file with all installed dependencies:***
````bash
pip freeze > requirements.txt
````
