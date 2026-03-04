# dse2026.html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2026 DSE 倒數計時器</title>
    <style>
        :root {
            --primary-color: #2c3e50;
            --accent-color: #e74c3c;
            --bg-color: #f4f7f6;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--primary-color);
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }
        h1 { margin-top: 20px; color: #34495e; }
        .container {
            width: 100%;
            max-width: 800px;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }
        .main-countdown {
            text-align: center;
            padding: 20px;
            background: #fff;
            border-bottom: 2px solid #eee;
            margin-bottom: 30px;
        }
        .timer {
            font-size: 3rem;
            font-weight: bold;
            color: var(--accent-color);
        }
        .subject-list {
            display: grid;
            grid-template-columns: 1fr;
            gap: 15px;
        }
        .subject-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            background: #f9f9f9;
            border-radius: 8px;
            transition: transform 0.2s;
        }
        .subject-item:hover { transform: scale(1.02); }
        .subject-info { flex-grow: 1; }
        .subject-name { font-weight: bold; font-size: 1.1rem; }
        .subject-date { color: #7f8c8d; font-size: 0.9rem; }
        .days-left {
            font-weight: bold;
            color: #2980b9;
            background: #ebf5fb;
            padding: 5px 15px;
            border-radius: 20px;
            min-width: 80px;
            text-align: center;
        }
        .expired { color: #bdc3c7; background: #eee; }
        footer { margin-top: 40px; font-size: 0.8rem; color: #95a5a6; }
    </style>
</head>
<body>

    <h1>2026 DSE 應試倒數</h1>
    
    <div class="container">
        <div class="main-countdown">
            <h3>距離筆試首日 (4月8日) 還有</h3>
            <div id="main-timer" class="timer">計算中...</div>
        </div>

        <div class="subject-list" id="subject-list">
            </div>
    </div>

    <footer>* 實際日期以考評局最終公佈為準。加油，準考生！</footer>

    <script>
        // 2026 DSE 考試時間表 (暫定日期)
        const exams = [
            { name: "視覺藝術 (一/二)", date: "2026-04-08" },
            { name: "中國語文 (一/二)", date: "2026-04-09" },
            { name: "英國語文 (一/二)", date: "2026-04-10" },
            { name: "英國語文 (三) 聆聽", date: "2026-04-11" },
            { name: "數學必修部分 (一/二)", date: "2026-04-13" },
            { name: "公民與社會發展", date: "2026-04-14" },
            { name: "化學 (一/二)", date: "2026-04-16" },
            { name: "生物 (一/二)", date: "2026-04-20" },
            { name: "物理 (一/二)", date: "2026-04-22" },
            { name: "中國歷史 (一/二)", date: "2026-04-29" },
            { name: "放榜日 (暫定)", date: "2026-07-15" }
        ];

        function updateTimers() {
            const now = new Date();
            const listContainer = document.getElementById('subject-list');
            listContainer.innerHTML = '';

            // 處理主倒數 (第一科)
            const firstExam = new Date("2026-04-08T08:30:00");
            const diff = firstExam - now;
            
            if (diff > 0) {
                const d = Math.floor(diff / (1000 * 60 * 60 * 24));
                const h = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                const s = Math.floor((diff % (1000 * 60)) / 1000);
                document.getElementById('main-timer').innerText = `${d}天 ${h}小時 ${m}分 ${s}秒`;
            } else {
                document.getElementById('main-timer').innerText = "考試已開始！";
            }

            // 處理各科列表
            exams.forEach(exam => {
                const examDate = new Date(exam.date + "T00:00:00");
                const timeDiff = examDate - now;
                const daysDiff = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));

                const item = document.createElement('div');
                item.className = 'subject-item';
                
                let countdownText = "";
                let extraClass = "";

                if (daysDiff > 0) {
                    countdownText = `還有 ${daysDiff} 天`;
                } else if (daysDiff === 0) {
                    countdownText = "今天考試！";
                    extraClass = "expired";
                } else {
                    countdownText = "已完結";
                    extraClass = "expired";
                }

                item.innerHTML = `
                    <div class="subject-info">
                        <div class="subject-name">${exam.name}</div>
                        <div class="subject-date">${exam.date}</div>
                    </div>
                    <div class="days-left ${extraClass}">${countdownText}</div>
                `;
                listContainer.appendChild(item);
            });
        }

        setInterval(updateTimers, 1000);
        updateTimers();
    </script>
</body>
</html>
