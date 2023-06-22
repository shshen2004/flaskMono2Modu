# flaskMono2Modu
Scaling Flask for Modularity: Leveraging Blueprints for Development, Testing, and Deployment
Flask建立Web應用程序時，使用藍圖（Blueprint）是一種組織和管理路由的方便方法。藍圖允許你將相關的路由分組在一起，以便更好地組織和維護代碼。
在這篇教學中，將向你展示如何使用Flask藍圖。

首先，確保你已經安裝了Flask。你可以使用以下命令在終端中安裝Flask：

```
pip install flask
```

接下來，創建一個新的文件夾作為你的項目根目錄，並在其中創建一個名為`app`的文件夾。在`app`文件夾中，創建一個名為`__init__.py`的文件。這個文件將是你的Flask應用程序的入口點。

在`__init__.py`文件中，首先導入必要的模塊：

```python
from flask import Flask
```

然後，創建一個Flask應用程序實例：

```python
app = Flask(__name__)
```

現在，我們可以開始定義一個藍圖。在`__init__.py`文件中，新增以下代碼：

```python
from .blueprints import example_blueprint

app.register_blueprint(example_blueprint)
```

上面的代碼將導入名為`example_blueprint`的藍圖對象，然後將其註冊到應用程序實例中。

現在，我們需要創建藍圖對象。在`app`文件夾中，創建一個名為`blueprints.py`的文件。在這個文件中，新增以下代碼：

```python
from flask import Blueprint

example_blueprint = Blueprint('example', __name__)

@example_blueprint.route('/')
def index():
    return 'Hello, World!'
```

上面的代碼定義了一個名為`example_blueprint`的藍圖對象。我們使用`Blueprint`類創建藍圖，並指定了藍圖的名稱和模塊名稱。然後，我們定義了一個名為`index`的路由，當訪問根路徑時，將返回字符串`Hello, World!`。

現在，我們已經完成了藍圖的定義。接下來，我們需要運行Flask應用程序。在`__init__.py`文件中新增以下代碼：

```python
if __name__ == '__main__':
    app.run()
```

最後，你可以在終端中運行應用程序：

```
python app/__init__.py
```

現在，你可以在瀏覽器

使用 Flask 的藍圖 (Blueprint) 的環境中創建一個登錄 (login) 程序，並展示登錄成功後進入名為 "home" 的頁面，並且示範如何在登入後管理後續的會話 (session)。

請按照以下步驟進行操作：

1. 在項目根目錄中，創建一個名為 "app" 的文件夾。
2. 在 "app" 文件夾中，創建以下文件：

- `__init__.py`：作為 Flask 應用程序的入口點。
- `auth.py`：處理身份驗證相關功能。
- `views.py`：定義網頁路由和處理器。

現在，我們將在這些文件中添加所需的代碼。

首先，打開 `__init__.py` 文件，添加以下代碼：

```python
from flask import Flask
from .auth import auth_blueprint

app = Flask(__name__)
app.secret_key = 'your_secret_key'
app.register_blueprint(auth_blueprint)

from .views import *

if __name__ == '__main__':
    app.run()
```

上述代碼創建了 Flask 應用程序實例，並註冊了一個名為 `auth_blueprint` 的藍圖。我們還設置了 `app.secret_key`，這是用於保護 session 的密鑰。然後，我們導入了 `views.py` 中的所有路由和處理器。

接下來，打開 `auth.py` 文件，添加以下代碼：

```python
from flask import Blueprint, render_template, request, redirect, session

auth_blueprint = Blueprint('auth', __name__)

@auth_blueprint.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        
        # 執行身份驗證邏輯，例如檢查用戶名和密碼是否有效
        # ...
        
        # 假設驗證成功，將用戶名存儲在 session 中
        session['username'] = username
        return redirect('/home')
    
    return render_template('login.html')

@auth_blueprint.route('/logout')
def logout():
    # 從 session 中移除用戶名
    session.pop('username', None)
    return redirect('/')
```

上述代碼定義了 `auth_blueprint` 藍圖中的兩個路由。`login` 路由處理使用者的登錄請求，並在驗證成功後將用戶名存儲在 session 中。`logout` 路由處理用戶的登出請求，從 session 中移除用戶名。

最後，打開 `views.py` 文件，添加以下代碼：

```python
from flask import render_template, session
