import os
import threading
from flask import Flask
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

TOKEN = os.environ["TELEGRAM_BOT_TOKEN"]

app = Flask(__name__)


@app.route("/")
def home():
    return "Geographer GIS is running!"


async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "🌍 أهلاً بيكِ في Geographer GIS 👩🏻‍💻\n\n"
        "مساعدك الذكي في الجغرافيا وGIS.\n\n"
        "🚀 دي أول نسخة من Geographer GIS، "
        "وهنبنيها خطوة بخطوة.\n\n"
        "📚 Learn • Explore • Analyze"
    )


def run_bot():
    bot_app = ApplicationBuilder().token(TOKEN).build()
    bot_app.add_handler(CommandHandler("start", start))
    bot_app.run_polling()


if __name__ == "__main__":
    threading.Thread(target=run_bot, daemon=True).start()

    port = int(os.environ.get("PORT", 10000))
    app.run(host="0.0.0.0", port=port)