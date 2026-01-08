<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reaction Tracker</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        :root {
            --tg-bg: var(--tg-theme-bg-color, #ffffff);
            --tg-text: var(--tg-theme-text-color, #000000);
            --tg-hint: var(--tg-theme-hint-color, #999999);
            --tg-button: var(--tg-theme-button-color, #2481cc);
            --tg-button-text: var(--tg-theme-button-text-color, #ffffff);
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--tg-bg);
            color: var(--tg-text);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 400px;
        }

        .header {
            text-align: center;
            margin-bottom: 20px;
        }

        .status-card {
            background: var(--tg-theme-secondary-bg-color, #f4f4f5);
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 20px;
            border: 1px solid rgba(0,0,0,0.1);
        }

        .user-list {
            list-style: none;
            padding: 0;
        }

        .user-item {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            border-bottom: 1px solid rgba(0,0,0,0.05);
        }

        .reaction {
            font-weight: bold;
        }

        button {
            width: 100%;
            padding: 12px;
            background-color: var(--tg-button);
            color: var(--tg-button-text);
            border: none;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            margin-top: 10px;
        }

        .hint {
            color: var(--tg-hint);
            font-size: 12px;
            text-align: center;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h2>Анализ реакций</h2>
        </div>

        <div class="status-card">
            <div id="channel-info">Загрузка данных канала...</div>
        </div>

        <div id="analysis-results">
            <p>Список последних активностей:</p>
            <div class="user-list" id="reactions-list">
                <!-- Сюда будут подгружаться данные через JS -->
            </div>
        </div>

        <button id="main-btn" onclick="sendReport()">Отправить отчет в ЛС</button>
        <p class="hint">Бот проанализирует доступную историю и отправит подробный лог вам в личные сообщения.</p>
    </div>

    <script>
        const tg = window.Telegram.WebApp;
        tg.expand(); // Развернуть на весь экран

        // Настройка главной кнопки Telegram
        tg.MainButton.setText("ЗАКРЫТЬ ПРИЛОЖЕНИЕ").hide();

        // Пример данных (в реальности они должны приходить с вашего бэкенда)
        document.getElementById('channel-info').innerText = "Канал: Название канала\nПостов проанализировано: 150";

        const mockReactions = [
            { name: "Иван Иванов", reaction: "🔥", post: "#124" },
            { name: "Мария С.", reaction: "❤️", post: "#124" },
            { name: "Alex Code", reaction: "👍", post: "#123" }
        ];

        const listContainer = document.getElementById('reactions-list');
        mockReactions.forEach(item => {
            const div = document.createElement('div');
            div.className = 'user-item';
            div.innerHTML = `<span>${item.name} (Пост ${item.post})</span> <span class="reaction">${item.reaction}</span>`;
            listContainer.appendChild(div);
        });

        function sendReport() {
            // Отправка данных боту
            const data = {
                action: "send_report",
                userId: tg.initDataUnsafe.user.id
            };

            tg.sendData(JSON.stringify(data));
            tg.close();
        }
    </script>
</body>
</html>
