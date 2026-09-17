from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

# Здесь временно хранится последний результат
last_result = None


@app.route("/")
def index():
    return render_template("index.html")


@app.route("/rate")
def rate():
    return render_template("rate.html")


@app.route("/calculate", methods=["POST"])
def calculate():
    global last_result

    try:
        sleep = float(request.form["sleep"])
        work = float(request.form["work"])
        mood = int(request.form["mood"])
        sport = request.form["sport"]

        # Проверяем введённые данные
        if sleep < 0 or sleep > 24:
            return render_template(
                "rate.html",
                error="Количество часов сна должно быть от 0 до 24."
            )

        if work < 0 or work > 24:
            return render_template(
                "rate.html",
                error="Количество часов работы или учёбы должно быть от 0 до 24."
            )

        if mood < 1 or mood > 10:
            return render_template(
                "rate.html",
                error="Настроение должно быть от 1 до 10."
            )

        # Баллы за сон
        if 7 <= sleep <= 9:
            sleep_score = 30
        elif 6 <= sleep < 7 or 9 < sleep <= 10:
            sleep_score = 25
        else:
            sleep_score = 15

        # Баллы за работу или учёбу
        if 4 <= work <= 8:
            work_score = 30
        elif 2 <= work < 4 or 8 < work <= 10:
            work_score = 20
        else:
            work_score = 10

        # Баллы за настроение
        mood_score = mood * 2.5

        # Баллы за физическую активность
        if sport == "yes":
            sport_score = 15
        else:
            sport_score = 5

        # Итоговый результат
        score = round(
            sleep_score +
            work_score +
            mood_score +
            sport_score
        )

        # Текстовый результат
        if score >= 85:
            message = "Очень продуктивный день!"
        elif score >= 70:
            message = "Хороший день."
        elif score >= 50:
            message = "Неплохой результат, но есть что улучшить."
        else:
            message = "Сегодня стоит больше отдохнуть."

        last_result = {
            "score": score,
            "message": message
        }

        return redirect(url_for("result"))

    except (ValueError, KeyError):
        return render_template(
            "rate.html",
            error="Пожалуйста, заполните все поля корректно."
        )


@app.route("/result")
def result():
    if last_result is None:
        return redirect(url_for("rate"))

    # Получаем имя из GET-параметра
    name = request.args.get("name", "")

    return render_template(
        "result.html",
        score=last_result["score"],
        message=last_result["message"],
        name=name
    )


@app.route("/about")
def about():
    return render_template("about.html")


if __name__ == "__main__":
    app.run(debug=True)
