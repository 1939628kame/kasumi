from flask import Flask, render_template, request
app = Flask(__name__)

ITEMS = [
    {"type": "youtube", "title": "猫動画", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ"},
    {"type": "youtube", "title": "ギター講座", "url": "https://www.youtube.com/embed/2Vv-BfVoq4g"},
    {"type": "game", "title": "2048", "url": "https://play2048.co/"},
    {"type": "game", "title": "スネークゲーム", "url": "https://www.google.com/fbx?fbx=snake_arcade"},
]

@app.route("/", methods=["GET", "POST"])
def index():
    results = ITEMS
    if request.method == "POST":
        query = request.form["query"]
        results = [item for item in ITEMS if query.lower() in item["title"].lower()]
    return render_template("index.html", results=results)

if __name__ == "__main__":
    app.run(debug=True)

    Flask==3.0.0
gunicorn==21.2.0

#!/bin/bash
gunicorn app:app --bind 0.0.0.0:$PORT

<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>検索サイト - 動画とゲーム</title>
    <style>
        body { font-family: sans-serif; margin: 30px; }
        h1 { color: #333; }
        form { margin-bottom: 20px; }
        ul { list-style: none; padding: 0; }
        li { margin-bottom: 30px; }
        .title { font-weight: bold; }
        iframe { margin-top: 10px; }
        .game-link { display: inline-block; margin-top: 10px; font-size: 16px; background: #e0e0e0; padding: 8px 16px; border-radius: 5px; text-decoration: none; }
    </style>
</head>
<body>
    <h1>動画・ゲーム検索サイト</h1>
    <form method="post">
        <input type="text" name="query" placeholder="キーワード検索" style="font-size:16px; padding:6px;">
        <button type="submit" style="font-size:16px;">検索</button>
    </form>
    <ul>
        {% for item in results %}
            <li>
                <span class="title">{{ item.title }}</span>
                {% if item.type == "youtube" %}
                    <br>
                    <iframe width="360" height="215" src="{{ item.url }}" frameborder="0" allowfullscreen></iframe>
                {% elif item.type == "game" %}
                    <br>
                    <a class="game-link" href="{{ item.url }}" target="_blank">ゲームで遊ぶ</a>
                {% endif %}
            </li>
        {% endfor %}
        {% if results|length == 0 %}
            <li>該当する結果はありません。</li>
        {% endif %}
    </ul>
</body>
</html>
