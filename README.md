- 👋 Hi, I’m @lambrightn17
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
lambrightn17/lambrightn17 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
from telegram.ext import Updater, MessageHandler, Filters
from collections import Counter
import re

# Replace with your actual Telegram bot token
TOKEN = '6873450444:AAFKVZWGLsgXg6ZhKrGNWHb4zml6sr7pD0E'

def count_words(text):
    # Clean text by removing special characters and converting to lowercase
    cleaned_text = re.sub(r'[^\w\s]', '', text.lower())
    # Split text into words and count frequency using Counter
    word_counts = Counter(cleaned_text.split())
    return word_counts

def handle_message(update, context):
    message = update.message.text
    word_counts = count_words(message)
    sorted_word_counts = sorted(word_counts.items(), key=lambda x: x[1], reverse=True)
   
    # Prepare response
    response = "Word frequencies:\n"
    for word, count in sorted_word_counts[:20]:  # Show top 20 words
        response += f"{word}: {count}\n"
   
    # Send response back to the user
    context.bot.send_message(chat_id=update.effective_chat.id, text=response)

def main():
    updater = Updater(token=TOKEN, use_context=True)
    dispatcher = updater.dispatcher
   
    # Handle incoming messages
    message_handler = MessageHandler(Filters.text & (~Filters.command), handle_message)
    dispatcher.add_handler(message_handler)
   
    # Start the Bot
    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()

