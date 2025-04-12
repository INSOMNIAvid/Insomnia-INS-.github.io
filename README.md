
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Insomnia Clicker</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            background: #1a1a2e;
            margin: 0;
            padding: 0;
            color: #fff;
            height: 100vh;
            display: flex;
            flex-direction: column;
            transition: background 0.5s;
        }
        .header {
            padding: 20px;
            background: rgba(22, 33, 62, 0.8);
        }
        .main-content {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        #click-area {
            width: 200px;
            height: 200px;
            background: linear-gradient(45deg, #6e45e2, #88d3ce);
            margin: 20px auto;
            cursor: pointer;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
            color: white;
            user-select: none;
            transition: transform 0.1s, background 0.5s;
            box-shadow: 0 0 20px rgba(110, 69, 226, 0.6);
            position: relative;
        }
        .progress-container {
            width: 200px;
            margin: 0 auto 20px;
            background: #0f3460;
            border-radius: 10px;
            padding: 3px;
        }
        .progress-bar {
            height: 20px;
            background: linear-gradient(90deg, #6e45e2, #88d3ce);
            border-radius: 8px;
            width: 100%;
            transition: width 0.3s;
        }
        #clicks-left {
            margin-top: 5px;
            font-weight: bold;
            font-size: 14px;
        }
        .bottom-nav {
            display: flex;
            background: #0f3460;
            position: sticky;
            bottom: 0;
        }
        .nav-tab {
            flex: 1;
            padding: 15px;
            cursor: pointer;
            transition: background 0.3s;
        }
        .nav-tab.active {
            background: #6e45e2;
        }
        .tab-content {
            display: none;
            padding: 15px;
            width: 100%;
        }
        .tab-content.active {
            display: block;
        }
        button {
            background: #6e45e2;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            margin: 5px;
            transition: background 0.3s;
        }
        #premium-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 100;
            justify-content: center;
            align-items: center;
        }
        .premium-options {
            background: #16213e;
            padding: 20px;
            border-radius: 10px;
            width: 90%;
            max-width: 400px;
            text-align: center;
        }
        .color-option {
            width: 40px;
            height: 40px;
            display: inline-block;
            margin: 5px;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid transparent;
        }
        .color-option.selected {
            border-color: white;
        }
        .premium-feature {
            margin: 10px 0;
            text-align: left;
        }
        .premium-badge {
            background: gold;
            color: black;
            padding: 3px 8px;
            border-radius: 10px;
            font-size: 12px;
            margin-left: 5px;
            font-weight: bold;
        }
        .wallet-option {
            background: #0f3460;
            padding: 15px;
            margin: 10px 0;
            border-radius: 5px;
            cursor: pointer;
        }
        #time-left {
            margin-top: 10px;
            font-size: 14px;
            color: gold;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Insomnia Clicker <span id="premium-badge" class="premium-badge" style="display:none;">PREMIUM</span></h1>
        <p>Баланс: <span id="balance">0</span> $INSOMNIA</p>
        <div id="time-left" style="display:none;"></div>
    </div>

    <div class="main-content">
        <!-- Кликер -->
        <div id="click-tab" class="tab-content active">
            <div class="progress-container">
                <div id="progress-bar" class="progress-bar"></div>
            </div>
            <div id="clicks-left">Осталось кликов: 200/200</div>
            <div id="click-area">INSOMNIA</div>
            <button id="buy-premium">Купить PREMIUM</button>
        </div>

        <!-- Друзья -->
        <div id="friends-tab" class="tab-content">
            <h3>Реферальная система</h3>
            <p id="ref-bonus-text">Получи +100 $INSOMNIA за каждого друга!</p>
            <div id="ref-link" style="background:#0d7377;padding:10px;border-radius:5px;cursor:pointer;">
                Скопировать реферальную ссылку
            </div>
            <div id="ref-stats" style="margin-top:10px;">
                Приглашено: <span id="ref-count">0</span> друзей | Получено: <span id="ref-earned">0</span> $INSOMNIA
            </div>
        </div>

        <!-- Кошелёк -->
        <div id="wallet-tab" class="tab-content">
            <h3>Подключение кошелька</h3>
            <div class="wallet-option" id="metamask-connect">
                <img src="https://upload.wikimedia.org/wikipedia/commons/3/36/MetaMask_Fox.svg" width="30" style="vertical-align:middle;">
                MetaMask
            </div>
            <div class="wallet-option" id="walletconnect-connect">
                <img src="https://walletconnect.com/walletconnect-logo.png" width="30" style="vertical-align:middle;">
                WalletConnect
            </div>
            <div class="wallet-option" id="phantom-connect">
                <img src="https://phantom.app/favicon.ico" width="30" style="vertical-align:middle;">
                Phantom (Solana)
            </div>
            <div id="wallet-info" style="display:none; margin-top:20px;">
                <p>Подключен: <span id="wallet-address">0x...0000</span></p>
                <p>Сеть: <span id="wallet-network">-</span></p>
            </div>
        </div>
    </div>

    <!-- Нижнее меню -->
    <div class="bottom-nav">
        <div class="nav-tab active" data-tab="click-tab">Кликер</div>
        <div class="nav-tab" data-tab="friends-tab">Друзья</div>
        <div class="nav-tab" data-tab="wallet-tab">Кошелёк</div>
    </div>

    <!-- Модальное окно премиума -->
    <div id="premium-modal">
        <div class="premium-options">
            <h2>PREMIUM подписка</h2>
            <div id="premium-time-info" style="margin-bottom:15px;"></div>
            
            <div class="premium-feature">✅ 2X монет за клики</div>
            <div class="premium-feature">✅ 2X монет за рефералов</div>
            <div class="premium-feature">✅ Безлимитные клики</div>
            <div class="premium-feature">🎨 Выбор цвета кнопки и фона</div>
            
            <h3>Выберите цвет кнопки:</h3>
            <div>
                <div class="color-option selected" style="background: linear-gradient(45deg, #6e45e2, #88d3ce);" data-color1="#6e45e2" data-color2="#88d3ce"></div>
                <div class="color-option" style="background: linear-gradient(45deg, #FF416C, #FF4B2B);" data-color1="#FF416C" data-color2="#FF4B2B"></div>
                <div class="color-option" style="background: linear-gradient(45deg, #3a7bd5, #00d2ff);" data-color1="#3a7bd5" data-color2="#00d2ff"></div>
                <div class="color-option" style="background: linear-gradient(45deg, #11998e, #38ef7d);" data-color1="#11998e" data-color2="#38ef7d"></div>
                <div class="color-option" style="background: linear-gradient(45deg, #fc4a1a, #f7b733);" data-color1="#fc4a1a" data-color2="#f7b733"></div>
            </div>
            
            <h3>Выберите цвет фона:</h3>
            <div>
                <div class="color-option selected" style="background: #1a1a2e;" data-bg="#1a1a2e"></div>
                <div class="color-option" style="background: #0f2027;" data-bg="#0f2027"></div>
                <div class="color-option" style="background: #24243e;" data-bg="#24243e"></div>
                <div class="color-option" style="background: #1e3c72;" data-bg="#1e3c72"></div>
                <div class="color-option" style="background: #2c3e50;" data-bg="#2c3e50"></div>
            </div>
            
            <button id="confirm-premium" style="margin-top:20px;background:gold;color:black;font-weight:bold;">
                Купить за $4.99
            </button>
            <button id="close-premium-modal" style="margin-top:10px;">
                Закрыть
            </button>
        </div>
    </div>

    <script>
        // Игровая логика
        let coins = parseInt(localStorage.getItem('insomniaCoins')) || 0;
        let clickPower = 1;
        let refCount = parseInt(localStorage.getItem('refCount')) || 0;
        let refEarned = parseInt(localStorage.getItem('refEarned')) || 0;
        const REF_REWARD = 100;
        let premiumActive = false;
        let premiumEndDate = localStorage.getItem('premiumEndDate');
        let selectedButtonColor1 = "#6e45e2";
        let selectedButtonColor2 = "#88d3ce";
        let selectedBgColor = "#1a1a2e";
        const MAX_CLICKS = 200;
        let clicksLeft = parseInt(localStorage.getItem('clicksLeft')) || MAX_CLICKS;
        let timerInterval;
        let clickRefreshInterval;
        let lastClickTime = localStorage.getItem('lastClickTime') ? new Date(localStorage.getItem('lastClickTime')) : new Date();

        // Проверка премиум-статуса
        function checkPremiumStatus() {
            if (premiumEndDate && new Date() < new Date(premiumEndDate)) {
                activatePremium();
                startTimer();
            }
        }

        // Активация премиума
        function activatePremium() {
            premiumActive = true;
            clickPower = 2;
            document.getElementById('premium-badge').style.display = "inline";
            document.getElementById('ref-bonus-text').textContent = "Получи +200 $INSOMNIA за каждого друга! (PREMIUM 2X)";
            document.getElementById('buy-premium').textContent = "PREMIUM активен";
            document.getElementById('buy-premium').style.background = "gold";
            document.getElementById('buy-premium').style.color = "black";
            document.getElementById('time-left').style.display = "block";
            
            // Восстанавливаем клики при активации премиума
            clicksLeft = Infinity;
            updateClicksCounter();
        }

        // Таймер обратного отсчета
        function startTimer() {
            clearInterval(timerInterval);
            
            timerInterval = setInterval(() => {
                const now = new Date();
                const endDate = new Date(premiumEndDate);
                const diff = endDate - now;
                
                if (diff <= 0) {
                    clearInterval(timerInterval);
                    premiumActive = false;
                    document.getElementById('premium-badge').style.display = "none";
                    document.getElementById('buy-premium').textContent = "Купить PREMIUM";
                    document.getElementById('buy-premium').style.background = "#6e45e2";
                    document.getElementById('buy-premium').style.color = "white";
                    document.getElementById('time-left').style.display = "none";
                    clicksLeft = MAX_CLICKS;
                    updateClicksCounter();
                    return;
                }
                
                const days = Math.floor(diff / (1000 * 60 * 60 * 24));
                const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((diff % (1000 * 60)) / 1000);
                
                document.getElementById('time-left').textContent = 
                    `PREMIUM истекает через: ${days}д ${hours}ч ${minutes}м ${seconds}с`;
                
                updatePremiumTimeInfo();
            }, 1000);
        }

        // Автоматическое восстановление кликов
        function startClickRefresh() {
            clearInterval(clickRefreshInterval);
            
            clickRefreshInterval = setInterval(() => {
                const now = new Date();
                const timeDiff = now - lastClickTime;
                const minutesPassed = timeDiff / (1000 * 60);
                
                if (minutesPassed >= 1 && !premiumActive) {
                    lastClickTime = now;
                    localStorage.setItem('lastClickTime', lastClickTime.toISOString());
                    clicksLeft = MAX_CLICKS;
                    updateClicksCounter();
                }
            }, 1000); // Проверяем каждую секунду
        }

        // Обновление информации о времени в модальном окне
        function updatePremiumTimeInfo() {
            if (!premiumActive) return;
            
            const now = new Date();
            const endDate = new Date(premiumEndDate);
            const diff = endDate - now;
            
            if (diff <= 0) return;
            
            const days = Math.floor(diff / (1000 * 60 * 60 * 24));
            const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
            
            document.getElementById('premium-time-info').innerHTML = 
                `<p style="color:gold;">Ваша подписка активна ещё ${days}д ${hours}ч ${minutes}м</p>`;
        }

        // Обновление счетчика кликов
        function updateClicksCounter() {
            const progressPercent = premiumActive ? 100 : (clicksLeft / MAX_CLICKS) * 100;
            document.getElementById('progress-bar').style.width = `${progressPercent}%`;
            document.getElementById('clicks-left').textContent = premiumActive 
                ? "Безлимитные клики (PREMIUM)" 
                : `Осталось кликов: ${clicksLeft}/${MAX_CLICKS}`;
            localStorage.setItem('clicksLeft', clicksLeft);
        }

        // Инициализация
        function init() {
            // Восстановление кликов при загрузке
            const now = new Date();
            const timeDiff = now - new Date(lastClickTime);
            const minutesPassed = timeDiff / (1000 * 60);
            
            if (minutesPassed >= 1 && !premiumActive) {
                clicksLeft = MAX_CLICKS;
                lastClickTime = now;
                localStorage.setItem('lastClickTime', lastClickTime.toISOString());
            }
            
            document.getElementById('balance').textContent = coins;
            document.getElementById('ref-count').textContent = refCount;
            document.getElementById('ref-earned').textContent = refEarned;
            updateClicksCounter();
            checkPremiumStatus();
            startClickRefresh();
        }

        // Кликер
        document.getElementById('click-area').addEventListener('click', () => {
            if (clicksLeft > 0 || premiumActive) {
                if (!premiumActive) {
                    clicksLeft--;
                    lastClickTime = new Date();
                    localStorage.setItem('lastClickTime', lastClickTime.toISOString());
                    updateClicksCounter();
                }
                coins += clickPower;
                updateBalance();
                animateClick();
                
                if (clicksLeft === 0 && !premiumActive) {
                    alert('Лимит кликов исчерпан! Новые клики станут доступны через 1 минуту после последнего клика или купите PREMIUM для безлимитных кликов.');
                }
            }
        });

        function animateClick() {
            const clickArea = document.getElementById('click-area');
            clickArea.style.transform = 'scale(0.95)';
            setTimeout(() => {
                clickArea.style.transform = 'scale(1)';
            }, 100);
        }

        function updateBalance() {
            document.getElementById('balance').textContent = coins;
            localStorage.setItem('insomniaCoins', coins);
        }

        // Рефералы
        document.getElementById('ref-link').addEventListener('click', () => {
            const refLink = `${window.location.origin}${window.location.pathname}?ref=${generateRefCode()}`;
            navigator.clipboard.writeText(refLink).then(() => {
                alert('Реферальная ссылка скопирована!\nОтправьте её друзьям!');
            });
        });

        function generateRefCode() {
            return Math.random().toString(36).substring(2, 8).toUpperCase();
        }

        function checkReferral() {
            const urlParams = new URLSearchParams(window.location.search);
            if (urlParams.has('ref')) {
                const refCode = urlParams.get('ref');
                if (!localStorage.getItem('refProcessed')) {
                    refCount++;
                    const reward = premiumActive ? REF_REWARD * 2 : REF_REWARD;
                    refEarned += reward;
                    coins += reward;
                    updateBalance();
                    localStorage.setItem('refCount', refCount);
                    localStorage.setItem('refEarned', refEarned);
                    localStorage.setItem('refProcessed', 'true');
                    document.getElementById('ref-count').textContent = refCount;
                    document.getElementById('ref-earned').textContent = refEarned;
                    alert(`Вы получили ${reward} $INSOMNIA за приглашение друга!`);
                }
            }
        }

        // Премиум подписка
        document.getElementById('buy-premium').addEventListener('click', () => {
            document.getElementById('premium-modal').style.display = 'flex';
            updatePremiumTimeInfo();
        });

        document.getElementById('close-premium-modal').addEventListener('click', () => {
            document.getElementById('premium-modal').style.display = 'none';
        });

        // Выбор цвета (только для премиум)
        document.querySelectorAll('.color-option').forEach(option => {
            option.addEventListener('click', function() {
                if (!premiumActive) {
                    alert('Эта функция доступна только для PREMIUM пользователей!');
                    return;
                }
                
                document.querySelectorAll('.color-option').forEach(opt => opt.classList.remove('selected'));
                this.classList.add('selected');
                
                if (this.dataset.color1) {
                    selectedButtonColor1 = this.dataset.color1;
                    selectedButtonColor2 = this.dataset.color2;
                    document.getElementById('click-area').style.background = 
                        `linear-gradient(45deg, ${selectedButtonColor1}, ${selectedButtonColor2})`;
                    localStorage.setItem('buttonColor1', selectedButtonColor1);
                    localStorage.setItem('buttonColor2', selectedButtonColor2);
                } else if (this.dataset.bg) {
                    selectedBgColor = this.dataset.bg;
                    document.body.style.backgroundColor = selectedBgColor;
                    localStorage.setItem('bgColor', selectedBgColor);
                }
            });
        });

        // Восстановление сохраненных цветов для премиум
        function restoreColors() {
            if (premiumActive) {
                const savedColor1 = localStorage.getItem('buttonColor1');
                const savedColor2 = localStorage.getItem('buttonColor2');
                const savedBg = localStorage.getItem('bgColor');
                
                if (savedColor1 && savedColor2) {
                    selectedButtonColor1 = savedColor1;
                    selectedButtonColor2 = savedColor2;
                    document.getElementById('click-area').style.background = 
                        `linear-gradient(45deg, ${selectedButtonColor1}, ${selectedButtonColor2})`;
                }
                
                if (savedBg) {
                    selectedBgColor = savedBg;
                    document.body.style.backgroundColor = selectedBgColor;
                }
            }
        }

        // Покупка премиума (имитация)
        document.getElementById('confirm-premium').addEventListener('click', () => {
            // В реальном приложении здесь будет интеграция с платежной системой
            const endDate = new Date();
            endDate.setDate(endDate.getDate() + 7);
            premiumEndDate = endDate.toISOString();
            localStorage.setItem('premiumEndDate', premiumEndDate);
            
            activatePremium();
            document.getElementById('premium-modal').style.display = 'none';
            alert('PREMIUM подписка активирована на 7 дней!');
            
            // Восстанавливаем клики
            clicksLeft = Infinity;
            updateClicksCounter();
            restoreColors();
            startTimer();
        });

        // Подключение кошельков
        document.getElementById('metamask-connect').addEventListener('click', async () => {
            if (typeof window.ethereum !== 'undefined') {
                try {
                    const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
                    const address = accounts[0];
                    const chainId = await window.ethereum.request({ method: 'eth_chainId' });
                    
                    let networkName = "Unknown";
                    switch(chainId) {
                        case '0x1': networkName = "Ethereum Mainnet"; break;
                        case '0x89': networkName = "Polygon"; break;
                        case '0x38': networkName = "BNB Chain"; break;
                    }
                    
                    document.getElementById('wallet-address').textContent = 
                        `${address.substring(0, 6)}...${address.substring(address.length - 4)}`;
                    document.getElementById('wallet-network').textContent = networkName;
                    document.getElementById('wallet-info').style.display = 'block';
                    
                    alert(`Кошелёк MetaMask успешно подключен!\nСеть: ${networkName}`);
                } catch (error) {
                    console.error(error);
                    alert('Ошибка подключения MetaMask');
                }
            } else {
                alert('Установите MetaMask!');
                window.open('https://metamask.io/download.html', '_blank');
            }
        });

        // WalletConnect и другие кошельки (заглушки)
        document.getElementById('walletconnect-connect').addEventListener('click', () => {
            alert('WalletConnect integration would go here');
        });

        document.getElementById('phantom-connect').addEventListener('click', () => {
            if (window.solana && window.solana.isPhantom) {
                window.solana.connect()
                    .then(() => {
                        const address = window.solana.publicKey.toString();
                        document.getElementById('wallet-address').textContent = 
                            `${address.substring(0, 6)}...${address.substring(address.length - 4)}`;
                        document.getElementById('wallet-network').textContent = "Solana";
                        document.getElementById('wallet-info').style.display = 'block';
                        alert('Кошелёк Phantom успешно подключен!');
                    })
                    .catch(err => {
                        console.error(err);
                        alert('Ошибка подключения Phantom');
                    });
            } else {
                alert('Установите Phantom Wallet!');
                window.open('https://phantom.app/', '_blank');
            }
        });

        // Переключение табов
        document.querySelectorAll('.nav-tab').forEach(tab => {
            tab.addEventListener('click', () => {
                document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
                document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
                
                tab.classList.add('active');
                document.getElementById(tab.dataset.tab).classList.add('active');
            });
        });

        // Проверка реферала при загрузке
        checkReferral();
        restoreColors();
        init();
    </script>
</body>
</html>
        });
    </script>
</body>
</html>
