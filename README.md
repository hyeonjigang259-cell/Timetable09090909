# Timetable09090909
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2026 실시간 다크 시간표</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/GmarketSansMedium.woff">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'GmarketSansMedium', sans-serif;
        }

        body {
            background-color: #121212;
            color: #ffffff;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            background-color: #1e1e1e;
            border: 3px solid #0055ff; 
            border-radius: 20px;
            box-shadow: 0px 10px 30px rgba(0, 85, 255, 0.2);
            width: 100%;
            max-width: 520px;
            padding: 25px;
            text-align: center;
        }

        h1 {
            font-size: 1.6rem;
            margin-bottom: 5px;
            color: #ffffff;
        }

        .subtitle {
            font-size: 0.9rem;
            color: #aaaaaa;
            margin-bottom: 25px;
        }

        /* 현재 교시 정보 카드 */
        .current-card {
            background-color: #252525;
            border: 2px solid #333333;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 25px;
            position: relative;
            overflow: hidden;
        }

        .current-card::before {
            content: '';
            position: absolute;
            top: 0; left: 0; width: 6px; height: 100%;
            background-color: #0055ff; 
        }

        .current-label {
            display: inline-block;
            background-color: #0055ff;
            color: #ffffff;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            margin-bottom: 10px;
        }

        .current-subject {
            font-size: 2rem;
            font-weight: bold;
            color: #ffffff;
            margin-bottom: 8px;
        }

        .current-info {
            font-size: 1rem;
            color: #cccccc;
            margin-bottom: 12px;
        }

        /* 남은 시간 타이머 */
        .timer-box {
            font-size: 0.95rem;
            color: #00ffcc; 
            background-color: #1a2436;
            display: inline-block;
            padding: 6px 16px;
            border-radius: 10px;
            font-weight: bold;
        }

        /* 전체 시간표 섹션 */
        .timetable-section {
            text-align: left;
            margin-top: 15px;
        }

        .section-title {
            font-size: 1.1rem;
            color: #3388ff; 
            margin-bottom: 10px;
            font-weight: bold;
        }

        .table-wrapper {
            width: 100%;
            overflow-x: auto;
            border-radius: 10px;
            border: 2px solid #0055ff; 
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85rem;
            text-align: center;
            background-color: #1a1a1a !important; /* 💡 표 바탕을 강제로 검은색 고정 */
            min-width: 440px; 
        }

        th, td {
            padding: 12px 6px;
            border: 1px solid #333333;
        }

        th {
            background-color: #252525 !important; /* 💡 요일 칸 배경 진한 검은색 고정 */
            color: #3388ff !important; /* 요일 글씨는 진한 파란색 */
            font-weight: bold;
            font-size: 0.9rem;
        }

        /* 💡 과목이 표시되는 칸의 스타일 강제 지정 */
        td {
            background-color: #1a1a1a !important; /* 💡 무조건 바탕을 어두운 검은색으로 */
            color: #ffffff !important; /* 기본 일반 텍스트는 흰색 */
            white-space: pre-line; 
            line-height: 1.4;
        }

        /* 과목 강조 텍스트 (진한 파란색) */
        td strong {
            color: #3388ff !important; 
            font-size: 0.9rem;
            display: inline-block;
            margin-bottom: 3px;
        }

        /* 교실 및 선생님 정보 (형광 민트) */
        .info-text {
            color: #00ffcc !important; 
            font-size: 0.75rem;
        }

        /* 오늘 요일 하이라이트 효과 (검은 바탕 위에 파란색 필터를 겹친 느낌) */
        .today-highlight {
            background-color: #162447 !important; /* 💡 흰색이 섞이지 않는 딥블루/검은색 계열로 변경 */
            border: 1px solid #0055ff !important;
        }

        .status-box {
            font-size: 0.8rem;
            color: #666666;
            margin-top: 20px;
            border-top: 1px dashed #333333;
            padding-top: 15px;
            text-align: center;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📅 2026 1학기 라이브 대시보드</h1>
    <div class="subtitle" id="current-time-display">현재 시간 확인 중...</div>

    <div class="current-card">
        <div class="current-label" id="period-label">확인 중</div>
        <div class="current-subject" id="subject-display">로딩 중...</div>
        <div class="current-info" id="info-display">선생님 | 교실</div>
        <div class="timer-box" id="timer-display">⏳ 남은 시간 계산 중...</div>
    </div>

    <div class="timetable-section">
        <div class="section-title">📊 이번 주 전체 시간표</div>
        <div class="table-wrapper">
            <table>
                <thead>
                    <tr>
                        <th style="color: #ffffff !important;">교시</th>
                        <th id="th-월">월</th>
                        <th id="th-화">화</th>
                        <th id="th-수">수</th>
                        <th id="th-목">목</th>
                        <th id="th-금">금</th>
                    </tr>
                </thead>
                <tbody id="timetable-body">
                    </tbody>
            </table>
        </div>
    </div>

    <div class="status-box">
        ⏱️ 10초마다 현재 수업을 스캔하고, 타이머는 실시간으로 흐릅니다.
    </div>
</div>

<script>
    const timetableData = {
        '월': {
            '1교시': { subject: '자율', room: '3-6', teacher: '고익준' },
            '2교시': { subject: '사회과제연구', room: '1 전용실', teacher: '김현욱' },
            '3교시': { subject: '영어 독해와 작문', room: '위국실', teacher: '서버들' },
            '4교시': { subject: '고전윤리', room: '3-2', teacher: '조현응' },
            '5교시': { subject: '예배', room: '3-6', teacher: '고익준' },
            '6교시': { subject: '사회문화', room: '3 전용실', teacher: '은종서' },
            '7교시': { subject: '논술', room: '3-8', teacher: '서버들' },
            '8교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' },
            '9교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' },
            '10교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' }
        },
        '화': {
            '1교시': { subject: '고전윤리', room: '3-2', teacher: '조현응' },
            '2교시': { subject: '영어 독해와 작문', room: '위국실', teacher: '서버들' },
            '3교시': { subject: '확률과 통계', room: '3 전용실', teacher: '신소운' },
            '4교시': { subject: '사회문화', room: '3 전용실', teacher: '은종서' },
            '5교시': { subject: '생명과학 2', room: '3-6', teacher: '최효주' },
            '6교시': { subject: '일본어 2', room: '위국실', teacher: '정주리' },
            '7교시': null, '8교시': null, '9교시': null, '10교시': null
        },
        '수': {
            '1교시': { subject: '고전윤리', room: '3-2', teacher: '조현응' },
            '2교시': { subject: '사회과제연구', room: '1 전용실', teacher: '김현욱' },
            '3교시': { subject: '심화국어', room: '3-6', teacher: '김재린' },
            '4교시': { subject: '화법과 작문', room: '3-8', teacher: '주영진' },
            '5교시': { subject: '생명과학 2', room: '3-6', teacher: '최효주' },
            '6교시': { subject: '심화 영어 독해', room: '3-7', teacher: '서버들' },
            '7교시': { subject: '확률과 통계 자율 방과후', room: '3-2', teacher: '신소운' },
            '8교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' },
            '9교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' },
            '10교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' }
        },
        '목': {
            '1교시': { subject: '심화국어', room: '3-6', teacher: '김재린' },
            '2교시': { subject: '사회문화', room: '3 전용실', teacher: '은종서' },
            '3교시': { subject: '심화 영어 독해', room: '3-7', teacher: '서버들' },
            '4교시': { subject: '화법과 작문', room: '3-8', teacher: '주영진' },
            '5교시': { subject: '미술 창작', room: '미술실', teacher: '전아영' },
            '6교시': { subject: '확률과 통계', room: '3 전용실', teacher: '신소운' },
            '7교시': { subject: '일본어 2', room: '위국실', teacher: '정주리' },
            '8교시': null, '9교시': null, '10교시': null
        },
        '금': {
            '1교시': { subject: '확률과 통계', room: '3 전용실', teacher: '신소운' },
            '2교시': { subject: '스포츠 생활', room: '체육관', teacher: '이정우' },
            '3교시': { subject: '진로', room: '3-6', teacher: '고익준' },
            '4교시': { subject: '동아리', room: '애인실', teacher: '주영진' },
            '5교시': { subject: '화법과 작문', room: '3-8', teacher: '주영진' },
            '6교시': { subject: '생명과학 2', room: '3-6', teacher: '최효주' },
            '7교시': { subject: '영어 독해와 작문', room: '위국실', teacher: '서버들' },
            '8교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' },
            '9교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' },
            '10교시': { subject: '야간자율학습', room: '3 전용실', teacher: '-' }
        }
    };

    const timeSlots = [
        { name: '1교시', start: [8, 40], end: [9, 30], timeStr: '08:40' },
        { name: '2교시', start: [9, 40], end: [10, 30], timeStr: '09:40' },
        { name: '3교시', start: [10, 40], end: [11, 30], timeStr: '10:40' },
        { name: '4교시', start: [11, 40], end: [12, 30], timeStr: '11:40' },
        { name: '점심', start: [12, 30], end: [13, 30], timeStr: '12:30' },
        { name: '5교시', start: [13, 30], end: [14, 20], timeStr: '13:30' },
        { name: '6교시', start: [14, 30], end: [15, 20], timeStr: '14:30' },
        { name: '청소', start: [15, 20], end: [15, 40], timeStr: '15:20' },
        { name: '7교시', start: [15, 40], end: [16, 30], timeStr: '15:40' },
        { name: '8교시', start: [16, 40], end: [17, 30], timeStr: '16:40' },
        { name: '저녁', start: [17, 30], end: [18, 30], timeStr: '17:30' },
        { name: '9교시', start: [18, 30], end: [20, 10], timeStr: '18:30' },
        { name: '10교시', start: [20, 20], end: [21, 30], timeStr: '20:20' }
    ];

    const dayKeys = ['월', '화', '수', '목', '금'];

    function generateTableHTML() {
        const tbody = document.getElementById('timetable-body');
        let html = '';

        timeSlots.forEach(slot => {
            html += `<tr>`;
            html += `<td style="font-weight:bold; background-color:#222 !important; color:#fff !important;">${slot.name}<br><span style="font-size:0.65rem; color:#888;">${slot.timeStr}</span></td>`;
            
            dayKeys.forEach(day => {
                if (['점심', '저녁', '청소'].includes(slot.name)) {
                    html += `<td class="cell-${day}" style="color:#555 !important; font-size:0.75rem; background-color:#151515 !important;">${slot.name}</td>`;
                } else {
                    const data = timetableData[day][slot.name];
                    if (data) {
                        const tInfo = (data.teacher === '-' || !data.teacher) ? '' : ` • ${data.teacher}`;
                        html += `<td class="cell-${day}"><strong>${data.subject}</strong><br><span class="info-text">${data.room}${tInfo}</span></td>`;
                    } else {
                        html += `<td class="cell-${day}" style="color:#444 !important;">-</td>`;
                    }
                }
            });
            html += `</tr>`;
        });
        tbody.innerHTML = html;
    }

    function updateTimetable() {
        const now = new Date();
        const days = ['일', '월', '화', '수', '목', '금', '토'];
        const dayName = days[now.getDay()];
        const hours = now.getHours();
        const minutes = now.getMinutes();
        const seconds = now.getSeconds();

        dayKeys.forEach(day => {
            const cells = document.querySelectorAll(`.cell-${day}`);
            const th = document.getElementById(`th-${day}`);
            if (day === dayName) {
                cells.forEach(c => c.classList.add('today-highlight'));
                if (th) th.classList.add('today-highlight');
            } else {
                cells.forEach(c => c.classList.remove('today-highlight'));
                if (th) th.classList.remove('today-highlight');
            }
        });

        const timeString = `${now.getMonth() + 1}월 ${now.getDate()}일 (${dayName}) ${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        document.getElementById('current-time-display').innerText = timeString;

        if (dayName === '토' || dayName === '일') {
            showFreeTime("즐거운 주말! ✨", "푹 쉬고 재충전하세요.");
            return;
        }

        const currentTotalSeconds = (hours * 3600) + (minutes * 60) + seconds;
        let currentSlot = null;

        for (const slot of timeSlots) {
            const startTotalSeconds = (slot.start[0] * 3600) + (slot.start[1] * 60);
            const endTotalSeconds = (slot.end[0] * 3600) + (slot.end[1] * 60);

            if (currentTotalSeconds >= startTotalSeconds && currentTotalSeconds < endTotalSeconds) {
                currentSlot = slot;
                
                const remainSecs = endTotalSeconds - currentTotalSeconds;
                const rMin = Math.floor(remainSecs / 60);
                const rSec = remainSecs % 60;
                
                document.getElementById('timer-display').innerText = `⏱️ 종료까지 ${rMin}분 ${rSec}초 남음`;
                break;
            }
        }

        if (currentSlot) {
            document.getElementById('period-label').innerText = currentSlot.name;

            if (['점심', '저녁', '청소'].includes(currentSlot.name)) {
                document.getElementById('subject-display').innerText = currentSlot.name;
                document.getElementById('info-display').innerText = "맛있게 먹고 푹 쉬기! 🎈";
                return;
            }

            const todaySchedule = timetableData[dayName];
            if (todaySchedule && todaySchedule[currentSlot.name]) {
                const info = todaySchedule[currentSlot.name];
                document.getElementById('subject-display').innerText = info.subject;
                if (info.teacher === '-' || !info.teacher) {
                    document.getElementById('info-display').innerText = `📍 위치: ${info.room}`;
                } else {
                    document.getElementById('info-display').innerText = `👤 ${info.teacher} 선생님 | 📍 ${info.room}`;
                }
            } else {
                document.getElementById('subject-display').innerText = "일정 없음";
                document.getElementById('info-display').innerText = "자유 시간 또는 빠른 귀가! 🏡";
            }
        } else {
            showFreeTime("정규 시간 외", "현재는 일과 외 시간입니다. 자유 시간! ✨");
        }
    }

    function showFreeTime(label, message) {
        document.getElementById('period-label').innerText = label;
        document.getElementById('subject-display').innerText = "자유 시간";
        document.getElementById('info-display').innerText = message;
        document.getElementById('timer-display').innerText = "⏱️ 운영 시간 아님";
    }

    generateTableHTML();
    updateTimetable();
    setInterval(updateTimetable, 1000);
</script>

</body>
</html>
