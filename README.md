<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>스피드 영어 퀴즈 (Week 5~6)</title>
    <style>
        body {
            font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, Roboto, sans-serif;
            background-color: #f0f4f8;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.08);
            max-width: 650px;
            width: 100%;
        }
        h1 { text-align: center; margin-bottom: 20px; color: #1e293b; font-size: 26px; }
        
        /* Week 탭 스타일 */
        .week-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 25px;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 10px;
            justify-content: center;
            flex-wrap: wrap;
        }
        .week-tab {
            padding: 10px 16px;
            border: none;
            background: none;
            font-size: 15px;
            font-weight: bold;
            color: #94a3b8;
            cursor: pointer;
            border-radius: 8px;
            transition: all 0.2s;
        }
        .week-tab.active {
            background-color: #e0f2fe;
            color: #0284c7;
        }
        .week-tab:hover:not(.active) { background-color: #f1f5f9; }

        /* 카테고리 영역 */
        .category-section {
            margin-bottom: 25px;
            background: #f8fafc;
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #e2e8f0;
        }
        .category-title {
            font-size: 18px; font-weight: bold; color: #334155;
            margin-top: 0; margin-bottom: 15px; display: flex; align-items: center; gap: 8px;
        }
        
        /* 순한맛 / 매운맛 버튼 */
        .flavor-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        @media (max-width: 480px) { .flavor-grid { grid-template-columns: 1fr; } }
        .flavor-btn {
            background: white; border: 2px solid #cbd5e1; border-radius: 10px;
            padding: 15px; text-align: left; cursor: pointer; transition: all 0.2s ease;
        }
        .flavor-btn:hover { border-color: #3b82f6; box-shadow: 0 4px 12px rgba(59, 130, 246, 0.15); transform: translateY(-2px); }
        .flavor-title { font-size: 16px; font-weight: bold; display: block; margin-bottom: 6px; }
        .flavor-desc { font-size: 13px; color: #64748b; line-height: 1.4; word-break: keep-all; }
        .t-easy { color: #10b981; } .t-hard { color: #ef4444; }
        .hidden { display: none !important; }
        
        /* 퀴즈 영역 스타일 */
        #quiz-area { display: flex; flex-direction: column; gap: 20px; }
        #question-counter { font-size: 15px; color: #64748b; font-weight: bold; text-align: center;}
        
        .question-box { 
            font-size: 22px; font-weight: bold; word-break: keep-all; line-height: 1.5; 
            background: #f8fafc; padding: 30px 20px; border-radius: 12px;
            border: 2px dashed #cbd5e1; cursor: pointer; transition: background 0.2s; text-align: center;
        }
        .question-box:hover { background: #e2e8f0; }
        .question-box::after { content: "(클릭하여 정답 확인)"; font-size: 13px; color: #94a3b8; display: block; margin-top: 10px; font-weight: normal; }

        .answer-box { 
            background: #ecfdf5; padding: 25px 20px; border-radius: 12px; border: 1px solid #a7f3d0; text-align: left;
        }
        .en-text { font-size: 22px; color: #059669; font-weight: bold; margin-bottom: 15px; line-height: 1.4; word-break: keep-all;}
        .meta-info { font-size: 14px; color: #475569; margin-bottom: 5px; background: #fff; display: inline-block; padding: 4px 10px; border-radius: 20px; border: 1px solid #e2e8f0; }
        .meaning-info { font-size: 15px; color: #b45309; margin-bottom: 15px; background: #fef3c7; padding: 8px 12px; border-radius: 8px; font-weight: 500;}
        
        .controls { display: flex; justify-content: space-between; align-items: center; margin-top: 10px; flex-wrap: wrap; gap: 10px;}
        .left-controls { display: flex; gap: 10px; }
        
        /* 버튼 스타일 */
        .btn { padding: 10px 20px; border: none; border-radius: 8px; cursor: pointer; font-weight: bold; font-size: 15px; transition: 0.2s;}
        .btn-tts { background-color: #8b5cf6; color: white; display: flex; align-items: center; gap: 5px; }
        .btn-tts:hover { background-color: #7c3aed; }
        .btn-star { background-color: #e2e8f0; color: #475569; }
        .btn-star.active { background-color: #fef08a; color: #ca8a04; border: 1px solid #fde047; }
        .btn-next { background-color: #10b981; color: white; }
        .btn-next:hover { background-color: #059669; }
        .btn-finish { background-color: #3b82f6; color: white; width: 100%; padding: 15px; font-size: 16px; margin-top: 10px; }
        .btn-finish:hover { background-color: #2563eb; }
        .btn-home { background-color: #64748b; color: white; width: 100%; padding: 15px; font-size: 16px; margin-top: 20px;}
        .btn-home:hover { background-color: #475569; }

        /* 복습(결과) 영역 스타일 */
        #review-area { display: flex; flex-direction: column; gap: 15px; }
        .review-card { background: #fff; border: 1px solid #e2e8f0; border-radius: 10px; padding: 15px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); text-align: left;}
        .review-ko { font-size: 16px; font-weight: bold; color: #334155; margin-bottom: 8px; }
        .review-en { font-size: 18px; font-weight: bold; color: #059669; margin-bottom: 8px; }
        .review-empty { text-align: center; padding: 40px 20px; font-size: 18px; color: #10b981; font-weight: bold; background: #ecfdf5; border-radius: 12px;}
    </style>
</head>
<body>

<div class="container">
    <h1 id="main-title">🚀 스피드 영어 퀴즈</h1>
    
    <div id="mode-selection">
        <div class="week-tabs">
            <button class="week-tab active" onclick="setWeek(5, this)">Week 5</button>
            <button class="week-tab" onclick="setWeek(6, this)">Week 6</button>
            <button class="week-tab" onclick="setWeek('all', this)">Week 5~6 누적</button>
        </div>

        <div class="category-section">
            <h3 class="category-title">🧩 구동사 (Phrasal Verbs)</h3>
            <div class="flavor-grid">
                <button class="flavor-btn" onclick="startQuiz('phrasal-easy')">
                    <span class="flavor-title t-easy">🟢 순한맛</span>
                    <span class="flavor-desc">구동사 의미별로 짧고 쉬운 문장들이 선별해서 담겨있음</span>
                </button>
                <button class="flavor-btn" onclick="startQuiz('phrasal-hard')">
                    <span class="flavor-title t-hard">🔴 매운맛</span>
                    <span class="flavor-desc">전체 예문에서 선별됨</span>
                </button>
            </div>
        </div>

        <div class="category-section">
            <h3 class="category-title">🗣️ 영어회화 (Conversation)</h3>
            <div class="flavor-grid">
                <button class="flavor-btn" onclick="startQuiz('conv-easy')">
                    <span class="flavor-title t-easy">💬 순한맛</span>
                    <span class="flavor-desc">'대표문장'과 '교재1'만 담고 있음</span>
                </button>
                <button class="flavor-btn" onclick="startQuiz('conv-hard')">
                    <span class="flavor-title t-hard">🔥 매운맛</span>
                    <span class="flavor-desc">전체 예문에서 선별됨</span>
                </button>
            </div>
        </div>
    </div>

    <div id="quiz-area" class="hidden">
        <div id="question-counter">문제 1 / 7</div>
        
        <div class="question-box" id="ko-box" onclick="showAnswer()">
            <span id="ko-text">한국어 문장이 여기에 표시됩니다.</span>
        </div>

        <div class="answer-box hidden" id="answer-section">
            <div class="meta-info" id="source-info">출처: Day001 교재1</div>
            <div class="en-text" id="en-text">English Answer</div>
            <div class="meaning-info hidden" id="meaning-info">의미: </div>
            
            <div class="controls">
                <div class="left-controls">
                    <button class="btn btn-tts" onclick="playTTS()">🔊 듣기</button>
                    <button class="btn btn-star" id="btn-star" onclick="toggleStar()">⭐ 어려워요</button>
                </div>
                <button class="btn btn-next" id="btn-next" onclick="nextQuestion()">다음 문제 ➡</button>
            </div>
        </div>

        <button class="btn btn-finish hidden" id="btn-finish" onclick="showReview()">결과 보기 (오답 노트) 📝</button>
    </div>

    <div id="review-area" class="hidden">
        <h2 style="text-align: center; color: #1e293b; margin-top:0;">⭐ 나의 오답 노트</h2>
        <p style="text-align: center; color: #64748b; font-size: 14px; margin-top:-10px;">어려웠던 문장들을 다시 확인해 보세요!</p>
        
        <div id="review-list"></div>

        <button class="btn btn-home" onclick="resetToHome()">🏠 처음으로 돌아가기</button>
    </div>
</div>

<script>
    let currentWeekFilter = 5; // 기본 시작은 Week 5

    function setWeek(week, btn) {
        currentWeekFilter = week;
        document.querySelectorAll('.week-tab').forEach(el => el.classList.remove('active'));
        btn.classList.add('active');
    }

    // 📌 구동사 전체 의미 매핑 (Week 5 & Week 6)
    const meanings = {
        "find out_1": "find out: (조사나 질문을 통해 몰랐던) 정보를 알아내다",
        "fit in_1": "fit in: (디자인이나 분위기가 주변과) 조화를 이루다",
        "fit in_2": "fit in: (새로운 그룹이나 공동체에) 위화감 없이 적응하다",
        "fit in_3": "fit in: (좁은 공간이나 빽빽한 일정 사이에) 짬을 내어 끼워넣다",
        "get around_1": "get around: (여러 곳을) 돌아다니다, 이동하다",
        "get around_2": "get around: (신체적 불편함을 극복하고) 혼자서 다니다",
        "get around_3": "get around: (소문이나 소식이) 사람들에게 널리 알려지다",
        "get around_4": "get around: (사교 모임 등에) 활발하게 활동하며 다니다",
        "get around_5": "get around: (교묘하게 규정이나 법망을) 피해 가다",
        "get around_6": "get around: (인정하고 싶지 않은 사실을) 부인하거나 회피하다",
        "get around to_1": "get around to: (시간이 없어서 미뤄왔던 일을) 마침내 짬을 내어 하다",
        "get away with_1": "get away with: (물건 등을) 훔쳐서 달아나다",
        "get away with_2": "get away with: (잘못에 대해) 처벌을 받지 않고 그냥 넘어가다",
        "get by_1": "get by: (좁은 공간을 사람이나 차가) 간신히 지나가다",
        "get by_2": "get by: (넉넉하지 않아도) 근근이 먹고살다, 버티다",
        "get by_3": "get by: (부족한 도구나 지식으로) 아쉬운 대로 헤쳐 나가다",
        "get into_1": "get into: (비좁은 곳 등에) 힘들게 비집고 들어가다, 물리적으로 접속/접근하다",
        "get into_2": "get into: (원하는 대학이나 팀에) 합격하여 입학/소속되다",
        "get into_3": "get into: (어떤 것에) 재미가 붙다, 관심이 생기다",
        "get into_4": "get into (it): (무언가를 두고) 싸우다, 논쟁하다",
        "get into_5": "get into: (대화 등에서 특정 주제를) 깊이 다루다, 이야기를 꺼내다",
        "get over_1": "get over: (장애물을) 물리적으로 넘다",
        "get over_2": "get over: (두려움, 상실감 등을) 극복하다",
        "get over_3": "get over: (너무 놀라거나 어이가 없어서) 믿어지지 않다, 얼떨떨하다",
        "go through_1": "go through: (터널이나 산을 물리적으로) 통과하다, 지나가다",
        "go through_2": "go through: (물건을 찾으려고 서랍이나 파일을) 샅샅이 뒤지다",
        "go through_3": "go through: (거래 시 중간에 전문가를) 거쳐 가다, 중개를 끼다",
        "go through_4": "go through: (인생의 힘든 고난이나 상황을) 몸소 겪다, 경험하다",
        "go through_5": "go through: (목록이나 단계를 처음부터 끝까지) 하나하나 살펴보다",
        "go through_6": "go through: (짧은 시간 내에 많은 양을) 소비하다, 써버리다",
        "go through_7": "go through: (부정문에서) 메시지가 전송되지 않거나 결제가 승인되지 않다",
        "hang out_1": "hang out: (사람들과 특별한 목적 없이) 시간을 보내며 놀다, 어울리다",
        "meet up_1": "meet up: (특정 장소와 목적을 강조하여) 만나다",
        "mingle with_1": "mingle with: (파티 등에서 낯선 이와 친해지려고 대화하며) 어울리다, 섞이다",
        "hold back_1": "hold back: (군중이나 물결 등을) 물리적으로 막아내다",
        "hold back_2": "hold back: (성장이나 발전을) 뒤처지게 가로막다, 발목을 잡다",
        "hold back_3": "hold back: (하고 싶은 말을 꾹) 참다, 내뱉지 않다",
        "hold back_4": "hold back: (웃음, 눈물, 생리 현상 등을) 억지로 참다",
        "hold back_5": "hold back: (정보 등을 다 말하지 않고) 일부러 숨기다",
        "hold off_1": "hold off: (적절한 때를 기다리며 결정을) 보류하다, 미루다",
        "hold off_2": "hold off: (다가오는 적이나 공격을) 물리적으로 막다, 저지하다",
        "hold up_1": "hold up: (시각적으로 잘 보이게 사물을) 높이 들다, 갖다 대다",
        "hold up_2": "hold up: (무거운 것이 떨어지지 않게 밑에서) 지탱하다, 받치다",
        "hold up_3": "hold up: (교통 정체 등으로 진행을) 지연시키다, 가로막다",
        "hold up_4": "hold up: (힘든 상황 속에서도 정신적으로) 잘 견디다, 버티다",
        "hold up_5": "hold up: (건물이나 다리 등 사물이 무게/압력을) 잘 견뎌내다",
        "keep up_1": "keep up: (앞서가는 상대를) 뒤처지지 않고 보조를 맞추어 따르다",
        "keep up_2": "keep up: (좋은 상태나 습관을 중단 없이) 계속 유지하다",
        "keep up_3": "keep up: (빠르게 변하는 트렌드나 정보를) 놓치지 않고 접하다",
        "leave out_1": "leave out: (목록이나 재료에서) 포함하지 않고 빼놓다",
        "leave out_2": "leave out: (이야기를 할 때 특정 사실을) 빼고 말하다, 생략하다",
        "leave out_3": "leave out: (주변 사람들에게서) 따돌림을 당하거나 소외감을 느끼다",
        "let go_1": "let go: (꽉 붙들고 있던 손을) 놓다",
        "let go_2": "let go: (과거의 상처나 안 좋은 감정을) 깨끗이 털어버리다"
    };

    // 1. 구동사 순한맛 (Week 5 & Week 6)
    const phrasalEasy = [
        // Week 5 (Day 21~25)
        { week: 5, ko: "영화관에 뭐 상영하고 있는지 한번 알아보자.", en: "Let’s find out what’s playing at the movie theater.", source: "Day 021 순한맛", meaning: meanings["find out_1"] },
        { week: 5, ko: "빨강은 우리 집 거실의 차분한 분위기와는 따로 놀 테니까.", en: "Red doesn’t fit in with the living room’s calm vibe.", source: "Day 022 순한맛", meaning: meanings["fit in_1"] },
        { week: 5, ko: "나는 늘 겉도는 느낌이었거든.", en: "I never truly felt like I fit in.", source: "Day 022 순한맛", meaning: meanings["fit in_2"] },
        { week: 5, ko: "요즘 너무 바빠서 운동할 짬도 안 납니다.", en: "I have been too busy to fit a workout in.", source: "Day 022 순한맛", meaning: meanings["fit in_3"] },
        { week: 5, ko: "새 자전거도 마련했으니 동네를 쉽게 돌아다닐 수 있겠군.", en: "Now that I got a new bike, I can easily get around the neighborhood.", source: "Day 023 순한맛", meaning: meanings["get around_1"] },
        { week: 5, ko: "Mobility devices 덕분에 몸이 불편한 분들도 혼자서 다닐 수 있다.", en: "Mobility devices allow disabled individuals to get around independently.", source: "Day 023 순한맛", meaning: meanings["get around_2"] },
        { week: 5, ko: "Nick이 결혼한다는 소문이 사무실에 쫙 퍼졌다.", en: "Word got around the office that Nick was getting married.", source: "Day 023 순한맛", meaning: meanings["get around_3"] },
        { week: 5, ko: "와, 너 정말 여기저기 많이 다니는구나.", en: "Wow, you certainly get around.", source: "Day 024 순한맛", meaning: meanings["get around_4"] },
        { week: 5, ko: "일부 기업들은 법을 지키지 않으려고 늘 허점을 찾는다.", en: "Some companies are always looking for loopholes to get around the law.", source: "Day 024 순한맛", meaning: meanings["get around_5"] },
        { week: 5, ko: "요즘 집 사는 데 돈이 많이 든다는 점은 부인할 수 없는 사실이다.", en: "There’s no getting around the fact that buying a house is expensive.", source: "Day 024 순한맛", meaning: meanings["get around_6"] },
        { week: 5, ko: "드디어 내 차 엔진 오일을 갈았어.", en: "I finally got around to changing the oil in my car.", source: "Day 025 순한맛", meaning: meanings["get around to_1"] },
        // Week 6 (Day 42~51)
        { week: 6, ko: "이 고속도로는 산을 몇 개나 지나간다.", en: "The highway goes through several mountains.", source: "Day 042 순한맛", meaning: meanings["go through_1"] },
        { week: 6, ko: "한 시간 동안 컴퓨터를 뒤져 봐도 없었다.", en: "I went through my computer for like an hour and couldn’t find it.", source: "Day 042 순한맛", meaning: meanings["go through_2"] },
        { week: 6, ko: "주식 투자를 할 때는 재무 상담가를 거치는 것이 현명하다.", en: "When investing in stocks, it’s wise to go through a financial advisor.", source: "Day 042 순한맛", meaning: meanings["go through_3"] },
        { week: 6, ko: "너 최근에 여러 가지로 힘들었다는 것 알아.", en: "I know you have gone through a lot lately.", source: "Day 042 순한맛", meaning: meanings["go through_4"] },
        { week: 6, ko: "1장을 자세히 살펴보고 익숙하지 않은 어휘가 있는지 봅시다.", en: "Let’s go through chapter 1 and find any vocabulary you are not familiar with.", source: "Day 043 순한맛", meaning: meanings["go through_5"] },
        { week: 6, ko: "우리 벌써 우유를 다 먹었네!", en: "I can’t believe we’ve already gone through all of our milk!", source: "Day 043 순한맛", meaning: meanings["go through_6"] },
        { week: 6, ko: "결제가 이루어지지 않았습니다.", en: "Your payment didn’t go through.", source: "Day 043 순한맛", meaning: meanings["go through_7"] },
        { week: 6, ko: "조만간 얼굴 한번 보자.", en: "We should hang out soon.", source: "Day 044 순한맛", meaning: meanings["hang out_1"] },
        { week: 6, ko: "7번 출구에서 4시쯤 만나서 저녁 먹을 때까지 놀자고.", en: "Let’s meet up at exit 7 around 4 p.m.", source: "Day 044 순한맛", meaning: meanings["meet up_1"] },
        { week: 6, ko: "워크숍은 다른 학생들과 어울릴 수 있는 기회를 줄 것입니다.", en: "The workshop will give you a chance to mingle with other students.", source: "Day 044 순한맛", meaning: meanings["mingle with_1"] },
        { week: 6, ko: "보안 요원들이 군중을 막아야만 했다.", en: "The security guards had to hold back the crowd.", source: "Day 045 순한맛", meaning: meanings["hold back_1"] },
        { week: 6, ko: "우리 대부분은 두려움에 발목이 잡힌다.", en: "Most of us are often held back by fear.", source: "Day 045 순한맛", meaning: meanings["hold back_2"] },
        { week: 6, ko: "내가 그를 어떻게 생각하는지 말할 뻔했지만, 참았다.", en: "I nearly told him what I thought of him, but I held back.", source: "Day 045 순한맛", meaning: meanings["hold back_3"] },
        { week: 6, ko: "굳이 방귀를 참아야 하나 생각했다.", en: "I thought I didn’t have to hold back my fart.", source: "Day 045 순한맛", meaning: meanings["hold back_4"] },
        { week: 6, ko: "그 사람은 틀림없이 뭔가 숨기고 있어.", en: "He is obviously holding something back.", source: "Day 045 순한맛", meaning: meanings["hold back_5"] },
        { week: 6, ko: "우선은 크리스마스 선물 사는 거 보류하자.", en: "Let’s hold off on buying Christmas presents for now.", source: "Day 046 순한맛", meaning: meanings["hold off_1"] },
        { week: 6, ko: "어느 선량한 시민이 공격하려는 사람을 제지했다.", en: "A good Samaritan held off an attacker.", source: "Day 046 순한맛", meaning: meanings["hold off_2"] },
        { week: 6, ko: "전화기를 단말기에 갖다 대세요.", en: "Hold your phone up to the card reader.", source: "Day 047 순한맛", meaning: meanings["hold up_1"] },
        { week: 6, ko: "어떻게 저렇게 작은 테이블이 이렇게 무거운 유리를 지탱하는 걸까?", en: "How does that little table hold up such a heavy piece of glass?", source: "Day 047 순한맛", meaning: meanings["hold up_2"] },
        { week: 6, ko: "왜 이렇게 늦어?", en: "What’s the hold-up?", source: "Day 047 순한맛", meaning: meanings["hold up_3"] },
        { week: 6, ko: "잘 견디고 계시나요?", en: "How is she holding up?", source: "Day 048 순한맛", meaning: meanings["hold up_4"] },
        { week: 6, ko: "이 교각은 많은 교통량의 무게에도 잘 견뎌 왔다.", en: "This bridge has held up under the weight of the heavy traffic.", source: "Day 048 순한맛", meaning: meanings["hold up_5"] },
        { week: 6, ko: "따라갈 수가 없어.", en: "I can’t keep up.", source: "Day 049 순한맛", meaning: meanings["keep up_1"] },
        { week: 6, ko: "그동안 근력 운동을 꾸준히 했습니다. 근육량이 유지되고 있는 기분입니다.", en: "I’ve kept up with weightlifting. I feel like I’m maintaining muscle mass.", source: "Day 049 순한맛", meaning: meanings["keep up_2"] },
        { week: 6, ko: "요즘은 신규 앱이 너무 많이 출시되어서 따라갈 수가 없다.", en: "I just can’t keep up with all these new apps coming out.", source: "Day 049 순한맛", meaning: meanings["keep up_3"] },
        { week: 6, ko: "아몬드는 넣지 말아 주시겠어요?", en: "Can you leave them out?", source: "Day 050 순한맛", meaning: meanings["leave out_1"] },
        { week: 6, ko: "클럽에서 춤추다가 만났다는 건 이야기 안 합니다.", en: "I leave out the part where we met dancing at a club.", source: "Day 050 순한맛", meaning: meanings["leave out_2"] },
        { week: 6, ko: "파티에서 따돌림을 당한 기분이 들더라고.", en: "I felt left out at the party.", source: "Day 050 순한맛", meaning: meanings["leave out_3"] },
        { week: 6, ko: "잠깐 운전대에서 손을 놓아서 운전면허 시험에서 떨어졌다.", en: "I failed my driving exam after letting go of the wheel for a moment.", source: "Day 051 순한맛", meaning: meanings["let go_1"] },
        { week: 6, ko: "다른 사람에 대한 안 좋은 감정은 최대한 빨리 털어 버리려고 한다.", en: "I try to let go of hard feelings toward others as soon as possible.", source: "Day 051 순한맛", meaning: meanings["let go_2"] }
    ];

    // 2. 구동사 매운맛 (Week 5 & Week 6)
    const phrasalHard = [
        // Week 5 (Day 21~25)
        { week: 5, ko: "그 선생님의 강의 스타일과 가능한 시간대에 대해 좀 더 알고 싶습니다.", en: "I was hoping to find out more about his teaching style and his availability.", source: "Day 021 교재3", meaning: meanings["find out_1"] },
        { week: 5, ko: "역에 도착해 보니 엉뚱한 날짜의 표를 예매했더라고요.", en: "When we got to the station, we found out that we had booked tickets for the wrong day.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "이 대학의 경우 지원 절차가 좀 느립니다. 그래서 합격 여부를 알려면 몇 주 더 기다려야 합니다.", en: "The application process is pretty slow for this university. I’ll have to wait a couple more weeks to find out whether I’ve gotten in or not.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "너를 많이 좋아해. 하지만 결혼하기 전에 (우린) 서로에 대해 좀 더 알아야 해.", en: "I like you a lot, but we should find out more about each other before getting married.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "블랙핑크에 대해 알 수 있는 거의 모든 것을 알아봅시다.", en: "Let’s find out almost everything there is to know about BLACKPINK.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "그 사람이 그동안 저한테 나이를 속여 왔다는 걸 알게 되었어요.", en: "I just found out (that) he has been lying to me about his age.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "영화관에 뭐 상영하고 있는지 한번 알아보자.", en: "Let’s find out what’s playing at the movie theater.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "애당초 그 둘이 어디서 만났는지 알면 놀랄 거야. 이태원에 있는 클럽에서 만났는데 여자가 바텐더였어.", en: "You will be surprised to find out how they first met. It was actually at a club in Itaewon and she was the bartender.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "<베첼러>에 나오는 커플이 결혼에 골인할지 너무 궁금하다.", en: "I’m curious to find out if the couple on The Bachelor will actually get married.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "워크숍 연기 여부를 알아보기 위해 이메일을 확인해야 한다.", en: "I should check my e-mail to find out whether the workshop has been pushed back or not.", source: "Day 021 교재1", meaning: meanings["find out_1"] },
        { week: 5, ko: "죄송한데요. 어제 헬스장 갔다가 청소 때문에 휴관이라는 걸 알았어요. 이런 건 사전에 공지가 되어야 하지 않나요? 하루 계획이 엉망이 되었어요!", en: "Excuse me. I went to the gym yesterday and only then found out that it was closed for cleaning. Shouldn’t there have been advance notice? It really ruined my plans for the day!", source: "Day 021 교재2", meaning: meanings["find out_1"] },
        { week: 5, ko: "불편을 드려 죄송합니다. 하지만 지난주 마포구청 앱에 휴관 관련 공지가 있었습니다. 혹시 휴대폰에 앱을 깔아 두셨을까요?", en: "I’m sorry about the inconvenience. However, an announcement about the gym closure was made last week on the Mapo District app. Do you happen to have that on your phone?", source: "Day 021 교재2", meaning: null },
        { week: 5, ko: "무엇을 도와드릴까요?", en: "How may I help you?", source: "Day 021 교재2", meaning: null },
        { week: 5, ko: "사실 어제 작은 접촉 사고가 났어요. 차 수리 비용이 어느 정도일지 알고 싶어서 연락드렸습니다.", en: "I actually got in a little fender bender yesterday, so I wanted to find out how much it would cost to get my car fixed.", source: "Day 021 교재2", meaning: meanings["find out_1"] },
        { week: 5, ko: "Sarah랑 Tom이 사귀게 되었다는 거 들었어? 어떻게 둘이 죽이 잘 맞게 되었는지 정말 궁금하네.", en: "Have you heard that Sarah and Tom are a thing now? I really want to find out how they hit it off.", source: "Day 021 교재2", meaning: meanings["find out_1"] },
        { week: 5, ko: "정말? 나는 너무 잘 어울린다고 늘 생각했는데. 지난 주말에 그 둘이 이태원에 춤추러 갔다고 들었어. 거기서 무슨 일이 있었을 거야.", en: "Really? I always thought they’d make a great couple. I heard they both went out clubbing in Itaewon last weekend. Maybe something happened there.", source: "Day 021 교재2", meaning: null },
        { week: 5, ko: "안녕, 얘들아. 그래서… 내일 저녁에 뭐 먹을지는 결정했어?", en: "Hey, guys. So… have we decided what we’re going to eat tomorrow night?", source: "Day 021 교재3", meaning: null },
        { week: 5, ko: "종로에 가 보고 싶은 이탈리아 음식점이 있는데, 네이버에는 메뉴가 안 나와 있네. 전화해서 무슨 메뉴가 있는지 물어봐야겠다.", en: "There’s this Italian restaurant in Jongno that I’m curious about, but I don’t see any of their dishes listed on Naver. I guess I’ll have to call them to find out what’s on their menu.", source: "Day 021 교재3", meaning: meanings["find out_1"] },
        { week: 5, ko: "잘됐네! 안 그래도 며칠 전부터 이탈리아 음식이 먹고 싶었는데.", en: "Perfect! I’ve been craving Italian food for the past few days.", source: "Day 021 교재3", meaning: null },
        { week: 5, ko: "잘 들어 봐. 방금 전화해서 확인해 봤는데 내일이랑 모레 이틀간 전 메뉴 20% 할인이래!", en: "Get this – I just called to check, and they said everything is 20% off for the next two days!", source: "Day 021 교재3", meaning: null },
        { week: 5, ko: "좋은 넥타이긴 한데 동생이 가지고 있는 다른 것들이랑 좀 안 맞아.", en: "It’s a nice tie, but it doesn’t really fit in with my brother’s collection.", source: "Day 022 교재1", meaning: meanings["fit in_1"] },
        { week: 5, ko: "내가 어떻게 2년이나 그 직장에 다녔는지 모르겠어. 나는 늘 겉도는 느낌이었거든.", en: "I can’t believe I stayed at that workplace for two whole years. I never truly felt like I fit in.", source: "Day 022 교재1", meaning: meanings["fit in_2"] },
        { week: 5, ko: "SUV 구매는 제 라이프스타일과 맞지 않습니다. 제가 다른 사람을 태우고 다니는 일도 많지 않고, 많이 다니지도 않으며, 야외 활동을 그렇게 많이 하지도 않거든요.", en: "Buying an SUV doesn’t fit in with my lifestyle because I don’t often have passengers, and I don’t get out and do outdoor activities very much.", source: "Day 022 교재1", meaning: meanings["fit in_1"] },
        { week: 5, ko: "여긴 경차 주차 공간인 것 같아. 들어갈 수 있는 공간이 충분한 것 같아?", en: "I think this is a space for compact cars. Do you really think you have enough space to fit in?", source: "Day 022 교재1", meaning: meanings["fit in_3"] },
        { week: 5, ko: "빨간색 쿠션은 너무 튈 것 같아. 빨강은 우리 집 거실의 차분한 분위기와는 따로 놀 테니까.", en: "I’m afraid the red cushion stands out because red doesn’t fit in with the living room’s calm vibe.", source: "Day 022 교재1", meaning: meanings["fit in_1"] },
        { week: 5, ko: "중학교 때는 매 학기 초에 적응하는 데 늘 애를 먹었어요.", en: "Back in middle school, I always had trouble fitting in early on in the semester.", source: "Day 022 교재1", meaning: meanings["fit in_2"] },
        { week: 5, ko: "기념품을 너무 많이 샀어. 여행 가방에 다 안 들어갈 것 같아.", en: "I bought way too many souvenirs. I don’t think I can fit all this stuff in my suitcase.", source: "Day 022 교재1", meaning: meanings["fit in_3"] },
        { week: 5, ko: "요즘 너무 바빠서 운동할 짬도 안 납니다.", en: "I have been too busy to fit a workout in (my schedule).", source: "Day 022 교재1", meaning: meanings["fit in_3"] },
        { week: 5, ko: "Samantha가 그만둔다는 이야기 들었어요? 이미 사직서를 제출했대요.", en: "Did you hear Samantha is leaving? She already turned in her resignation.", source: "Day 022 교재2", meaning: null },
        { week: 5, ko: "그만두는 게 당연할지도요. 이럴 줄 알았어요. 어쨌든 이 회사에서 늘 겉돌았잖아요.", en: "It’s no surprise she’s leaving. I actually saw this coming. She never really fit in, after all.", source: "Day 022 교재2", meaning: meanings["fit in_2"] },
        { week: 5, ko: "... 그럼 이 덴마크산 가죽 소파를 추천해 드립니다. 추가 혜택으로 이번 주에 한해 15% 세일도 하거든요.", en: "... then I would recommend this Danish leather couch. As an added bonus, it’s actually on sale for 15% off, this week only.", source: "Day 022 교재2", meaning: null },
        { week: 5, ko: "가구의 경우 덴마크산이 진리이긴 하죠. 근데 가죽 소파는 제가 추구하는 저희 집의 아늑한 분위기와는 따로 놀 것 같아서요.", en: "I know you can’t go wrong with Danish craftsmanship when it comes to furniture, but I’m afraid a leather couch wouldn’t fit in with the cozy vibe I’m going for at my place.", source: "Day 022 교재2", meaning: meanings["fit in_1"] },
        { week: 5, ko: "다음 달 마라톤에 출전하기 전에 Thompson 박사님을 뵙고 무릎 진료를 받고 싶습니다. 혹시 진료 가능한 시간 있을까요?", en: "I’d really like to see Dr. Thompson about my knee before the marathon next month. Are there any openings available?", source: "Day 022 교재2", meaning: null },
        { week: 5, ko: "선생님께서 꽤 바쁘십니다. 언제 가능할지 스케줄 확인해 드릴게요. 마라톤을 앞두고 좋은 몸 상태를 유지하시는 게 중요한 점 알고 있습니다.", en: "She’s been quite busy, but let me check her schedule to see when she can fit you in. I understand it is important for you to be in good shape for the race.", source: "Day 022 교재2", meaning: meanings["fit in_3"] },
        { week: 5, ko: "골프는 나랑 진짜 안 맞다.", en: "Playing golf isn’t really for me.", source: "Day 022 교재3", meaning: null },
        { week: 5, ko: "뭔가 모르게 재미가 안 붙는다.", en: "For some reason, I just can’t get into it.", source: "Day 022 교재3", meaning: null },
        { week: 5, ko: "내게 골프는 스포츠가 아닌 게임이다.", en: "I think of it as a game, not a sport.", source: "Day 022 교재3", meaning: null },
        { week: 5, ko: "하지만 영업직이다 보니 (사람들과) 어울리기 위해서는 골프를 시작할 수밖에 없었다.", en: "Working in sales, though, I had to take up golf just to fit in.", source: "Day 022 교재3", meaning: meanings["fit in_2"] },
        { week: 5, ko: "솔직히 토요일 아침 일찍 일어나는 게 너무 싫지만 그래도 어쩌겠나.", en: "Honestly, I dread waking up early on Saturday mornings, but it is what it is.", source: "Day 022 교재3", meaning: null },
        { week: 5, ko: "사실 내게 골프는 선택의 문제가 아닌 인맥 유지의 수단이다.", en: "Actually, for me, playing golf is not an option, but a means for networking.", source: "Day 022 교재3", meaning: null },
        { week: 5, ko: "그래도 야외에서 인맥을 쌓을 수 있어서 감사해야 할 것 같다. (사람들과) 이야기도 나누고 신선한 공기도 마실 수 있으니 말이다.", en: "I suppose I should be grateful that I can network outdoors, where it’s easy to talk and get some fresh air.", source: "Day 022 교재3", meaning: null },
        { week: 5, ko: "다만 내가 골프를 조금만 더 좋아했어도 좋으련만.", en: "But I just wish I enjoyed golf a bit more.", source: "Day 022 교재3", meaning: null },
        { week: 5, ko: "새 자전거도 마련했으니 동네를 쉽게 돌아다닐 수 있겠군.", en: "Now that I got a new bike, I can easily get around the neighborhood.", source: "Day 023 교재1", meaning: meanings["get around_1"] },
        { week: 5, ko: "저희 할아버지는 91세이신데도 여전히 (여기저기) 잘 다니십니다.(= 외출하시는데 문제가 없습니다.)", en: "My grandpa is 91, but he still gets around well.", source: "Day 023 교재1", meaning: meanings["get around_2"] },
        { week: 5, ko: "Taylor Swift가 도착한다는 소식이 빠르게 알려졌고, 공항에서 수백 명의 열혈 팬들이 그녀를 맞이했다.", en: "Word got around fast about the arrival of Taylor Swift, and she was met at the airport by hundreds of eager fans.", source: "Day 023 교재1", meaning: meanings["get around_3"] },
        { week: 5, ko: "디지털 노마드인 Sarah는 이 나라 저 나라를 (돌아) 다닌다.", en: "As a digital nomad, Sarah gets around by hopping from one country to another.", source: "Day 023 교재1", meaning: meanings["get around_1"] },
        { week: 5, ko: "모빌리티 디바이스(개인용 이동 수단) 덕분에 몸이 불편한 분들도 혼자서 다닐 수 있다.", en: "Mobility devices allow disabled individuals to get around independently.", source: "Day 023 교재1", meaning: meanings["get around_2"] },
        { week: 5, ko: "Nick이 결혼한다는 소문이 사무실에 쫙 퍼졌다.", en: "Word got around the office that Nick was getting married.", source: "Day 023 교재1", meaning: meanings["get around_3"] },
        { week: 5, ko: "최근에 사무실 주변에 주차 공간 찾기가 이렇게 어렵다니 말이 돼? 아예 차를 완전히 버리고 자전거로 다녀야 할까 봐.", en: "Can you believe how difficult it is to find parking near the office lately? I’m considering ditching my car altogether and using a bike to get around.", source: "Day 023 교재2", meaning: meanings["get around_1"] },
        { week: 5, ko: "정말? 그것도 괜찮은 생각이다. 난 지난달에 주차 위반 딱지를 세 번이나 끊겼어. (차 안 가지고 다니면) 돈도 절약할 수 있겠네.", en: "Really? That’s not a bad idea. I got three parking tickets last month. You might save some money, too.", source: "Day 023 교재2", meaning: null },
        { week: 5, ko: "Dereck, 시내에 새로 생긴 쇼핑몰에서 만나서 저녁 먹을까?", en: "Hey, Dereck, why don’t we meet at the new mall downtown for dinner?", source: "Day 023 교재2", meaning: null },
        { week: 5, ko: "지난번에 거기 갔을 때 통로가 너무 좁아서 휠체어 타고 다니는 데 애를 먹었어. 이동이 쉬운 곳에 가 보는 게 어떨까?", en: "Last time I went there I had trouble getting around the narrow aisles in my wheelchair. Could we try a place with better accessibility?", source: "Day 023 교재2", meaning: meanings["get around_2"] },
        { week: 5, ko: "Samantha가 동네방네 깜짝파티를 떠벌리고 다녔다네.", en: "I heard Samantha blabbed about the surprise party to everyone.", source: "Day 023 교재2", meaning: null },
        { week: 5, ko: "하이고, 이제 Peter한테 이야기가 들어가서 깜짝파티 다 들통나겠어!", en: "Ugh, now it’s definitely going to get around to Peter and spoil his surprise!", source: "Day 023 교재2", meaning: meanings["get around_3"] },
        { week: 5, ko: "지은아, 발리 여행은 어땠어? 발리에서는 어떻게 다녔어?", en: "Hey, Jieun, how was Bali? How did you get around on the island?", source: "Day 023 교재3", meaning: meanings["get around_1"] },
        { week: 5, ko: "무슨 일이 있었는지 알면 믿기지 않을 거야. ‘택시 문제’로 아주 난리였어. 거기 택시 기사들 대부분이 정말 제멋대로더라고. 관광객들에게 바가지를 씌우려고 했거든.", en: "You won’t believe what happened. We had a lot of “taxi drama.” Most of the taxi drivers there have become so spoiled, trying to rip off tourists.", source: "Day 023 교재3", meaning: null },
        { week: 5, ko: "정말?", en: "Really?", source: "Day 023 교재3", meaning: null },
        { week: 5, ko: "응. 어떤 택시 기사는 팁을 안 줬다고 남편에게 소리를 질렀어. 너무 화가 나서 경찰을 부르려고 했는데, 다행히 지역 주민들이 개입해서 나를 말렸지.", en: "Yeah, this taxi driver yelled at my husband for not tipping. I was so angry. I was ready to call the police, but thankfully, the locals stepped in and talked me out of it.", source: "Day 023 교재3", meaning: null },
        { week: 5, ko: "끔찍하다.", en: "That sounds awful.", source: "Day 023 교재3", meaning: null },
        { week: 5, ko: "응. 택시와 관련된 이런 안 좋은 경험이 여행 전체를 망쳐 버렸어.", en: "Yeah, those unpleasant experiences with the taxis ruined our entire trip.", source: "Day 023 교재3", meaning: null },
        { week: 5, ko: "저희 어머니가 70대 후반인데도 저보다 더 활동적입니다. 늘 지역 사회 행사에 참여하셔서 자원봉사를 하십니다. 정말 대단하다고 생각됩니다.", en: "My mom is in her late 70s, but she still gets around more than I do. She’s always attending and volunteering at community events. I really admire that.", source: "Day 024 교재1", meaning: meanings["get around_4"] },
        { week: 5, ko: "A: Kevin, 새 직장은 어때?", en: "A: Kevin, how’s the new job?", source: "Day 024 교재1", meaning: null },
        { week: 5, ko: "B: 바빠. 영업팀 소속이라 주 5회씩 밤에 이런저런 모임과 행사에 참석하거든.", en: "B: Busy. I’m on the sales team and we get around to some kind of social function five nights a week!", source: "Day 024 교재1", meaning: meanings["get around_4"] },
        { week: 5, ko: "세금 안 낼 생각은 하지도 마. 반드시 걸리게 되어 있어.", en: "Don’t even think of trying to get around paying taxes. You would never get away with it.", source: "Day 024 교재1", meaning: meanings["get around_5"] },
        { week: 5, ko: "엔지니어는 안이하게 배움을 멈추면 안 된다. 신기술에 뒤처지지 않으려면 지속적인 커리어 개발을 피할 수 없다.", en: "You can’t just relax and stop learning as an engineer. There’s no getting around the continuous professional development to keep up with new technology.", source: "Day 024 교재1", meaning: meanings["get around_6"] },
        { week: 5, ko: "일부 기업들은 법을 지키지 않으려고 늘 (법의) 허점을 찾는다.", en: "Some companies are always looking for loopholes to get around the law.", source: "Day 024 교재1", meaning: meanings["get around_5"] },
        { week: 5, ko: "나이 드는 것을 피하고 부정할 수는 없어. 우리 모두가 겪는 일이니까.", en: "There’s no getting around getting older. It happens to us all.", source: "Day 024 교재1", meaning: meanings["get around_6"] },
        { week: 5, ko: "A: 한번 들어 봐. 월요일에는 골프장에 갔어. 화요일에는 회의차 부산에 갔지. 수요일에는 여러 저녁 모임 때문에 서울에 다시 올라왔어. 그리고 내일은 추가 회의 때문에 뉴욕에 가.", en: "A: Listen to this: Monday, I was on the golf course. Tuesday, I was in Busan for a conference. Wednesday, I was back in Seoul for multiple dinner gatherings. Then tomorrow, I’m flying out to New York for more meetings.", source: "Day 024 교재1", meaning: null },
        { week: 5, ko: "B: 와, 너 정말 여기저기 많이 다니는구나.", en: "B: Wow, you certainly get around.", source: "Day 024 교재1", meaning: meanings["get around_4"] },
        { week: 5, ko: "어떤 날 아침에는 정말 청소하기가 싫다. 그래서 (회사에) 늦은 척하며 설거지하는 걸 피해 버린다. 아내가 하도록 싱크대 옆에 두고는 조용히 출근하러 나가 버린다. 이러면 정말 머저리가 된다는 걸 나도 안다.", en: "Some mornings, I really don’t feel like cleaning, so I get around doing the dishes by pretending I’m late. I leave them by the sink for my wife to do and quietly head out to work. I know that makes me kind of a jerk.", source: "Day 024 교재1", meaning: meanings["get around_5"] },
        { week: 5, ko: "요즘 집 사는 데 돈이 많이 든다는 점은 부인할 수 없는 사실이다.", en: "There’s no getting around the fact that buying a house is expensive.", source: "Day 024 교재1", meaning: meanings["get around_6"] },
        { week: 5, ko: "최근에 Nick 봤어?", en: "Have you seen Nick lately?", source: "Day 024 교재2", meaning: null },
        { week: 5, ko: "응, 그 친구 이번 주에는 매일 밤 외출하더라. 여기저기 정말 많이 다녀.", en: "Oh yeah, he’s been out every night this week. He certainly gets around.", source: "Day 024 교재2", meaning: meanings["get around_4"] },
        { week: 5, ko: "Jeff, 나한테 <뉴욕타임스> 기사 계속 보내는구나. 페이월이 있을 텐데. 구독하는 거야?", en: "Jeff, you keep sending me these New York Times articles. They have a paywall. Do you subscribe to them?", source: "Day 024 교재2", meaning: null },
        { week: 5, ko: "아, 앱을 쓰는데 이걸 쓰면 페이월을 피할 수 있어. 여기 링크 보낼게. 한번 봐 봐.", en: "Oh, I use an app that allows me to get around paywalls. Here’s the link. Check it out.", source: "Day 024 교재2", meaning: meanings["get around_5"] },
        { week: 5, ko: "최근에 안 오르는 게 없군. 자장면까지.", en: "The prices of everything keep going up lately. Even jajangmyeon.", source: "Day 024 교재2", meaning: null },
        { week: 5, ko: "누가 아니래. 인플레이션을 피해갈 수는 없지.", en: "That’s for sure. There’s no getting around inflation.", source: "Day 024 교재2", meaning: meanings["get around_6"] },
        { week: 5, ko: "아, 몇 주간 위에 불편함이 계속 느껴져.", en: "Ugh, I’ve been feeling this discomfort in my stomach for weeks now.", source: "Day 024 교재3", meaning: null },
        { week: 5, ko: "어떡해. 병원에는 가 본 거야?", en: "That doesn’t sound good. Have you seen a doctor?", source: "Day 024 교재3", meaning: null },
        { week: 5, ko: "한 달 이상 통증을 그냥 무시하고는 있는데, 회피하면 안 될 듯해. 병원에 가 봐야 할 것 같아.", en: "I’ve been trying to ignore the pain for over a month now, but there’s no getting around it. I guess I do need to see one.", source: "Day 024 교재3", meaning: meanings["get around_6"] },
        { week: 5, ko: "맞아, Mark. 더 늦기 전에 서둘러서 검사를 받아 보는 게 좋을 거야.", en: "You’re right, Mark. It’s better to get it checked out sooner rather than later.", source: "Day 024 교재3", meaning: null },
        { week: 5, ko: "알아. 계속 미뤄 왔거든. 더 이상은 그냥 넘길 수가 없어.", en: "I know. I’ve been putting it off, but I just can’t ignore it any longer.", source: "Day 024 교재3", meaning: null },
        { week: 5, ko: "필요하면 내가 태워 줄게.", en: "I’ll give you a ride if you need it.", source: "Day 024 교재3", meaning: null },
        { week: 5, ko: "고마워. 나 자신을 좀 더 잘 챙겨야 할 듯해.", en: "Thanks. I appreciate it. I guess I need to take better care of myself.", source: "Day 024 교재3", meaning: null },
        { week: 5, ko: "드디어 내 차 엔진 오일을 갈았어. 이상한 소리가 나기 시작했거든.", en: "I finally got around to changing the oil in my car. It was starting to make strange noises.", source: "Day 025 교재1", meaning: meanings["get around to_1"] },
        { week: 5, ko: "제발 잔소리 좀 그만해. 쓰레기 내다 버린다고 내가 말했잖아. 시간 될 때 할게!", en: "Please stop nagging me. I already said I’ll take out the garbage. I’ll get around to it when I have time!", source: "Day 025 교재1", meaning: meanings["get around to_1"] },
        { week: 5, ko: "이건 ‘미뤘다가 할 수 있는 일’이 아니야. 파이프가 새는데 오늘 당장 고쳐야겠어!", en: "This isn’t something you can just “get around to.” That leaking pipe needs fixing today!", source: "Day 025 교재1", meaning: meanings["get around to_1"] },
        { week: 5, ko: "이제 진짜 네 방 청소해야 하지 않겠니?", en: "Don’t you think it’s about time you got around to cleaning your room?", source: "Day 025 교재1", meaning: meanings["get around to_1"] },
        { week: 5, ko: "이번 주에 크리스마스 카드 주문할 시간 되겠어? 아니면 내가 직접 주문할까?", en: "Do you think you can get around to ordering the Christmas cards this week? Or should I just go ahead and order them myself?", source: "Day 025 교재1", meaning: meanings["get around to_1"] },
        { week: 5, ko: "A: 이봐, 모네 전시회 언제 끝나는지 기억해? 몇 주째 보러 가기로 마음을 먹고 있잖아.", en: "A: Hey, do you remember when the Monet exhibit is over? We’ve been wanting to go see it for weeks.", source: "Day 025 교재1", meaning: null },
        { week: 5, ko: "B: 정확한 날짜는 모르겠어. 근데 얼마 남지 않았을 거야. 너무 늦기 전에 보러 가야 해.", en: "B: I’m not exactly sure of the date, but there must not be much time left. We should get around to seeing it before it’s too late.", source: "Day 025 교재1", meaning: meanings["get around to_1"] },
        { week: 5, ko: "그거 아직 안 한 거야? 새로 만든 쿠키 사진 찍은 거 올리는 거 말이야.", en: "Did you get around to it yet? I mean putting up the photos we took of our new cookies.", source: "Day 025 교재2", meaning: meanings["get around to_1"] },
        { week: 5, ko: "완전 깜박했어. 오늘 꼭 할 게. 요즘 왜 이렇게 정신이 없는지 모르겠어.", en: "I completely forgot. I swear I’ll do it today. I don’t know where my mind is these days.", source: "Day 025 교재2", meaning: null },
        { week: 5, ko: "드디어 이번 주에 세금 신고했어. 미루던 걸 하고 나니 속이 시원해.", en: "I finally got around to filing my taxes this week. It feels really nice to have gotten it over with.", source: "Day 025 교재2", meaning: meanings["get around to_1"] },
        { week: 5, ko: "잘했다! 그나저나 세금은 얼마를 내야 해? 환급이나 그런 거 받는 거야?", en: "That’s great! How much do you have to pay in taxes, by the way? Are you getting a refund or anything?", source: "Day 025 교재2", meaning: null },
        { week: 5, ko: "아홉 달째 치과 검진받으러 가야지 하고 있는데도 아직도 못 가고 있어.", en: "I have been meaning to go to the dentist for a checkup for, like, the last nine months, but I just haven’t gotten around to it.", source: "Day 025 교재2", meaning: meanings["get around to_1"] },
        { week: 5, ko: "안 돼. 치아 관리를 좀 더 잘해야 해. 정기적으로 치과에 가지 않으면 나중에 더 큰 문제가 생길 거야!", en: "No way. You should take better care of your teeth. If you don’t go to the dentist regularly, you’re going to have even bigger problems later!", source: "Day 025 교재2", meaning: null },
        { week: 5, ko: "지난주에 차에 타다가 아파트 카드 키를 운전석 아래쪽 손이 닿지 않는 곳에 떨어뜨렸다.", en: "Last week, I was getting in my car and I dropped my apartment key card somewhere beneath my driver’s seat where I can’t reach it.", source: "Day 025 교재3", meaning: null },
        { week: 5, ko: "(지금) 세차를 안 한 지 몇 달이나 되어서, 차 안이 좀 엉망이다. 그래서 카드 키를 찾기가 더 어렵다.", en: "I haven’t had my car washed for months, so the inside is a bit of a mess, making it even harder to find the card.", source: "Day 025 교재3", meaning: null },
        { week: 5, ko: "(이 정도면) 세차를 해야 할 이유가 충분한데도 아직 안 하고 있다.", en: "Now I have all the reason to take my car to the car wash, but still, I haven’t gotten around to it.", source: "Day 025 교재3", meaning: meanings["get around to_1"] },
        { week: 5, ko: "하려고 생각할 때마다 회사에 무슨 일이 생기거나, 그냥 하루를 더 미루게 된다.", en: "Every time I think about doing it, something else comes up at work or I just put it off for another day.", source: "Day 025 교재3", meaning: null },
        { week: 5, ko: "그래, 이번 주말에는 반드시 하기로 다짐한다. 더 이상의 핑계는 안 된다.", en: "Okay; I’m making a promise to myself to get it done this weekend. No more excuses.", source: "Day 025 교재3", meaning: null },

        // Week 6 (Day 26~30)
        { week: 6, ko: "새들을 쫓아 보냈지만, 이미 우리 음식의 반을 훔쳐 달아난 뒤였다.", en: "We chased the birds off, but not before they got away with half our food.", source: "Day 026 교재1", meaning: meanings["get away with_1"] },
        { week: 6, ko: "그 쌍둥이는 최소 일주일에 한 번 반을 바꿔서 수업을 들었다. 재미 삼아 한 장난이었다. 그리고 한 번도 들키지 않았다.", en: "The twins swapped classes at least once a week. It was a fun game they played, and they always got away with it.", source: "Day 026 교재1", meaning: meanings["get away with_2"] },
        { week: 6, ko: "고등학교에 가면 숙제를 안 하고 그냥 넘어가지 못할 거야.", en: "You can’t get away with not doing your homework once you get to high school.", source: "Day 026 교재1", meaning: meanings["get away with_2"] },
        { week: 6, ko: "(깜박하고) 공원에 두고 온 지갑을 찾았다. 하지만 이미 누군가가 그 안에 있는 돈을 가지고 달아난 뒤였다.", en: "I found the wallet that I left in the park, but someone had already gotten away with the cash that was in it.", source: "Day 026 교재1", meaning: meanings["get away with_1"] },
        { week: 6, ko: "몇 달간 우리 건물 앞에 (불법) 주차하고도 무사할 수 있었는데, 이번달에 주차 위반 딱지를 여섯 번이나 끊겼다. 아무래도 다른 곳을 찾아봐야 할 것 같다.", en: "I got away with parking in front of my building for months, but this month I’ve received six parking tickets. I guess I should find somewhere else to park.", source: "Day 026 교재1", meaning: meanings["get away with_2"] },
        { week: 6, ko: "A: Sam이 나한테도 돈 갚을 게 있는데!", en: "A: Sam still owes me money, too!", source: "Day 026 교재1", meaning: null },
        { week: 6, ko: "B: 음, 우리한테 돈 안 갚고 그냥 넘어가게 두면 안 되겠다.", en: "B: Well, we can’t let him get away with not paying us back.", source: "Day 026 교재1", meaning: meanings["get away with_2"] },
        { week: 6, ko: "진짜 괜찮겠어? 나도 예전에 인터넷에서 찾은 문장을 베끼려고 한 적이 있는데, Harrison 교수님이 눈치채시더라고.", en: "Are you sure you can get away with it? I tried copying sentences from the Internet before too, and Mr. Harrison figured it out.", source: "Day 026 교재2", meaning: meanings["get away with_2"] },
        { week: 6, ko: "정말? 내가 교수님을 너무 얕봤네. 미리 귀띔해 줘서 고마워.", en: "Really? Maybe I underestimated him. Thanks for the heads-up.", source: "Day 026 교재2", meaning: null },
        { week: 6, ko: "이제 나이를 먹으니 자기 직전에 뭐 먹으면 문제가 생겨. 살찌기 딱 좋아.", en: "Now that I’m older, I can’t get away with eating just before going to bed. That’s a good way to pack on the pounds.", source: "Day 026 교재2", meaning: meanings["get away with_2"] },
        { week: 6, ko: "누가 아니래! 나이가 들면서 신진대사도 둔해져. 그래서 난 자기 전에는 절대 안 먹어. 물조차도 안 마시거든.", en: "I couldn’t agree with you more on that! Metabolism slows down with age. That’s why I never eat anything before going to sleep. I don’t even drink water.", source: "Day 026 교재2", meaning: null },
        { week: 6, ko: "Tom은 매일 같이 회사에 지각하는데도 어떻게 아무 탈이 없을 수 있는지 이해가 안 돼.", en: "I don’t understand how Tom gets away with arriving late to work every day.", source: "Day 026 교재2", meaning: meanings["get away with_2"] },
        { week: 6, ko: "맞아. 아무도 눈치 못 채는 것처럼 말이야. 회사에서 영업 실적이 제일 좋아서 그런지도 모르지.", en: "Yeah, it’s like nobody even notices. Maybe it’s because he leads the company in sales.", source: "Day 026 교재2", meaning: null },
        { week: 6, ko: "의류 태그에 보면 울과 실크 의류는 드라이클리닝을 해야 한다고 적혀 있습니다.", en: "Clothing tags often say you need to dry clean wool and silk garments.", source: "Day 026 교재3", meaning: null },
        { week: 6, ko: "하지만 손빨래를 해도 괜찮은 경우가 대부분이죠.", en: "But more often than not, you can get away with hand washing.", source: "Day 026 교재3", meaning: meanings["get away with_2"] },
        { week: 6, ko: "그건 그렇고 의류 관리기 장만하는 거 한번 생각해 보세요.", en: "By the way, you might want to think about buying a steam closet.", source: "Day 026 교재3", meaning: null },
        { week: 6, ko: "새 옷같이 유지할 수 있고, 위생적으로 관리할 수 있답니다.", en: "That would help to keep your clothes in mint condition and even sanitize them.", source: "Day 026 교재3", meaning: null },
        { week: 6, ko: "게다가 의류 관리기가 세탁소 가는 수고를 덜어 줄 것이고요.", en: "On top of that, a steam closet would save you trips to the dry cleaner.", source: "Day 026 교재3", meaning: null },
        { week: 6, ko: "의류 관리기는 여러 종류가 있는데 백화점에 가서 한번 보세요.", en: "There are different types of steam closets available, so you should check them out at department stores.", source: "Day 026 교재3", meaning: null },
        { week: 6, ko: "나는 학기 초에 하는 자기소개 시간이 너무 싫었다.", en: "I really hated doing self-introductions at the beginning of a new semester.", source: "Day 026 교재3", meaning: null },
        { week: 6, ko: "그런데 한번은 내 차례가 되었을 때, (겨우) 이름만 이야기했는데, 누군가가 의자에서 떨어졌다.", en: "Once, however, it was my turn to speak, I only said my name before somebody accidentally fell out of their chair.", source: "Day 026 교재3", meaning: null },
        { week: 6, ko: "모두가 거기에 정신이 팔렸고, 웃는 걸 멈췄을 때는 이미 다음 사람 소개로 넘어갔다.", en: "Everyone was distracted, and when people finished laughing, they moved on to the next person’s introduction.", source: "Day 026 교재3", meaning: null },
        { week: 6, ko: "이름만 이야기하고 무사히 넘어갈 수 있어서 너무 다행이었다.", en: "I was glad I got away with not saying anything but my name.", source: "Day 026 교재3", meaning: meanings["get away with_2"] },
        { week: 6, ko: "A: 좀 지나가도 될까요?", en: "A: Can I get by[get past], please?", source: "Day 027 교재1", meaning: meanings["get by_1"] },
        { week: 6, ko: "B: 네, 그럼요. 박스 치워 드릴게요.", en: "B: Oh, sure. Let me just move these boxes out of the way.", source: "Day 027 교재1", meaning: null },
        { week: 6, ko: "근근이 버티고 있어요. 여윳돈이 없습니다.", en: "We’re just getting by and don’t have any money to spare.", source: "Day 027 교재1", meaning: meanings["get by_2"] },
        { week: 6, ko: "그가 정말 많이 도와줬어요. 그 친구 아니었으면 혼자서는 힘들었을 거예요.", en: "He was so helpful. I couldn’t get by without him.", source: "Day 027 교재1", meaning: meanings["get by_3"] },
        { week: 6, ko: "우리 새 차 살 필요 없을 것 같아. 지금 차를 15년 동안 탔지만 아직 멀쩡하잖아. 완전히 고장 나지 않으면 계속 이걸로 버틸 수 있을 듯해.", en: "I don’t think we need to buy a new car. We’ve had this one for 15 years, but it’s still in good shape. I think we can continue to get by with it unless it completely breaks down.", source: "Day 027 교재1", meaning: meanings["get by_3"] },
        { week: 6, ko: "이 정도면 차 한 대 지나가기에 충분한 공간이야.", en: "There is enough room here for a car to get by.", source: "Day 027 교재1", meaning: meanings["get by_1"] },
        { week: 6, ko: "대부분 가정은 외벌이 수입으로는 생활이 힘들다.", en: "Most households can’t get by on a single salary.", source: "Day 027 교재1", meaning: meanings["get by_2"] },
        { week: 6, ko: "차를 살 형편이 될 때까지는 자전거로 버티면 되겠네.", en: "You can get by with a bicycle until you can afford a car.", source: "Day 027 교재1", meaning: meanings["get by_3"] },
        { week: 6, ko: "의자 조금 옆으로 옮겨 줄래? 창문 쪽으로 가려는데 좀 좁아서 말이야.", en: "Can you move your chair a bit? There’s not enough room for me to get to the window.", source: "Day 027 교재2", meaning: null },
        { week: 6, ko: "물론이지! 조금 옆으로 비킬게. 좀 나아? 이제 지나갈 공간이 충분할 거야.", en: "Oh, sure! Let me scoot over. Is that better? Now you should have enough space to get by.", source: "Day 027 교재2", meaning: meanings["get by_1"] },
        { week: 6, ko: "어떻게 지내? 새로 시작한 택시 운전 일은 잘 되어 가?", en: "How have you been? Is your new job as a taxi driver coming along well?", source: "Day 027 교재2", meaning: null },
        { week: 6, ko: "나쁘지는 않지만, 겨우 먹고 사는 수준이야.", en: "It’s not bad, but I’m just barely making enough to get by.", source: "Day 027 교재2", meaning: meanings["get by_2"] },
        { week: 6, ko: "하루에 5시간도 안 자고 어떻게 버티니?", en: "How can you get by on less than five hours of sleep a day?", source: "Day 027 교재2", meaning: meanings["get by_3"] },
        { week: 6, ko: "시험이 3주밖에 안 남았어. 그러니 어떡해? 시간을 더 들여서라도 공부를 해야지.", en: "I’m only three weeks away from taking the test. What else can I do? I need the extra time to study.", source: "Day 027 교재2", meaning: null },
        { week: 6, ko: "서울 생활은 어때요?", en: "How is life treating you in Seoul?", source: "Day 027 교재3", meaning: null },
        { week: 6, ko: "음, 괜찮아요. 서울 같은 활기찬 도시에서 생활할 수 있는 기회를 가진 건 행운이라고 생각해요.", en: "Well, it’s going alright. I consider myself lucky that I got the chance to live in a vibrant city like Seoul.", source: "Day 027 교재3", meaning: null },
        { week: 6, ko: "서울에서의 생활에 만족하시는 듯하네요. 그래도 좀 불편한 것이 있을 텐데요?", en: "You sound like you are quite happy living here. There must be some things that bother you, though, right?", source: "Day 027 교재3", meaning: null },
        { week: 6, ko: "그렇지는 않아요. 대체로 만족해요. 나쁘지 않아요.", en: "Not really. I am satisfied, overall. I can’t complain.", source: "Day 027 교재3", meaning: null },
        { week: 6, ko: "한국어도 상당히 잘하시던데요. 그 정도면 서울에서 혼자 생활하기에 충분한 듯해요. 한국어 실력이 큰 도움이 되었겠네요!", en: "I’m pretty impressed with your Korean as well. It’s good enough to get by on your own in the city. That must have helped a lot!", source: "Day 027 교재3", meaning: meanings["get by_3"] },
        { week: 6, ko: "말씀 고마워요. 그래도 아직 한국어 실력이 부족한 기분이네요.", en: "Thank you for saying so. I still feel like my Korean could use some work, though.", source: "Day 027 교재3", meaning: null },
        { week: 6, ko: "직원들은 사원증 없이는 건물에 들어갈 수 없다.", en: "Employees can’t get into the building without their company ID card.", source: "Day 028 교재1", meaning: meanings["get into_1"] },
        { week: 6, ko: "오늘 오전에 구글 독스 폴더에 접속하려고 했는데 접속이 안 되었다.", en: "I tried to get into the Google Docs folder this morning, but I couldn’t.", source: "Day 028 교재1", meaning: meanings["get into_1"] },
        { week: 6, ko: "동생이 초콜릿 칩 쿠키에 손을 못 대게 해야 해. 쿠키를 먹으면 밤새 잠을 못 잘 테니.", en: "Make sure your brother doesn’t get into the chocolate chip cookies. He’ll be up all night.", source: "Day 028 교재1", meaning: meanings["get into_1"] },
        { week: 6, ko: "재수를 했음에도 불구하고 제가 꿈꾸던 대학에 들어가지 못했습니다.", en: "I couldn’t get into my dream university even on my second attempt.", source: "Day 028 교재1", meaning: meanings["get into_2"] },
        { week: 6, ko: "그 건물에 들어가는 방법을 알아내는 데에만 10분이 걸렸다.", en: "It took me 10 minutes just to figure out how to get into the building.", source: "Day 028 교재1", meaning: meanings["get into_1"] },
        { week: 6, ko: "암호가 걸려 있어서 폴더 접속이 안 됩니다.", en: "I can’t get into the folder because it’s password protected.", source: "Day 028 교재1", meaning: meanings["get into_1"] },
        { week: 6, ko: "민수가 약상자에 손을 대는 바람에 응급실로 데려가야만 했다.", en: "We had to take Minsu to the emergency room because he got into the medicine cabinet.", source: "Day 028 교재1", meaning: meanings["get into_1"] },
        { week: 6, ko: "우리 만난 지 한참 되었네. 목표로 하던 학교에는 입학한 거야?", en: "It’s been a while since we talked. Did you get into the school you were aiming for?", source: "Day 028 교재2", meaning: meanings["get into_2"] },
        { week: 6, ko: "아니, 떨어졌어. 그래서 수능을 다시 봐야만 했는데, 점수가 (합격 선에) 못 미쳤어. 두 번을 봤는데도 말이야.", en: "No, I couldn’t get in. I had to take the SAT again and I still didn’t get a high enough score, even though it was the second time.", source: "Day 028 교재2", meaning: null },
        { week: 6, ko: "안녕하세요. 다름이 아니고 제가 구글 문서에 접속을 시도했는데 안 되더라고요. 죄송한데 방법 한 번만 더 설명해 주실 수 있을까요? 제가 뭔가를 놓친 것 같아요.", en: "Good evening. I just wanted to let you know I tried, but couldn’t get into the Google document. Would you mind going over the directions again with me? It seems like I missed something.", source: "Day 028 교재2", meaning: meanings["get into_1"] },
        { week: 6, ko: "제 실수예요. 액세스 권한을 드리는 걸 깜박했네요. 지금 바로 해드리겠습니다. 네, 이제 접속될 거예요. 한번 해 보세요.", en: "My bad. I just forgot to give you access, so let me do that right now. Okay, you should be able to get into it now. Give it a try.", source: "Day 028 교재2", meaning: meanings["get into_1"] },
        { week: 6, ko: "보고 있는 서류가 뭔가요?", en: "What are those papers you’re looking at?", source: "Day 028 교재2", meaning: null },
        { week: 6, ko: "이웃집 고양이 한 마리가 저희 마당에 들어와서 못을 밟았어요. 그래서 (주인이) 수의사한테 (고양이를) 데려갔는데 병원비를 변상하라고 하네요. 근데 좀 어이없는 액수라서!", en: "One of our neighbor’s cats got into our yard and stepped on a nail. They had to take him to the vet, and now they’re demanding that we reimburse them. But the amount is ridiculous!", source: "Day 028 교재2", meaning: meanings["get into_1"] },
        { week: 6, ko: "어느 날 아침 일어나 보니 휴대폰에 가족과 친구들로부터 적어도 20건의 메시지가 와 있었다.", en: "I woke up one morning and saw that I had at least twenty messages on my phone from family and friends.", source: "Day 028 교재3", meaning: null },
        { week: 6, ko: "내 인스타그램 계정에 이상한 사진을 올린 이유를 물었다.", en: "They were asking me why I posted weird pictures on my Instagram account.", source: "Day 028 교재3", meaning: null },
        { week: 6, ko: "당연히 무슨 소리를 하는 건지 알 수가 없었다.", en: "Obviously, I had no idea what they were talking about.", source: "Day 028 교재3", meaning: null },
        { week: 6, ko: "계정에 접속하려고 했더니 비번이 변경되었다는 것을 알게 되었다.", en: "When I tried to get into my account, though, I realized that my password had been changed!", source: "Day 028 교재3", meaning: meanings["get into_1"] },
        { week: 6, ko: "다들 내가 해킹을 당한 것이며, 사진을 올린 건 내가 아니라는 것을 알게 되었을 때는 마음이 좀 놓였다.", en: "Once everyone knew I had been hacked and it wasn’t me, I felt a little better.", source: "Day 028 교재3", meaning: null },
        { week: 6, ko: "하지만 더 이상 5년 치 사진에 접속할 수 없게 되었다는 걸 알게 되었고, 정말 끔찍한 기분이 들었다.", en: "But then I realized that I could no longer access about five years’ worth of photos, and that was a terrible feeling.", source: "Day 028 교재3", meaning: null },
        { week: 6, ko: "최근에 레코드판 모으는 데 재미가 붙기 시작했다.(= 관심이 생겼다.)", en: "I recently got into collecting records.", source: "Day 029 교재1", meaning: meanings["get into_3"] },
        { week: 6, ko: "작년에 사진 찍는 데 관심이 생겼는데 이젠 너무 재미있는 취미가 되었다.", en: "I got into photography last year, and it has become a fun hobby for me.", source: "Day 029 교재1", meaning: meanings["get into_3"] },
        { week: 6, ko: "지금 읽고 있는 책에 재미가 안 붙어요.", en: "I can’t get into this book I am reading.", source: "Day 029 교재1", meaning: meanings["get into_3"] },
        { week: 6, ko: "프랑스 영화는 매력적인 것 같다. 하지만 재미를 붙이는 게 너무 어렵다. 프랑스 문화는 너무 매력적이라서 정말 아쉽게 느껴진다.", en: "French cinema sounds fascinating, but I just can’t get into it. It’s such a shame, because I find the French culture so charming.", source: "Day 029 교재1", meaning: meanings["get into_3"] },
        { week: 6, ko: "남편과 저는 아무것도 아닌 것 가지고 늘 싸웁니다. 지난번에는 누가 고양이를 씻길 것인가를 두고 싸웠어요.", en: "My husband and I always get into it over silly things. Last time we fought over who would wash the cat.", source: "Day 029 교재1", meaning: meanings["get into_4"] },
        { week: 6, ko: "축구 경기 중에 두 선수가 파울한 것을 두고 싸우는 바람에 주심이 경기를 중단하고 두 선수를 떼 놓아야만 했다.", en: "During the soccer match, two players got into it over a foul, causing the referee to stop the game and separate them.", source: "Day 029 교재1", meaning: meanings["get into_4"] },
        { week: 6, ko: "점심 먹으면서 정치 이야기는 하지 말자. 다들 소화가 잘 안될 거야.", en: "Let’s not get into politics over lunch. Everyone will get indigestion.", source: "Day 029 교재1", meaning: meanings["get into_5"] },
        { week: 6, ko: "최근에 그림 그리는 데 재미가 붙었어요. 늘 해 보고 싶었던 거예요.", en: "I recently got into painting. It’s something that I always wanted to do.", source: "Day 029 교재1", meaning: meanings["get into_3"] },
        { week: 6, ko: "추천해 준 (미드) 시리즈를 보려고 노력했는데, 무슨 이유에서인지 재미가 안 붙더라.", en: "I tried watching the series you recommended, but I just couldn’t get into it for some reason.", source: "Day 029 교재1", meaning: meanings["get into_3"] },
        { week: 6, ko: "Jake와 나는 만나기만 하면 어떤 선수가 더 뛰어난지를 두고 싸운다. 마이클 조던이냐 코비 브라이언트냐.", en: "Jake and I can’t hang out without getting into it over who was better: Michael Jordan or Kobe Bryant.", source: "Day 029 교재1", meaning: meanings["get into_4"] },
        { week: 6, ko: "애들 앞에서 돈 문제 얘기하고 싶지 않아. 나중에 얘기하면 안 돼?", en: "I don’t want to get into our money problem in front of the kids. Can’t we talk about it later?", source: "Day 029 교재1", meaning: meanings["get into_5"] },
        { week: 6, ko: "요즘 Sara가 왜 금요일 밤 영화 모임에 안 오는 거지?", en: "Why hasn’t Sara been coming to our Friday movie nights?", source: "Day 029 교재2", meaning: null },
        { week: 6, ko: "그 친구 최근에 코딩에 재미가 붙었어. 대박 앱을 개발할 거라고 믿고 있어. 저녁 시간의 대부분을 프로그래밍 공부와 소프트웨어 아이디어 개발에 보내고 있지.", en: "She got into coding recently. She believes she’s going to create a hit app, so she spends most of her evenings learning programming and developing her software ideas.", source: "Day 029 교재2", meaning: meanings["get into_3"] },
        { week: 6, ko: "그 사람 네 첫 번째 남자 친구잖아. 근데 네가 그 사람 첫 번째 여자 친구일까? 무슨 이유에서인지 아닌 것 같아. 그 사람 경험이 많아 보여.", en: "I know he’s your first boyfriend, but are you his first girlfriend? For some reason, I doubt it. He seems experienced.", source: "Day 029 교재2", meaning: null },
        { week: 6, ko: "이전 연애 경험에 대해 물어봤는데, 자세히 말을 안 해 주더라고.", en: "I asked him about his relationship history, but he didn’t really get into it.", source: "Day 029 교재2", meaning: meanings["get into_5"] },
        { week: 6, ko: "우리 팀장이랑 사장이 카페에서 싸웠다고 하던데. 왜 그랬는지 알아?", en: "I heard that our supervisor and the boss started getting into it in the cafeteria. Do you have any idea why?", source: "Day 029 교재2", meaning: meanings["get into_4"] },
        { week: 6, ko: "새로운 근무 시간 때문이지. 사장은 우리가 좀 더 일찍 근무를 시작하길 원해. 그래야 인도 사무실이랑 시간이 맞거든. 하지만 팀장은 우리 대부분이 출퇴근 시간이 굉장히 길다는 이유로 반대했거든.", en: "It was about the new office hours. The boss wants us to start earlier to better match the India office, but our supervisor didn’t agree because many of us have long commutes.", source: "Day 029 교재2", meaning: null },
        { week: 6, ko: "이번 주말에 드디어 조금 쉴 수 있는 시간이 생겼다. 그래서 그동안 못했던 독서를 할 생각에 들떴다.", en: "This weekend I finally had a little downtime, and I was looking forward to catching up on some reading.", source: "Day 029 교재3", meaning: null },
        { week: 6, ko: "하지만 아내는 자기랑 같이 리얼리티 연애 프로그램을 보자고 졸랐다.", en: "My wife, however, begged me to watch this new reality dating series with her.", source: "Day 029 교재3", meaning: null },
        { week: 6, ko: "내가 리얼리티 쇼를 그다지 좋아하지 않는다는 건 아내도 안다. 하지만 꼭 자기랑 봐야 한다고 했다. 이번 프로그램은 다르다고 했다.", en: "She knows I’m not a fan of reality TV, but she was adamant that I watch it with her, because she said that this show was different.", source: "Day 029 교재3", meaning: null },
        { week: 6, ko: "그래서 결국 그럼 한번 보기나 해보자고 했다. 하지만 전혀 기대를 하지 않았다.", en: "I eventually agreed to give it a shot, but my expectations could not have been lower.", source: "Day 029 교재3", meaning: null },
        { week: 6, ko: "그런데 놀랍게도 내가 틀렸다! 웃고, 울고, 심지어 얼굴도 본 적 없는 출연자가 진정한 짝을 찾을 수 있도록 응원을 한 것이다.", en: "To my surprise, I was wrong! I laughed, I cried, and found myself rooting for these strangers to find their true love.", source: "Day 029 교재3", meaning: null },
        { week: 6, ko: "보자마자 빠져들었다.", en: "I got into it right away.", source: "Day 029 교재3", meaning: meanings["get into_3"] },
        { week: 6, ko: "너무 재미있어서 아내에게 다음 에피소드를 나 없이 혼자 보지 않겠다는 약속을 받았다.", en: "So much so that I made my wife promise not to watch the next episode without me.", source: "Day 029 교재3", meaning: null },
        { week: 6, ko: "Josh가 어제 이사 가면서 소파를 문 앞에 두고 갔다. 그래서 밖에 나갈 때 마다 타넘고 가는 게 여간 번거로운 일이 아니었다.", en: "When Josh was moving yesterday, he left the sofa in front of the door. It was such a hassle to get over it every time I went outside.", source: "Day 030 교재1", meaning: meanings["get over_1"] },
        { week: 6, ko: "휴지통이 그렇게 무거운 줄은 몰랐다. (마당에 있는) 큰 쓰레기통 위로 넘길 수가 없었다. 그래서 휴지통 전체가 뒤로 기울면서 (안에 있던) 쓰레기가 전부 밖으로 쏟아져 버렸다.", en: "I didn’t realize how heavy the trash can was, and I couldn’t get it all the way over the top of the dumpster. So the whole garbage can tipped backward, and the trash emptied out of the bags.", source: "Day 030 교재1", meaning: meanings["get over_1"] },
        { week: 6, ko: "제가 주삿바늘을 너무 무서워해서 독감 주사를 맞히려면 형이랑 아빠가 저를 붙들고 있어야만 했답니다. 이 공포를 극복하는 데 한참이 걸렸습니다.", en: "I had such a fear of needles that my brother and dad had to hold me down to get a flu shot. It took me a while to get over that fear.", source: "Day 030 교재1", meaning: meanings["get over_2"] },
        { week: 6, ko: "제가 나사 직원이라는 게 아직 믿기지가 않아서 얼떨떨합니다.", en: "I still can’t get over the fact that I actually work at NASA.", source: "Day 030 교재1", meaning: meanings["get over_3"] },
        { week: 6, ko: "한국 정부가 외국인 거주자를 대하는 방식에 너무 화가 납니다. 우리를 국가 건강 보험에서도 제외시키려고까지 하거든요.", en: "I still can’t get over how the government is treating foreign residents of this country. They’re even thinking of excluding us from the national health insurance.", source: "Day 030 교재1", meaning: meanings["get over_3"] },
        { week: 6, ko: "이런 날씨 상황에서 우리 차가 언덕을 넘을 수 있을 것 같지 않군. 돌아가더라도 둘러서 가는 게 나을 것 같아.", en: "I don’t think my car will be able to get over that hill in this weather. Better take the long way around.", source: "Day 030 교재1", meaning: meanings["get over_1"] },
        { week: 6, ko: "수줍음을 이기고 자신감을 얻는 방법", en: "How to Get Over Shyness and Gain Confidence", source: "Day 030 교재1", meaning: meanings["get over_2"] },
        { week: 6, ko: "낯선 사람에게 말을 거는 두려움만 떨치면 자네는 좋은 영업사원이 될 재목이야.", en: "You would make a really good salesperson if you could just get over your fear of talking to strangers.", source: "Day 030 교재1", meaning: meanings["get over_2"] },
        { week: 6, ko: "저주를 깨고 우승을 했다는 사실이 믿기지가 않습니다. 저희 팀이 마지막으로 우승했을 때는 저는 태어나지도 않았었죠.", en: "I still can’t get over the fact that we’ve finally broken the curse and won a championship. The last time this happened, I wasn’t even born yet.", source: "Day 030 교재1", meaning: meanings["get over_3"] },
        { week: 6, ko: "그가 미안하다고 했지만 아직 화가 안 풀려. 둘이 같이 있었다는 생각이 멈추질 않아.", en: "He said he was sorry, but I just can’t seem to get over it. I can’t stop thinking about the two of them together.", source: "Day 030 교재1", meaning: meanings["get over_2"] },
        { week: 6, ko: "지난번 갔을 때와 너무 달라진 게 믿기지가 않아요. 아름다운 역사적인 건물들을 너무 많이 없앴더라고요.", en: "I can’t get over how different the city looks since I last visited. They’ve gotten rid of so many beautiful, historical buildings.", source: "Day 030 교재1", meaning: meanings["get over_3"] },
        { week: 6, ko: "여기서 만나니 정말 반갑네요. 잘 지내시죠? 천안은 어떤가요?", en: "I’m so glad I ran into you here. Is everything alright with you? How do you like living in Cheonan?", source: "Day 030 교재2", meaning: null },
        { week: 6, ko: "새로 이사 간 동네가 꽤나 마음에 듭니다. 그나저나 사장님 집 빵이 너무 맛있어서 자꾸 생각납니다. 삼각지 갈 일 있으면 꼭 들를게요.", en: "I am quite happy with my new neighborhood. By the way, I still can’t get over how good your bread is. I will definitely drop by your bakery when I get a chance to visit Samgakji.", source: "Day 030 교재2", meaning: meanings["get over_3"] },
        { week: 6, ko: "Jeff, 잘 들어 봐요. 나도 영업을 처음 할 때는 딱 당신 같았어요. 거절에 대한 공포를 이기지 못하면 성공할 수 없어요.", en: "Jeff, let me tell you ‒ I was just like you when I was starting out in sales. If you can’t get over your fear of rejection, you’ll never be successful.", source: "Day 030 교재2", meaning: meanings["get over_2"] },
        { week: 6, ko: "조언 감사드립니다. 거절에 대한 두려움을 이겨 내야 한다는 점 잘 알고 있어요. 꼭 극복하고 나아지겠습니다.", en: "Thanks for the advice. I know my fear of rejection is something I need to work on. I’m determined to overcome it and improve.", source: "Day 030 교재2", meaning: null },
        { week: 6, ko: "시내에 새로 생긴 식당 들어 봤어? 제일 싼 음식이 20만 원이 넘는대. 너무 비싸서 믿기지가 않네.", en: "Did you hear about the new restaurant downtown? Their cheapest dish is over 200,000 won. I still can’t get over how pricey it is.", source: "Day 030 교재2", meaning: meanings["get over_3"] },
        { week: 6, ko: "정말? 말도 안 돼! 이런 고급 식당은 좋은 음식을 파는 게 아니라 과시용 성격이 강한 듯해. 그래도 예약하고 인스타에 자랑하려고 줄 서는 사람들이 넘쳐날 거야.", en: "Really? That’s outrageous! It’s like these fancy places are more about status than actually serving good food. Still, I’m sure plenty of people are lining up to get a table and show off (their status) on Instagram.", source: "Day 030 교재2", meaning: null },
        { week: 6, ko: "제 브랜드 ‘Me.Me’가 다음 주 공식 론칭을 하게 되었음을 알리게 되어 기쁩니다.", en: "I’m pleased to tell you my personal brand, ‘Me.Me’, will officially be launched next week.", source: "Day 030 교재3", meaning: null },
        { week: 6, ko: "제 자신의 의류 브랜드를 가지게 된 게 아직도 얼떨떨합니다... 한번 꼬집어 주세요!", en: "I still can’t get over the fact that I’ve got my own clothing brand… just pinch me, please!", source: "Day 030 교재3", meaning: meanings["get over_3"] },
        { week: 6, ko: "제 옷과 제가 만든 모든 것을 사랑해 주신 점에 어떻게 감사를 드려야 할지 모르겠네요.", en: "I can’t thank you all enough for loving my clothes and everything I’ve come up with.", source: "Day 030 교재3", meaning: null },
        { week: 6, ko: "여러분들의 성원이 없었다면 불가능했을 겁니다.", en: "This wouldn’t have been possible without your support.", source: "Day 030 교재3", meaning: null },
        { week: 6, ko: "감사의 마음과 브랜드 론칭 기념으로 작은 선물을 준비했습니다. 모든 제품에 대해 30% 할인을 해드립니다!", en: "To thank you, and to celebrate the launch of my brand, I have a small gift for everyone ‒ 30% off everything in the store!", source: "Day 030 교재3", meaning: null },
        { week: 6, ko: "별도의 코드는 필요 없습니다. 홈페이지에 모든 제품이 할인가로 되어있습니다.", en: "You don’t need a code; everything’s reduced on the website.", source: "Day 030 교재3", meaning: null },
        { week: 6, ko: "전 세계에 배송하며, 세일은 일요일에 끝납니다. 즐겁게 쇼핑하세요, 천사님들!", en: "We ship worldwide, and the sale ends on Sunday. Happy shopping, Angels!", source: "Day 030 교재3", meaning: null }
    ];

    // 3. 영어회화 데이터 (Week 5 & Week 6)
    const rawConvData = [
        // Week 5 (Day 21~25)
        { week: 5, source: "Day021 교재1", ko: "그 친구는 저희가 진지하게 사귀는 관계인 줄 아는데, 저는 안 그렇거든요.", en: "She thinks we’re in a serious relationship, but I don’t see it that way." },
        { week: 5, source: "Day021 교재1", ko: "이 케이크 너무 달다고 그랬나? 안 그런 것 같은데.", en: "You said the cake is too sweet? I don’t see it that way." },
        { week: 5, source: "Day021 교재1", ko: "많은 사람들이 부동산 가격이 계속 하락할 거라고 생각하지만 제 생각은 다릅니다.", en: "Many people believe real estate prices will keep falling, but I don’t see it that way." },
        { week: 5, source: "Day021 교재1", ko: "일부 언론에서는 Elon Musk가 트위터를 망치고 있다고 하는데, 제 생각은 다릅니다.", en: "Some news agencies claim that Elon Musk is ruining Twitter, but I don’t see it that way." },
        { week: 5, source: "Day021 교재1", ko: "좋은 예문이긴 한데 출판사 생각은 다를 거라는 게 문제죠.", en: "I think that’s a great example, but the thing is, I don’t think the publisher is going to see it that way." },
        { week: 5, source: "Day021 교재2", ko: "우리 부모님은 늘 이렇게 가르치셨어. 노숙자를 도와주면 상황이 더 나빠진다고.", en: "My parents always taught me that supporting the homeless just makes the problem worse." },
        { week: 5, source: "Day021 교재2", ko: "나는 좀 생각이 다른데. 그들을 도와주면 그들의 삶이 더 나아질 거야.", en: "I don’t really see it that way. Supporting them could change their lives for the better." },
        { week: 5, source: "Day021 교재2", ko: "요즘 한국 출산율이 너무 낮아. 가정을 꾸리기에 알맞은 집을 마련하는 걸 감당할 수 없어서 그렇다고 봐. 좋은 집이 없으면, 어떻게 애들을 키우겠어?", en: "The birth rate in Korea is so low these days. I really think it’s because people can’t afford a proper house for a family. Without a good home, how could you raise kids?" },
        { week: 5, source: "Day021 교재2", ko: "좋은 지적이긴 한데, 내 생각은 좀 달라. 교육비가 너무 많이 들기 때문이라고 생각해.", en: "That’s a good point, but I don’t really see it that way. I think it’s because the cost of educating kids is too expensive." },
        { week: 5, source: "Day021 교재3", ko: "(CEO 인터뷰 내용)\n일부 전문가들은 저희가 사업을 온라인으로 전환해야 한다고 합니다. 특히, 비싼 시내 중심가에서 물리적인 사무실 공간을 임대하는 건 합리적이지 않다고들 합니다. 저는 그렇게 생각하지 않습니다. 직원들이 같은 공간에서 함께 일하게 될 때 업무 능률이 오르는 뭔가가 있습니다.", en: "Some experts say we should move business online. They argue that renting physical office space, especially in expensive downtown areas, doesn’t make any sense. I don’t see it that way. There is something about working together in the same room that helps employees stay productive." },
        { week: 5, source: "Day021 교재4", ko: "일리 있는 말씀입니다만…", en: "You’ve got a point there, but I actually..." },
        { week: 5, source: "Day021 교재4", ko: "왜 그런 말씀을 하시는지는 알겠습니다. 하지만 제 생각은 다릅니다.", en: "I see where you are coming from, but I had a different idea." },
        { week: 5, source: "Day021 교재4", ko: "그 점은 동의하기 힘듭니다.", en: "I’m not sure I agree with you on that." },
        { week: 5, source: "Day021 교재4", ko: "그 점에 있어서 동의하지는 않지만, 왜 그런 말을 하는지는 알 것 같네요.", en: "I don’t really agree with you on that, but I see where you are coming from." },
        { week: 5, source: "Day021 대표", ko: "제 생각은 좀 다릅니다.", en: "I don’t see it that way." },
        { week: 5, source: "Day022 교재1", ko: "제 월급으로 그 차를 살 수 있을지 모르겠네요.", en: "I’m not sure if I can afford that car on my salary." },
        { week: 5, source: "Day022 교재1", ko: "TV 큰 걸로 하자. 감당할 수 있어.", en: "Let’s go with the bigger TV. We can afford it." },
        { week: 5, source: "Day022 교재1", ko: "외식할 형편이 안 됩니다.", en: "We can’t afford to eat out." },
        { week: 5, source: "Day022 교재1", ko: "강남에 살 형편이 안 됩니다.", en: "I just can’t afford to live in Gangnam." },
        { week: 5, source: "Day022 교재1", ko: "저희가 귀사의 서비스료를 감당하기 힘들 것 같습니다.", en: "I’m afraid we can’t afford your fees." },
        { week: 5, source: "Day022 교재2", ko: "폭스바겐 비틀 갖는 게 평생소원이었어. 이제 한 대 살 수 있을 줄 알았는데, 보니까 내가 감당하기 힘들 것 같아.", en: "I’ve wanted a Volkswagen Beetle my entire life. I thought I could get one now, but it looks like it’s more than I can afford." },
        { week: 5, source: "Day022 교재2", ko: "우선 돈을 모으고 몇 년 있다가 한 대 사. 아니면 꼭 갖고 싶으면 할부로 해. 너의 드림카잖아!", en: "Start saving and buy one in a few years. Or if you really want it, pay for it in installments. It’s your dream car!" },
        { week: 5, source: "Day022 교재2", ko: "매달 옷 사는 데 돈을 그렇게나 많이 쓰다니!", en: "I can't believe how much money you spend on clothes every month!" },
        { week: 5, source: "Day022 교재2", ko: "응, 나도 무리하는 거야. 빚이 산더미야.", en: "Yeah, I can’t really afford it. I’m deeply in debt." },
        { week: 5, source: "Day022 교재3", ko: "유니클로는 일본 어디에서나 볼 수 있으며 캐시미어 점퍼, 버튼다운셔츠, 경량 다운재킷과 같은 누구나 살수 있는 저렴한 기본 패션 아이템으로 전 세계적으로도 성공을 거두었습니다. 호주 매장은 2014년 4월멜버른에서 처음 오픈했으며 빠른 확장세를 보이면서 지금은 호주 전역에 걸쳐 16개의 매장을 가지고 있습니다.", en: "Uniqlo is ubiquitous in Japan and has found success across the globe with its range of basic fashion items anyone can afford, like cashmere jumpers, button-down shirts and lightweight down jackets. The chain opened its first Australian store in Melbourne in April 2014 and has expanded rapidly to now have 16 stores across all mainland states." },
        { week: 5, source: "Day022 교재4", ko: "네가 우리 딸 좀 맡아 줘야 할 것 같아.", en: "I think you could take her on." },
        { week: 5, source: "Day022 교재4", ko: "나 무지 비싸거든.", en: "You can’t afford me." },
        { week: 5, source: "Day022 교재4", ko: "점심시간은 꽉 채워서 한 시간이죠?", en: "Are you taking a full hour for lunch?" },
        { week: 5, source: "Day022 교재4", ko: "아니요, 바빠서 그렇게 오랜 시간 점심을 먹을 형편이 안 됩니다.", en: "No, I can’t afford to take that long." },
        { week: 5, source: "Day022 대표", ko: "저한테는 좀 부담스러운 금액이었어요.", en: "It was something I could barely afford." },
        { week: 5, source: "Day023 교재1", ko: "전부 제가 생각하고 있는 예산 밖이군요. 이십만 원 미만은 없을까요?", en: "This is all out of my price range. Don’t you have anything under 200,000 won?" },
        { week: 5, source: "Day023 교재1", ko: "제일 저렴한 것도 제 예산 밖이더라고요.", en: "Even the cheapest one was out of my price range." },
        { week: 5, source: "Day023 교재1", ko: "추천해 주신 나무 테이블이 제 예산을 훨씬 초과하네요.", en: "I’m afraid the wooden table you recommended is way out of my price range." },
        { week: 5, source: "Day023 교재1", ko: "제철이 아닌 과일이나 채소는 늘 너무 비싸요.", en: "Out-of-season fruit and vegetables are always out of my price range." },
        { week: 5, source: "Day023 교재1", ko: "우리 아파트 근처에 있는 쉐보레 매장에 가 봤는데, 내가 생각하는 가격대의 차는 없었어요.", en: "I checked out the Chevrolet dealership near my apartment, but they didn’t have anything in my price range." },
        { week: 5, source: "Day023 교재2", ko: "이 키보드에 관심 있는데요, 삼십만 원은 조금 비싸네요. 혹시 조금 깎아 주실 수 있는지요?", en: "I’m interested in this keyboard, but 300,000 won is a bit out of my price range. Could you go any lower?" },
        { week: 5, source: "Day023 교재2", ko: "어느 정도 생각하셨는데요?", en: "How much lower were you thinking?" },
        { week: 5, source: "Day023 교재2", ko: "저희 제품들은 모두 특별히 덴마크에서 수입해요. 이건 천만 원이에요.", en: "All of our selections are specially imported from Denmark. This one is 10 million won." },
        { week: 5, source: "Day023 교재2", ko: "아. 제 예산보다 훨씬 비싸군요. 좀 더 저렴한 건 없나요?", en: "Oh. That’s way out of my price range. Do you have anything cheaper?" },
        { week: 5, source: "Day023 교재2", ko: "매물로 나온 것을 보기 전에, 우선 생각하고 있는 금액대를 물어봐도 될까요?", en: "Before we get started looking at what’s available, can I ask your price range?" },
        { week: 5, source: "Day023 교재2", ko: "삼억 원 이상은 쓰고 싶지 않습니다.", en: "We wouldn’t want to spend more than 300 million won." },
        { week: 5, source: "Day023 교재3", ko: "(견적을 받아 본 고객이 인테리어 업자에게 보내는 이메일)\n견적서 감사드립니다. 귀사에서 제안 주신 내용이 저희 예산을 조금 초과합니다. (공사 비용을) 추가로 10% 깎아 주실 여지가 있을까요? 이해해 주시면 감사하겠습니다.", en: "Thank you for your quote. What you’re offering is a little out of our price range. Is there any way you can come down another 10%? I would appreciate your understanding." },
        { week: 5, source: "Day023 교재4", ko: "파리는 차도 너무 많고 식당도 너무 비싸.", en: "Paris is full of traffic and overpriced restaurants." },
        { week: 5, source: "Day023 교재4", ko: "강남에서는 커피가 너무 비싸다.", en: "Coffee is overpriced in Gangnam." },
        { week: 5, source: "Day023 교재4", ko: "광고 대행업체는 너무 비싸.", en: "Agencies are overpriced." },
        { week: 5, source: "Day023 교재4", ko: "호텔 식당은 항상 너무 비싸.", en: "Hotel restaurants are (always) overpriced." },
        { week: 5, source: "Day023 대표", ko: "제가 생각했던 것보다 비싸네요.", en: "This is all out of my price range." },
        { week: 5, source: "Day024 교재1", ko: "소파가 일 일 년밖에 안 됐는데 너덜너덜하네. 싼 게 비지떡이지 뭐.", en: "The couch is falling apart after only a year. We got what we paid for." },
        { week: 5, source: "Day024 교재1", ko: "4천 원도 안 되니 양질의 햄버거는 기대 안 해. 그래도 먹을 만은 해. 딱 그 가격인 듯.", en: "I don’t expect a quality fast food hamburger for less than 4,000 won, so it’s okay. I get what I pay for." },
        { week: 5, source: "Day024 교재1", ko: "왠지 너무 싸다 싶었어요. 싼 게 비지떡이죠.", en: "I should have known it was too good to be true. You get what you pay for." },
        { week: 5, source: "Day024 교재1", ko: "싼 게 비지떡이라는 점 꼭 기억하렴.", en: "Just keep in mind, you get what you pay for." },
        { week: 5, source: "Day024 교재1", ko: "(고가의 외제차 주인이 하는 말) 일억 주고 산 게 이 모양이네.", en: "This is what I get for 100 million won." },
        { week: 5, source: "Day024 교재2", ko: "나 이 헤어드라이어 동네 시장에서 샀는데 싸게 잘 샀다고 생각했거든. 근데 한 번 사용하고 고장나 버렸지 뭐야.", en: "I bought this hair dryer at a local market and I thought I got a great deal. But actually, it broke the first time I used it." },
        { week: 5, source: "Day024 교재2", ko: "싼 게 다 그렇지 뭐. 좋은 걸 원하면 다른 데 가 봐야지.", en: "Well, you get what you pay for. If you want something good, you need to go somewhere else." },
        { week: 5, source: "Day024 교재2", ko: "십만 원 버렸네.", en: "What a waste of 100,000 won!" },
        { week: 5, source: "Day024 교재2", ko: "왠지 너무 싸다고 했어. 싼 게 비지떡이지 뭐.", en: "I knew it was too good to be true. You get what you pay for, I guess." },
        { week: 5, source: "Day024 교재2", ko: "이 오븐 오만 원에 샀는데 한 달 만에 고장 났지 뭐야.", en: "I got this oven for just 50,000 won, but it broke after just a month." },
        { week: 5, source: "Day024 교재2", ko: "그럼 뭘 기대한 거니? 싼 게 비지떡이지.", en: "Well, what were you expecting? You get what you pay for." },
        { week: 5, source: "Day024 교재3", ko: "(허먼 밀러 의자 광고)\n경쟁사 제품과 비교하면 저희 의자가 최고 50%는 더 비쌉니다. 하지만 싼 게 비지떡이라는 점을 꼭 기억해 주세요. 저희 의자는 수리하지 않고 십 년까지 쓸 수 있는 의자입니다", en: "If you compare our prices to our competitors’, it is true that our chairs are up to 50% more expensive. Please keep in mind, you get what you pay for. Our chairs are guaranteed to last up to 10 years without needing repairs." },
        { week: 5, source: "Day024 교재4", ko: "쉐라톤 호텔에 묵을 거면 1일 패키지를 끊어야 해. 풀장 이용이 가능하고 라운지에서 음식과 음료를 무제한으로 즐길 수 있거든. 가성비로 치면 최고지!", en: "If you stay at the Sheraton, you gotta go with the all-day package. It comes with pool access and unlimited food and drinks at the lounge. It definitely gives you the best bang for your buck." },
        { week: 5, source: "Day024 교재4", ko: "이 청소기를 구매하실 것을 추천하는데요, 가성비가 가장 좋기 때문입니다.", en: "I recommend you buy this vacuum cleaner because it is the best bang for the buck." },
        { week: 5, source: "Day024 교재4", ko: "아이폰이 부담되시면, 이 삼성폰을 추천해 드립니다. 이 모델이 가성비가 가장 좋을 겁니다.", en: "If you can’t afford an iPhone, I’d recommend this Samsung. This model will give you the best bang for the buck." },
        { week: 5, source: "Day024 대표", ko: "싼 게 다 그렇지 뭐.", en: "You get what you pay for." },
        { week: 5, source: "Day025 교재1", ko: "마음에 들었다니 다행이네요.", en: "I’m glad you liked it." },
        { week: 5, source: "Day025 교재1", ko: "(파티에서 친구에게) 재미있다니 다행이네.", en: "I’m glad you are enjoying it." },
        { week: 5, source: "Day025 교재1", ko: "오늘 아침 발표를 잘했다니 다행입니다.", en: "I’m glad your presentation went well this morning." },
        { week: 5, source: "Day025 교재1", ko: "(늦게까지 술을 마시는 상황) 내일 일찍 안 일어나도 돼서 얼마나 다행인지.", en: "I’m glad I don’t have to wake up early tomorrow." },
        { week: 5, source: "Day025 교재1", ko: "제 말에 공감해 줘서 다행이네요.", en: "I’m glad you can relate." },
        { week: 5, source: "Day025 교재2", ko: "이 과자 어디서 샀어요? 너무 맛있어요!", en: "Where did you get these cookies? They’re great!" },
        { week: 5, source: "Day025 교재2", ko: "삼촌이 한 통 보내주셨는데 그 과자 회사에서 일하세요. 좋아하시니 다행입니다. 저희는 맛이 질려서요.", en: "We got a whole carton from my uncle, who works for the company. I’m glad you like them. We’re kind of sick of the taste." },
        { week: 5, source: "Day025 교재2", ko: "늦어서 미안. 일을 최대한 빨리 마치고 왔어.", en: "Sorry I’m late. I finished my work as fast as I could." },
        { week: 5, source: "Day025 교재2", ko: "못 올 줄 알았더니 와서 다행이다! 방금 시켰어. 앉아!", en: "I’m glad you could make it! We just ordered. Take a seat!" },
        { week: 5, source: "Day025 교재2", ko: "남자 친구랑 우리 감정에 대해 길게 이야기했고, 결국 화해했어.", en: "My boyfriend and I had a long conversation about our feelings, and we finally made up." },
        { week: 5, source: "Day025 교재2", ko: "이야기가 잘 됐다니 다행이다! 너희 둘은 너무 잘 어울려.", en: "I’m glad things worked out in the end! You two are great together." },
        { week: 5, source: "Day025 교재3", ko: "새 사무실 의자를 알아보러 갔더니 허먼 밀러를 추천해 주었다. 온라인에서 허먼 밀러 의자들을 봤을 때는 예쁘긴 했지만 이백사십만 원이나 하는 게 이해가 안 됐다. 그래도 그냥 사 버렸고, 잘했다 싶다. 정말 너무 편하다.", en: "I was in the market for a new office chair, and Herman Miller was recommended to me. When I saw a good-looking chair of theirs online, I didn’t see how it could possibly be worth 2.4 million won. I went ahead and bought it anyway, and I’m glad I did. I can’t believe how comfy it is." },
        { week: 5, source: "Day025 교재4", ko: "이건 어디서 샀나요?", en: "Where did you get these?" },
        { week: 5, source: "Day025 교재4", ko: "좋아하시니 다행이네요. 있잖아요, 아침마다 이걸 가지고 오겠습니다.", en: "I’m glad you like them. You know what? I’ll start bringing these to you every morning." },
        { week: 5, source: "Day025 교재4", ko: "시즌 3도 나온다니 너무 좋아.", en: "I am happy that they are going to come out with a third season." },
        { week: 5, source: "Day025 교재4", ko: "또 승진에서 누락되어서 실망입니다.", en: "I am disappointed that I have been passed over for a promotion once again." },
        { week: 5, source: "Day025 대표", ko: "베이비시터 구했다니 다행입니다", en: "I’m glad you found a babysitter." },

        // Week 6 (Day 26~30)
        { week: 6, source: "Day026 교재1", ko: "안 되면 부담 없이 알려 주세요.", en: "Feel free to say no." },
        { week: 6, source: "Day026 교재1", ko: "이번 주말에 부모님이 서울에 오신다면서요. 필요하면 부담 갖지 말고 금요일은 쉬세요.", en: "I heard you have your parents coming into Seoul this weekend. Feel free to take Friday off if you need to." },
        { week: 6, source: "Day026 교재1", ko: "제 에세이 보시고 피드백 좀 주실 수 있을까요? 바쁘시면 부담 갖지 말고 안 된다고 하시고요.", en: "Can I get your opinion on my essay? Feel free to say no if you don’t have the time." },
        { week: 6, source: "Day026 교재1", ko: "편하게 제 비서에게 연락해서 회의 잡으시면 됩니다.", en: "Feel free to call my secretary to arrange a meeting." },
        { week: 6, source: "Day026 교재1", ko: "뭐든 골라 봐. 내가 사 줄게.", en: "Feel free to pick what you want, and I’ll pay for it." },
        { week: 6, source: "Day026 교재2", ko: "안 되면 부담 가지지 말고, 혹시 나랑 같이 자라섬 재즈 페스티벌에 갈 수 있나 해서.", en: "Feel free to say no, but I was just wondering if you’d like to come with me to the Jaraseom Jazz Festival." },
        { week: 6, source: "Day026 교재2", ko: "음... 잘 모르겠어. 내일 다시 연락해도 돼?", en: "Umm... I’m not sure. Can I get back to you tomorrow?" },
        { week: 6, source: "Day026 교재2", ko: "안녕하세요. 중고 신발 내놓으신 거 봤습니다. 사고는 싶은데 돈을 마련하려면 시간이 좀 필요해서요.", en: "Hello. I saw your ad for the used shoes. I want to buy them, but I could use some time to come up with the money." },
        { week: 6, source: "Day026 교재2", ko: "관심 감사합니다. 홀딩해 두겠습니다. 구매 준비되시면 편히 알려 주세요.", en: "Thank you for your interest. I’ll put them aside for you. Feel free to let me know when you’re ready to make the purchase." },
        { week: 6, source: "Day026 교재2", ko: "하룻밤 재워 줘서 너무 고마워.", en: "I really appreciate you letting me stay the night." },
        { week: 6, source: "Day026 교재2", ko: "정말 괜찮아. 편하게 샤워하고 그래.", en: "Not a problem! Feel free to use the shower and get comfortable." },
        { week: 6, source: "Day026 교재3", ko: "부서장님 대신 메일 드립니다. 부서장님도 귀사의 제안을 좋다고 하시고 프로젝트를 진행하고자 합니다.다음주 목요일로 회의를 잡았으면 하는데, 중간에 질문이나 우려되는 점 있으시면 부담 가지지 마시고 연락해 주세요. 함께 할 수 있기를 기대합니다.", en: "I’m emailing on behalf of our department head. He agrees with your business proposal and intends to move forward on the project. Let’s set up a meeting for next Thursday, but in the meantime, feel free to contact us with any questions or concerns. We look forward to working with you." },
        { week: 6, source: "Day026 교재4", ko: "부담 드리고 싶은 생각은 없어요.", en: "I didn’t want to make you feel like you owe me ~" },
        { week: 6, source: "Day026 교재4", ko: "자네가 우리 제주도 여행 경비도 내줬는데, 내 생일이라고 괜히 뭐 해 줘야 한다는 부담 가지면 안 되네!", en: "You already paid for our trip to Jeju, so don’t feel like you have to get me anything for my birthday!" },
        { week: 6, source: "Day026 대표", ko: "평일 오전 9시에서 오후 6시 사이에 언제라도 편하게 연락 주시기 바랍니다", en: "Please feel free to contact me any time between 9 and 6 on weekdays." },
        { week: 6, source: "Day027 교재1", ko: "시간이 몇 신데 안 자고 뭐해?", en: "What are you doing up at this hour?" },
        { week: 6, source: "Day027 교재1", ko: "이 늦은 시간에 누가 문을 두드리는 거지?", en: "Who could possibly be knocking on our door at this hour?" },
        { week: 6, source: "Day027 교재1", ko: "대도시에 사는 건 처음이야. 지금 이 시간에 음식 배달이 된다는 게 말이 돼?", en: "This is my first time in a big city. It’s amazing that we can still have food delivered at this hour." },
        { week: 6, source: "Day027 교재1", ko: "내일 배송받고 싶은데요. 지금 이 시간에 주문해도 가능할까요?", en: "I’d like to have it delivered by tomorrow. Is it possible at this hour?" },
        { week: 6, source: "Day027 교재1", ko: "늦은 시간에 연락드려 죄송해요.", en: "Sorry to contact you at this hour." },
        { week: 6, source: "Day027 교재2", ko: "늦은 시간에 질문드려 죄송해요.", en: "Sorry to bother you with a question at this hour." },
        { week: 6, source: "Day027 교재2", ko: "괜찮습니다.", en: "Not a problem." },
        { week: 6, source: "Day027 교재2", ko: "저 오늘 오후에 학부모 간담회 가야 해요.", en: "I have to go to a parent-teacher conference this afternoon." },
        { week: 6, source: "Day027 교재2", ko: "그래서 이렇게 이른 시간에 오신 거예요?", en: "Is that why you came here at this hour?" },
        { week: 6, source: "Day027 교재2", ko: "아침 7시에는 문을 연 카페가 왜 없는 거죠? 사람들이 출근 전에 커피를 안 마시나요?", en: "Why aren’t there any cafes open at 7 a.m.? Don’t people want coffee before going to work?" },
        { week: 6, source: "Day027 교재2", ko: "네, 근데 편의점 가서 사 먹으면 되죠. 카페는 주로 친구들 만나고 동료들이랑 어울리는 장소인데, 이렇게 이른 시간에 그렇게 하는 사람은 흔하지 않거든요.", en: "Yeah, but you can just go to a convenience store for it. Cafes are places to meet friends or coworkers, and that’s not very common at this hour." },
        { week: 6, source: "Day027 교재3", ko: "(인스타그램으로 제품 수령과 관련해 문의하는 상황)\n늦은 시간에 연락드려 죄송합니다. 제가 내일 탁자를 픽업 가려 했는데, 보니까 제가 너무 바쁠 것 같습니다. 혹시 배송 가능할까요? 필요한 비용은 제가 부담하겠습니다. 만일 안 되면 다음 주는 가능할까요?", en: "I’m sorry to contact you at this hour. I was planning on picking up the table tomorrow, but it turns out I’m going to be busy. Is it possible to have it shipped? I’m willing to pay the necessary costs. If not, does next week work for you?" },
        { week: 6, source: "Day027 교재4", ko: "그 집 몇 시에 문 열어?", en: "What time do they open?" },
        { week: 6, source: "Day027 교재4", ko: "그 집 몇 시에 마쳐?", en: "What time do they close?" },
        { week: 6, source: "Day027 교재4", ko: "11시나 되어서 열지.", en: "They don’t open until 11." },
        { week: 6, source: "Day027 교재4", ko: "내일은 영업시간이 어떻게 되나요?", en: "Could you tell me what the hours are for tomorrow?" },
        { week: 6, source: "Day027 교재4", ko: "그 집 영업시간을 잘 몰라.", en: "I am not sure about their hours." },
        { week: 6, source: "Day027 대표", ko: "시간이 몇 신데 커피를 마셔?", en: "Are you drinking coffee at this hour?" },
        { week: 6, source: "Day028 교재1", ko: "집에 오는 길에 밀가루 좀 사 올 수 있어? 밀가루가 다 떨어졌어.", en: "Can you grab some flour on your way home? We just ran out." },
        { week: 6, source: "Day028 교재1", ko: "다시 사무실 들어가기 전에 커피 한 잔 사서 들어갈까 했더니, 안 되겠다. 저기 사람들 줄 좀 봐.", en: "I wanted to grab a coffee before heading back to work, but I don’t think I can. Look at that line." },
        { week: 6, source: "Day028 교재1", ko: "우리 몇 명이서 워크숍 마치고 한잔하려고 하는데. 너도 같이 한잔?", en: "A couple of us are going to grab some drinks after the workshop. You down?" },
        { week: 6, source: "Day028 교재1", ko: "나 써브웨이인데. 너도 샌드위치 좀 사다 줄까?", en: "I’m at Subway. Want me to grab you a sandwich, too, while I’m here?" },
        { week: 6, source: "Day028 교재1", ko: "집에 오는 길에 붕어빵 좀 사다 줘.", en: "I want you to grab me some fish-shaped pastries on your way home." },
        { week: 6, source: "Day028 교재2", ko: "안녕, Henry, 퇴근하고 우리랑 맥주 한잔할래?", en: "Hey, Henry, you feel like grabbing a beer with us after work?" },
        { week: 6, source: "Day028 교재2", ko: "집에 가 봐야 해. 다음에 하자.", en: "I should really get home. Maybe next time." },
        { week: 6, source: "Day028 교재2", ko: "간단하게 아침 먹을까요?", en: "You wanna grab some breakfast?" },
        { week: 6, source: "Day028 교재2", ko: "아니요. 시리얼을 한 그릇 먹어서 배가 너무 불러요. 근데 커피는 좋습니다.", en: "Nah, I just had a big bowl of cereal and I’m pretty stuffed. I am up for coffee, though." },
        { week: 6, source: "Day028 교재2", ko: "미안. 30분 늦을 것 같아.", en: "Sorry, it looks like I’ll be 30 minutes late." },
        { week: 6, source: "Day028 교재2", ko: "그렇게나? 알았어. 스타벅스 가서 커피나 한 잔 사야겠다. 다행히 나 전자책 리더를 가져왔어.", en: "That late? Okay. I’ll go by Starbucks and grab a coffee. I’m glad I brought my e-book reader with me." },
        { week: 6, source: "Day028 교재3", ko: "(여행사가 고객들에게 하루 일정을 알리는 내용)\n박물관 견학 후에, 약 45분간 자유시간을 가진 후 다음 장소로 이동하겠습니다. 그동안은 자유롭게 돌아 다니셔도 됩니다. 입구 근처에 카페가 두세 군데 있는데 기다리는 동안에 커피 한잔하셔도 되고요.", en: "After the museum tour, you will have about 45 minutes of free time before we move on to our next location. You can look around more on your own. There are also a couple of cafes near the entrance, so feel free to grab a coffee while you wait." },
        { week: 6, source: "Day028 교재4", ko: "뭐 사다 줄까요?", en: "What do you want me to get you?" },
        { week: 6, source: "Day028 교재4", ko: "오는 길에 커피 좀 사다 줄 수 있어요?", en: "Do you mind grabbing me some coffee on your way?" },
        { week: 6, source: "Day028 교재4", ko: "저는 매일 택시를 타고 출근합니다.", en: "I take a taxi every day to work." },
        { week: 6, source: "Day028 교재4", ko: "자정이 넘으면 강남에서 택시 잡기가 너무 어려워요.", en: "It’s very hard to grab a taxi in Gangnam after midnight." },
        { week: 6, source: "Day028 대표", ko: "간단하게 아침 식사 하실래요?", en: "You wanna grab some breakfast?" },
        { week: 6, source: "Day029 교재1", ko: "저는 처음 보는 사람들 옆에 있으면 불편해요.", en: "I am not used to being around new people." },
        { week: 6, source: "Day029 교재1", ko: "저는 삼합은 별로예요. 냄새가 적응이 안 됩니다.", en: "I am not really into fermented skate. I can’t get used to how it smells." },
        { week: 6, source: "Day029 교재1", ko: "재택근무에 적응이 안 되네요. 계속 딴짓을 하게 됩니다.", en: "I can’t get used to working from home. I always get distracted." },
        { week: 6, source: "Day029 교재1", ko: "이 갤럭시 폰에 적응하는 데 한참 걸렸어요.", en: "It took me a while to get used to this Galaxy phone." },
        { week: 6, source: "Day029 교재1", ko: "아침에 일찍 일어나는 게 쉽지가 않군요.", en: "I can’t get used to waking up early in the morning." },
        { week: 6, source: "Day029 교재2", ko: "너 일본으로 휴가 가는 거 맞지? 거기서 운전할 거야?", en: "You’re going on vacation to Japan, right? Are you going to drive while you’re there?" },
        { week: 6, source: "Day029 교재2", ko: "절대 아니야. 일본은 우리와 반대 차로로 주행하잖아. 난 적응 못 할 것 같아서.", en: "No way, they drive on the other side of the road. I don’t think I could get used to it." },
        { week: 6, source: "Day029 교재2", ko: "두바이 날씨에 적응이 안 되네요. 11년간 살고 있는데 영원히 적응을 못할 것 같아요.", en: "I can’t get used to the weather in Dubai. I’ve been here 11 years and I’ll just never get used to it." },
        { week: 6, source: "Day029 교재2", ko: "무슨 말인지 너무 잘 알 것 같네요.", en: "I totally get what you mean." },
        { week: 6, source: "Day029 교재2", ko: "여수 날씨가 너무 후텁지근해서 적응이 안 돼요.", en: "I just can’t get used to how muggy it is in Yeosu." },
        { week: 6, source: "Day029 교재2", ko: "나도 처음 왔을 때는 그랬죠.", en: "I felt the same way when I first came here." },
        { week: 6, source: "Day029 교재3", ko: "(애플의 마케팅 전략 회의 내용)\n애플 컴퓨터는 한국 시장 점유율이 매우 낮습니다. 소비자들이 워낙 윈도우 운영 체제에 길들어 있어서 인지, 애플로 바꿀 생각조차 하지 않을 겁니다. 저희로서는 극복하기 어려운 문제입니다.", en: "Apple computers have such a low market share in the Korean market. Consumers are so used to using Windows OS that they won’t even consider making the switch. It’s a problem that’s going to be nearly impossible for us to overcome." },
        { week: 6, source: "Day029 교재4", ko: "우리 만난 지가 1년이 넘었는데, 아직 결혼 이야기도 안 했다.", en: "We’ve been dating for over a year, but we haven’t even talked about marriage, yet." },
        { week: 6, source: "Day029 교재4", ko: "노점상에 가면 옷을 싸게 살 수 있긴 하지. 근데, 온라인이 더 싼 거 같아.", en: "You can find some pretty good deals on clothes at the street market, but they’re even cheaper online, I think." },
        { week: 6, source: "Day029 교재4", ko: "최근 들어 더블샷 아메리카노를 마시기 시작했는데, 이것도 나한테는 카페인이 부족하다.", en: "I’ve started drinking double-shot Americanos, but even they don’t have enough caffeine for me." },
        { week: 6, source: "Day029 대표", ko: "냄새가 적응이 안 되네요.", en: "I can’t really get used to the smell." },
        { week: 6, source: "Day030 교재1", ko: "그곳은 11시나 돼야 열어. 11시 20분에 보자.", en: "They don’t open until 11. See you there at 11:20." },
        { week: 6, source: "Day030 교재1", ko: "저는 열두 살이 되어서야 처음 비행기를 타 봤어요.", en: "I didn’t get on a plane until I was 12." },
        { week: 6, source: "Day030 교재1", ko: "지금은 제가 골초지만, 스물다섯 살이 되어서야 처음으로 담배를 피워 봤답니다.", en: "I’m a big smoker now, but I didn’t try my first cigarette until I was 25." },
        { week: 6, source: "Day030 교재1", ko: "전 서른여덟이 되어서야 처음으로 해외여행을 했답니다.", en: "I didn’t travel outside of Korea until I was 38." },
        { week: 6, source: "Day030 교재1", ko: "죄송한데 9시 반은 되어야 회사에 도착할 수 있을 것 같습니다. 저 없이 회의 시작하시죠.", en: "I’m afraid I won’t be able to get to the office until 9:30. Feel free to start the meeting without me." },
        { week: 6, source: "Day030 교재2", ko: "나는 집에 가봐야겠어. 최소 6시간을 못 자면 다음 날 헤롱헤롱하거든.", en: "I need to head home. I can barely function if I don’t get at least 6 hours of sleep." },
        { week: 6, source: "Day030 교재2", ko: "난 괜찮아. 이 공식들 다 외우고 나서 집에 갈 거야.", en: "I’ll be fine. I’m not leaving until I’ve memorized all of these formulas." },
        { week: 6, source: "Day030 교재2", ko: "잠깐만! 첫 남자 친구라고? 너 스물다섯이잖아!", en: "Wait! Is this your first boyfriend? You’re 25 years old!" },
        { week: 6, source: "Day030 교재2", ko: "응, 근데 기다리길 잘한 것 같아. 올인할 준비가 됐을 때 남자를 사귀고 싶었거든.", en: "Yes, but I think waiting was the right choice. I didn’t want to start a relationship until I was ready to commit." },
        { week: 6, source: "Day030 교재2", ko: "택시 기본요금이 인상되긴 하는데 2월 돼야 올라.", en: "Taxi base fares are going to rise, but not until next February." },
        { week: 6, source: "Day030 교재2", ko: "오, 다행이다. 당분간은 걱정 안 해도 되겠네.", en: "Oh, that’s a relief. I don’t have to worry about it for a while yet." },
        { week: 6, source: "Day030 교재3", ko: "John, 거의 다 온 거니? 나 방금 영화관 도착했는데, 영화가 15분 후에 시작해. 너 오면 그때 들어가려고. 거의 다 왔기를. 첫 장면 놓치고 싶지 않아!", en: "Hey, John. Are you almost here? I just got to the movie theater, and the movie starts in 15 minutes, but I don’t want to go in until you arrive.I hope you’re not far. I don’t want to miss the beginning!" },
        { week: 6, source: "Day030 교재4", ko: "제 아기가 이제 겨우 한 살입니다. 첫돌이 지난 주말이었어요.", en: "My baby is barely one year old. His first birthday was last weekend." },
        { week: 6, source: "Day030 교재4", ko: "(줌 회의 중에) 들리긴 하는데 너무 작게 들려요. 볼륨을 좀 높일게요.", en: "I can barely hear you. Let me turn up the volume." },
        { week: 6, source: "Day030 교재4", ko: "겨우 빚은 지지 않고 삽니다.", en: "I can barely make ends meet." },
        { week: 6, source: "Day030 교재4", ko: "월세도 겨우 내고 있어요.", en: "I can barely afford rent." },
        { week: 6, source: "Day030 교재4", ko: "도착한 거야?", en: "Have you arrived yet?" },
        { week: 6, source: "Day030 교재4", ko: "거의 다 왔어. 2분만 더 가면 돼.", en: "Almost, I’m two minutes away." },
        { week: 6, source: "Day030 대표", ko: "내일이나 올 줄 알았더니.", en: "I wasn’t expecting you until tomorrow." }
    ];

    // 영어회화 난이도 분리
    const convEasy = rawConvData.filter(item => item.source.includes('대표') || item.source.includes('교재1'));
    const convHard = rawConvData.filter(item => !(item.source.includes('대표') || item.source.includes('교재1')));

    // 퀴즈 진행 상태 변수
    let currentQuestions = [];
    let currentIndex = 0;
    let isPhrasalMode = false;
    let starredQuestions = []; 

    function shuffleArray(array) {
        let shuffled = [...array];
        for (let i = shuffled.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
        }
        return shuffled;
    }

    // 선택된 주차에 맞게 필터링 (Week 5 & Week 6 누적)
    function filterByWeek(dataArray) {
        if (currentWeekFilter === 'all') {
            return dataArray.filter(item => item.week >= 5 && item.week <= 6);
        }
        return dataArray.filter(item => item.week === currentWeekFilter);
    }

    function startQuiz(mode) {
        let sourceArray = [];
        let titleMode = "";

        if (mode === 'phrasal-easy') { 
            sourceArray = phrasalEasy; isPhrasalMode = true; 
            titleMode = "🟢 구동사 순한맛 퀴즈"; 
        }
        else if (mode === 'phrasal-hard') { 
            sourceArray = phrasalHard; isPhrasalMode = true; 
            titleMode = "🔴 구동사 매운맛 퀴즈"; 
        }
        else if (mode === 'conv-easy') { 
            sourceArray = convEasy; isPhrasalMode = false; 
            titleMode = "💬 영어회화 순한맛 퀴즈"; 
        }
        else if (mode === 'conv-hard') { 
            sourceArray = convHard; isPhrasalMode = false; 
            titleMode = "🔥 영어회화 매운맛 퀴즈"; 
        }

        const filteredData = filterByWeek(sourceArray);

        if (filteredData.length === 0) {
            alert(`선택하신 주차(${currentWeekFilter === 'all' ? '누적' : 'Week ' + currentWeekFilter})에 해당하는 데이터가 아직 없습니다.`);
            return;
        }

        document.getElementById('mode-selection').classList.add('hidden');
        document.getElementById('quiz-area').classList.remove('hidden');
        
        let weekText = currentWeekFilter === 'all' ? " (5~6주차 누적)" : ` (Week ${currentWeekFilter})`;
        document.getElementById('main-title').innerText = titleMode + weekText; 
        
        starredQuestions = []; 
        // 7문제만 추출 (데이터가 7개 미만이면 전체 추출)
        const qCount = Math.min(filteredData.length, 7);
        currentQuestions = shuffleArray(filteredData).slice(0, qCount);
        currentIndex = 0;
        
        loadQuestion();
    }

    function loadQuestion() {
        document.getElementById('answer-section').classList.add('hidden');
        document.getElementById('btn-next').classList.add('hidden');
        document.getElementById('btn-finish').classList.add('hidden');
        document.getElementById('meaning-info').classList.add('hidden');
        
        const koBox = document.getElementById('ko-box');
        koBox.style.cursor = 'pointer';
        koBox.style.pointerEvents = 'auto';

        const q = currentQuestions[currentIndex];
        document.getElementById('question-counter').innerText = `문제 ${currentIndex + 1} / ${currentQuestions.length}`;
        document.getElementById('ko-text').innerText = q.ko;
        document.getElementById('en-text').innerText = q.en;
        document.getElementById('source-info').innerText = `출처: ${q.source}`;
        
        if(isPhrasalMode && q.meaning) {
            document.getElementById('meaning-info').innerText = q.meaning;
        }

        const starBtn = document.getElementById('btn-star');
        if (starredQuestions.includes(q)) {
            starBtn.classList.add('active');
            starBtn.innerText = "⭐ 어려워요 (저장됨)";
        } else {
            starBtn.classList.remove('active');
            starBtn.innerText = "⭐ 어려워요";
        }
    }

    function showAnswer() {
        const koBox = document.getElementById('ko-box');
        koBox.style.cursor = 'default';
        koBox.style.pointerEvents = 'none';

        document.getElementById('answer-section').classList.remove('hidden');
        
        if(isPhrasalMode && currentQuestions[currentIndex].meaning) {
            document.getElementById('meaning-info').classList.remove('hidden');
        }
        
        if (currentIndex < currentQuestions.length - 1) {
            document.getElementById('btn-next').classList.remove('hidden');
        } else {
            document.getElementById('btn-finish').classList.remove('hidden');
        }
    }

    function toggleStar() {
        const q = currentQuestions[currentIndex];
        const starBtn = document.getElementById('btn-star');
        const index = starredQuestions.indexOf(q);
        
        if (index > -1) {
            starredQuestions.splice(index, 1);
            starBtn.classList.remove('active');
            starBtn.innerText = "⭐ 어려워요";
        } else {
            starredQuestions.push(q);
            starBtn.classList.add('active');
            starBtn.innerText = "⭐ 어려워요 (저장됨)";
        }
    }

    function playTTS() {
        const textToSpeak = currentQuestions[currentIndex].en;
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(textToSpeak);
            utterance.lang = 'en-US';
            utterance.rate = 0.9; 
            window.speechSynthesis.speak(utterance);
        } else {
            alert('이 브라우저에서는 음성 듣기 기능을 지원하지 않습니다.');
        }
    }

    function nextQuestion() {
        currentIndex++;
        loadQuestion();
    }

    function showReview() {
        document.getElementById('quiz-area').classList.add('hidden');
        document.getElementById('review-area').classList.remove('hidden');
        document.getElementById('main-title').innerText = "결과 및 오답 노트";

        const reviewList = document.getElementById('review-list');
        reviewList.innerHTML = '';

        if (starredQuestions.length === 0) {
            reviewList.innerHTML = `<div class="review-empty">🎉 어려운 문장이 없습니다! 완벽해요! 🎉</div>`;
        } else {
            starredQuestions.forEach((q, idx) => {
                let meaningHtml = (isPhrasalMode && q.meaning) ? `<div style="font-size: 13px; color: #b45309; background: #fef3c7; padding: 4px 8px; border-radius: 4px; display: inline-block; margin-bottom: 5px;">${q.meaning}</div>` : '';
                
                reviewList.innerHTML += `
                    <div class="review-card">
                        <div style="font-size: 12px; color: #94a3b8; margin-bottom: 5px;">${idx + 1}. 출처: ${q.source}</div>
                        ${meaningHtml}
                        <div class="review-ko">${q.ko}</div>
                        <div class="review-en">${q.en}</div>
                    </div>
                `;
            });
        }
    }

    function resetToHome() {
        document.getElementById('review-area').classList.add('hidden');
        document.getElementById('mode-selection').classList.remove('hidden');
        document.getElementById('main-title').innerText = "🚀 스피드 영어 퀴즈";
    }
</script>

</body>
</html>
