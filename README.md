# Timetable09090909
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Real-time Timetable</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/GmarketSansMedium.woff">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'GmarketSansMedium', sans-serif;
        }

        body {
            background-color: #f8f9fa;
            color: #222222;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            background-color: #ffffff;
            border: 3px solid #222222;
            border-radius: 20px;
            box-shadow: 5px 5px 0px #222222;
            width: 100%;
            max-width: 480px;
            padding: 25px;
            text-align: center;
        }

        h1 {
            font-size: 1.5rem;
            margin-bottom: 5px;
            color: #222222;
        }

        .subtitle {
            font-size: 0.9rem;
            color: #666666;
            margin-bottom: 25px;
        }

        /* 현재 교시 정보 카드 */
        .current-card {
            background-color: #eef3ff;
            border: 2px solid #0055ff; /* 포인트 파란색 */
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
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
            font-size: 1.8rem;
            font-weight: bold;
            color: #222222;
            margin-bottom: 8px;
            word-break: keep-all;
        }

        .current-info {
            font-size: 1rem;
            color: #444444;
        }

        /* 시간 및 상태 메시지 */
        .status-box {
            font-size: 0.85rem;
            color: #888888;
            margin-top: 15px;
            border-top: 1px dashed #cccccc;
            padding-top: 15px;
        }

        .highlight-blue {
            color: #0055ff;
            font-weight: bold;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📅 2026 1학기 실시간 시간표</h1>
    <div class="subtitle" id="current-time-display">현재 시간 확인 중...</div>

    <div class="current-card">
        <div class="current-label" id="period-label">확인 중</div>
        <div class="current-subject" id="subject-display">로딩 중...</div>
        <div class="current-info" id="info-display">선생님 | 교실</div>
    </div>

    <div class="status-box">
        ⏱️ 이 페이지는 <span class="highlight-blue">10초마다</span> 자동으로 갱신됩니다.
    </div>
</div>

<script>
    // 입력해주신 요일별 과목, 교실, 선생님 데이터 적용
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
            '6교시': { subject: '일본어 2', room: '위국실', teacher: '정주리' }
            // 7~10교시는 일정 없음으로 자동 인식됩니다.
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
            '7교시': { subject: '일본어 2', room: '위국실', teacher: '정주리' }
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

    // 제공해주신 타임테이블 구성
    const timeSlots = [
        { name: '1교시', start: [8, 40], end: [9, 30] },
        { name: '2교시', start: [9, 40], end: [10, 30] },
        { name: '3교시', start: [10, 40], end: [11, 30] },
        { name: '4교시', start: [11, 40], end: [12, 30] },
        { name: '점심시간', start: [12, 30], end: [13, 30] },
        { name: '5교시', start: [13, 30], end: [14, 20] },
        { name: '6교시', start: [14, 30], end: [15, 20] },
        { name: '청소시간', start: [15, 20], end: [15, 40] },
        { name: '7교시', start: [15, 40], end: [16, 30] },
        { name: '8교시', start: [16, 40], end: [17, 30] },
        { name: '저녁시간', start: [17, 30], end: [18, 30] },
        { name: '9교시', start: [18, 30], end: [20, 10] },
        { name: '10교시', start: [20, 20], end: [21, 30] }
    ];

    function updateTimetable() {
        const now = new Date();
        const days = ['일', '월', '화', '수', '목', '금', '토'];
        const dayName = days[now.getDay()];
        const hours = now.getHours();
        const minutes = now.getMinutes();

        // 실시간 상단 현재 시간 갱신
        const timeString = `${now.getMonth() + 1}월 ${now.getDate()}일 (${dayName}) ${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}`;
        document.getElementById('current-time-display').innerText = timeString;

        // 주말 처리
        if (dayName === '토' || dayName === '일') {
            showFreeTime("즐거운 주말! 🥳");
            return;
        }

        // 현재 시각 분단위 환산
        const currentTotalMinutes = hours * 60 + minutes;
        let currentSlot = null;

        for (const slot of timeSlots) {
            const startTotal = slot.start[0] * 60 + slot.start[1];
            const endTotal = slot.end[0] * 60 + slot.end[1];

            if (currentTotalMinutes >= startTotal && currentTotalMinutes < endTotal) {
                currentSlot = slot;
                break;
            }
        }

        if (currentSlot) {
            document.getElementById('period-label').innerText = currentSlot.name;

            // 공통 고정 시간대 (식사 및 청소)
            if (['점심시간', '저녁시간', '청소시간'].includes(currentSlot.name)) {
                document.getElementById('subject-display').innerText = currentSlot.name;
                document.getElementById('info-display').innerText = "잠시 쉬어가는 시간 🎈";
                return;
            }

            // 일반 일과 교시 매칭
            const todaySchedule = timetableData[dayName];
            if (todaySchedule && todaySchedule[currentSlot.name]) {
                const info = todaySchedule[currentSlot.name];
                document.getElementById('subject-display').innerText = info.subject;
                
                // 선생님 이름이 '-' 이거나 없을 경우 정보 깔끔하게 조정
                if (info.teacher === '-' || !info.teacher) {
                    document.getElementById('info-display').innerText = `📍 교실: ${info.room}`;
                } else {
                    document.getElementById('info-display').innerText = `👤 ${info.teacher} 선생님 | 📍 ${info.room}`;
                }
            } else {
                // 데이터 등록이 안 된 7~10교시 등
                document.getElementById('subject-display').innerText = "일정 없음";
                document.getElementById('info-display').innerText = "자유 시간 또는 귀가 🏡";
            }
        } else {
            // 정규 시간표 시간 외
            showFreeTime("현재는 일과 외 시간입니다. ✨");
        }
    }

    function showFreeTime(message) {
        document.getElementById('period-label').innerText = "정규 시간 외";
        document.getElementById('subject-display').innerText = "자유 시간";
        document.getElementById('info-display').innerText = message;
    }

    // 실행 및 10초 간격 타이머 돌리기
    updateTimetable();
    setInterval(updateTimetable, 10000);
</script>

</body>
</html>
