<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>이전가격 커피챗</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap');
        body {
            font-family: 'Noto Sans KR', sans-serif;
            background-color: #f3f4f6;
        }
        .app-container {
            width: 100%;
            max-width: 420px;
            height: 85vh;
            max-height: 800px;
            background-color: #A8C5D4;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }
        .main-screen {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100%;
            text-align: center;
            color: #4A3728;
            background: linear-gradient(135deg, #FFFDE7, #FFF8E1);
            padding: 2rem;
        }
        .chat-header {
            background-color: #92B3C3;
            color: #3C474D;
            flex-shrink: 0;
        }
        .chat-body {
            scroll-behavior: smooth;
        }
        .chat-bubble {
            max-width: 100%;
            word-wrap: break-word;
        }
        .chat-bubble.me {
            background-color: #FEE500; /* 카카오톡 노란색 */
            color: #3C1E1E;
        }
        .chat-bubble.you {
            background-color: #FFFFFF;
        }
        .timestamp {
            font-size: 0.7rem;
            color: #6b7280;
            flex-shrink: 0;
        }
        .chat-date-separator {
            font-size: 0.75rem;
            color: #ffffff;
            background-color: #83A2B1;
        }
        .message-group {
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.5s ease-out, transform 0.5s ease-out;
            max-width: 85%;
        }
        .message-group.me {
            margin-left: auto;
        }
        .message-group.you {
            margin-right: auto;
        }
        .message-group.visible {
            opacity: 1;
            transform: translateY(0);
        }
        .controls {
            flex-shrink: 0;
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">
    <div class="app-container rounded-lg shadow-lg border">
        <!-- Main Screen -->
        <div id="main-screen" class="main-screen">
            <div class="flex-grow flex flex-col justify-center items-center">
                <h1 class="text-xl font-bold text-gray-600">월간회계 X 리스크마</h1>
                <h2 class="text-3xl font-bold mt-4 text-gray-800">이전가격이 뭔가요?</h2>
            </div>
            <button id="start-btn" class="w-full bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-3 px-4 rounded-lg transition duration-300 mt-8 shadow-md">
                시작하기
            </button>
        </div>

        <!-- Chat Container (Initially Hidden) -->
        <div id="chat-container" class="flex flex-col h-full" style="display: none;">
            <!-- Chat Header -->
            <div class="chat-header p-3 flex items-center shadow-md">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
                </svg>
                <h1 class="text-lg font-bold">권팀장님 (재무회계)</h1>
            </div>

            <!-- Chat Body -->
            <div id="chat-body" class="flex-1 p-4 overflow-y-auto space-y-4 bg-[#A8C5D4]">
                <!-- Date Separator -->
                <div class="text-center my-2">
                    <span class="chat-date-separator text-white px-3 py-1 rounded-full">2025년 8월 29일 금요일</span>
                </div>
                <!-- Conversation Messages will be injected here by JS -->
            </div>

            <!-- Controls -->
            <div class="controls p-4 bg-white border-t">
                <button id="next-btn" class="w-full bg-yellow-400 hover:bg-yellow-500 text-black font-bold py-3 px-4 rounded-lg transition duration-300">
                    다음 대화 보기
                </button>
            </div>
        </div>
    </div>

<script>
    const mainScreen = document.getElementById('main-screen');
    const startButton = document.getElementById('start-btn');
    const chatContainer = document.getElementById('chat-container');
    const chatBody = document.getElementById('chat-body');
    const nextButton = document.getElementById('next-btn');

    // 대화 내용 수정
    const messages = [
        { from: 'me', time: '오후 2:01', text: '팀장님! 내일 리스크매니지먼트 워크숍에서 이전가격 강의를 한다는데... 솔직히 이름부터 너무 어렵습니다. 대체 이전가격이 뭔가요?' },
        { from: 'you', name: '권팀장', time: '오후 2:01', text: '아, 김경환 대리님. 좋은 질문이에요. <b class="font-bold">移轉價格(이전가격)</b>. 말 그대로 <b class="font-bold">가격을 옮긴다</b>는 뜻이에요. 우리 같은 그룹사끼리 거래할 때 정하는 내부 가격을 말하는 거죠.' },
        { from: 'me', time: '오후 2:02', text: '가족끼리 정하는 가격인데... 왜 그게 중요한 경영 리스크가 되는 건가요?' },
        { from: 'you', name: '권팀장', time: '오후 2:03', text: "바로 <b class='font-bold text-red-600'>세금</b> 때문이에요. 국세청은 \"너희 그룹사끼리 거래하더라도, <b class='font-bold'>완전히 독립된 회사끼리 거래하는 것처럼 공정한 시장 가격(정상가격)</b>으로 해야 해!\" 라고 요구하고 있어요. 여기서 '정상'은 우리가 흔히 쓰는 '정상/비정상'과는 다른 의미예요. 세법에서 정한 '공정한 시장 가격'을 뜻하는 전문용어랍니다. 세금을 부당하게 줄이는 걸 막기 위해서죠." },
        { from: 'me', time: '오후 2:04', text: '아하! 그럼 그 공정한 시장 가격, 즉 \'정상가격\'은 어떻게 찾나요? 기준이 있나요?' },
        { from: 'you', name: '권팀장', time: '오후 2:05', text: "물론이죠. 여러 방법이 있는데, 동네 빵집 이야기로 설명해 줄게요.<br><br><b>① 옆집 빵값 보기(CUP):</b> 우리와 똑같은 빵을 옆집에서 얼마에 파는지 보는 것.<br><b>② 편의점 마진 보기(RPM):</b> 편의점이 빵을 얼마에 떼와서 파는지 보고 우리 마진을 역산하는 것.<br><b>③ 원가에 이익 붙이기(C+):</b> 빵 원가에 다른 빵집처럼 적정 이익을 붙이는 것." },
        { from: 'you', name: '권팀장', time: '오후 2:06', text: "하지만 현실에서 가장 많이 쓰는 방법은 우리 회사가 쓰는 <b class='font-bold'>'거래순이익률법(TNMM)'</b>이에요.<br><br>\"우리 빵집과 사업 내용이 비슷한 <b class='font-bold'>'동종업계 다른 빵집'</b>들은 최종적으로 몇 %의 이익을 남길까?\"를 보는 거죠. 가장 현실적인 방법이라 제일 많이 쓰여요." },
        { from: 'me', time: '오후 2:07', text: '동종업계 다른 빵집이요? 그럼 그 빵집들의 이익률하고 우리 이익률을 비교하는 거군요!' },
        { from: 'you', name: '권팀장', time: '오후 2:08', text: "정확해요. 그 유사한 빵집들을 <b class='font-bold'>'비교가능기업'</b>이라고 불러요. 그 회사들의 이익률 성적표에서 너무 잘하거나 못한 곳을 빼고, 남은 중간 그룹의 이익률 구간을 <b class='font-bold text-blue-600'>'사분위 범위'</b>, 즉 <b class='font-bold'>'안전지대'</b>라고 해요. 우리 회사 이익률이 그 안에만 있으면 국세청도 OK하는 거죠." },
        { from: 'me', time: '오후 2:09', text: '그럼 그 안전지대는 매년 똑같나요?' },
        { from: 'you', name: '권팀장', time: '오후 2:10', text: "아니요, 아주 중요한 포인트예요. <b class='font-bold'>안전지대는 매년 바뀝니다.</b> 경기가 좋으면 유사 기업들 이익률이 다 같이 올라가서 안전지대도 올라가고, 경기가 나쁘면 같이 내려가죠. 그래서 매년 체크해야 해요." },
        { from: 'me', time: '오후 2:11', text: '팀장님, 그럼 저희 사업부 중에 Offer 판매만 하는 곳이 있는데, 이건 좀 다르지 않나요? 저희는 물건을 사 오는 게 아니라 중개만 하는데요.' },
        { from: 'you', name: '권팀장', time: '오후 2:12', text: "정말 중요한 질문이네요. 여기서 큰 세금 문제가 생길 수 있어요. 빵집 비유를 계속해 볼게요.<br><br>단순히 손님과 빵집을 <b class='font-bold'>연결해주고 수수료만 받으면 그게 Offer 판매</b>예요. 하지만, 서류상 이름만 Offer고 <b class='font-bold text-red-600'>실제로는 우리가 빵을 다 사서 재고 위험을 지고 직접 파는 것이라면(Buy-Sell)</b> 이야기가 완전히 달라져요." },
        { from: 'you', name: '권팀장', time: '오후 2:13', text: "국세청은 \"너희는 단순 중개인이 아니네? 빵 전체(취급고)를 책임지고 파는 사업자네? 그럼 수수료가 아니라 <b class='font-bold'>빵 판매액 전체를 기준으로 이익률을 계산해야지!</b>\" 라고 할 수 있어요. 그럼 세금 계산의 기준이 되는 매출액이 엄청나게 커져서 세금 폭탄을 맞을 수 있죠. 그래서 기능과 리스크의 '실질'이 중요해요." },
        { from: 'you', name: '권팀장', time: '오후 2:13', text: "직전 세무조사에서 저희 오퍼 판매가 이슈됐던 것도 바로 그 '실질' 때문이었어요. 가장 큰 이유는 서류상으론 Offer 판매지만, <b class='font-bold'>실제 업무 내용이 Buy-Sell에 가깝다고 보였기 때문이에요.</b> 저희가 고객 상담부터 가격 협상까지 깊숙이 관여했거든요. 그 외에도 <b class='font-bold'>①가격 협상 시 우리 마진이 할인액만큼 줄어든다는 점, ②매출 대비 사업부 인원이 많아 단순 중개 이상의 역할을 한다고 보인 점</b> 등이 종합적으로 고려되어 '이건 단순 중개가 아니라 실질적인 판매 활동이다'라고 본 거죠." },
        { from: 'me', time: '오후 2:14', text: '와... 정말 아찔하네요. 그럼 APA라는 제도는 뭔가요? 그런 위험을 막아주나요?' },
        { from: 'you', name: '권팀장', time: '오후 2:15', text: "APA는 <b class='font-bold'>'5년짜리 세금 안전 보장 각서'</b> 같은 거예요. 국세청과 미리 만나서 \"앞으로 5년간은 이 정도 이익률로 하겠습니다!\"라고 사전에 약속하고 세무조사 위험을 없애는 제도죠." },
        { from: 'me', time: '오후 2:16', text: '그렇게 좋은 걸 왜 저희는 안 하나요? 본사에서 반대한다는 이야기도 들은 것 같아요.' },
        { from: 'you', name: '권팀장', time: '오후 2:17', text: "본사에서는 <b class='font-bold'>신중한 입장</b>을 보이고 있어요. 몇 가지 <b class='font-bold'>우려하는 점</b>이 있기 때문이죠.<br><br>첫째, APA를 하려면 일본 본사의 원가 같은 <b class='font-bold'>그룹 전체의 민감한 정보가 공개되는 점을 우려</b>하고 있습니다.<br><br>둘째, 한국에서의 약속이 다른 국가에 영향을 미치는 <b class='font-bold'>선례가 될 수 있다는 점을 신중하게 고려</b>하고 있고요." },
        { from: 'me', time: '오후 2:18', text: '와... 정말 복잡하네요. 팀장님 덕분에 내일 워크숍 때 큰 그림을 그리고 들을 수 있을 것 같습니다! 정말 감사합니다!' },
        { from: 'you', name: '권팀장', time: '오후 2:19', text: '천만에요, 김 대리님. 이 정도만 알아도 내일 강의 듣는 데 큰 도움이 될 거예요. 궁금한 거 있으면 언제든 물어보세요!' }
    ];

    let currentMessageIndex = 0;

    function showNextMessage() {
        if (currentMessageIndex >= messages.length) {
            return;
        }

        const msg = messages[currentMessageIndex];
        const messageGroup = document.createElement('div');

        if (msg.from === 'me') {
            messageGroup.className = 'message-group me';
            messageGroup.innerHTML = `
                <div class="flex items-end justify-end">
                    <span class="timestamp mr-2">${msg.time}</span>
                    <div class="chat-bubble me p-3 rounded-lg">
                        <p class="text-sm">${msg.text}</p>
                    </div>
                </div>
            `;
        } else {
            messageGroup.className = 'message-group you';
            messageGroup.innerHTML = `
                <div class="flex flex-col items-start">
                    <p class="text-xs font-medium text-gray-800 ml-1 mb-1">${msg.name}</p>
                    <div class="flex items-end">
                        <div class="chat-bubble you p-3 rounded-lg shadow">
                            <p class="text-sm">${msg.text}</p>
                        </div>
                        <span class="timestamp ml-2">${msg.time}</span>
                    </div>
                </div>
            `;
        }

        chatBody.appendChild(messageGroup);

        // Animate the new message
        setTimeout(() => {
            messageGroup.classList.add('visible');
            chatBody.scrollTop = chatBody.scrollHeight;
        }, 10);

        currentMessageIndex++;

        if (currentMessageIndex >= messages.length) {
            nextButton.textContent = '모든 대화 보기 완료';
            nextButton.disabled = true;
            nextButton.classList.remove('hover:bg-yellow-500');
            nextButton.classList.add('bg-gray-400', 'cursor-not-allowed');
        }
    }

    // Event listener for the start button
    startButton.addEventListener('click', () => {
        mainScreen.style.display = 'none';
        chatContainer.style.display = 'flex';
        showNextMessage(); // Show the first message automatically
    });

    nextButton.addEventListener('click', showNextMessage);
</script>
</body>
</html>
