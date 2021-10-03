# -
Как улучшить
import requests

from flask import Flask, request

from pprint import pprint

app = Flask(__name__)

APPID = "969511fc8e7d7f29b4b66ff1417a221e"  # <-- Put your OpenWeatherMap appid here!
URL_BASE = "http://api.openweathermap.org/data/2.5/"

# получаем свой IP в браузере если убрать while True
@app.route('/')
def get_ip():
    ip = request.environ['HTTP_X_FORWARDED_FOR']
    r = requests.get(f'http://ip-api.com/json/{ip}' + "onecall", params=locals())
    return r.json()

# получаем погоду в питоне по вводу города; вставил русский ввод
@app.route('/')
def current_weather(q: str = "Chicago", appid: str = APPID, lang: str = 570) -> dict:
    """https://openweathermap.org/api"""
    return requests.get(URL_BASE + "weather", params=locals()).json()


if __name__ == '__main__':

    while True:
        location = input("Enter a location:").strip()
        if location:
            pprint(current_weather(location))
        else:
            break
    app.run(debug=True, port=5000)
