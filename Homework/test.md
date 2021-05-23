%pip install line-bot-sdk
from flask import Flask, request, abort

from linebot import (
    LineBotApi, WebhookHandler
)
from linebot.exceptions import (
    InvalidSignatureError
)
from linebot.models import (
    MessageEvent, TextMessage, TextSendMessage,
)
app = Flask(__name__)

line_bot_api = LineBotApi('u0Hm8J/a/Hrxdn0k8Fq2uXWSr745zj436qTPlIAzgC0ztDcSvjqP67So5AqzQ8FmgX9LRlcwIvzEsre3GqfdnpnAcY3A4lEapa5gZnxot5W1Mp3UuCR9mwE3RBk3vyHquEjonjBTcoQNKuufw044jQdB04t89/1O/w1cDnyilFU=')
handler = WebhookHandler('ec37e7c4f78e0b088c43a1bbe82a244f')
@app.route("/callback", methods=['POST'])
def callback():

    signature = request.headers['X-Line-Signature']

    body = request.get_data(as_text=True)
    #app.logger.info("Request body: " + body)

    try:
        parser.parse(body, signature)
        #handler.handle(body, signature)
    except InvalidSignatureError:
        print("Invalid signature. Please check your channel access token/channel secret.")
        abort(400)

    return 'OK'
@handler.add(MessageEvent, message=TextMessage)
def test_message(event):
    mtext = event.message.text
    line_bot_api.reply_message(event.reply_token,message)
    if mtext == 'high':
        try:
            message = TextSendMessage(
                text = " a different transaction"
            )
            line_bot_api.reply_message(event.reply_token,message)
        except:
             line_bot_api.reply_message(event.reply_token,
                  TextSendMessage(text = "wrong"))
if __name__ == '__main__':
    app.run()

