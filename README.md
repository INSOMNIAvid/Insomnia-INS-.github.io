<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Insomnia Clicker - интерактивная игра для заработка виртуальной валюты">
    <title>Insomnia Clicker</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #6e45e2;
            --secondary-color: #88d3ce;
            --dark-bg: #1a1a2e;
            --darker-bg: #0f3460;
            --header-bg: rgba(22, 33, 62, 0.8);
            --text-color: #fff;
            --premium-color: gold;
            --success-color: #4CAF50;
            --error-color: #f44336;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Montserrat', 'Arial', sans-serif;
            text-align: center;
            background: var(--dark-bg);
            margin: 0;
            padding: 0;
            color: var(--text-color);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            transition: background 0.5s, color 0.3s;
            line-height: 1.6;
        }

        .header {
            padding: 15px;
            background: var(--header-bg);
            position: relative;
            backdrop-filter: blur(5px);
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header-content {
            flex: 1;
            text-align: center;
        }

        .header h1 {
            font-size: 1.5rem;
            margin-bottom: 5px;
        }

        .settings-btn {
            background: transparent;
            border: none;
            color: white;
            font-size: 20px;
            cursor: pointer;
            transition: transform 0.3s;
            width: 35px;
            height: 35px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            margin-left: auto;
        }

        .settings-btn:hover {
            transform: rotate(30deg);
            background: rgba(255, 255, 255, 0.1);
        }

        .settings-menu {
            display: none;
            position: absolute;
            top: 50px;
            right: 15px;
            background: var(--darker-bg);
            padding: 12px;
            border-radius: 10px;
            z-index: 10;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
            text-align: left;
            min-width: 140px;
        }

        .settings-menu.active {
            display: block;
            animation: fadeIn 0.3s ease-out;
        }

        .settings-menu-item {
            padding: 8px 10px;
            cursor: pointer;
            border-radius: 5px;
            transition: background 0.2s;
            margin: 3px 0;
            display: flex;
            align-items: center;
            font-size: 14px;
        }

        .settings-menu-item:hover {
            background: var(--primary-color);
        }

        .settings-menu-item i {
            margin-right: 8px;
        }

        .language-menu {
            display: none;
            position: absolute;
            top: 0;
            right: 100%;
            background: var(--darker-bg);
            padding: 12px;
            border-radius: 10px;
            z-index: 10;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
            text-align: left;
            min-width: 140px;
        }

        .language-menu.active {
            display: block;
            animation: fadeIn 0.3s ease-out;
        }

        .language-option {
            padding: 8px 10px;
            cursor: pointer;
            border-radius: 5px;
            transition: background 0.2s;
            margin: 3px 0;
            font-size: 14px;
        }

        .language-option:hover {
            background: var(--primary-color);
        }

        .main-content {
            flex: 1;
            padding: 15px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        #click-area {
            width: 180px;
            height: 180px;
            background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
            margin: 15px auto;
            cursor: pointer;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 22px;
            font-weight: bold;
            color: white;
            user-select: none;
            transition: transform 0.1s, background 0.5s, box-shadow 0.3s;
            box-shadow: 0 0 20px rgba(110, 69, 226, 0.6);
            position: relative;
            border: 3px solid rgba(255, 255, 255, 0.1);
        }

        #click-area:active {
            transform: scale(0.95);
        }

        .progress-container {
            width: 100%;
            max-width: 280px;
            margin: 0 auto 15px;
            background: var(--darker-bg);
            border-radius: 10px;
            padding: 3px;
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.2);
        }

        .progress-bar {
            height: 18px;
            background: linear-gradient(90deg, var(--primary-color), var(--secondary-color));
            border-radius: 8px;
            width: 100%;
            transition: width 0.3s, background 0.5s;
            position: relative;
            overflow: hidden;
        }

        .progress-bar::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(
                to right,
                rgba(255, 255, 255, 0.15) 0%,
                rgba(255, 255, 255, 0) 50%,
                rgba(255, 255, 255, 0.15) 100%
            );
            animation: progressShine 2s infinite linear;
        }

        #clicks-left {
            margin-top: 6px;
            font-weight: bold;
            font-size: 13px;
        }

        #regen-info {
            font-size: 12px;
            color: var(--secondary-color);
            margin-top: 5px;
            min-height: 18px;
        }

        .bottom-nav {
            display: flex;
            background: var(--darker-bg);
            position: sticky;
            bottom: 0;
            box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.3);
        }

        .nav-tab {
            flex: 1;
            padding: 12px 8px;
            cursor: pointer;
            transition: background 0.3s, color 0.3s;
            font-weight: 600;
            position: relative;
            font-size: 14px;
        }

        .nav-tab:hover {
            background: rgba(110, 69, 226, 0.3);
        }

        .nav-tab.active {
            background: var(--primary-color);
            color: white;
        }

        .nav-tab.active::after {
            content: '';
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 50%;
            height: 3px;
            background: var(--secondary-color);
            border-radius: 0 0 3px 3px;
        }

        .tab-content {
            display: none;
            padding: 12px;
            width: 100%;
            max-width: 100%;
            animation: fadeIn 0.5s ease-out;
        }

        .tab-content.active {
            display: block;
        }

        button {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 15px;
            margin: 6px;
            transition: all 0.3s;
            font-weight: 600;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15);
        }

        button:active {
            transform: translateY(0);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .premium-btn {
            background: gold;
            color: black;
            font-weight: bold;
        }

        #premium-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 100;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(5px);
            animation: fadeIn 0.3s ease-out;
        }

        .premium-options {
            background: var(--darker-bg);
            padding: 20px;
            border-radius: 15px;
            width: 90%;
            max-width: 100%;
            text-align: center;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
            max-height: 90vh;
            overflow-y: auto;
        }

        .premium-options h2 {
            margin-bottom: 12px;
            color: var(--premium-color);
            font-size: 1.3rem;
        }

        .premium-options h3 {
            margin: 12px 0 8px;
            color: var(--secondary-color);
            font-size: 1.1rem;
        }

        .color-option {
            width: 35px;
            height: 35px;
            display: inline-block;
            margin: 4px;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid transparent;
            transition: transform 0.2s, border-color 0.2s;
        }

        .color-option:hover {
            transform: scale(1.1);
        }

        .color-option.selected {
            border-color: white;
            transform: scale(1.1);
        }

        .premium-feature {
            margin: 10px 0;
            text-align: left;
            padding: 8px 10px;
            background: rgba(110, 69, 226, 0.1);
            border-radius: 8px;
            display: flex;
            align-items: center;
            font-size: 14px;
        }

        .premium-feature::before {
            content: '✓';
            margin-right: 8px;
            color: var(--success-color);
            font-weight: bold;
        }

        .premium-badge {
            background: var(--premium-color);
            color: black;
            padding: 3px 8px;
            border-radius: 15px;
            font-size: 11px;
            margin-left: 6px;
            font-weight: bold;
            display: inline-block;
            transform: translateY(-3px);
        }

        .wallet-option {
            background: rgba(15, 52, 96, 0.7);
            padding: 12px;
            margin: 8px 0;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            font-size: 14px;
        }

        .wallet-option:hover {
            background: rgba(110, 69, 226, 0.3);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        #time-left {
            margin-top: 8px;
            font-size: 13px;
            color: var(--premium-color);
            font-weight: 600;
        }

        .emoji-effect {
            position: absolute;
            font-size: 22px;
            animation: float-up 1s forwards;
            opacity: 0.8;
            filter: brightness(0.7);
            pointer-events: none;
            z-index: 5;
        }

        .ref-link-container {
            background: rgba(13, 115, 119, 0.7);
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            margin: 12px 0;
            position: relative;
            overflow: hidden;
            font-size: 14px;
        }

        .ref-link-container:hover {
            background: rgba(13, 115, 119, 0.9);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .ref-link-container::after {
            content: 'Скопировать';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .ref-link-container:hover::after {
            opacity: 1;
        }

        .wallet-info {
            background: rgba(15, 52, 96, 0.7);
            padding: 12px;
            border-radius: 10px;
            margin-top: 15px;
            text-align: left;
            font-size: 14px;
        }

        .wallet-info p {
            margin: 6px 0;
        }

        .toast {
            position: fixed;
            bottom: 15px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 10px 20px;
            border-radius: 30px;
            z-index: 1000;
            display: none;
            animation: fadeIn 0.3s, fadeOut 0.3s 2.7s forwards;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
            font-size: 14px;
            max-width: 90%;
            white-space: pre-line;
            text-align: center;
        }

        /* Модальное окно конфиденциальности */
        #privacy-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 100;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(5px);
        }

        .privacy-content {
            background: var(--darker-bg);
            padding: 20px;
            border-radius: 15px;
            width: 90%;
            max-width: 100%;
            max-height: 80vh;
            overflow-y: auto;
            text-align: left;
            font-size: 14px;
        }

        .privacy-content h2 {
            color: var(--secondary-color);
            margin-bottom: 12px;
            text-align: center;
            font-size: 1.3rem;
        }

        .privacy-content h3 {
            color: var(--secondary-color);
            margin: 12px 0 8px;
            font-size: 1.1rem;
        }

        .privacy-content p {
            margin-bottom: 8px;
        }

        .privacy-content ul {
            margin-left: 18px;
            margin-bottom: 12px;
        }

        .close-privacy {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 30px;
            cursor: pointer;
            margin-top: 12px;
            display: block;
            margin-left: auto;
            margin-right: auto;
            font-size: 14px;
        }

        /* Стили для адреса кошелька */
        .wallet-address {
            background: rgba(110, 69, 226, 0.2);
            padding: 8px;
            border-radius: 5px;
            margin: 12px 0;
            word-break: break-all;
            font-family: monospace;
            font-size: 13px;
        }

        /* Стили для заданий */
        .quest-item {
            background: rgba(15, 52, 96, 0.7);
            padding: 12px;
            border-radius: 10px;
            margin: 10px 0;
            text-align: left;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .quest-title {
            font-weight: 600;
            margin-bottom: 5px;
        }

        .quest-description {
            font-size: 13px;
            color: rgba(255, 255, 255, 0.8);
        }

        .quest-reward {
            background: gold;
            color: black;
            padding: 4px 8px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: bold;
            margin-left: 10px;
            white-space: nowrap;
        }

        .quest-completed {
            background: rgba(76, 175, 80, 0.3);
            border: 1px solid rgba(76, 175, 80, 0.5);
        }

        .quest-completed .quest-title {
            text-decoration: line-through;
            opacity: 0.7;
        }

        .quest-completed .quest-reward {
            background: rgba(76, 175, 80, 0.5);
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes fadeOut {
            from { opacity: 1; }
            to { opacity: 0; }
        }

        @keyframes float-up {
            0% {
                transform: translateY(0);
                opacity: 0.8;
            }
            100% {
                transform: translateY(-80px);
                opacity: 0;
            }
        }

        @keyframes progressShine {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        /* Анимация появления монет */
        @keyframes coinBounce {
            0% { transform: translateY(0) scale(1); opacity: 1; }
            50% { transform: translateY(-15px) scale(1.2); opacity: 0.8; }
            100% { transform: translateY(-30px) scale(0.5); opacity: 0; }
        }

        .coin-effect {
            position: absolute;
            font-size: 18px;
            animation: coinBounce 1s forwards;
            pointer-events: none;
            z-index: 5;
            color: gold;
            font-weight: bold;
        }

        @media (max-width: 350px) {
            .header h1 {
                font-size: 1.3rem;
            }
            
            #click-area {
                width: 160px;
                height: 160px;
                font-size: 18px;
            }
            
            .nav-tab {
                padding: 10px 6px;
                font-size: 12px;
            }
            
            .premium-options {
                padding: 12px;
            }
            
            .privacy-content {
                padding: 12px;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="header-content">
            <h1 data-i18n="title">Insomnia Clicker <span id="premium-badge" class="premium-badge" style="display:none;" data-i18n="premiumBadge">PREMIUM</span></h1>
            <p data-i18n="balance">Баланс: <span id="balance">0</span> $INSOMNIA</p>
            <div id="time-left" style="display:none;"></div>
        </div>
        
        <button class="settings-btn" id="settings-btn" aria-label="Настройки">⚙️</button>
        <div class="settings-menu" id="settings-menu">
            <div class="settings-menu-item" id="language-btn">
                <i>🌐</i> <span data-i18n="language">Язык</span>
            </div>
            <div class="language-menu" id="language-menu">
                <div class="language-option" data-lang="ru">Русский</div>
                <div class="language-option" data-lang="en">English</div>
            </div>
            <div class="settings-menu-item" id="privacy-btn">
                <i>🔒</i> <span data-i18n="privacy">Политика конфиденциальности</span>
            </div>
            <div class="settings-menu-item" id="support-btn">
                <i>💬</i> <span data-i18n="support">Поддержка</span>
            </div>
            <div class="settings-menu-item" id="reset-progress">
                <i>🔄</i> <span data-i18n="resetProgress">Сбросить прогресс</span>
            </div>
        </div>
    </div>

    <div class="main-content">
        <!-- Кликер -->
        <div id="click-tab" class="tab-content active">
            <div class="progress-container">
                <div id="progress-bar" class="progress-bar"></div>
            </div>
            <div id="clicks-left" data-i18n="clicksLeft">Осталось кликов: 200/200</div>
            <div id="regen-info" data-i18n="regenInfo">Готово! Максимум кликов</div>
            <div id="click-area" aria-label="Кликните для заработка монет">INSOMNIA</div>
            <button id="buy-premium" data-i18n="buyPremium">Купить PREMIUM</button>
        </div>

        <!-- Друзья -->
        <div id="friends-tab" class="tab-content">
            <h3 data-i18n="referralTitle">Реферальная система</h3>
            <p id="ref-bonus-text" data-i18n="refBonusText">Получи +100 $INSOMNIA за каждого друга!</p>
            <div id="ref-link" class="ref-link-container" data-i18n="copyRefLink">
                Скопировать реферальную ссылку
            </div>
            <div id="ref-stats">
                <span data-i18n="invitedFriends">Приглашено</span>: <span id="ref-count">0</span> <span data-i18n="friends">друзей</span> |
                <span data-i18n="earned">Получено</span>: <span id="ref-earned">0</span> $INSOMNIA
            </div>
        </div>

        <!-- Кошелёк -->
        <div id="wallet-tab" class="tab-content">
            <h3 data-i18n="walletTitle">Подключение кошелька</h3>
            <div class="wallet-option" id="metamask-connect">
                <img src="https://upload.wikimedia.org/wikipedia/commons/3/36/MetaMask_Fox.svg" width="25" alt="MetaMask">
                MetaMask
            </div>
            <div class="wallet-option" id="walletconnect-connect">
                <img src="https://walletconnect.com/walletconnect-logo.png" width="25" alt="WalletConnect">
                WalletConnect
            </div>
            <div class="wallet-option" id="phantom-connect">
                <img src="https://phantom.app/favicon.ico" width="25" alt="Phantom">
                Phantom (Solana)
            </div>
            <div id="wallet-info" class="wallet-info" style="display:none;">
                <p><span data-i18n="connected">Подключен</span>: <span id="wallet-address">0x...0000</span></p>
                <p><span data-i18n="network">Сеть</span>: <span id="wallet-network">-</span></p>
            </div>
        </div>

        <!-- Задания -->
        <div id="quests-tab" class="tab-content">
            <h3 data-i18n="questsTitle">Задания</h3>
            
            <div class="quest-item" id="telegram-quest">
                <div>
                    <div class="quest-title">Подпишись на Telegram канал</div>
                    <div class="quest-description">Подпишись на наш канал и получи 500 $INSOMNIA</div>
                </div>
                <div class="quest-reward">+500 $INSOMNIA</div>
            </div>
            
            <div class="quest-item" id="click-quest">
                <div>
                    <div class="quest-title">Сделай 100 кликов</div>
                    <div class="quest-description">Кликни 100 раз и получи 200 $INSOMNIA</div>
                </div>
                <div class="quest-reward">+200 $INSOMNIA</div>
            </div>
            
            <div class="quest-item" id="premium-quest">
                <div>
                    <div class="quest-title">Активируй PREMIUM</div>
                    <div class="quest-description">Активируй PREMIUM подписку и получи 1000 $INSOMNIA</div>
                </div>
                <div class="quest-reward">+1000 $INSOMNIA</div>
            </div>
        </div>
    </div>

    <!-- Нижнее меню -->
    <div class="bottom-nav">
        <div class="nav-tab active" data-tab="click-tab" role="button" data-i18n="clickerTab">Кликер</div>
        <div class="nav-tab" data-tab="quests-tab" role="button" data-i18n="questsTab">Задания</div>
        <div class="nav-tab" data-tab="friends-tab" role="button" data-i18n="friendsTab">Друзья</div>
        <div class="nav-tab" data-tab="wallet-tab" role="button" data-i18n="walletTab">Кошелёк</div>
   </div>

    <!-- Модальное окно премиума -->
    <div id="premium-modal" role="dialog" aria-modal="true" aria-labelledby="premium-modal-title">
        <div class="premium-options">
            <h2 id="premium-modal-title" data-i18n="premiumTitle">PREMIUM подписка</h2>
            <div id="premium-time-info" style="margin-bottom:12px;"></div>
            
            <div class="premium-feature" data-i18n="premiumFeature1">2X монет за клики</div>
            <div class="premium-feature" data-i18n="premiumFeature2">2X монет за рефералов</div>
            <div class="premium-feature" data-i18n="premiumFeature3">Безлимитные клики</div>
            <div class="premium-feature" data-i18n="premiumFeature4">🎨 Выбор цвета кнопки и фона</div>
            
            <h3 data-i18n="chooseButtonColor">Выберите цвет кнопки:</h3>
            <div>
                <div class="color-option selected" style="background: linear-gradient(45deg, #6e45e2, #88d3ce);" data-color1="#6e45e2" data-color2="#88d3ce" aria-label="Фиолетово-голубой градиент"></div>
                <div class="color-option" style="background: linear-gradient(45deg, #FF416C, #FF4B2B);" data-color1="#FF416C" data-color2="#FF4B2B" aria-label="Красно-оранжевый градиент"></div>
                <div class="color-option" style="background: linear-gradient(45deg, #3a7bd5, #00d2ff);" data-color1="#3a7bd5" data-color2="#00d2ff" aria-label="Сине-голубой градиент"></div>
                <div class="color-option" style="background: linear-gradient(45deg, #11998e, #38ef7d);" data-color1="#11998e" data-color2="#38ef7d" aria-label="Зелено-бирюзовый градиент"></div>
                <div class="color-option" style="background: linear-gradient(45deg, #fc4a1a, #f7b733);" data-color1="#fc4a1a" data-color2="#f7b733" aria-label="Оранжево-желтый градиент"></div>
            </div>
            
            <h3 data-i18n="chooseBgColor">Выберите цвет фона:</h3>
            <div>
                <div class="color-option selected" style="background: #1a1a2e;" data-bg="#1a1a2e" aria-label="Темно-синий фон"></div>
                <div class="color-option" style="background: #0a0a1a;" data-bg="#0a0a1a" aria-label="Черно-синий фон"></div>
                <div class="color-option" style="background: #121230;" data-bg="#121230" aria-label="Синий фон"></div>
                <div class="color-option" style="background: #1e3c72;" data-bg="#1e3c72" aria-label="Темно-голубой фон"></div>
                <div class="color-option" style="background: #2c3e50;" data-bg="#2c3e50" aria-label="Серо-синий фон"></div>
            </div>
            
            <h3 data-i18n="chooseProgressColor">Выберите цвет шкалы кликов:</h3>
            <div>
                <div class="color-option" style="background: linear-gradient(90deg, #6e45e2, #88d3ce);" data-progress1="#6e45e2" data-progress2="#88d3ce" aria-label="Фиолетово-голубой градиент"></div>
                <div class="color-option" style="background: linear-gradient(90deg, #FF416C, #FF4B2B);" data-progress1="#FF416C" data-progress2="#FF4B2B" aria-label="Красно-оранжевый градиент"></div>
                <div class="color-option" style="background: linear-gradient(90deg, #3a7bd5, #00d2ff);" data-progress1="#3a7bd5" data-progress2="#00d2ff" aria-label="Сине-голубой градиент"></div>
                <div class="color-option" style="background: linear-gradient(90deg, #11998e, #38ef7d);" data-progress1="#11998e" data-progress2="#38ef7d" aria-label="Зелено-бирюзовый градиент"></div>
            </div>
            
            <div id="payment-options">
                <h3 data-i18n="paymentMethods">Способы оплаты:</h3>
                <button id="pay-crypto" class="wallet-option" style="margin: 10px 0;">
                    <img src="https://cryptologos.cc/logos/bitcoin-btc-logo.png" width="25" alt="Crypto">
                    Оплатить криптовалютой
                </button>
                <button id="pay-card" class="wallet-option" style="margin: 10px 0;">
                    <img src="https://cdn-icons-png.flaticon.com/512/179/179457.png" width="25" alt="Card">
                    Оплатить картой
                </button>
            </div>
            
            <div id="crypto-payment" style="display: none;">
                <div class="wallet-address" data-i18n="paymentWallet">
                    Для оплаты отправьте 4.99 USDT на адрес:<br>
                    <span id="crypto-address">UQA6AB1e1jlNtn9SzVyNThIjJqBhnUz_O5dZcMNHTDV2L6US</span>
                </div>
                <button id="check-payment" style="margin-top:15px;" data-i18n="checkPayment">
                    Проверить оплату
                </button>
            </div>
            
            <div id="card-payment" style="display: none;">
                <iframe id="payment-frame" src="about:blank" style="width: 100%; height: 300px; border: none; border-radius: 10px;"></iframe>
            </div>
            
            <button id="close-premium-modal" style="margin-top:8px;" data-i18n="close">
                Закрыть
            </button>
        </div>
    </div>

    <!-- Модальное окно конфиденциальности -->
    <div id="privacy-modal">
        <div class="privacy-content">
            <h2 data-i18n="privacyTitle">Политика конфиденциальности Insomnia Clicker</h2>
            
            <p data-i18n="privacyIntro">Настоящая Политика конфиденциальности описывает, как собирается, используется и защищается ваша информация при использовании нашего приложения Insomnia Clicker.</p>
            
            <h3 data-i18n="dataCollection">1. Сбор данных</h3>
            <p data-i18n="dataCollectionText">Приложение Insomnia Clicker собирает следующие данные:</p>
            <ul>
                <li data-i18n="data1">Локально сохраняемые данные о вашем игровом прогрессе (баланс, количество кликов, рефералы)</li>
                <li data-i18n="data2">Информация о подключенных кошельках (если вы решите подключить)</li>
                <li data-i18n="data3">Анонимная статистика использования для улучшения приложения</li>
            </ul>
            
            <h3 data-i18n="dataUsage">2. Использование данных</h3>
            <p data-i18n="dataUsageText">Собранные данные используются для:</p>
            <ul>
                <li data-i18n="usage1">Обеспечения работы игровых функций</li>
                <li data-i18n="usage2">Улучшения пользовательского опыта</li>
                <li data-i18n="usage3">Анализа эффективности функций приложения</li>
            </ul>
            
            <h3 data-i18n="dataStorage">3. Хранение данных</h3>
            <p data-i18n="dataStorageText">Все игровые данные хранятся локально в вашем браузере. Вы можете в любой момент сбросить прогресс в настройках.</p>
            
            <h3 data-i18n="thirdParty">4. Третьи стороны</h3>
            <p data-i18n="thirdPartyText">Приложение может интегрироваться со следующими сервисами:</p>
            <ul>
                <li data-i18n="service1">MetaMask и другие кошельки (по вашему выбору)</li>
                <li data-i18n="service2">Аналитические сервисы для сбора анонимной статистики</li>
            </ul>
            
            <h3 data-i18n="changes">5. Изменения в политике</h3>
            <p data-i18n="changesText">Мы оставляем за собой право обновлять данную Политику конфиденциальности. Все изменения будут отражены в этом документе.</p>
            
            <button class="close-privacy" id="close-privacy" data-i18n="understand">Понятно</button>
        </div>
    </div>

    <!-- Toast уведомления -->
    <div id="toast" class="toast"></div>

    <script>
        // Игровая логика
        const gameState = {
            coins: parseInt(localStorage.getItem('insomniaCoins')) || 0,
            clickPower: 1,
            refCount: parseInt(localStorage.getItem('refCount')) || 0,
            refEarned: parseInt(localStorage.getItem('refEarned')) || 0,
            REF_REWARD: 100,
            premiumActive: localStorage.getItem('premiumActive') === 'true' || false,
            premiumEndDate: localStorage.getItem('premiumEndDate'),
            selectedButtonColor1: localStorage.getItem('buttonColor1') || "#6e45e2",
            selectedButtonColor2: localStorage.getItem('buttonColor2') || "#88d3ce",
            selectedBgColor: localStorage.getItem('bgColor') || "#1a1a2e",
            selectedProgressColor1: localStorage.getItem('progressColor1') || "#6e45e2",
            selectedProgressColor2: localStorage.getItem('progressColor2') || "#88d3ce",
            MAX_CLICKS: 200,
            clicksLeft: parseInt(localStorage.getItem('clicksLeft')) || 200,
            regenStartTime: localStorage.getItem('regenStartTime') ? parseInt(localStorage.getItem('regenStartTime')) : Date.now(),
            REGEN_RATE: 200 / 60, // 200 кликов в минуту = ~3.33 клика/сек
            CLICK_DELAY: 300, // Задержка перед прибавлением клика в мс
            timerInterval: null,
            clickRefreshInterval: null,
            lastClickTime: 0,
            CLICK_COOLDOWN: 100, // Задержка между кликами в мс
            currentLanguage: localStorage.getItem('language') || 'ru',
            totalClicks: parseInt(localStorage.getItem('totalClicks')) || 0,
            completedQuests: JSON.parse(localStorage.getItem('completedQuests')) || [],
            paymentStatus: localStorage.getItem('paymentStatus') || 'not_started'
        };

        // Локализация
        const translations = {
            ru: {
                title: "Insomnia Clicker",
                premiumBadge: "PREMIUM",
                balance: "Баланс:",
                clicksLeft: "Осталось кликов:",
                regenInfo: "Готово! Максимум кликов",
                buyPremium: "Купить PREMIUM",
                referralTitle: "Реферальная система",
                refBonusText: "Получи +100 $INSOMNIA за каждого друга!",
                refBonusTextPremium: "Получи +200 $INSOMNIA за каждого друга! (PREMIUM 2X)",
                copyRefLink: "Скопировать реферальную ссылку",
                invitedFriends: "Приглашено",
                friends: "друзей",
                earned: "Получено",
                walletTitle: "Подключение кошелька",
                connected: "Подключен",
                network: "Сеть",
                clickerTab: "Кликер",
                friendsTab: "Друзья",
                walletTab: "Кошелёк",
                questsTab: "Задания",
                premiumTitle: "PREMIUM подписка",
                premiumFeature1: "2X монет за клики",
                premiumFeature2: "2X монет за рефералов",
                premiumFeature3: "Безлимитные клики",
                premiumFeature4: "🎨 Выбор цвета кнопки и фона",
                chooseButtonColor: "Выберите цвет кнопки:",
                chooseBgColor: "Выберите цвет фона:",
                chooseProgressColor: "Выберите цвет шкалы кликов:",
                confirmPayment: "Я оплатил",
                close: "Закрыть",
                privacy: "Политика конфиденциальности",
                support: "Поддержка",
                resetProgress: "Сбросить прогресс",
                language: "Язык",
                privacyTitle: "Политика конфиденциальности Insomnia Clicker",
                privacyIntro: "Настоящая Политика конфиденциальности описывает, как собирается, используется и защищается ваша информация при использовании нашего приложения Insomnia Clicker.",
                dataCollection: "1. Сбор данных",
                dataCollectionText: "Приложение Insomnia Clicker собирает следующие данные:",
                data1: "Локально сохраняемые данные о вашем игровом прогрессе (баланс, количество кликов, рефералы)",
                data2: "Информация о подключенных кошельках (если вы решите подключить)",
                data3: "Анонимная статистика использования для улучшения приложения",
                dataUsage: "2. Использование данных",
                dataUsageText: "Собранные данные используются для:",
                usage1: "Обеспечения работы игровых функций",
                usage2: "Улучшения пользовательского опыта",
                usage3: "Анализа эффективности функций приложения",
                dataStorage: "3. Хранение данных",
                dataStorageText: "Все игровые данные хранятся локально в вашем браузере. Вы можете в любой момент сбросить прогресс в настройках.",
                thirdParty: "4. Третьи стороны",
                thirdPartyText: "Приложение может интегрироваться со следующими сервисами:",
                service1: "MetaMask и другие кошельки (по вашему выбору)",
                service2: "Аналитические сервисы для сбора анонимной статистики",
                changes: "5. Изменения в политике",
                changesText: "Мы оставляем за собой право обновлять данную Политику конфиденциальности. Все изменения будут отражены в этом документе.",
                understand: "Понятно",
                paymentWallet: "Для оплаты отправьте 4.99 USDT на адрес:",
                premiumActive: "PREMIUM активен",
                unlimitedClicks: "Безлимитные клики",
                premiumExpiresIn: "PREMIUM истекает через",
                premiumActiveFor: "Ваша подписка активна ещё",
                premiumOnly: "Эта функция доступна только для PREMIUM пользователей!",
                premiumActivated: "PREMIUM активирован!",
                premiumActivated7Days: "PREMIUM подписка активирована на 7 дней!",
                premiumExpired: "PREMIUM подписка истекла",
                refReward: "Вы получили",
                forFriendInvite: "за приглашение друга!",
                refLinkCopied: "Реферальная ссылка скопирована!\nОтправьте её друзьям!",
                walletConnected: "Кошелёк подключен",
                walletError: "Ошибка подключения кошелька",
                installMetamask: "Установите MetaMask!",
                walletConnectIntegration: "Интеграция с WalletConnect будет здесь",
                phantomConnected: "Кошелёк Phantom успешно подключен!",
                phantomError: "Ошибка подключения Phantom",
                installPhantom: "Установите Phantom Wallet!",
                languageChanged: "Язык изменен на",
                resetConfirm: "Вы уверены, что хотите сбросить весь прогресс? Это действие нельзя отменить.",
                progressReset: "Прогресс сброшен",
                regenInfo: "Восстановление",
                nextIn: "следующий +1 через",
                seconds: "сек",
                regenReady: "Готово! Максимум кликов",
                unlimitedClicksPremium: "Безлимитные клики (PREMIUM)",
                paymentMethods: "Способы оплаты:",
                checkPayment: "Проверить оплату",
                paymentSuccess: "Оплата подтверждена! PREMIUM активирован!",
                paymentPending: "Оплата еще не подтверждена. Попробуйте позже.",
                paymentFailed: "Оплата не найдена. Пожалуйста, попробуйте еще раз.",
                questsTitle: "Задания",
                telegramQuestTitle: "Подпишись на Telegram канал",
                telegramQuestDesc: "Подпишись на наш канал и получи 500 $INSOMNIA",
                clickQuestTitle: "Сделай 100 кликов",
                clickQuestDesc: "Кликни 100 раз и получи 200 $INSOMNIA",
                premiumQuestTitle: "Активируй PREMIUM",
                premiumQuestDesc: "Активируй PREMIUM подписку и получи 1000 $INSOMNIA",
                questCompleted: "Задание выполнено! +",
                subscribeTelegram: "Подписаться на канал",
                telegramSubscribed: "Спасибо за подписку! Награда зачислена."
            },
            en: {
                title: "Insomnia Clicker",
                premiumBadge: "PREMIUM",
                balance: "Balance:",
                clicksLeft: "Clicks left:",
                regenInfo: "Ready! Maximum clicks",
                buyPremium: "Buy PREMIUM",
                referralTitle: "Referral system",
                refBonusText: "Get +100 $INSOMNIA for each friend!",
                refBonusTextPremium: "Get +200 $INSOMNIA for each friend! (PREMIUM 2X)",
                copyRefLink: "Copy referral link",
                invitedFriends: "Invited",
                friends: "friends",
                earned: "Earned",
                walletTitle: "Wallet connection",
                connected: "Connected",
                network: "Network",
                clickerTab: "Clicker",
                friendsTab: "Friends",
                walletTab: "Wallet",
                questsTab: "Quests",
                premiumTitle: "PREMIUM subscription",
                premiumFeature1: "2X coins per click",
                premiumFeature2: "2X coins for referrals",
                premiumFeature3: "Unlimited clicks",
                premiumFeature4: "🎨 Button and background color selection",
                chooseButtonColor: "Choose button color:",
                chooseBgColor: "Choose background color:",
                chooseProgressColor: "Choose progress bar color:",
                confirmPayment: "I have paid",
                close: "Close",
                privacy: "Privacy Policy",
                support: "Support",
                resetProgress: "Reset progress",
                language: "Language",
                privacyTitle: "Insomnia Clicker Privacy Policy",
                privacyIntro: "This Privacy Policy describes how your information is collected, used and protected when using our Insomnia Clicker application.",
                dataCollection: "1. Data collection",
                dataCollectionText: "The Insomnia Clicker app collects the following data:",
                data1: "Locally stored data about your game progress (balance, number of clicks, referrals)",
                data2: "Information about connected wallets (if you choose to connect)",
                data3: "Anonymous usage statistics to improve the app",
                dataUsage: "2. Data usage",
                dataUsageText: "The collected data is used for:",
                usage1: "Providing game functionality",
                usage2: "Improving user experience",
                usage3: "Analyzing the effectiveness of application features",
                dataStorage: "3. Data storage",
                dataStorageText: "All game data is stored locally in your browser. You can reset progress at any time in the settings.",
                thirdParty: "4. Third parties",
                thirdPartyText: "The application may integrate with the following services:",
                service1: "MetaMask and other wallets (at your choice)",
                service2: "Analytics services for collecting anonymous statistics",
                changes: "5. Policy changes",
                changesText: "We reserve the right to update this Privacy Policy. All changes will be reflected in this document.",
                understand: "I understand",
                paymentWallet: "For payment send 4.99 USDT to address:",
                premiumActive: "PREMIUM active",
                unlimitedClicks: "Unlimited clicks",
                premiumExpiresIn: "PREMIUM expires in",
                premiumActiveFor: "Your subscription is active for",
                premiumOnly: "This feature is for PREMIUM users only!",
                premiumActivated: "PREMIUM activated!",
                premiumActivated7Days: "PREMIUM subscription activated for 7 days!",
                premiumExpired: "PREMIUM subscription expired",
                refReward: "You got",
                forFriendInvite: "for inviting a friend!",
                refLinkCopied: "Referral link copied!\nShare it with friends!",
                walletConnected: "Wallet connected",
                walletError: "Wallet connection error",
                installMetamask: "Install MetaMask!",
                walletConnectIntegration: "WalletConnect integration would go here",
                phantomConnected: "Phantom wallet connected!",
                phantomError: "Phantom connection error",
                installPhantom: "Install Phantom Wallet!",
                languageChanged: "Language changed to",
                resetConfirm: "Are you sure you want to reset all progress? This action cannot be undone.",
                progressReset: "Progress reset",
                regenInfo: "Regeneration",
                nextIn: "next +1 in",
                seconds: "sec",
                regenReady: "Ready! Maximum clicks",
                unlimitedClicksPremium: "Unlimited clicks (PREMIUM)",
                paymentMethods: "Payment methods:",
                checkPayment: "Check payment",
                paymentSuccess: "Payment confirmed! PREMIUM activated!",
                paymentPending: "Payment not confirmed yet. Please try again later.",
                paymentFailed: "Payment not found. Please try again.",
                questsTitle: "Quests",
                telegramQuestTitle: "Subscribe to Telegram channel",
                telegramQuestDesc: "Subscribe to our channel and get 500 $INSOMNIA",
                clickQuestTitle: "Make 100 clicks",
                clickQuestDesc: "Click 100 times and get 200 $INSOMNIA",
                premiumQuestTitle: "Activate PREMIUM",
                premiumQuestDesc: "Activate PREMIUM subscription and get 1000 $INSOMNIA",
                questCompleted: "Quest completed! +",
                subscribeTelegram: "Subscribe to channel",
                telegramSubscribed: "Thanks for subscribing! Reward credited."
            }
        };

        // DOM элементы
        const elements = {
            balance: document.getElementById('balance'),
            premiumBadge: document.getElementById('premium-badge'),
            refBonusText: document.getElementById('ref-bonus-text'),
            buyPremiumBtn: document.getElementById('buy-premium'),
            timeLeft: document.getElementById('time-left'),
            regenInfo: document.getElementById('regen-info'),
            progressBar: document.getElementById('progress-bar'),
            clicksLeft: document.getElementById('clicks-left'),
            clickArea: document.getElementById('click-area'),
            refCount: document.getElementById('ref-count'),
            refEarned: document.getElementById('ref-earned'),
            refLink: document.getElementById('ref-link'),
            premiumModal: document.getElementById('premium-modal'),
            premiumTimeInfo: document.getElementById('premium-time-info'),
            confirmPremium: document.getElementById('confirm-premium'),
            closePremiumModal: document.getElementById('close-premium-modal'),
            walletInfo: document.getElementById('wallet-info'),
            walletAddress: document.getElementById('wallet-address'),
            walletNetwork: document.getElementById('wallet-network'),
            metamaskConnect: document.getElementById('metamask-connect'),
            walletconnectConnect: document.getElementById('walletconnect-connect'),
            phantomConnect: document.getElementById('phantom-connect'),
            settingsBtn: document.getElementById('settings-btn'),
            settingsMenu: document.getElementById('settings-menu'),
            languageBtn: document.getElementById('language-btn'),
            languageMenu: document.getElementById('language-menu'),
            resetProgress: document.getElementById('reset-progress'),
            privacyBtn: document.getElementById('privacy-btn'),
            supportBtn: document.getElementById('support-btn'),
            privacyModal: document.getElementById('privacy-modal'),
            closePrivacy: document.getElementById('close-privacy'),
            toast: document.getElementById('toast'),
            telegramQuest: document.getElementById('telegram-quest'),
            clickQuest: document.getElementById('click-quest'),
            premiumQuest: document.getElementById('premium-quest'),
            payCrypto: document.getElementById('pay-crypto'),
            payCard: document.getElementById('pay-card'),
            cryptoPayment: document.getElementById('crypto-payment'),
            cardPayment: document.getElementById('card-payment'),
            paymentOptions: document.getElementById('payment-options'),
            checkPayment: document.getElementById('check-payment'),
            paymentFrame: document.getElementById('payment-frame'),
            cryptoAddress: document.getElementById('crypto-address')
        };

        // Вспомогательные функции
        function showToast(message, duration = 3000) {
            elements.toast.textContent = message;
            elements.toast.style.display = 'block';
            
            // Сбрасываем предыдущую анимацию
            elements.toast.style.animation = 'none';
            void elements.toast.offsetWidth; // Trigger reflow
            elements.toast.style.animation = `fadeIn 0.3s, fadeOut 0.3s ${duration/1000 - 0.3}s forwards`;
            
            setTimeout(() => {
                elements.toast.style.display = 'none';
            }, duration);
        }

        function animateClick() {
            elements.clickArea.style.transform = 'scale(0.95)';
            setTimeout(() => {
                elements.clickArea.style.transform = 'scale(1)';
            }, 100);
        }

        function createEmojiEffect(x, y) {
            const emoji = document.createElement('div');
            emoji.className = 'emoji-effect';
            emoji.textContent = '🌙';
            emoji.style.left = `${x}px`;
            emoji.style.top = `${y}px`;
            document.body.appendChild(emoji);
            
            setTimeout(() => {
                emoji.remove();
            }, 1000);
        }

        function createCoinEffect(x, y, amount) {
            const coin = document.createElement('div');
            coin.className = 'coin-effect';
            coin.textContent = `+${amount}`;
            coin.style.left = `${x}px`;
            coin.style.top = `${y}px`;
            document.body.appendChild(coin);
            
            setTimeout(() => {
                coin.remove();
            }, 1000);
        }

        // Функция для перевода интерфейса
        function translatePage(lang) {
            gameState.currentLanguage = lang;
            localStorage.setItem('language', lang);
            
            document.querySelectorAll('[data-i18n]').forEach(element => {
                const key = element.getAttribute('data-i18n');
                if (translations[lang][key]) {
                    if (element.tagName === 'INPUT' || element.tagName === 'TEXTAREA') {
                        element.placeholder = translations[lang][key];
                    } else {
                        // Для элементов, содержащих другие элементы (например, span с балансом)
                        if (element.childNodes.length === 1 && element.childNodes[0].nodeType === Node.TEXT_NODE) {
                            element.textContent = translations[lang][key];
                        } else {
                            // Для элементов с вложенным текстом
                            const textNodes = [];
                            const walker = document.createTreeWalker(
                                element,
                                NodeFilter.SHOW_TEXT,
                                null,
                                false
                            );
                            
                            let node;
                            while (node = walker.nextNode()) {
                                textNodes.push(node);
                            }
                            
                            if (textNodes.length > 0 && textNodes[0].nodeValue.trim() !== '') {
                                textNodes[0].nodeValue = translations[lang][key];
                            }
                        }
                    }
                }
            });
            
            // Обновляем динамические тексты
            updateClicksCounter();
            updatePremiumTimeInfo();
            
            // Для кнопки премиума с особым текстом
            if (gameState.premiumActive) {
                elements.buyPremiumBtn.textContent = translations[lang]['premiumActive'];
                elements.buyPremiumBtn.classList.add('premium-btn');
                elements.refBonusText.textContent = translations[lang]['refBonusTextPremium'];
            } else {
                elements.buyPremiumBtn.textContent = translations[lang]['buyPremium'];
                elements.buyPremiumBtn.classList.remove('premium-btn');
            }
        }

        // Проверка премиум-статуса
        function checkPremiumStatus() {
            if (gameState.premiumEndDate && new Date() < new Date(gameState.premiumEndDate)) {
                activatePremium();
                startTimer();
            } else if (gameState.premiumActive) {
                // Если премиум активен, но дата истекла, деактивируем
                gameState.premiumActive = false;
                localStorage.setItem('premiumActive', 'false');
            }
        }

        // Активация премиума
        function activatePremium() {
            gameState.premiumActive = true;
            localStorage.setItem('premiumActive', 'true');
            gameState.clickPower = 2;
            elements.premiumBadge.style.display = "inline";
            elements.refBonusText.textContent = translations[gameState.currentLanguage]['refBonusTextPremium'];
            elements.buyPremiumBtn.textContent = translations[gameState.currentLanguage]['premiumActive'];
            elements.buyPremiumBtn.classList.add('premium-btn');
            elements.timeLeft.style.display = "block";
            elements.regenInfo.textContent = translations[gameState.currentLanguage]['unlimitedClicks'];
            
            // Восстанавливаем клики при активации премиума
            gameState.clicksLeft = Infinity;
            updateClicksCounter();
            
            // Применяем сохраненные цвета
            applySavedColors();
            
            showToast(translations[gameState.currentLanguage]['premiumActivated']);
            
            // Проверяем задание на премиум
            checkPremiumQuest();
        }

        // Таймер обратного отсчета
        function startTimer() {
            clearInterval(gameState.timerInterval);
            
            gameState.timerInterval = setInterval(() => {
                const now = new Date();
                const endDate = new Date(gameState.premiumEndDate);
                const diff = endDate - now;
                
                if (diff <= 0) {
                    clearInterval(gameState.timerInterval);
                    gameState.premiumActive = false;
                    localStorage.setItem('premiumActive', 'false');
                    elements.premiumBadge.style.display = "none";
                    elements.buyPremiumBtn.textContent = translations[gameState.currentLanguage]['buyPremium'];
                    elements.buyPremiumBtn.classList.remove('premium-btn');
                    elements.timeLeft.style.display = "none";
                    gameState.clicksLeft = gameState.MAX_CLICKS;
                    updateClicksCounter();
                    startClickRefresh();
                    showToast(translations[gameState.currentLanguage]['premiumExpired']);
                    return;
                }
                
                const days = Math.floor(diff / (1000 * 60 * 60 * 24));
                const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((diff % (1000 * 60)) / 1000);
                
                elements.timeLeft.textContent =
                    `${translations[gameState.currentLanguage]['premiumExpiresIn']}: ${days}d ${hours}h ${minutes}m ${seconds}s`;
                
                updatePremiumTimeInfo();
            }, 1000);
        }

        // Постоянное восстановление кликов
        function startClickRefresh() {
            clearInterval(gameState.clickRefreshInterval);
            
            if (gameState.premiumActive) return;
            
            gameState.clickRefreshInterval = setInterval(() => {
                const now = Date.now();
                const timePassed = (now - gameState.regenStartTime) / 1000; // в секундах
                const regenerated = Math.floor(timePassed * gameState.REGEN_RATE);
                
                if (gameState.clicksLeft < gameState.MAX_CLICKS) {
                    gameState.clicksLeft = Math.min(gameState.MAX_CLICKS, regenerated);
                    updateClicksCounter();
                    
                    if (gameState.clicksLeft < gameState.MAX_CLICKS) {
                        const nextClickIn = Math.ceil((1 - (timePassed * gameState.REGEN_RATE % 1)) / gameState.REGEN_RATE);
                        elements.regenInfo.textContent =
                            `${translations[gameState.currentLanguage]['regenInfo']}: ${gameState.clicksLeft}/${gameState.MAX_CLICKS} ` +
                            `(${translations[gameState.currentLanguage]['nextIn']} ${nextClickIn.toFixed(1)}${translations[gameState.currentLanguage]['seconds']})`;
                    } else {
                        elements.regenInfo.textContent = translations[gameState.currentLanguage]['regenReady'];
                    }
                }
            }, 100); // Частое обновление для плавности
        }

        // Обновление информации о времени в модальном окне
        function updatePremiumTimeInfo() {
            if (!gameState.premiumActive) return;
            
            const now = new Date();
            const endDate = new Date(gameState.premiumEndDate);
            const diff = endDate - now;
            
            if (diff <= 0) return;
            
            const days = Math.floor(diff / (1000 * 60 * 60 * 24));
            const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
            
            elements.premiumTimeInfo.innerHTML =
                `<p style="color:gold;">${translations[gameState.currentLanguage]['premiumActiveFor']} ${days}d ${hours}h ${minutes}m</p>`;
        }

        // Обновление счетчика кликов
        function updateClicksCounter() {
            const progressPercent = gameState.premiumActive ? 100 : (gameState.clicksLeft / gameState.MAX_CLICKS) * 100;
            elements.progressBar.style.background =
                `linear-gradient(90deg, ${gameState.selectedProgressColor1}, ${gameState.selectedProgressColor2})`;
            elements.progressBar.style.width = `${progressPercent}%`;
            
            if (gameState.premiumActive) {
                elements.clicksLeft.textContent = translations[gameState.currentLanguage]['unlimitedClicksPremium'];
            } else {
                const [before] = elements.clicksLeft.textContent.split(':');
                elements.clicksLeft.textContent = `${before || translations[gameState.currentLanguage]['clicksLeft']}: ${gameState.clicksLeft}/${gameState.MAX_CLICKS}`;
            }
            
            localStorage.setItem('clicksLeft', gameState.clicksLeft);
        }

        // Обновление баланса
        function updateBalance() {
            elements.balance.textContent = gameState.coins;
            localStorage.setItem('insomniaCoins', gameState.coins);
        }

        // Применение сохраненных цветов
        function applySavedColors() {
            if (gameState.premiumActive) {
                elements.clickArea.style.background =
                    `linear-gradient(45deg, ${gameState.selectedButtonColor1}, ${gameState.selectedButtonColor2})`;
                document.body.style.backgroundColor = gameState.selectedBgColor;
                updateClicksCounter();
            }
        }

        // Генерация реферального кода
        function generateRefCode() {
            return Math.random().toString(36).substring(2, 8).toUpperCase();
        }

        // Проверка реферала
        function checkReferral() {
            const urlParams = new URLSearchParams(window.location.search);
            if (urlParams.has('ref')) {
                const refCode = urlParams.get('ref');
                if (!localStorage.getItem('refProcessed')) {
                    gameState.refCount++;
                    const reward = gameState.premiumActive ? gameState.REF_REWARD * 2 : gameState.REF_REWARD;
                    gameState.refEarned += reward;
                    gameState.coins += reward;
                    updateBalance();
                    localStorage.setItem('refCount', gameState.refCount);
                    localStorage.setItem('refEarned', gameState.refEarned);
                    localStorage.setItem('refProcessed', 'true');
                    elements.refCount.textContent = gameState.refCount;
                    elements.refEarned.textContent = gameState.refEarned;
                    showToast(`${translations[gameState.currentLanguage]['refReward']} ${reward} $INSOMNIA ${translations[gameState.currentLanguage]['forFriendInvite']}`);
                }
            }
        }

        // Проверка заданий
        function checkQuests() {
            // Проверяем задание на клики
            if (gameState.totalClicks >= 100 && !gameState.completedQuests.includes('click_quest')) {
                completeQuest('click_quest', 200);
            }
            
            // Обновляем статус заданий
            updateQuestStatus();
        }

        function completeQuest(questId, reward) {
            gameState.completedQuests.push(questId);
            gameState.coins += reward;
            updateBalance();
            localStorage.setItem('completedQuests', JSON.stringify(gameState.completedQuests));
            showToast(`${translations[gameState.currentLanguage]['questCompleted']}${reward} $INSOMNIA`);
            updateQuestStatus();
        }

        function updateQuestStatus() {
            // Telegram quest
            if (gameState.completedQuests.includes('telegram_quest')) {
                elements.telegramQuest.classList.add('quest-completed');
            }
            
            // Click quest
            if (gameState.completedQuests.includes('click_quest')) {
                elements.clickQuest.classList.add('quest-completed');
            }
            
            // Premium quest
            if (gameState.completedQuests.includes('premium_quest')) {
                elements.premiumQuest.classList.add('quest-completed');
            }
        }

        function checkPremiumQuest() {
            if (gameState.premiumActive && !gameState.completedQuests.includes('premium_quest')) {
                completeQuest('premium_quest', 1000);
            }
        }

        // Проверка платежа (имитация)
        function checkPaymentStatus() {
            // В реальном приложении здесь будет запрос к серверу для проверки платежа
            if (gameState.paymentStatus === 'completed') {
                showToast(translations[gameState.currentLanguage]['paymentSuccess']);
                const endDate = new Date();
                endDate.setDate(endDate.getDate() + 7);
                gameState.premiumEndDate = endDate.toISOString();
                localStorage.setItem('premiumEndDate', gameState.premiumEndDate);
                activatePremium();
                startTimer();
                elements.premiumModal.style.display = 'none';
                elements.cryptoPayment.style.display = 'none';
                elements.cardPayment.style.display = 'none';
                elements.paymentOptions.style.display = 'block';
            } else if (gameState.paymentStatus === 'pending') {
                showToast(translations[gameState.currentLanguage]['paymentPending']);
            } else {
                showToast(translations[gameState.currentLanguage]['paymentFailed']);
            }
        }

        // Сброс прогресса
        function resetProgress() {
            if (confirm(translations[gameState.currentLanguage]['resetConfirm'])) {
                // Сохраняем премиум данные перед сбросом
                const premiumData = {
                    active: gameState.premiumActive,
                    endDate: gameState.premiumEndDate,
                    buttonColor1: gameState.selectedButtonColor1,
                    buttonColor2: gameState.selectedButtonColor2,
                    bgColor: gameState.selectedBgColor,
                    progressColor1: gameState.selectedProgressColor1,
                    progressColor2: gameState.selectedProgressColor2
                };
                
                // Сбрасываем все данные
                localStorage.clear();
                
                // Восстанавливаем премиум данные
                if (premiumData.active) {
                    localStorage.setItem('premiumActive', 'true');
                    localStorage.setItem('premiumEndDate', premiumData.endDate);
                    localStorage.setItem('buttonColor1', premiumData.buttonColor1);
                    localStorage.setItem('buttonColor2', premiumData.buttonColor2);
                    localStorage.setItem('bgColor', premiumData.bgColor);
                    localStorage.setItem('progressColor1', premiumData.progressColor1);
                    localStorage.setItem('progressColor2', premiumData.progressColor2);
                }
                
                // Обновляем состояние игры
                gameState.coins = 0;
                gameState.refCount = 0;
                gameState.refEarned = 0;
                gameState.clicksLeft = gameState.MAX_CLICKS;
                gameState.regenStartTime = Date.now();
                gameState.totalClicks = 0;
                gameState.completedQuests = [];
                gameState.paymentStatus = 'not_started';
                
                // Восстанавливаем премиум статус
                gameState.premiumActive = premiumData.active;
                gameState.premiumEndDate = premiumData.endDate;
                gameState.selectedButtonColor1 = premiumData.buttonColor1;
                gameState.selectedButtonColor2 = premiumData.buttonColor2;
                gameState.selectedBgColor = premiumData.bgColor;
                gameState.selectedProgressColor1 = premiumData.progressColor1;
                gameState.selectedProgressColor2 = premiumData.progressColor2;
                
                // Обновление UI
                updateBalance();
                elements.refCount.textContent = gameState.refCount;
                elements.refEarned.textContent = gameState.refEarned;
                updateClicksCounter();
                updateQuestStatus();
                
                if (gameState.premiumActive) {
                    activatePremium();
                } else {
                    elements.premiumBadge.style.display = "none";
                    elements.buyPremiumBtn.textContent = translations[gameState.currentLanguage]['buyPremium'];
                    elements.buyPremiumBtn.classList.remove('premium-btn');
                    elements.timeLeft.style.display = "none";
                    startClickRefresh();
                }
                
                // Сброс цветов интерфейса
                elements.clickArea.style.background =
                    `linear-gradient(45deg, ${gameState.selectedButtonColor1}, ${gameState.selectedButtonColor2})`;
                document.body.style.backgroundColor = gameState.selectedBgColor;
                updateClicksCounter();
                
                showToast(translations[gameState.currentLanguage]['progressReset']);
            }
        }

        // Инициализация игры
        function init() {
            // Восстановление кликов при загрузке
            if (!gameState.premiumActive) {
                const now = Date.now();
                const timePassed = (now - gameState.regenStartTime) / 1000;
                const regenerated = Math.floor(timePassed * gameState.REGEN_RATE);
                gameState.clicksLeft = Math.min(gameState.MAX_CLICKS, regenerated);
                updateClicksCounter();
                
                if (gameState.clicksLeft < gameState.MAX_CLICKS) {
                    const nextClickIn = Math.ceil((1 - (timePassed * gameState.REGEN_RATE % 1)) / gameState.REGEN_RATE);
                    elements.regenInfo.textContent =
                        `${translations[gameState.currentLanguage]['regenInfo']}: ${gameState.clicksLeft}/${gameState.MAX_CLICKS} ` +
                        `(${translations[gameState.currentLanguage]['nextIn']} ${nextClickIn.toFixed(1)}${translations[gameState.currentLanguage]['seconds']})`;
                } else {
                    elements.regenInfo.textContent = translations[gameState.currentLanguage]['regenReady'];
                }
            }
            
            elements.balance.textContent = gameState.coins;
            elements.refCount.textContent = gameState.refCount;
            elements.refEarned.textContent = gameState.refEarned;
            updateClicksCounter();
            checkPremiumStatus();
            startClickRefresh();
            checkQuests();
            
            // Применяем выбранный язык
            translatePage(gameState.currentLanguage);
        }

        // Обработчики событий
        function setupEventListeners() {
            // Кликер с задержкой и эффектом
            elements.clickArea.addEventListener('click', (e) => {
                const now = Date.now();
                if (now - gameState.lastClickTime < gameState.CLICK_COOLDOWN) return;
                gameState.lastClickTime = now;
                
                if (gameState.clicksLeft > 0 || gameState.premiumActive) {
                    // Создаем эффект эмодзи
                    createEmojiEffect(e.clientX, e.clientY);
                    
                    if (!gameState.premiumActive) {
                        if (gameState.clicksLeft <= 0) return;
                        
                        // Визуальная реакция на клик
                        animateClick();
                        
                        // Задержка перед обработкой клика
                        setTimeout(() => {
                            gameState.clicksLeft--;
                            gameState.totalClicks++;
                            localStorage.setItem('totalClicks', gameState.totalClicks);
                            updateClicksCounter();
                            
                            if (gameState.clicksLeft <= 0) {
                                gameState.regenStartTime = Date.now();
                                localStorage.setItem('regenStartTime', gameState.regenStartTime);
                                elements.regenInfo.textContent =
                                    `${translations[gameState.currentLanguage]['regenInfo']}: 0/${gameState.MAX_CLICKS} ` +
                                    `(${translations[gameState.currentLanguage]['nextIn']} 0.3${translations[gameState.currentLanguage]['seconds']})`;
                            }
                            
                            gameState.coins += gameState.clickPower;
                            createCoinEffect(e.clientX, e.clientY, gameState.clickPower);
                            updateBalance();
                            
                            // Проверяем задание на клики
                            if (gameState.totalClicks >= 100 && !gameState.completedQuests.includes('click_quest')) {
                                completeQuest('click_quest', 200);
                            }
                        }, gameState.CLICK_DELAY);
                    } else {
                        // Для премиум-пользователей без задержки
                        animateClick();
                        gameState.coins += gameState.clickPower;
                        gameState.totalClicks++;
                        localStorage.setItem('totalClicks', gameState.totalClicks);
                        createCoinEffect(e.clientX, e.clientY, gameState.clickPower);
                        updateBalance();
                    }
                }
            });

            // Рефералы
            elements.refLink.addEventListener('click', () => {
                const refLink = `${window.location.origin}${window.location.pathname}?ref=${generateRefCode()}`;
                navigator.clipboard.writeText(refLink).then(() => {
                    showToast(translations[gameState.currentLanguage]['refLinkCopied']);
                });
            });

            // Премиум подписка
            elements.buyPremiumBtn.addEventListener('click', () => {
                elements.premiumModal.style.display = 'flex';
                updatePremiumTimeInfo();
            });

            elements.closePremiumModal.addEventListener('click', () => {
                elements.premiumModal.style.display = 'none';
                elements.cryptoPayment.style.display = 'none';
                elements.cardPayment.style.display = 'none';
                elements.paymentOptions.style.display = 'block';
            });

            // Выбор цвета (только для премиум)
            document.querySelectorAll('.color-option').forEach(option => {
                option.addEventListener('click', function() {
                    if (!gameState.premiumActive) {
                        showToast(translations[gameState.currentLanguage]['premiumOnly']);
                        return;
                    }
                    
                    document.querySelectorAll('.color-option').forEach(opt => opt.classList.remove('selected'));
                    this.classList.add('selected');
                    
                    if (this.dataset.color1) {
                        gameState.selectedButtonColor1 = this.dataset.color1;
                        gameState.selectedButtonColor2 = this.dataset.color2;
                        elements.clickArea.style.background =
                            `linear-gradient(45deg, ${gameState.selectedButtonColor1}, ${gameState.selectedButtonColor2})`;
                        localStorage.setItem('buttonColor1', gameState.selectedButtonColor1);
                        localStorage.setItem('buttonColor2', gameState.selectedButtonColor2);
                    } else if (this.dataset.bg) {
                        gameState.selectedBgColor = this.dataset.bg;
                        document.body.style.backgroundColor = gameState.selectedBgColor;
                        localStorage.setItem('bgColor', gameState.selectedBgColor);
                    } else if (this.dataset.progress1) {
                        gameState.selectedProgressColor1 = this.dataset.progress1;
                        gameState.selectedProgressColor2 = this.dataset.progress2;
                        updateClicksCounter();
                        localStorage.setItem('progressColor1', gameState.selectedProgressColor1);
                        localStorage.setItem('progressColor2', gameState.selectedProgressColor2);
                    }
                });
            });

            // Оплата криптовалютой
            elements.payCrypto.addEventListener('click', () => {
                elements.paymentOptions.style.display = 'none';
                elements.cryptoPayment.style.display = 'block';
                elements.cardPayment.style.display = 'none';
                
                // В реальном приложении здесь будет генерация уникального адреса для пользователя
                elements.cryptoAddress.textContent = 'UQA6AB1e1jlNtn9SzVyNThIjJqBhnUz_O5dZcMNHTDV2L6US';
            });

            // Оплата картой
            elements.payCard.addEventListener('click', () => {
                elements.paymentOptions.style.display = 'none';
                elements.cryptoPayment.style.display = 'none';
                elements.cardPayment.style.display = 'block';
                
                // В реальном приложении здесь будет загрузка платежного шлюза
                elements.paymentFrame.src = 'about:blank';
                showToast('Платежный шлюз загружается...');
                
                // Имитация загрузки платежного шлюза
                setTimeout(() => {
                    showToast('Платежный шлюз готов к оплате');
                    // В реальном приложении здесь будет iframe с платежной формой
                }, 1500);
            });

            // Проверка оплаты
            elements.checkPayment.addEventListener('click', () => {
                // В реальном приложении здесь будет запрос к серверу для проверки платежа
                // Для демонстрации имитируем успешную оплату
                gameState.paymentStatus = 'completed';
                localStorage.setItem('paymentStatus', gameState.paymentStatus);
                checkPaymentStatus();
            });

            // Подключение кошельков
            elements.metamaskConnect.addEventListener('click', async () => {
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
                        
                        elements.walletAddress.textContent =
                            `${address.substring(0, 6)}...${address.substring(address.length - 4)}`;
                        elements.walletNetwork.textContent = networkName;
                        elements.walletInfo.style.display = 'block';
                        
                        showToast(`${translations[gameState.currentLanguage]['walletConnected']}!\n${translations[gameState.currentLanguage]['network']}: ${networkName}`);
                    } catch (error) {
                        console.error(error);
                        showToast(translations[gameState.currentLanguage]['walletError']);
                    }
                } else {
                    showToast(translations[gameState.currentLanguage]['installMetamask']);
                    window.open('https://metamask.io/download.html', '_blank');
                }
            });

            // WalletConnect и другие кошельки (заглушки)
            elements.walletconnectConnect.addEventListener('click', () => {
                showToast(translations[gameState.currentLanguage]['walletConnectIntegration']);
            });

            elements.phantomConnect.addEventListener('click', () => {
                if (window.solana && window.solana.isPhantom) {
                    window.solana.connect()
                        .then(() => {
                            const address = window.solana.publicKey.toString();
                            elements.walletAddress.textContent =
                                `${address.substring(0, 6)}...${address.substring(address.length - 4)}`;
                            elements.walletNetwork.textContent = "Solana";
                            elements.walletInfo.style.display = 'block';
                            showToast(translations[gameState.currentLanguage]['phantomConnected']);
                        })
                        .catch(err => {
                            console.error(err);
                            showToast(translations[gameState.currentLanguage]['phantomError']);
                        });
                } else {
                    showToast(translations[gameState.currentLanguage]['installPhantom']);
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

            // Настройки
            elements.settingsBtn.addEventListener('click', () => {
                elements.settingsMenu.classList.toggle('active');
                // Закрываем меню языка при открытии/закрытии основного меню
                elements.languageMenu.classList.remove('active');
            });

            // Меню языка
            elements.languageBtn.addEventListener('click', (e) => {
                e.stopPropagation();
                elements.languageMenu.classList.toggle('active');
            });

            // Смена языка
            document.querySelectorAll('.language-option').forEach(option => {
                option.addEventListener('click', function() {
                    const lang = this.getAttribute('data-lang');
                    translatePage(lang);
                    elements.languageMenu.classList.remove('active');
                    showToast(`${translations[gameState.currentLanguage]['languageChanged']} ${this.textContent}`);
                });
            });

            // Политика конфиденциальности
            elements.privacyBtn.addEventListener('click', () => {
                elements.privacyModal.style.display = 'flex';
                elements.settingsMenu.classList.remove('active');
            });

            elements.closePrivacy.addEventListener('click', () => {
                elements.privacyModal.style.display = 'none';
            });

            // Поддержка
            elements.supportBtn.addEventListener('click', () => {
                window.open('https://t.me/vigorous_north', '_blank');
                elements.settingsMenu.classList.remove('active');
            });

            // Задание на подписку Telegram
            elements.telegramQuest.addEventListener('click', () => {
                if (!gameState.completedQuests.includes('telegram_quest')) {
                    window.open('https://t.me/+WTwhgx9lxdU4NDRi', '_blank');
                    // В реальном приложении здесь будет проверка подписки через Telegram бота
                    // Для демонстрации сразу зачисляем награду
                    completeQuest('telegram_quest', 500);
                    showToast(translations[gameState.currentLanguage]['telegramSubscribed']);
                }
            });

            // Сброс прогресса
            elements.resetProgress.addEventListener('click', resetProgress);
        }

        // Запуск игры
        document.addEventListener('DOMContentLoaded', () => {
            setupEventListeners();
            checkReferral();
            init();
        });
    </script>
</body>
</html>
