<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dancing Banana</title>
    <style>
        /* 1. 기본 설정 및 중앙 정렬 */
        body {
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #f0f8ff; /* 부드러운 배경색 */
            overflow: hidden;
        }

        /* 2. 바나나 컨테이너 */
        .banana-container {
            position: relative;
            width: 100px;
            height: 200px;
            cursor: pointer;
            /* 클릭 시 깜빡임 방지 */
            -webkit-tap-highlight-color: transparent; 
        }

        /* 3. 바나나 알맹이 (춤추는 부분) */
        .banana-body {
            position: absolute;
            width: 60px;
            height: 180px;
            background-color: #fdfdbd; /* 연한 바나나 속살색 */
            border-radius: 30px;
            left: 50%;
            top: 10px;
            transform: translateX(-50%);
            z-index: 1; /* 껍질보다 뒤에 위치 */
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            box-shadow: inset 0 0 10px rgba(0,0,0,0.1);
        }

        /* 춤추는 바나나 얼굴 (초기엔 숨김) */
        .face {
            opacity: 0;
            transition: opacity 0.5s ease;
            font-size: 24px;
        }

        /* 4. 바나나 껍질 디자인 */
        .peel {
            position: absolute;
            width: 60px;
            height: 190px;
            background-color: #ffe135; /* 진한 노란색 껍질 */
            border-radius: 30px 30px 50px 50px;
            top: 5px;
            left: 50%;
            /* 회전 중심점을 하단 중앙으로 설정하여 아래로 벗겨지게 함 */
            transform-origin: bottom center;
            transition: transform 1s cubic-bezier(0.68, -0.55, 0.27, 1.55), background-color 1s ease;
            z-index: 2;
            border: 3px solid #e6c300;
            box-sizing: border-box;
        }

        /* 왼쪽 껍질 */
        .peel.left {
            transform: translateX(-50%) rotate(0deg);
        }
        
        /* 오른쪽 껍질 (약간 겹치게 표현) */
        .peel.right {
            transform: translateX(-50%) rotate(0deg);
            opacity: 0.9;
        }

        /* 바나나 꼭지 */
        .stem {
            position: absolute;
            top: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 20px;
            height: 20px;
            background-color: #6d4c41;
            border-radius: 5px;
            z-index: 3;
        }


        /* --- 애니메이션 상태 --- */

        /* 상태 1: 껍질이 벗겨진 상태 (.peeled 클래스 추가 시) */
        .banana-container.peeled .peel.left {
            transform: translateX(-120%) rotate(-70deg);
            background-color: #e6c300; /* 안쪽 껍질 색 */
        }

        .banana-container.peeled .peel.right {
            transform: translateX(20%) rotate(70deg);
            background-color: #e6c300;
        }
        
        .banana-container.peeled .stem {
            display: none; /* 벗겨지면 꼭지 숨김 */
        }


        /* 상태 2: 춤추는 상태 (.dancing 클래스 추가 시) */
        .banana-container.dancing .banana-body {
            animation: dance-wiggle 0.8s infinite ease-in-out alternate;
        }
        
        .banana-container.dancing .face {
            opacity: 1; /* 얼굴 등장 */
        }

        /* 춤추는 키프레임 애니메이션 */
        @keyframes dance-wiggle {
            0% { transform: translateX(-50%) rotate(0deg) translateY(0); }
            25% { transform: translateX(-50%) rotate(-10deg) translateY(-10px); }
            50% { transform: translateX(-50%) rotate(5deg) translateY(-5px) scale(1.05); }
            75% { transform: translateX(-50%) rotate(-5deg) translateY(-15px); }
            100% { transform: translateX(-50%) rotate(10deg) translateY(0); }
        }

    </style>
</head>
<body>

    <div class="banana-container" id="banana">
        <div class="stem"></div>
        <div class="banana-body">
            <div class="face">(>‿<)</div>
        </div>
        <div class="peel left"></div>
        <div class="peel right"></div>
    </div>

    <script>
        const bananaContainer = document.getElementById('banana');
        let currentState = 'idle'; // 현재 상태: idle(대기), peeling(벗겨지는 중), dancing(춤추는 중)

        bananaContainer.addEventListener('click', () => {
            // 대기 상태일 때만 클릭 동작 수행
            if (currentState === 'idle') {
                currentState = 'peeling';
                
                // 1. 껍질 벗기기 시작
                bananaContainer.classList.add('peeled');

                // 2. 껍질 애니메이션 시간(1초) 후에 춤추기 시작
                setTimeout(() => {
                    bananaContainer.classList.add('dancing');
                    currentState = 'dancing';
                }, 1000); // CSS transition 시간과 동일하게 맞춤
            } 
            // 춤추는 도중에 클릭하면 리셋 (선택 사항)
            else if (currentState === 'dancing') {
                 bananaContainer.classList.remove('peeled', 'dancing');
                 currentState = 'idle';
            }
        });
    </script>

</body>
</html>
