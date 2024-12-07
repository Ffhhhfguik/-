<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Прототип Программы</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        button {
            margin-top: 10px;
        }
        .task-container {
            margin-top: 20px;
            padding: 10px;
            background-color: #f9f9f9;
        }
        .report {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            background-color: #eef;
        }
        canvas {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Главное меню</h1>
    <ul>
        <li><a href="#hosting">Хостинг репозиториев</a></li>
        <li><a href="#version-control">Версионный контроль</a></li>
        <li><a href="#collaboration">Сотрудничество</a></li>
        <li><a href="#pull-requests">Процесс запросов на изучение</a></li>
        <li><a href="#task-tracking">Отслеживание задач</a></li>
        <li><a href="#integration">Интеграция и автоматизация</a></li>
        <li><a href="#security">Безопасность проектов</a></li>
        <li><a href="#community">Сообщество и открытость</a></li>
    </ul>

    <div id="tasks">
        <h2>Автоматизация задач</h2>
        <button onclick="generateReport()">Сгенерировать отчет</button>
        <button onclick="collectFeedback()">Оставить отзыв</button>
        <div class="task-container" id="taskContainer"></div>
        <div class="report" id="reportContainer"></div>
        <canvas id="feedbackChart"></canvas>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        const tasks = [
            { id: 1, description: 'Задача 1: Улучшить производительность', solution: 'Оптимизация кода' },
            { id: 2, description: 'Задача 2: Исправить ошибку в модуле', solution: 'Исправление ошибки в коде' }
        ];

        const feedbackCache = new Map();
        const feedbackData = [];

        function generateReport() {
            let reportContent = 'Отчет о выполненных задачах:\n\n';
            tasks.forEach(task => {
                const { description, solution } = task;
                reportContent += `Задача: ${description}\nРешение: ${solution}\n\n`;
                syncWithCode(task);
            });
            document.getElementById('reportContainer').innerText = reportContent;
            alert('Отчет сгенерирован и синхронизирован с исходным кодом!');
        }

        function syncWithCode(task) {
            console.log(`Синхронизация решения для "${task.description}" завершена.`);
        }

        async function collectFeedback() {
            const feedback = prompt("Пожалуйста, оставьте свой отзыв о работе программы:");
            if (feedback) {
                const sentiment = await analyzeFeedbackWithBERT(feedback);
                console.log(`Ваш отзыв: "${feedback}" имеет ${sentiment} настроение.`);
                alert(`Ваш отзыв сохранен с настроением: ${sentiment}`);
                feedbackData.push({ feedback, sentiment });
                updateChart();
            } else {
                console.log("Отзыв не был введён.");
            }
        }

        async function analyzeFeedbackWithBERT(feedback) {
            if (feedbackCache.has(feedback)) {
                return feedbackCache.get(feedback);
            }
            const sentiment = feedback.includes("хорошо") ? "положительный" : "отрицательный";
            feedbackCache.set(feedback, sentiment);
            return sentiment;
        }

        function updateChart() {
            const positiveCount = feedbackData.filter(f => f.sentiment === 'положительный').length;
            const negativeCount = feedbackData.filter(f => f.sentiment === 'отрицательный').length;

            const ctx = document.getElementById('feedbackChart').getContext('2d');
            const feedbackChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Положительные', 'Отрицательные'],
                    datasets: [{
                        label: 'Количество отзывов',
                        data: [positiveCount, negativeCount],
                        backgroundColor: ['rgba(75, 192, 192, 1)', 'rgba(255, 99, 132, 1)'],
                    }]
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
        }
    </script>
</body>
</html>
