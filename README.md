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
