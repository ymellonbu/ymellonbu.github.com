<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RPG 어드벤처 게임</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            color: #fff;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .auth-form {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 40px;
            margin: 50px auto;
            max-width: 400px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
        }
        
        .game-container {
            display: none;
            background: rgba(0, 0, 0, 0.8);
            border-radius: 20px;
            padding: 30px;
            margin-top: 20px;
            border: 2px solid #ffd700;
            box-shadow: 0 0 30px rgba(255, 215, 0, 0.3);
        }
        
        h1, h2 {
            text-align: center;
            margin-bottom: 30px;
            color: #ffd700;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .input-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            color: #fff;
            font-weight: bold;
        }
        
        input[type="text"], input[type="password"], select {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.9);
            color: #333;
            font-size: 16px;
            transition: all 0.3s ease;
        }
        
        input:focus, select:focus {
            outline: none;
            background: rgba(255, 255, 255, 1);
            transform: scale(1.02);
        }
        
        .btn {
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 10px 0;
        }
        
        .btn-primary {
            background: linear-gradient(45deg, #ffd700, #ffed4a);
            color: #333;
        }
        
        .btn-secondary {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: #fff;
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        .character-stats {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin: 20px 0;
        }
        
        .stat-box {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            border: 1px solid rgba(255, 215, 0, 0.3);
        }
        
        .stat-value {
            font-size: 24px;
            font-weight: bold;
            color: #ffd700;
            margin-top: 5px;
        }
        
        .game-actions {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 20px;
        }
        
        .battle-log {
            background: rgba(0, 0, 0, 0.5);
            border-radius: 10px;
            padding: 15px;
            margin: 20px 0;
            max-height: 200px;
            overflow-y: auto;
            border: 1px solid #ffd700;
        }
        
        .log-entry {
            margin: 5px 0;
            padding: 5px;
            border-left: 3px solid #ffd700;
            padding-left: 10px;
        }
        
        .error-message {
            color: #ff6b6b;
            text-align: center;
            margin: 10px 0;
            padding: 10px;
            background: rgba(255, 107, 107, 0.1);
            border-radius: 5px;
            border: 1px solid rgba(255, 107, 107, 0.3);
        }
        
        .user-info {
            text-align: center;
            margin-bottom: 20px;
            padding: 15px;
            background: rgba(255, 215, 0, 0.1);
            border-radius: 10px;
            border: 1px solid rgba(255, 215, 0, 0.3);
        }
        
        .progress-bar {
            width: 100%;
            height: 20px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            overflow: hidden;
            margin: 10px 0;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #ff6b6b, #ffd700);
            transition: width 0.3s ease;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 회원가입 폼 -->
        <div id="signupForm" class="auth-form">
            <h2>🗡️ RPG 어드벤처 회원가입</h2>
            <div class="input-group">
                <label for="signupUsername">사용자명</label>
                <input type="text" id="signupUsername" placeholder="사용자명을 입력하세요">
            </div>
            <div class="input-group">
                <label for="signupPassword">비밀번호</label>
                <input type="password" id="signupPassword" placeholder="비밀번호를 입력하세요">
            </div>
            <div class="input-group">
                <label for="characterClass">캐릭터 클래스</label>
                <select id="characterClass">
                    <option value="warrior">전사 (높은 체력, 강한 공격력)</option>
                    <option value="mage">마법사 (마나 사용, 마법 공격)</option>
                    <option value="rogue">도적 (빠른 속도, 치명타)</option>
                </select>
            </div>
            <button class="btn btn-primary" onclick="signup()">회원가입</button>
            <button class="btn btn-secondary" onclick="showLogin()">로그인하기</button>
            <div id="signupError" class="error-message" style="display: none;"></div>
        </div>

        <!-- 로그인 폼 -->
        <div id="loginForm" class="auth-form" style="display: none;">
            <h2>🏰 RPG 어드벤처 로그인</h2>
            <div class="input-group">
                <label for="loginUsername">사용자명</label>
                <input type="text" id="loginUsername" placeholder="사용자명을 입력하세요">
            </div>
            <div class="input-group">
                <label for="loginPassword">비밀번호</label>
                <input type="password" id="loginPassword" placeholder="비밀번호를 입력하세요">
            </div>
            <button class="btn btn-primary" onclick="login()">로그인</button>
            <button class="btn btn-secondary" onclick="showSignup()">회원가입하기</button>
            <div id="loginError" class="error-message" style="display: none;"></div>
        </div>

        <!-- 게임 화면 -->
        <div id="gameContainer" class="game-container">
            <div class="user-info">
                <h1>🎮 RPG 어드벤처</h1>
                <p id="welcomeMessage"></p>
                <button class="btn btn-secondary" onclick="logout()" style="width: auto; padding: 8px 20px; margin-top: 10px;">로그아웃</button>
            </div>

            <div class="character-stats">
                <div class="stat-box">
                    <div>체력 (HP)</div>
                    <div class="stat-value" id="currentHP">100</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="hpBar" style="width: 100%;"></div>
                    </div>
                </div>
                <div class="stat-box">
                    <div>마나 (MP)</div>
                    <div class="stat-value" id="currentMP">50</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="mpBar" style="width: 100%; background: linear-gradient(90deg, #4ecdc4, #44a08d);"></div>
                    </div>
                </div>
                <div class="stat-box">
                    <div>레벨</div>
                    <div class="stat-value" id="level">1</div>
                </div>
                <div class="stat-box">
                    <div>경험치</div>
                    <div class="stat-value" id="experience">0</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="expBar" style="width: 0%; background: linear-gradient(90deg, #ffd700, #ffed4a);"></div>
                    </div>
                </div>
            </div>

            <div class="battle-log" id="battleLog">
                <div class="log-entry">🎮 게임에 오신 것을 환영합니다!</div>
            </div>

            <div class="game-actions">
                <button class="btn btn-primary" onclick="hunt()">🗡️ 사냥하기</button>
                <button class="btn btn-primary" onclick="rest()">💤 휴식하기</button>
                <button class="btn btn-primary" onclick="useSkill()">✨ 스킬 사용</button>
                <button class="btn btn-primary" onclick="showStats()">📊 상세 정보</button>
            </div>
        </div>
    </div>

    <script>
        let users = {}; // 사용자 데이터 저장
        let currentUser = null; // 현재 로그인된 사용자

        // 캐릭터 클래스별 기본 스탯
        const classStats = {
            warrior: { hp: 120, mp: 30, attack: 20, defense: 15, skill: '강력한 일격' },
            mage: { hp: 80, mp: 100, attack: 25, defense: 8, skill: '파이어볼' },
            rogue: { hp: 100, mp: 50, attack: 18, defense: 12, skill: '그림자 공격' }
        };

        const classNames = {
            warrior: '전사',
            mage: '마법사',
            rogue: '도적'
        };

        function showSignup() {
            document.getElementById('signupForm').style.display = 'block';
            document.getElementById('loginForm').style.display = 'none';
        }

        function showLogin() {
            document.getElementById('loginForm').style.display = 'block';
            document.getElementById('signupForm').style.display = 'none';
        }

        function signup() {
            const username = document.getElementById('signupUsername').value.trim();
            const password = document.getElementById('signupPassword').value.trim();
            const characterClass = document.getElementById('characterClass').value;

            if (!username || !password) {
                showError('signupError', '사용자명과 비밀번호를 모두 입력해주세요.');
                return;
            }

            if (users[username]) {
                showError('signupError', '이미 존재하는 사용자명입니다.');
                return;
            }

            // 새 사용자 생성
            const baseStats = classStats[characterClass];
            users[username] = {
                password: password,
                characterClass: characterClass,
                level: 1,
                experience: 0,
                maxHP: baseStats.hp,
                currentHP: baseStats.hp,
                maxMP: baseStats.mp,
                currentMP: baseStats.mp,
                attack: baseStats.attack,
                defense: baseStats.defense,
                skill: baseStats.skill
            };

            alert('회원가입이 완료되었습니다! 로그인해주세요.');
            showLogin();
            document.getElementById('loginUsername').value = username;
        }

        function login() {
            const username = document.getElementById('loginUsername').value.trim();
            const password = document.getElementById('loginPassword').value.trim();

            if (!username || !password) {
                showError('loginError', '사용자명과 비밀번호를 모두 입력해주세요.');
                return;
            }

            if (!users[username] || users[username].password !== password) {
                showError('loginError', '잘못된 사용자명 또는 비밀번호입니다.');
                return;
            }

            currentUser = username;
            startGame();
        }

        function logout() {
            currentUser = null;
            document.getElementById('gameContainer').style.display = 'none';
            showLogin();
            document.getElementById('loginUsername').value = '';
            document.getElementById('loginPassword').value = '';
        }

        function startGame() {
            document.getElementById('signupForm').style.display = 'none';
            document.getElementById('loginForm').style.display = 'none';
            document.getElementById('gameContainer').style.display = 'block';

            const user = users[currentUser];
            const className = classNames[user.characterClass];
            
            document.getElementById('welcomeMessage').textContent = 
                `환영합니다, ${currentUser}님! (${className} - 레벨 ${user.level})`;
            
            updateGameUI();
            addBattleLog(`${currentUser}님이 게임에 접속했습니다! (${className})`);
        }

        function updateGameUI() {
            const user = users[currentUser];
            document.getElementById('currentHP').textContent = user.currentHP;
            document.getElementById('currentMP').textContent = user.currentMP;
            document.getElementById('level').textContent = user.level;
            document.getElementById('experience').textContent = user.experience;

            // 진행바 업데이트
            const hpPercent = (user.currentHP / user.maxHP) * 100;
            const mpPercent = (user.currentMP / user.maxMP) * 100;
            const expPercent = (user.experience % 100) / 100 * 100;

            document.getElementById('hpBar').style.width = hpPercent + '%';
            document.getElementById('mpBar').style.width = mpPercent + '%';
            document.getElementById('expBar').style.width = expPercent + '%';
        }

        function hunt() {
            const user = users[currentUser];
            
            if (user.currentHP <= 10) {
                addBattleLog('체력이 너무 낮습니다! 휴식을 취하세요.');
                return;
            }

            const monsters = ['고블린', '오크', '스켈레톤', '늑대', '박쥐'];
            const monster = monsters[Math.floor(Math.random() * monsters.length)];
            
            const damage = Math.floor(Math.random() * 15) + 5;
            const expGain = Math.floor(Math.random() * 20) + 10;
            
            user.currentHP = Math.max(0, user.currentHP - damage);
            user.experience += expGain;
            
            addBattleLog(`⚔️ ${monster}와 전투! ${damage}의 피해를 받았지만 ${expGain} 경험치를 획득했습니다.`);
            
            // 레벨업 체크
            if (user.experience >= user.level * 100) {
                levelUp();
            }
            
            updateGameUI();
        }

        function rest() {
            const user = users[currentUser];
            
            const hpRecover = Math.floor(user.maxHP * 0.3);
            const mpRecover = Math.floor(user.maxMP * 0.5);
            
            user.currentHP = Math.min(user.maxHP, user.currentHP + hpRecover);
            user.currentMP = Math.min(user.maxMP, user.currentMP + mpRecover);
            
            addBattleLog(`💤 휴식을 취했습니다. HP ${hpRecover}, MP ${mpRecover} 회복!`);
            updateGameUI();
        }

        function useSkill() {
            const user = users[currentUser];
            
            if (user.currentMP < 20) {
                addBattleLog('마나가 부족합니다! 휴식을 취하거나 마나 포션을 사용하세요.');
                return;
            }
            
            user.currentMP -= 20;
            const damage = user.attack * 2;
            const expGain = 15;
            user.experience += expGain;
            
            addBattleLog(`✨ ${user.skill}을(를) 사용했습니다! ${damage}의 강력한 피해를 입히고 ${expGain} 경험치를 획득했습니다.`);
            
            if (user.experience >= user.level * 100) {
                levelUp();
            }
            
            updateGameUI();
        }

        function levelUp() {
            const user = users[currentUser];
            user.level++;
            user.experience = user.experience - (user.level - 1) * 100;
            
            // 스탯 증가
            const hpIncrease = Math.floor(Math.random() * 20) + 10;
            const mpIncrease = Math.floor(Math.random() * 15) + 5;
            const attackIncrease = Math.floor(Math.random() * 5) + 2;
            
            user.maxHP += hpIncrease;
            user.maxMP += mpIncrease;
            user.attack += attackIncrease;
            user.currentHP = user.maxHP; // 레벨업 시 체력 완전 회복
            user.currentMP = user.maxMP; // 레벨업 시 마나 완전 회복
            
            addBattleLog(`🎉 레벨업! 레벨 ${user.level}이 되었습니다! HP+${hpIncrease}, MP+${mpIncrease}, 공격력+${attackIncrease}`);
        }

        function showStats() {
            const user = users[currentUser];
            const className = classNames[user.characterClass];
            
            addBattleLog(`📊 === ${currentUser}님의 상세 정보 ===`);
            addBattleLog(`클래스: ${className} | 레벨: ${user.level}`);
            addBattleLog(`체력: ${user.currentHP}/${user.maxHP} | 마나: ${user.currentMP}/${user.maxMP}`);
            addBattleLog(`공격력: ${user.attack} | 방어력: ${user.defense}`);
            addBattleLog(`경험치: ${user.experience}/${user.level * 100} | 스킬: ${user.skill}`);
        }

        function addBattleLog(message) {
            const battleLog = document.getElementById('battleLog');
            const logEntry = document.createElement('div');
            logEntry.className = 'log-entry';
            logEntry.textContent = message;
            
            battleLog.appendChild(logEntry);
            battleLog.scrollTop = battleLog.scrollHeight;
            
            // 로그가 너무 많아지면 오래된 것들 제거
            const logEntries = battleLog.getElementsByClassName('log-entry');
            if (logEntries.length > 10) {
                battleLog.removeChild(logEntries[0]);
            }
        }

        function showError(elementId, message) {
            const errorElement = document.getElementById(elementId);
            errorElement.textContent = message;
            errorElement.style.display = 'block';
            
            setTimeout(() => {
                errorElement.style.display = 'none';
            }, 3000);
        }

        // 엔터키 이벤트 처리
        document.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                if (document.getElementById('signupForm').style.display !== 'none') {
                    signup();
                } else if (document.getElementById('loginForm').style.display !== 'none') {
                    login();
                }
            }
        });
    </script>
</body>
</html>
