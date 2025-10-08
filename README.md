<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>แบ่งเวลาสร้างA | AI Time Balance Planner</title>
    <link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+Thai:wght@300;400;500;600;700&family=Montserrat:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        /* ==================================== */
        /* === COLOR PALETTE & GLOBAL STYLES (Minimalist) === */
        /* ==================================== */
        :root {
            /* สีพื้นฐาน */
            --color-white: #FFFFFF;
            --color-black: #212121;
            --color-light-gray: #F0F2F5;   /* พื้นหลังอ่อนๆ */
            --color-medium-gray: #B0BEC5;  /* เส้นขอบ/ข้อความรอง */
            --color-dark-gray: #424242;    /* ข้อความหลัก */
            
            /* สีเน้น (Light Blue Palette) */
            --color-primary-blue: #03A9F4;  /* Deep Sky Blue */
            --color-accent-light-blue: #81D4FA; /* Lighter Accent Blue */
            --color-class: #0288D1;         /* Class (Darker Blue) */
            --color-break: #4CAF50;         /* Break (Fresh Green) */
            --color-read: #FFC107;          /* Read (Amber Yellow) */
            --color-danger: #F44336;        /* Red */
            
            --spacing-unit: 16px;
            --border-radius-minimal: 4px;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'IBM Plex Sans Thai', sans-serif;
        }

        body {
            background-color: var(--color-light-gray);
            color: var(--color-dark-gray);
            line-height: 1.6;
            min-height: 100vh;
            overflow-x: hidden;
            font-weight: 400;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: var(--spacing-unit) * 1.5;
        }

        /* -------------------------------------- */
        /* START PAGE (Minimalist) */
        /* -------------------------------------- */
        .start-page {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--color-white);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: var(--color-black);
            text-align: center;
            transition: opacity 0.8s ease-in-out;
            z-index: 1000;
        }

        .start-page.hidden {
            opacity: 0;
            pointer-events: none;
        }

        .start-page h1 {
            font-family: 'Montserrat', sans-serif;
            font-size: 4.5em; 
            margin-bottom: 10px;
            color: var(--color-primary-blue); 
            font-weight: 700;
            letter-spacing: -1px;
        }

        .start-page p {
            font-size: 1.5em;
            margin-bottom: 50px; 
            color: var(--color-dark-gray);
            font-weight: 300;
        }

        .start-page .start-button {
            background-color: var(--color-primary-blue); 
            color: var(--color-white);
            border: none;
            padding: 15px 40px;
            border-radius: var(--border-radius-minimal);
            font-size: 1.5em;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            letter-spacing: 0.5px;
            text-transform: uppercase;
            box-shadow: 0 4px 10px rgba(3, 169, 244, 0.3);
        }

        .start-page .start-button:hover {
            background-color: #0288D1; /* Darker blue on hover */
            transform: translateY(-2px); 
            box-shadow: 0 6px 15px rgba(3, 169, 244, 0.4);
        }

        /* -------------------------------------- */
        /* MAIN CONTENT STYLES (Minimalist) */
        /* -------------------------------------- */

        .header {
            padding: var(--spacing-unit) * 2 0;
            text-align: center;
            border-bottom: 1px solid var(--color-medium-gray);
            margin-bottom: var(--spacing-unit) * 2;
        }

        .header h1 {
            color: var(--color-black);
            font-family: 'Montserrat', sans-serif;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .header p {
            color: var(--color-medium-gray);
            font-weight: 300;
        }

        .dashboard {
            display: flex;
            flex-wrap: wrap; /* ให้ปุ่มลงบรรทัดใหม่ได้ถ้าหน้าจอเล็ก */
            justify-content: space-between;
            align-items: center;
            margin-bottom: var(--spacing-unit) * 2;
            padding: var(--spacing-unit);
            background-color: var(--color-white); 
            border-radius: var(--border-radius-minimal);
            border: 1px solid var(--color-light-gray);
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05); 
        }

        .dashboard p {
            color: var(--color-dark-gray);
            font-weight: 500;
            flex-basis: 100%; /* ให้ข้อความอยู่คนเดียว 1 บรรทัด */
            margin-bottom: var(--spacing-unit);
        }
        
        .ai-controls {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .ai-controls button {
            border: none;
            padding: 10px 18px;
            border-radius: var(--border-radius-minimal);
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            font-size: 0.9em;
            text-transform: uppercase;
        }

        .ai-controls .btn-primary {
            background-color: var(--color-primary-blue);
            color: var(--color-white);
        }
        
        .ai-controls .btn-primary:hover {
             background-color: #0288D1;
             transform: translateY(-1px);
             box-shadow: 0 2px 5px rgba(3, 169, 244, 0.2);
        }
        
        .ai-controls .btn-danger {
            background-color: var(--color-danger);
            color: var(--color-white);
        }
        
        .ai-controls .btn-danger:hover {
            background-color: #D32F2F; /* Darker red */
            transform: translateY(-1px);
            box-shadow: 0 2px 5px rgba(244, 67, 54, 0.2);
        }

        /* --- Legend (คำอธิบายสี) --- */
        .legend {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-bottom: var(--spacing-unit) * 1.5;
            padding: var(--spacing-unit) 0;
            justify-content: center;
            border-bottom: 1px solid var(--color-medium-gray);
            border-top: 1px solid var(--color-medium-gray);
            font-size: 0.9em;
        }
        
        .legend-item { color: var(--color-dark-gray); display: flex; align-items: center; } 
        .color-box { width: 16px; height: 16px; border-radius: 2px; margin-right: 8px; border: 1px solid rgba(0,0,0,0.1); }
        .color-box.class { background-color: var(--color-class); }
        .color-box.break { background-color: var(--color-break); }
        .color-box.read { background-color: var(--color-read); }


        /* --- ตาราง (Minimalist) --- */
        .schedule-table-wrapper {
            overflow-x: auto;
            border: 1px solid var(--color-medium-gray);
            border-radius: var(--border-radius-minimal);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
            background-color: var(--color-white);
        }

        .schedule-table {
            width: 100%;
            border-collapse: collapse;
            min-width: 1400px; 
            background-color: var(--color-white);
            color: var(--color-dark-gray);
            text-align: center;
            font-size: 0.9em;
        }
        
        .schedule-table th {
            width: 100px;
            background-color: var(--color-light-gray); 
            border-bottom: 1px solid var(--color-medium-gray);
            font-weight: 600;
            padding: 12px 0;
            color: var(--color-black);
        }
        
        .day-column {
            width: 120px; 
            background-color: #E3F2FD !important; /* ฟ้าอ่อนมากสำหรับคอลัมน์วัน */
            color: var(--color-black) !important;
            font-weight: 600;
            border-right: 1px solid var(--color-medium-gray) !important;
        }

        .schedule-table td {
            width: 100px; 
            padding: 0;
            border: 1px solid var(--color-light-gray); 
        }
        
        .time-slot {
            display: flex;
            height: 50px; 
            width: 100%;
        }

        .half-hour-slot {
            flex: 1;
            border-right: 1px solid var(--color-light-gray); 
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.7em; 
            font-weight: 400;
            color: var(--color-medium-gray);
            transition: background-color 0.1s;
            text-align: center;
            padding: 0 2px;
        }
        
        .half-hour-slot:hover:not(.selected-user):not(.ai-break-time):not(.ai-reading-time) {
            background-color: #EBF5FB; /* สีฟ้าอ่อนๆ บน hover */
        }
        
        .half-hour-slot:last-child {
            border-right: none;
        }
        
        /* สีของช่องเวลา (Minimalist) */
        .selected-user { 
            background-color: var(--color-class) !important; 
            color: var(--color-white) !important; 
            font-weight: 500 !important; 
            border-color: var(--color-class) !important;
        }
        .ai-break-time { 
            background-color: var(--color-break) !important; 
            color: var(--color-white) !important; 
            font-weight: 500 !important; 
            border-color: var(--color-break) !important;
        }
        .ai-reading-time { 
            background-color: var(--color-read) !important; 
            color: var(--color-white) !important; 
            font-weight: 500 !important; 
            border-color: var(--color-read) !important;
        }
        
        /* --- Recommendation Section (Minimalist) --- */
        .recommendation-section { margin-top: var(--spacing-unit) * 3; }
        .recommendation-section h2 {
            color: var(--color-black);
            font-family: 'Montserrat', sans-serif;
            margin-bottom: var(--spacing-unit);
            font-weight: 600;
            border-bottom: 1px solid var(--color-light-gray);
            padding-bottom: var(--spacing-unit) / 2;
        }
        
        .recommendation-section p {
            color: var(--color-dark-gray);
            margin-bottom: var(--spacing-unit);
        }

        .store-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: var(--spacing-unit) * 1.5;
            margin-top: var(--spacing-unit);
        }

        .store-card {
            background-color: var(--color-white); 
            border: 1px solid var(--color-light-gray);
            border-radius: var(--border-radius-minimal);
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }
        
        .store-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }
        
        /* ภาพประกอบใน Store Card (ถ้ามี) */
        .store-image-container {
            width: 100%;
            height: 150px; /* กำหนดความสูงคงที่ */
            overflow: hidden;
            background-color: var(--color-light-gray); /* สีสำรองถ้าไม่มีภาพ */
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .store-image {
            width: 100%;
            height: 100%;
            object-fit: cover; /* ครอบคลุมพื้นที่แต่ไม่ยืดเสียสัดส่วน */
            display: block;
        }
        /* ลบภาพ placeholder ที่ใส่ไปแล้วก็ได้ถ้าไม่ต้องการ */
        .store-image[src*="placeholder"] {
            object-fit: contain; /* ปรับให้แสดงข้อความ placeholder ได้ดี */
            padding: 10px;
        }

        .store-info { 
            padding: var(--spacing-unit);
            flex-grow: 1;
            display: flex;
            flex-direction: column;
        }
        
        .store-card h3 {
            color: var(--color-black);
            font-family: 'Montserrat', sans-serif;
            margin-bottom: 8px;
            font-weight: 600;
        }
        
        .store-card p {
            color: var(--color-dark-gray);
            font-size: 0.9em;
            margin-bottom: var(--spacing-unit);
            flex-grow: 1; /* ทำให้ข้อความ p ยืดได้ เพื่อให้ปุ่มอยู่ด้านล่างสุดเท่ากัน */
        }
        
        .store-link {
            display: inline-block;
            margin-top: auto; /* ดันปุ่มไปด้านล่างสุด */
            color: var(--color-white);
            background-color: var(--color-primary-blue);
            padding: 8px 12px;
            border-radius: var(--border-radius-minimal);
            text-decoration: none;
            font-weight: 500;
            font-size: 0.9em;
            transition: background-color 0.2s;
            align-self: flex-start; /* จัดปุ่มให้อยู่ซ้ายสุด */
        }
        
        .store-link:hover {
            background-color: #0288D1;
        }
        
    </style>
</head>
<body>

<div id="startPage" class="start-page">
    <h1>AI Time Planner</h1>
    <p>เครื่องมือช่วยจัดตารางเวลาเพื่อชีวิตที่สมดุล</p>
    <button class="start-button" onclick="hideStartPage()">เริ่มต้นวางแผน</button>
</div>

<div id="mainContent" class="main-content" style="display: none;">
    <div class="container">
        <header class="header">
            <h1>AI Time Planner</h1>
            <p>วางแผนการเรียนและชีวิตของคุณได้อย่างมีประสิทธิภาพ</p>
        </header>

        <section class="dashboard">
            <p>
                ขั้นตอนที่ 1: คลิกเลือกช่องตารางเรียนของคุณ | ขั้นตอนที่ 2: กดให้ AI จัดตาราง
            </p>
            <div class="ai-controls">
                <button class="btn-primary" onclick="openGoalInput()">🎯 ดูเป้าหมาย</button>
                <button class="btn-primary" onclick="runAIScheduling()">🧠 ให้ AI จัดตาราง</button>
                <button class="btn-danger" onclick="clearSchedule()">🗑️ ล้างตารางทั้งหมด</button>
            </div>
        </section>
        
        <div class="legend">
            <div class="legend-item"><div class="color-box class"></div>ตารางเรียน</div>
            <div class="legend-item"><div class="color-box break"></div>เวลาพักผ่อน</div>
            <div class="legend-item"><div class="color-box read"></div>เวลาอ่านหนังสือ</div>
        </div>

        <section class="schedule-section">
            <div class="schedule-table-wrapper">
                <table class="schedule-table">
                    <thead>
                        <tr>
                            <th class="day-column">วัน / เวลา</th>
                            <th>8:00-9:00</th><th>9:00-10:00</th><th>10:00-11:00</th><th>11:00-12:00</th>
                            <th>12:00-13:00</th><th>13:00-14:00</th><th>14:00-15:00</th><th>15:00-16:00</th>
                            <th>16:00-17:00</th><th>17:00-18:00</th><th>18:00-19:00</th><th>19:00-20:00</th>
                        </tr>
                    </thead>
                    <tbody id="schedule-body">
                        <tr>
                            <td class="day-column">จันทร์</td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="800"></div><div class="half-hour-slot" data-day="M" data-time="830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="900"></div><div class="half-hour-slot" data-day="M" data-time="930"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1000"></div><div class="half-hour-slot" data-day="M" data-time="1030"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1100"></div><div class="half-hour-slot" data-day="M" data-time="1130"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1200"></div><div class="half-hour-slot" data-day="M" data-time="1230"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1300"></div><div class="half-hour-slot" data-day="M" data-time="1330"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1400"></div><div class="half-hour-slot" data-day="M" data-time="1430"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1500"></div><div class="half-hour-slot" data-day="M" data-time="1530"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1600"></div><div class="half-hour-slot" data-day="M" data-time="1630"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1700"></div><div class="half-hour-slot" data-day="M" data-time="1730"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1800"></div><div class="half-hour-slot" data-day="M" data-time="1830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="M" data-time="1900"></div><div class="half-hour-slot" data-day="M" data-time="1930"></div></div></td>
                        </tr>
                        
                        <tr>
                            <td class="day-column">อังคาร</td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="800"></div><div class="half-hour-slot" data-day="T" data-time="830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="900"></div><div class="half-hour-slot" data-day="T" data-time="930"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1000"></div><div class="half-hour-slot" data-day="T" data-time="1030"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1100"></div><div class="half-hour-slot" data-day="T" data-time="1130"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1200"></div><div class="half-hour-slot" data-day="T" data-time="1230"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1300"></div><div class="half-hour-slot" data-day="T" data-time="1330"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1400"></div><div class="half-hour-slot" data-day="T" data-time="1430"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1500"></div><div class="half-hour-slot" data-day="T" data-time="1530"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1600"></div><div class="half-hour-slot" data-day="T" data-time="1630"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1700"></div><div class="half-hour-slot" data-day="T" data-time="1730"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1800"></div><div class="half-hour-slot" data-day="T" data-time="1830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="T" data-time="1900"></div><div class="half-hour-slot" data-day="T" data-time="1930"></div></div></td>
                        </tr>
                        
                        <tr>
                            <td class="day-column">พุธ</td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="800"></div><div class="half-hour-slot" data-day="W" data-time="830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="900"></div><div class="half-hour-slot" data-day="W" data-time="930"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1000"></div><div class="half-hour-slot" data-day="W" data-time="1030"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1100"></div><div class="half-hour-slot" data-day="W" data-time="1130"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1200"></div><div class="half-hour-slot" data-day="W" data-time="1230"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1300"></div><div class="half-hour-slot" data-day="W" data-time="1330"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1400"></div><div class="half-hour-slot" data-day="W" data-time="1430"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1500"></div><div class="half-hour-slot" data-day="W" data-time="1530"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1600"></div><div class="half-hour-slot" data-day="W" data-time="1630"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1700"></div><div class="half-hour-slot" data-day="W" data-time="1730"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1800"></div><div class="half-hour-slot" data-day="W" data-time="1830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="W" data-time="1900"></div><div class="half-hour-slot" data-day="W" data-time="1930"></div></div></td>
                        </tr>
                        
                        <tr>
                            <td class="day-column">พฤหัสบดี</td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="800"></div><div class="half-hour-slot" data-day="Th" data-time="830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="900"></div><div class="half-hour-slot" data-day="Th" data-time="930"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1000"></div><div class="half-hour-slot" data-day="Th" data-time="1030"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1100"></div><div class="half-hour-slot" data-day="Th" data-time="1130"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1200"></div><div class="half-hour-slot" data-day="Th" data-time="1230"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1300"></div><div class="half-hour-slot" data-day="Th" data-time="1330"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1400"></div><div class="half-hour-slot" data-day="Th" data-time="1430"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1500"></div><div class="half-hour-slot" data-day="Th" data-time="1530"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1600"></div><div class="half-hour-slot" data-day="Th" data-time="1630"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1700"></div><div class="half-hour-slot" data-day="Th" data-time="1730"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1800"></div><div class="half-hour-slot" data-day="Th" data-time="1830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Th" data-time="1900"></div><div class="half-hour-slot" data-day="Th" data-time="1930"></div></div></td>
                        </tr>
                        
                        <tr>
                            <td class="day-column">ศุกร์</td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="800"></div><div class="half-hour-slot" data-day="F" data-time="830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="900"></div><div class="half-hour-slot" data-day="F" data-time="930"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1000"></div><div class="half-hour-slot" data-day="F" data-time="1030"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1100"></div><div class="half-hour-slot" data-day="F" data-time="1130"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1200"></div><div class="half-hour-slot" data-day="F" data-time="1230"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1300"></div><div class="half-hour-slot" data-day="F" data-time="1330"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1400"></div><div class="half-hour-slot" data-day="F" data-time="1430"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1500"></div><div class="half-hour-slot" data-day="F" data-time="1530"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1600"></div><div class="half-hour-slot" data-day="F" data-time="1630"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1700"></div><div class="half-hour-slot" data-day="F" data-time="1730"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1800"></div><div class="half-hour-slot" data-day="F" data-time="1830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="F" data-time="1900"></div><div class="half-hour-slot" data-day="F" data-time="1930"></div></div></td>
                        </tr>
                        
                        <tr>
                            <td class="day-column">เสาร์</td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="800"></div><div class="half-hour-slot" data-day="Sa" data-time="830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="900"></div><div class="half-hour-slot" data-day="Sa" data-time="930"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1000"></div><div class="half-hour-slot" data-day="Sa" data-time="1030"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1100"></div><div class="half-hour-slot" data-day="Sa" data-time="1130"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1200"></div><div class="half-hour-slot" data-day="Sa" data-time="1230"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1300"></div><div class="half-hour-slot" data-day="Sa" data-time="1330"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1400"></div><div class="half-hour-slot" data-day="Sa" data-time="1430"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1500"></div><div class="half-hour-slot" data-day="Sa" data-time="1530"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1600"></div><div class="half-hour-slot" data-day="Sa" data-time="1630"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1700"></div><div class="half-hour-slot" data-day="Sa" data-time="1730"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1800"></div><div class="half-hour-slot" data-day="Sa" data-time="1830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Sa" data-time="1900"></div><div class="half-hour-slot" data-day="Sa" data-time="1930"></div></div></td>
                        </tr>
                        
                        <tr>
                            <td class="day-column">อาทิตย์</td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="800"></div><div class="half-hour-slot" data-day="Su" data-time="830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="900"></div><div class="half-hour-slot" data-day="Su" data-time="930"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1000"></div><div class="half-hour-slot" data-day="Su" data-time="1030"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1100"></div><div class="half-hour-slot" data-day="Su" data-time="1130"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1200"></div><div class="half-hour-slot" data-day="Su" data-time="1230"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1300"></div><div class="half-hour-slot" data-day="Su" data-time="1330"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1400"></div><div class="half-hour-slot" data-day="Su" data-time="1430"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1500"></div><div class="half-hour-slot" data-day="Su" data-time="1530"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1600"></div><div class="half-hour-slot" data-day="Su" data-time="1630"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1700"></div><div class="half-hour-slot" data-day="Su" data-time="1730"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1800"></div><div class="half-hour-slot" data-day="Su" data-time="1830"></div></div></td>
                            <td><div class="time-slot"><div class="half-hour-slot" data-day="Su" data-time="1900"></div><div class="half-hour-slot" data-day="Su" data-time="1930"></div></div></td>
                        </tr>
                        
                    </tbody>
                </table>
            </div>
        </section>
        
        
        
        <section class="recommendation-section">
            <h2>แนะนำสถานที่พักผ่อนและร้านอาหารใกล้มหาวิทยาลัยขอนแก่น</h2>
            <p>ใช้เวลาว่างที่ AI จัดสรรให้ไปผ่อนคลายในสถานที่เหล่านี้ เพื่อให้ชีวิตสมดุลอย่างแท้จริง!</p>

            <div class="store-grid">
                
                <div class="store-card">
                    <div class="store-image-container">
                        <img src="https://img.wongnai.com/p/1920x0/2021/08/03/731dae8da3374694bf6b02b66a5af58d.jpg" alt="Board Game Everyday Cafe" class="store-image">
                    </div>
                    <div class="store-info">
                        <h3>Board Game Everyday Cafe (คาเฟ่บอร์ดเกม)</h3>
                        <p>ร้านบอร์ดเกมยอดนิยมหลัง มข. มีเกมให้เลือกเยอะกว่า 200 เกม พร้อม GM คอยแนะนำ เหมาะสำหรับไปใช้เวลาว่างกับเพื่อน ๆ </p>
                        <a href="https://www.google.com/maps/search/Board+Game+Everyday+Cafe+ขอนแก่น" target="_blank" class="store-link">ดูบน Google Map</a>
                    </div>
                </div>
                
                <div class="store-card">
                    <div class="store-image-container">
                        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRhUxvzhRKambCCH5UgEkShKpybOcgJi8U08w&s" alt="นายตอง หมูกระทะ" class="store-image">
                    </div>
                    <div class="store-info">
                        <h3>นายตองหมูกระทะ (บุฟเฟต์หลัง มข.)</h3>
                        <p>บุฟเฟต์หมูกระทะราคาดีมากเพียง 189 บาท (รวมน้ำ) ร้านใหญ่ โล่งสบาย ย่านโนนม่วง หลัง มข. สายปิ้งย่างไม่ควรพลาด!</p>
                        <a href="https://www.google.com/maps/place/%E0%B8%99%E0%B8%B2%E0%B8%A2%E0%B8%95%E0%B8%AD%E0%B8%87+%E0%B8%AB%E0%B8%A1%E0%B8%B9%E0%B8%81%E0%B8%A3%E0%B8%B0%E0%B8%97%E0%B8%B0+%E0%B8%AA%E0%B8%B2%E0%B8%82%E0%B8%B2%E0%B9%82%E0%B8%99%E0%B8%99%E0%B8%A1%E0%B9%88%E0%B8%A7%E0%B8%87/@16.4891107,102.8209402,17z/data=!3m1!4b1!4m6!3m5!1s0x31228b00436e410d:0xeeb6c56f7db9bb6c!8m2!3d16.4891107!4d102.8209402!16s%2Fg%2F11xmrqfftw?entry=ttu&g_ep=EgoyMDI1MTAwMS4wIKXMDSoASAFQAw%3D%3D" target="_blank" class="store-link">ดูบน Google Map</a>
                    </div>
                </div>
                
                <div class="store-card">
                    <div class="store-image-container">
                        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQveZsMViGDi48bv9Svqp5B88NWyfDS_xF55Q&s" alt="Midnight Soon" class="store-image">
                    </div>
                    <div class="store-info">
                        <h3>Midnight Soon (อาหารตามสั่ง/กับข้าว)</h3>
                        <p>ร้านอาหารขวัญใจเด็กมข. ย่านกังสดาล เมนูอร่อยจัดเต็มทุกเมนู เหมาะสำหรับมื้อเย็นกับเพื่อนๆ ในบรรยากาศดี</p>
                        <a href="https://www.google.com/maps/search/Midnight+soon+ร้านอาหาร+กังสดาลมข" target="_blank" class="store-link">ดูบน Google Map</a>
                    </div>
                </div>

                <div class="store-card">
                    <div class="store-image-container">
                        <img src="https://p16-lemon8-sign-sg.tiktokcdn.com/tos-alisg-v-a3e477-sg/oMk6TaQEAXtUAQcIaCFoE6EDgUUfABACfXn9d0~tplv-sdweummd6v-text-logo-v1:QHBpa3Vua2Vhdw==:q75.jpeg?lk3s=c7f08e79&source=lemon8_seo&x-expires=1762214400&x-signature=1hGxe6MBaiQEY54ZDjvPWy3Q1UE%3D" alt="แซ่บยกครก" class="store-image">
                    </div>
                    <div class="store-info">
                        <h3>แซ่บยกครก ขอนแก่น (ร้านส้มตำ)</h3>
                        <p>ร้านส้มตำเปิดใหม่รสชาติจัดจ้านนัวมาก! ปลาร้านัว ปลาเผาแซ่บสุด ๆ ตั้งอยู่ตรงข้ามแจ่วฮ้อนอภิรมย์ ใกล้ มข.</p>
                        <a href="https://www.google.com/maps/search/ร้านแซ่บยกครกขอนแก่น" target="_blank" class="store-link">ดูบน Google Map</a>
                    </div>
                </div>

            </div>
        </section>
    </div>
</div>

<script>
    // เป้าหมายอัปเดตใหม่ตามการคำนวณที่เหมาะสม
    const TOTAL_READING_GOAL_SLOTS = 20; // 10 ชั่วโมง/สัปดาห์ (20 ช่อง 30 นาที)
    const TOTAL_BREAK_GOAL_SLOTS = 12;   // 6 ชั่วโมง/สัปดาห์ (12 ช่อง 30 นาที)
    
    // กำหนดขอบเขตเวลาที่สนใจ
    const DAYS_TO_SCHEDULE = ['M', 'T', 'W', 'Th', 'F', 'Sa', 'Su'];
    
    let slots = [];

    document.addEventListener('DOMContentLoaded', () => {
        slots = document.querySelectorAll('.half-hour-slot');
        setupSlotListeners();
        clearAISlots(); 
    });

    // --- ฟังก์ชันช่วยเหลือสำหรับการแสดงผลเวลา ---
    const formatTimeCode = (code) => {
        const timeStr = String(code).padStart(4, '0');
        return `${timeStr.substring(0, 2)}:${timeStr.substring(2)}`;
    };

    const getSlotTimeRange = (slot) => {
        const timeCode = parseInt(slot.getAttribute('data-time'));
        const startTime = formatTimeCode(timeCode);
        const endTime = formatTimeCode(timeCode + 30);
        return `(${startTime}-${endTime})`;
    };
    // ---------------------------------------------

    const setupSlotListeners = () => {
        slots.forEach(slot => {
            slot.removeEventListener('click', handleSlotClick);
            slot.addEventListener('click', handleSlotClick);
        });
    };

    const handleSlotClick = function() {
        const slot = this;
        
        // ห้ามคลิกทับช่องที่ AI จัดสรร
        if (slot.classList.contains('ai-reading-time') || slot.classList.contains('ai-break-time')) {
             return;
        }

        const isSelected = slot.classList.contains('selected-user');
        
        if (isSelected) {
            slot.classList.remove('selected-user');
            slot.textContent = ''; 
        } else {
            slot.classList.add('selected-user');
            // แสดงช่วงเวลาเมื่อเลือกตารางเรียน
            slot.textContent = `เรียน ${getSlotTimeRange(slot)}`; 
        }
    };

    const clearAISlots = () => {
        slots.forEach(slot => {
            if (!slot.classList.contains('selected-user')) {
                slot.classList.remove('ai-reading-time', 'ai-break-time');
                slot.textContent = '';
            } else {
                 // ตรวจสอบและอัปเดตการแสดงผลเวลาของช่อง Class ด้วย
                if (slot.textContent.includes('เรียน')) {
                    slot.textContent = `เรียน ${getSlotTimeRange(slot)}`;
                }
            }
        });
    }

    // ฟังก์ชันล้างตารางทั้งหมด
    window.clearSchedule = () => {
        if (confirm('คุณต้องการล้างข้อมูลตารางเรียน และตารางที่ AI จัดสรรทั้งหมดทั้งหมดหรือไม่?')) {
            slots.forEach(slot => {
                slot.classList.remove('selected-user', 'ai-reading-time', 'ai-break-time');
                slot.textContent = '';
            });
            setupSlotListeners(); 
            alert('ล้างตารางเรียบร้อยแล้ว! เริ่มต้นเลือกตารางเรียนใหม่ได้เลย');
        }
    };
    
    // =================================
    // LOGIC สำหรับปุ่ม "ให้ AI จัดตาราง" (ใช้ Random ในช่วงเวลาที่กำหนด)
    // =================================
    window.runAIScheduling = () => {
        clearAISlots();

        let readingSlotsAllocated = 0;
        let breakSlotsAllocated = 0;
        
        // 1. กรองหาช่องว่างที่อยู่ในขอบเขตเวลาที่ต้องการ
        let availableSlots = Array.from(slots).filter(slot => {
            if (slot.classList.contains('selected-user')) {
                return false; // ไม่รวมช่องเรียน
            }
            
            const dayCode = slot.getAttribute('data-day');
            const timeCode = parseInt(slot.getAttribute('data-time'));
            
            // เงื่อนไข: วันธรรมดาต้องเป็นเวลา 17:00 น. เป็นต้นไป
            if (DAYS_TO_SCHEDULE.slice(0, 5).includes(dayCode)) { 
                return timeCode >= 1700; 
            }
            
            // เงื่อนไข: วันเสาร์-อาทิตย์ ใช้ได้ตั้งแต่ 10:00 น. เป็นต้นไป
            if (dayCode === 'Sa' || dayCode === 'Su') {
                return timeCode >= 1000;
            }
            
            return false;
        });

        // 2. สลับลำดับของช่องว่างเพื่อเพิ่มความเป็นสุ่มในการจัดสรร
        availableSlots.sort(() => Math.random() - 0.5); 

        // 3. วนลูปจัดสรรตามช่องว่างที่สลับแล้ว
        availableSlots.forEach(slot => {
            if (slot.classList.length > 1) return; 
            
            const isReading = Math.random() < 0.6; 
            const timeRange = getSlotTimeRange(slot);

            if (isReading && readingSlotsAllocated < TOTAL_READING_GOAL_SLOTS) {
                slot.classList.add('ai-reading-time');
                slot.textContent = `อ่าน ${timeRange}`;
                readingSlotsAllocated++;
            } else if (!isReading && breakSlotsAllocated < TOTAL_BREAK_GOAL_SLOTS) {
                slot.classList.add('ai-break-time');
                slot.textContent = `พักผ่อน ${timeRange}`;
                breakSlotsAllocated++;
            } else if (readingSlotsAllocated < TOTAL_READING_GOAL_SLOTS) {
                slot.classList.add('ai-reading-time');
                slot.textContent = `อ่าน ${timeRange}`;
                readingSlotsAllocated++;
            } else if (breakSlotsAllocated < TOTAL_BREAK_GOAL_SLOTS) {
                slot.classList.add('ai-break-time');
                slot.textContent = `พักผ่อน ${timeRange}`;
                breakSlotsAllocated++;
            }
            
        });
        
        // 4. สรุปผลลัพธ์
        const finalReadingTime = readingSlotsAllocated / 2;
        const finalBreakTime = breakSlotsAllocated / 2;
        
        alert(`
            🎉 AI จัดสรรตารางเรียบร้อย! (เน้นสุ่มช่วงเย็น/วันหยุด)
            
            > เวลาอ่านหนังสือ: ${finalReadingTime} ชั่วโมง (เป้าหมาย ${TOTAL_READING_GOAL_SLOTS / 2} ชม.)
            > เวลาพักผ่อน: ${finalBreakTime} ชั่วโมง (เป้าหมาย ${TOTAL_BREAK_GOAL_SLOTS / 2} ชม.)
            
            *การจัดสรรมีความสุ่ม และช่องตารางแสดงช่วงเวลาแล้ว*
        `);

    };
    
    window.openGoalInput = () => {
        alert(`
            🎯 เป้าหมายการจัดสรรเวลาของ AI:
            
            * อ่านหนังสือ: ${TOTAL_READING_GOAL_SLOTS / 2} ชั่วโมง/สัปดาห์
            * พักผ่อน: ${TOTAL_BREAK_GOAL_SLOTS / 2} ชั่วโมง/สัปดาห์ 
            
            AI จะสุ่มจัดสรรในช่องว่าง:
            - วัน จ.-ศ.: หลัง 17:00 น.
            - วัน ส.-อา.: หลัง 10:00 น.
        `);
    };

    // =================================
    // === JavaScript สำหรับการเปลี่ยนหน้า ===
    // =================================
    const startPage = document.getElementById('startPage');
    const mainContent = document.getElementById('mainContent');

    window.hideStartPage = () => {
        startPage.classList.add('hidden'); // เพิ่ม class hidden เพื่อให้หน้า start จางหายไป
        setTimeout(() => {
            startPage.style.display = 'none'; // ซ่อนหน้า start อย่างสมบูรณ์
            mainContent.style.display = 'block'; // แสดงหน้าหลัก
        }, 800); // 0.8 วินาที เพื่อให้ transition ทำงาน
    };

</script>

</body>
</html>
