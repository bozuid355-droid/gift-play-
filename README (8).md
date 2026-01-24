<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Loot Play - Ultimate Edition</title>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* --- Core Variables --- */
        :root {
            --primary-purple: #6a1b9a;
            --dark-purple: #4a148c;
            --accent-gold: #ffc107;
            --bg-light: #f8f9fa;
            --white: #ffffff;
            --text-dark: #333;
            --shadow: 0 8px 30px rgba(0,0,0,0.08);
            --kiwi-green: #8bc34a;
            --danger-red: #e74c3c;
            --success-green: #2ecc71;
        }

        body { font-family: 'Tajawal', sans-serif; background-color: var(--bg-light); color: var(--text-dark); margin: 0; padding-bottom: 85px; direction: rtl; overflow-x: hidden; }

        /* --- Sidebar & UI --- */
        .sidebar-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 3000; display: none; }
        .sidebar { position: fixed; top: 0; right: -280px; bottom: 0; width: 280px; background: white; z-index: 3001; transition: 0.3s ease; display: flex; flex-direction: column; box-shadow: var(--shadow); }
        .sidebar.active { right: 0; }
        .sidebar-header { background: linear-gradient(135deg, var(--primary-purple), var(--dark-purple)); color: white; padding: 30px 20px; text-align: right; }
        .header-top-row { display: flex; align-items: center; gap: 15px; margin-bottom: 20px; }
        .avatar-box { width: 55px; height: 55px; background: rgba(255,255,255,0.2); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 24px; font-weight: bold; border: 2px solid rgba(255,255,255,0.3); }
        .header-buttons { display: flex; gap: 10px; }
        .profile-btn { background: rgba(255,255,255,0.15); border: none; color: white; padding: 8px 20px; border-radius: 8px; font-size: 14px; flex: 1; cursor: pointer; }
        .settings-btn { background: rgba(255,255,255,0.15); border: none; color: white; width: 35px; height: 35px; border-radius: 8px; display: flex; align-items: center; justify-content: center; cursor: pointer; }
        .sidebar-menu { padding: 10px 0; flex: 1; overflow-y: auto; }
        .menu-link { display: flex; align-items: center; gap: 15px; padding: 15px 20px; color: #444; text-decoration: none; border-bottom: 1px solid #f1f1f1; }
        .menu-link i { width: 25px; color: var(--primary-purple); }

        /* --- Authentication --- */
        #authScreen { position: fixed; inset: 0; background: linear-gradient(135deg, var(--primary-purple), var(--dark-purple)); display: flex; align-items: center; justify-content: center; z-index: 4000; }
        .auth-card { background: white; width: 88%; max-width: 350px; padding: 40px 30px; border-radius: 25px; text-align: center; box-shadow: 0 15px 35px rgba(0,0,0,0.2); }
        .auth-card h2 { color: var(--primary-purple); margin-bottom: 30px; font-weight: 900; font-size: 28px; }
        .auth-card input { width: 100%; padding: 15px; margin-bottom: 15px; border: 1.5px solid #eee; border-radius: 12px; box-sizing: border-box; font-family: 'Tajawal'; transition: 0.3s; background: #fafafa; }
        .btn-auth { background: linear-gradient(135deg, var(--primary-purple), var(--dark-purple)); color: white; border: none; width: 100%; padding: 15px; border-radius: 12px; font-weight: 900; font-size: 16px; cursor: pointer; box-shadow: 0 5px 15px rgba(106, 27, 154, 0.3); }

        /* --- Header & Stats --- */
        .main-header { background-color: var(--primary-purple); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; color: white; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        .header-stats { display: flex; gap: 12px; }
        .stat-item { background: rgba(255,255,255,0.2); padding: 6px 12px; border-radius: 20px; font-weight: bold; font-size: 14px; display: flex; align-items: center; gap: 6px; }

        /* --- Tabs & Content --- */
        .tab-section { display: none; padding: 20px; animation: fadeIn 0.4s ease; }
        .active-tab { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .tasks-placeholder { text-align: center; padding: 50px 20px; color: #aaa; }
        .tasks-placeholder i { font-size: 50px; margin-bottom: 15px; color: #ddd; }

        /* --- Leaderboard Styles --- */
        .podium-container { display: flex; justify-content: center; align-items: flex-end; gap: 10px; margin-bottom: 30px; padding-top: 40px; }
        .podium-item { background: white; border-radius: 20px; padding: 15px 10px; text-align: center; position: relative; box-shadow: var(--shadow); flex: 1; }
        .podium-item.rank-1 { height: 160px; border: 2px solid var(--accent-gold); z-index: 2; }
        .podium-item.rank-2 { height: 130px; }
        .podium-item.rank-3 { height: 110px; }
        .podium-avatar { width: 45px; height: 45px; border-radius: 50%; background: #eee; margin: 0 auto 10px; display: flex; align-items: center; justify-content: center; font-weight: bold; font-size: 18px; border: 3px solid white; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        .rank-1 .podium-avatar { background: var(--accent-gold); color: white; width: 60px; height: 60px; margin-top: -35px; font-size: 24px; }
        .rank-2 .podium-avatar { background: #C0C0C0; color: white; }
        .rank-3 .podium-avatar { background: #CD7F32; color: white; }
        .rank-item { display: flex; align-items: center; background: white; padding: 15px; border-radius: 15px; margin-bottom: 12px; box-shadow: var(--shadow); }
        .rank-badge { width: 28px; height: 28px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 12px; font-weight: 900; margin-left: 15px; background: #eee; }
        .rank-item.is-me { border: 2px solid var(--primary-purple); background: #f3e5f5; }

        /* --- Store & Tasks Items --- */
        .store-item { background: white; border-radius: 18px; padding: 15px; margin-bottom: 15px; display: flex; align-items: center; gap: 15px; box-shadow: var(--shadow); }
        .price-tag { background: var(--primary-purple); color: white; padding: 8px 12px; border-radius: 10px; font-weight: bold; font-size: 13px; }
        .item-image { width: 50px; height: 50px; object-fit: cover; border-radius: 10px; border: 1px solid #eee; }

        /* --- KiwiWall Card --- */
        .kiwi-card {
            background: linear-gradient(135deg, var(--kiwi-green), #689f38);
            border-radius: 18px; padding: 20px; margin-bottom: 15px;
            color: white; display: flex; align-items: center; gap: 15px;
            box-shadow: 0 5px 15px rgba(139, 195, 74, 0.3); cursor: pointer;
            position: relative; overflow: hidden;
        }
        .kiwi-card::after {
            content: ''; position: absolute; top: -50%; left: -50%; width: 200%; height: 200%;
            background: rgba(255,255,255,0.1); transform: rotate(45deg); pointer-events: none;
        }

        /* --- Navigation --- */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 12px 0; border-top: 1px solid #eee; box-shadow: 0 -5px 20px rgba(0,0,0,0.05); z-index: 1000; }
        .nav-link { text-align: center; color: #aaa; font-size: 10px; cursor: pointer; flex: 1; transition: 0.3s; }
        .nav-link.active { color: var(--primary-purple); font-weight: bold; transform: translateY(-3px); }
        .nav-link i { display: block; font-size: 22px; margin-bottom: 4px; }

        /* --- Modern Wheel Styles --- */
        #wheelModal { position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 8000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(8px); }
        .wheel-content { background: white; width: 92%; max-width: 380px; border-radius: 30px; padding: 30px 20px; text-align: center; position: relative; box-shadow: 0 25px 60px rgba(0,0,0,0.5); animation: zoomIn 0.3s cubic-bezier(0.18, 0.89, 0.32, 1.28); border: 4px solid var(--accent-gold); }
        
        .wheel-container { 
            position: relative; width: 300px; height: 300px; margin: 20px auto; border-radius: 50%; padding: 10px;
            background: linear-gradient(135deg, #FFD700, #FDB931); box-shadow: 0 10px 30px rgba(0,0,0,0.3), inset 0 5px 15px rgba(255,255,255,0.5);
        }
        #wheelCanvas { width: 100%; height: 100%; transition: transform 3s cubic-bezier(0.1, 0.7, 0.1, 1); filter: drop-shadow(0 5px 10px rgba(0,0,0,0.2)); border-radius: 50%; }
        .wheel-pointer { position: absolute; top: -15px; left: 50%; transform: translateX(-50%); width: 40px; height: 50px; z-index: 20; background-image: url('https://cdn-icons-png.flaticon.com/512/8066/8066223.png'); background-size: contain; background-repeat: no-repeat; filter: drop-shadow(0 2px 2px rgba(0,0,0,0.3)); }
        .wheel-pointer::after { content: ''; display: none; border-left: 20px solid transparent; border-right: 20px solid transparent; border-top: 40px solid #e74c3c; }
        .spin-btn { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 75px; height: 75px; background: radial-gradient(circle at 30% 30%, #ffffff, #e0e0e0); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 900; color: var(--primary-purple); font-size: 18px; border: 4px solid var(--accent-gold); cursor: pointer; box-shadow: 0 5px 15px rgba(0,0,0,0.2); z-index: 15; text-transform: uppercase; letter-spacing: 1px; }
        .spin-btn:active { transform: translate(-50%, -50%) scale(0.95); }
        .close-wheel { position: absolute; top: 15px; right: 15px; width: 35px; height: 35px; background: #f1f1f1; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; font-weight: bold; color: #555; transition: 0.2s; }
        .close-wheel:hover { background: #ffcdd2; color: #c62828; }
        
        /* --- Modern Toast Notifications --- */
        #toastContainer { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); z-index: 9999; display: flex; flex-direction: column; gap: 10px; width: 90%; max-width: 400px; pointer-events: none; }
        .toast { 
            background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); 
            padding: 16px 20px; border-radius: 16px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.15); 
            display: flex; align-items: center; gap: 15px; 
            animation: slideDown 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            pointer-events: auto; border: 1px solid rgba(255,255,255,0.5);
        }
        .toast-icon { font-size: 24px; }
        .toast-content { flex: 1; }
        .toast-title { font-weight: 900; font-size: 14px; margin-bottom: 2px; }
        .toast-msg { font-size: 12px; color: #666; line-height: 1.4; }
        .toast.success { border-right: 5px solid var(--success-green); }
        .toast.success .toast-icon { color: var(--success-green); }
        .toast.error { border-right: 5px solid var(--danger-red); }
        .toast.error .toast-icon { color: var(--danger-red); }
        .toast.info { border-right: 5px solid var(--primary-purple); }
        .toast.info .toast-icon { color: var(--primary-purple); }
        
        @keyframes slideDown { from { transform: translateY(-50px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        @keyframes fadeOut { to { opacity: 0; transform: scale(0.9); } }

        /* --- Modern Custom Alert/Confirm Modal --- */
        #customAlert { position: fixed; inset: 0; background: rgba(0,0,0,0.7); z-index: 9999; display: none; align-items: center; justify-content: center; backdrop-filter: blur(4px); }
        .custom-alert-box { 
            background: #222; width: 85%; max-width: 320px; padding: 25px; 
            border-radius: 12px; text-align: center; box-shadow: 0 15px 40px rgba(0,0,0,0.5); 
            animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); color: white; border: 1px solid #333;
        }
        .custom-alert-title { font-size: 18px; font-weight: 900; color: var(--accent-gold); margin-bottom: 10px; }
        .custom-alert-msg { font-size: 14px; color: #ddd; margin-bottom: 25px; line-height: 1.6; }
        .custom-alert-btns { display: flex; gap: 10px; justify-content: center; }
        .c-btn { flex: 1; padding: 12px; border-radius: 8px; font-weight: bold; border: none; cursor: pointer; font-size: 14px; transition: 0.2s; }
        .c-btn:active { transform: scale(0.95); }
        .c-btn-ok { background: var(--primary-purple); color: white; }
        .c-btn-cancel { background: #444; color: #ccc; }

        .code-box { 
            background: #f1f8e9; border: 2px dashed #4caf50; padding: 12px; 
            border-radius: 12px; margin-top: 10px; text-align: center; 
            font-family: monospace; font-weight: bold; color: #2e7d32; cursor: pointer;
        }
        
        .status-badge-history { padding: 4px 10px; border-radius: 20px; font-size: 11px; font-weight: bold; display: inline-block; }
        .status-approved { background: #e8f5e9; color: #2ecc71; }
        .status-rejected { background: #ffebee; color: #e74c3c; }
        .status-pending { background: #fff8e1; color: #f1c40f; }
        
        @keyframes zoomIn { from {transform: scale(0.8); opacity: 0;} to {transform: scale(1); opacity: 1;} }
        @keyframes popIn { from { transform: scale(0.8) translateY(20px); opacity: 0; } to { transform: scale(1) translateY(0); opacity: 1; } }

        /* --- Ad Timer Styles (New) --- */
        #adTimerOverlay { position: fixed; inset: 0; background: rgba(0,0,0,0.9); z-index: 10000; display: none; flex-direction: column; align-items: center; justify-content: center; backdrop-filter: blur(5px); text-align: center; color: white; }
        .timer-circle { width: 80px; height: 80px; border-radius: 50%; border: 5px solid var(--accent-gold); display: flex; align-items: center; justify-content: center; font-size: 30px; font-weight: 900; margin-bottom: 20px; box-shadow: 0 0 20px rgba(255, 193, 7, 0.4); animation: pulse 1s infinite; }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.05); } 100% { transform: scale(1); } }
    </style>
</head>
<body>

    <div id="adTimerOverlay">
        <div class="timer-circle" id="adCountDown">15</div>
        <h3 style="margin:0">جاري التحقق من العرض...</h3>
        <p style="opacity:0.7; font-size:12px;">يرجى عدم إغلاق صفحة الإعلان حتى ينتهي العد</p>
    </div>

    <div id="toastContainer"></div> <div class="sidebar-overlay" id="sideOverlay" onclick="closeSidebar()"></div>
    <div class="sidebar" id="sideMenu">
        <div class="sidebar-header">
            <div class="header-top-row">
                <div class="avatar-box" id="avatarLetter">-</div>
                <div class="user-info-side">
                    <h4 id="sideName">اسم المستخدم</h4>
                    <p id="sideID">ID: 000000</p>
                </div>
            </div>
            <div class="header-buttons">
                <button class="profile-btn">الملف الشخصي</button>
                <button class="settings-btn" onclick="logout()"><i class="fas fa-sign-out-alt"></i></button>
            </div>
        </div>
        <div class="sidebar-menu">
            <a href="javascript:void(0)" class="menu-link" onclick="switchTab('earnTab'); closeSidebar();"> <i class="fas fa-home"></i> <span>الرئيسية</span> </a>
            <a href="javascript:void(0)" class="menu-link" onclick="switchTab('storeTab'); closeSidebar();"> <i class="fas fa-store"></i> <span>المتجر</span> </a>
        </div>
        <div class="sidebar-footer">Loot Play v2.0</div>
    </div>

    <div id="customAlert">
        <div class="custom-alert-box">
            <div class="custom-alert-title" id="alertTitle">تنبيه</div>
            <div class="custom-alert-msg" id="alertMsg">نص الرسالة هنا</div>
            <div class="custom-alert-btns" id="alertBtns"></div>
        </div>
    </div>

    <div id="authScreen">
        <div class="auth-card">
            <h2>Loot Play</h2>
            <div id="loginBox">
                <input type="email" id="logEmail" placeholder="البريد الإلكتروني">
                <input type="password" id="logPass" placeholder="كلمة المرور">
                <button class="btn-auth" onclick="login()">تسجيل الدخول</button>
                <p onclick="toggleAuth(true)" style="font-size: 13px; color: #888; cursor: pointer; margin-top: 20px;">ليس لديك حساب؟ <span style="color:var(--primary-purple); font-weight:bold;">سجل الآن</span></p>
            </div>
            <div id="signBox" style="display:none;">
                <input type="text" id="regName" placeholder="الاسم الكامل">
                <input type="email" id="regEmail" placeholder="البريد الإلكتروني">
                <input type="password" id="regPass" placeholder="كلمة المرور">
                <button class="btn-auth" onclick="signup()">إنشاء الحساب</button>
                <p onclick="toggleAuth(false)" style="font-size: 13px; color: #888; cursor: pointer; margin-top: 20px;">لديك حساب؟ <span style="color:var(--primary-purple); font-weight:bold;">دخول</span></p>
            </div>
        </div>
    </div>

    <div id="wheelModal">
        <div class="wheel-content">
            <div class="close-wheel" onclick="closeWheel()">×</div>
            <h3 style="margin-top:0; color:var(--primary-purple)">عجلة الحظ الملكية</h3>
            <p id="spinsRemaining" style="background:#fff3cd; display:inline-block; padding:5px 15px; border-radius:20px; font-size:12px; font-weight:bold; color:#856404;">المحاولات: 15/15</p>
            <div class="wheel-container">
                <div class="wheel-pointer"></div>
                <div class="spin-btn" onclick="spinWheel()">SPIN</div>
                <canvas id="wheelCanvas" width="300" height="300"></canvas>
            </div>
            <p style="color:#888; font-size:13px; margin-top:10px;">تكلفة اللفة: 25 عملة 🪙</p>
        </div>
    </div>

    <div id="appContainer" style="display: none;">
        <header class="main-header">
            <div class="header-stats">
                <div class="stat-item"><img src="https://cdn-icons-png.flaticon.com/512/272/272525.png" width="18"> <span id="balanceDisplay">0</span></div>
            </div>
            <div style="font-weight: 900; font-size: 20px;">LOOT PLAY</div>
            <i class="fas fa-bars" style="font-size: 22px; cursor: pointer;" onclick="openSidebar()"></i>
        </header>

        <section id="earnTab" class="tab-section active-tab">
            <div class="quick-actions" style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 20px;">
                <div class="action-card" onclick="collectPiggy()" style="background: white; padding: 20px; border-radius: 20px; text-align: center; box-shadow: var(--shadow);">
                    <img src="https://cdn-icons-png.flaticon.com/512/2830/2830284.png" width="50" alt="piggy">
                    <p style="margin: 10px 0 5px; font-weight: bold;">الحصالة</p>
                    <div id="piggyTimer" style="font-size:12px; color: #888;">--:--:--</div>
                </div>
                <div class="action-card" onclick="switchTab('leaderboardTab')" style="background: white; padding: 20px; border-radius: 20px; text-align: center; box-shadow: var(--shadow);">
                    <img src="https://cdn-icons-png.flaticon.com/512/3112/3112946.png" width="50" alt="trophy">
                    <p style="margin: 10px 0 5px; font-weight: bold;">المتصدرين</p>
                    <div style="font-size: 10px; color: #666;">جوائز كبرى</div>
                </div>
            </div>
            
            <div class="menu-list" style="background:white; border-radius:20px; overflow:hidden; box-shadow: var(--shadow); margin-bottom: 20px;">
                <div class="menu-item" onclick="switchTab('realTasksTab')" style="display:flex; align-items:center; padding:18px; border-bottom:1px solid #eee; gap:15px;">
                    <i class="fas fa-tasks" style="color:var(--primary-purple); font-size:20px;"></i>
                    <div style="flex:1;"><h4>مركز المهام</h4><p style="font-size:11px; color:#999;">أكمل المهام لربح المزيد</p></div>
                    <i class="fas fa-chevron-left" style="font-size: 12px; opacity: 0.2;"></i>
                </div>
            </div>

            <div class="menu-list" style="background:white; border-radius:20px; overflow:hidden; box-shadow: var(--shadow);">
                <div class="menu-item" onclick="openWheel()" style="display:flex; align-items:center; padding:18px; gap:15px; cursor:pointer; background: linear-gradient(135deg, #fff, #f8f0ff);">
                    <i class="fas fa-dharmachakra" style="color:var(--accent-gold); font-size:24px;"></i>
                    <div style="flex:1;"><h4>عجلة الحظ</h4><p style="font-size:11px; color:#999;">جرب حظك واربح!</p></div>
                    <i class="fas fa-chevron-left" style="font-size: 12px; opacity: 0.2;"></i>
                </div>
            </div>
        </section>

        <section id="realTasksTab" class="tab-section">
            <div class="comp-header" style="background: linear-gradient(135deg, #6a1b9a, #8e24aa); color: white; padding: 25px; border-radius: 20px; text-align: center; margin-bottom: 25px;">
                <h4>قائمة المهام</h4>
                <p style="font-size: 12px; opacity: 0.8;">شركات العروض والمهام اليومية</p>
            </div>
            
            <div class="kiwi-card" onclick="openKiwiWall()">
                <i class="fas fa-gamepad fa-3x" style="color:white; opacity:0.8"></i>
                <div style="flex:1;">
                    <h4 style="margin:0; font-size:16px;">لعبة النهب (KiwiWall)</h4>
                    <p style="margin:5px 0 0; font-size:12px; opacity:0.8;">عروض ضخمة ونقاط عالية</p>
                </div>
                <button style="background:white; color:var(--kiwi-green); padding:8px 20px; border-radius:20px; font-weight:900; border:none;">ابدأ</button>
            </div>

            <div id="officialTasksList" class="tasks-placeholder"><i class="fas fa-clipboard-check"></i><p>جاري التحميل...</p></div>
        </section>

        <section id="leaderboardTab" class="tab-section">
            <div class="comp-header" style="background: linear-gradient(135deg, #6a1b9a, #8e24aa); color: white; padding: 25px; border-radius: 20px; text-align: center; margin-bottom: 25px;">
                <h4>المتصدرين</h4>
                <div id="compCountdown" style="font-size: 22px; font-weight: 900; color: var(--accent-gold); margin-top: 10px;">00:00:00</div>
            </div>
            <div id="leaderboardList"></div>
        </section>

        <section id="storeTab" class="tab-section"><h3 style="margin-bottom:20px">سوق المكافآت</h3><div id="storeItemsList"></div></section>

        <section id="tasksTab" class="tab-section">
            <div style="background:white; padding: 30px; border-radius: 20px; text-align: center; box-shadow: var(--shadow);">
                <p>كود الإحالة الخاص بك</p>
                <div id="myInviteCode" style="font-size: 26px; font-weight: 900; color: var(--primary-purple); margin: 15px 0;">------</div>
                <input type="text" id="friendCodeInput" placeholder="أدخل كود صديقك..." style="text-align: center; border: 2px solid #eee; width: 100%; padding: 12px; border-radius: 12px;">
                <button class="btn-auth" style="margin-top:15px" onclick="applyReferral()">تفعيل المكافأة 🎁</button>
                <hr style="margin:20px 0; border:none; border-top:1px dashed #eee;">
                <p>لديك كود كوبون؟</p>
                <input type="text" id="couponInput" placeholder="أدخل الكوبون هنا" style="text-align: center; border: 2px solid #eee; width: 100%; padding: 12px; border-radius: 12px;">
                <button class="btn-auth" style="margin-top:15px; background:var(--accent-gold); color:#333;" onclick="redeemCoupon()">استبدال الكوبون 🎟️</button>
            </div>
        </section>

        <section id="historyTab" class="tab-section">
            <h3 style="margin-bottom:20px">طلباتي</h3>
            <div id="historyListContainer"></div>
        </section>

        <nav class="bottom-nav">
            <div class="nav-link active" id="earnNavBtn" onclick="switchTab('earnTab', this)"><i class="fas fa-home"></i>الرئيسية</div>
            <div class="nav-link" id="storeNavBtn" onclick="switchTab('storeTab', this)"><i class="fas fa-shopping-cart"></i>المتجر</div>
            <div class="nav-link" id="tasksNavBtn" onclick="switchTab('tasksTab', this)"><i class="fas fa-user-plus"></i>دعوة</div>
            <div class="nav-link" id="historyNavBtn" onclick="switchTab('historyTab', this)"><i class="fas fa-history"></i>طلباتي</div>
        </nav>
    </div>

    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-database-compat.js"></script>

    <script>
        const firebaseConfig = {
            apiKey: "AIzaSyAWK5rma-FFx8lw9zXbAMHL6",
            authDomain: "loot-play-35dc4.firebaseapp.com",
            projectId: "loot-play-35dc4",
            storageBucket: "loot-play-35dc4.firebasestorage.app",
            messagingSenderId: "840591336390",
            appId: "1:840591336390:web:c39a36a32b4",
            measurementId: "G-7BENV44PQZ",
            databaseURL: "https://loot-play-35dc4-default-rtdb.firebaseio.com"
        };

        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();
        
        let user = null;
        let mainTimer;
        let currentRotation = 0;
        let sessionStartTime = Date.now();

        // --- Modern Toast System ---
        function showToast(msg, type = 'info') {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            toast.className = `toast ${type}`;
            
            let icon = type === 'success' ? 'fa-check-circle' : (type === 'error' ? 'fa-times-circle' : 'fa-bell');
            let title = type === 'success' ? 'نجاح' : (type === 'error' ? 'تنبيه' : 'إشعار');

            toast.innerHTML = `
                <i class="fas ${icon} toast-icon"></i>
                <div class="toast-content">
                    <div class="toast-title">${title}</div>
                    <div class="toast-msg">${msg}</div>
                </div>
            `;
            
            container.appendChild(toast);
            
            // Sound/Vibrate
            if(navigator.vibrate) navigator.vibrate(100);

            // Remove after 4 seconds
            setTimeout(() => {
                toast.style.animation = 'fadeOut 0.4s forwards';
                setTimeout(() => toast.remove(), 400);
            }, 4000);
        }

        // --- Custom Alerts (Modal) ---
        function showMessage(msg, type = 'alert', callback = null) {
            const modal = document.getElementById('customAlert');
            const title = document.getElementById('alertTitle');
            const content = document.getElementById('alertMsg');
            const btnContainer = document.getElementById('alertBtns');
            
            title.innerText = type === 'confirm' ? 'تأكيد' : 'تنبيه';
            content.innerText = msg;
            modal.style.display = 'flex';
            
            btnContainer.innerHTML = '';
            
            const okBtn = document.createElement('button');
            okBtn.className = 'c-btn c-btn-ok';
            okBtn.innerText = type === 'confirm' ? 'نعم' : 'موافق';
            okBtn.onclick = () => { modal.style.display = 'none'; if(callback) callback(true); };
            btnContainer.appendChild(okBtn);

            if(type === 'confirm') {
                const cancelBtn = document.createElement('button');
                cancelBtn.className = 'c-btn c-btn-cancel';
                cancelBtn.innerText = 'إلغاء';
                cancelBtn.onclick = () => { modal.style.display = 'none'; if(callback) callback(false); };
                btnContainer.appendChild(cancelBtn);
            }
        }

        // --- KiwiWall Logic ---
        function openKiwiWall() {
            if (!user) return showMessage("يرجى تسجيل الدخول!");
            const wallId = "2b3p3in8bxhfwdlaawvanauzhflcywy6";
            const userId = user.firebaseKey; 
            const url = `https://www.kiwiwall.com/wall/${wallId}/${userId}`;
            window.open(url, '_blank');
        }

        // --- Wheel Logic ---
        let wheelCanvas, wheelCtx;
        let isSpinning = false;
        const segments = ["10", "20", "50", "0", "100", "5", "25", "0"];
        
        const segmentColors = [
            {start: "#FF5733", end: "#C70039"}, {start: "#33FF57", end: "#28B463"},
            {start: "#3357FF", end: "#2E86C1"}, {start: "#F1C40F", end: "#D4AC0D"},
            {start: "#9B59B6", end: "#884EA0"}, {start: "#E67E22", end: "#BA4A00"},
            {start: "#1ABC9C", end: "#138D75"}, {start: "#34495E", end: "#2C3E50"}
        ];

        function openWheel() { document.getElementById('wheelModal').style.display = 'flex'; initWheel(); }
        function closeWheel() { if(!isSpinning) document.getElementById('wheelModal').style.display = 'none'; }
        
        function initWheel() { 
            wheelCanvas = document.getElementById('wheelCanvas'); 
            if (wheelCanvas.getContext) { 
                wheelCtx = wheelCanvas.getContext('2d'); 
                drawWheel(); 
            } 
        }
        
        function drawWheel() { 
            const arc = Math.PI * 2 / segments.length; 
            const radius = wheelCanvas.width / 2; 
            const center = wheelCanvas.width / 2;
            wheelCtx.clearRect(0, 0, wheelCanvas.width, wheelCanvas.height); 
            for (let i = 0; i < segments.length; i++) { 
                const angle = i * arc; 
                wheelCtx.beginPath(); 
                let grd = wheelCtx.createRadialGradient(center, center, 10, center, center, radius);
                grd.addColorStop(0, segmentColors[i].start); grd.addColorStop(1, segmentColors[i].end);
                wheelCtx.fillStyle = grd; 
                wheelCtx.moveTo(center, center); wheelCtx.arc(center, center, radius, angle, angle + arc); wheelCtx.lineTo(center, center); wheelCtx.fill(); 
                wheelCtx.strokeStyle = "#fff"; wheelCtx.lineWidth = 2; wheelCtx.stroke();
                wheelCtx.save(); wheelCtx.translate(center, center); wheelCtx.rotate(angle + arc / 2); 
                wheelCtx.textAlign = "right"; wheelCtx.fillStyle = "#fff"; wheelCtx.font = "900 22px 'Tajawal'"; 
                wheelCtx.shadowColor="rgba(0,0,0,0.5)"; wheelCtx.shadowBlur=4;
                wheelCtx.fillText(segments[i], radius - 20, 8); wheelCtx.restore(); 
            } 
        }
        
        function spinWheel() { 
            if (isSpinning) return; 
            const cost = 25; 
            if (user.balance < cost) return showMessage("رصيدك لا يكفي! تحتاج 25 عملة."); 
            const today = new Date().toDateString(); 
            if (user.spinDate === today && user.spinCount >= 15) return showMessage("أنهيت 15 محاولة اليوم!"); 
            isSpinning = true; 
            const randomIndex = Math.floor(Math.random() * segments.length);
            const prize = parseInt(segments[randomIndex]);
            const segmentArc = 360 / segments.length;
            const targetIndexAngle = 270 - (randomIndex * segmentArc) - (segmentArc / 2);
            const spinRoations = 15 * 360; 
            const currentVisualAngle = currentRotation % 360;
            let distance = targetIndexAngle - currentVisualAngle;
            while (distance < 0) distance += 360; 
            const totalSpin = spinRoations + distance;
            currentRotation += totalSpin; 
            wheelCanvas.style.transform = `rotate(${currentRotation}deg)`; 
            setTimeout(() => { 
                isSpinning = false; 
                const newCount = (user.spinDate === today) ? (user.spinCount || 0) + 1 : 1; 
                db.ref('users/' + user.firebaseKey).update({ balance: user.balance - cost + prize, spinCount: newCount, spinDate: today }).then(() => { 
                    showMessage(prize > 0 ? `🎉 مبروك! ربحت ${prize} عملة!` : "حظاً أوفر!"); 
                }); 
            }, 3000); 
        }

        // --- App Logic ---
        window.onload = () => { const savedEmail = localStorage.getItem('gp_active_session'); if (savedEmail) { db.ref('users').orderByChild('email').equalTo(savedEmail).once('value', (snapshot) => { if (snapshot.exists()) { const data = snapshot.val(); const key = Object.keys(data)[0]; user = data[key]; user.firebaseKey = key; loadApp(); } }); } };
        
        function applyReferral() { 
            if (user.hasUsedRef) return showMessage("لقد حصلت على مكافأة الإحالة مسبقاً!"); 
            const deviceCheck = localStorage.getItem('gp_ref_claimed'); 
            if (deviceCheck === 'true') return showMessage("⛔ خطأ: لا يمكنك تفعيل كود الإحالة أكثر من مرة من نفس الجهاز!"); 
            const code = document.getElementById('friendCodeInput').value.trim().toUpperCase(); 
            if (code === user.inviteCode) return showMessage("لا يمكنك استخدام الكود الخاص بك!"); 
            db.ref('users').orderByChild('inviteCode').equalTo(code).once('value', (snapshot) => { 
                if (snapshot.exists()) { db.ref('users/' + user.firebaseKey).update({ balance: user.balance + 500, hasUsedRef: true }); localStorage.setItem('gp_ref_claimed', 'true'); showMessage("✅ تم تفعيل الكود! ربحت 500 عملة."); } 
                else { showMessage("الكود غير صحيح!"); } 
            }); 
        }
        
        function redeemCoupon() {
            const coupon = document.getElementById('couponInput').value.trim();
            if(!coupon) return showMessage("الرجاء إدخال الكوبون");
            const usedCoupons = user.usedCoupons || {};
            if(usedCoupons[coupon]) return showMessage("لقد استخدمت هذا الكوبون مسبقاً!");
            db.ref('coupons').child(coupon).once('value', snap => {
                if(snap.exists()) {
                    const data = snap.val();
                    if(data.active) {
                        const newBalance = user.balance + data.value;
                        const updates = {};
                        updates['/users/' + user.firebaseKey + '/balance'] = newBalance;
                        updates['/users/' + user.firebaseKey + '/usedCoupons/' + coupon] = true;
                        db.ref().update(updates).then(() => { showMessage(`🎉 مبروك! حصلت على ${data.value} عملة`); });
                    } else { showMessage("الكوبون منتهي الصلاحية"); }
                } else { showMessage("الكوبون غير صحيح"); }
            });
        }

        function openSidebar() { document.getElementById('sideMenu').classList.add('active'); document.getElementById('sideOverlay').style.display = 'block'; }
        function closeSidebar() { document.getElementById('sideMenu').classList.remove('active'); document.getElementById('sideOverlay').style.display = 'none'; }
        function toggleAuth(showSign) { document.getElementById('loginBox').style.display = showSign ? 'none' : 'block'; document.getElementById('signBox').style.display = showSign ? 'block' : 'none'; }
        function login() { const email = document.getElementById('logEmail').value; const pass = document.getElementById('logPass').value; db.ref('users').orderByChild('email').equalTo(email).once('value', (snapshot) => { if (snapshot.exists()) { const data = snapshot.val(); const key = Object.keys(data)[0]; const found = data[key]; if (found.pass === pass) { if (found.banned) return showMessage("⛔ حسابك محظور!"); user = found; user.firebaseKey = key; localStorage.setItem('gp_active_session', user.email); loadApp(); } else { showMessage("كلمة المرور غير صحيحة!"); } } else { showMessage("الحساب غير موجود!"); } }); }
        function signup() { const name = document.getElementById('regName').value; const email = document.getElementById('regEmail').value; const pass = document.getElementById('regPass').value; if(!name || !email || !pass) return showMessage("يرجى ملء كافة الحقول"); db.ref('users').orderByChild('email').equalTo(email).once('value', (snapshot) => { if (snapshot.exists()) return showMessage("البريد الإلكتروني موجود مسبقاً!"); const inviteCode = Math.random().toString(36).substring(2, 8).toUpperCase(); const newUser = { name, email, pass, balance: 0, banned: false, inviteCode, hasUsedRef: false, lastPiggy: 0, id: Math.floor(100000 + Math.random() * 900000) }; db.ref('users').push(newUser).then(() => { showMessage("تم إنشاء الحساب بنجاح!"); toggleAuth(false); }); }); }
        
        function loadApp() {
            document.getElementById('authScreen').style.display = 'none';
            document.getElementById('appContainer').style.display = 'block';
            
            db.ref('users/' + user.firebaseKey).on('value', (snapshot) => { 
                if (snapshot.exists()) { 
                    user = snapshot.val(); 
                    user.firebaseKey = snapshot.key; 
                    if (user.banned === true) { localStorage.removeItem('gp_active_session'); alert("⛔ تم حظر حسابك من قبل الإدارة!"); location.reload(); return; }
                    updateUI(); 
                    const today = new Date().toDateString(); 
                    const rem = (user.spinDate === today) ? 15 - (user.spinCount || 0) : 15; 
                    document.getElementById('spinsRemaining').innerText = `المحاولات: ${rem}/15`; 
                } 
            });

            // 1. Listen for Admin Broadcasts
            db.ref('admin_notifications').limitToLast(1).on('child_added', (snapshot) => {
                const notif = snapshot.val();
                if (notif && notif.timestamp && notif.timestamp > sessionStartTime) {
                    showToast(notif.text, 'info');
                }
            });

            // 2. Listen for Order Status Changes (Approvals/Rejections)
            db.ref('orders').orderByChild('userEmail').equalTo(user.email).on('child_changed', (snapshot) => {
                const o = snapshot.val();
                if(o.status === 'approved') {
                    showToast(`✅ مبروك! تمت الموافقة على طلبك: ${o.prodName}`, 'success');
                } else if(o.status === 'rejected') {
                    showToast(`❌ تم رفض طلبك (${o.prodName}) واستعادة النقاط.`, 'error');
                }
            });

            startTickers(); renderStore(); renderLeaderboard(); renderTasks(); renderHistory();
        }

        function updateUI() { document.getElementById('balanceDisplay').innerText = user.balance.toLocaleString(); document.getElementById('myInviteCode').innerText = user.inviteCode; document.getElementById('sideName').innerText = user.name; document.getElementById('sideID').innerText = "ID: " + (user.id || '---'); document.getElementById('avatarLetter').innerText = user.name.charAt(0).toUpperCase(); }
        function switchTab(tabId, el) { document.querySelectorAll('.tab-section').forEach(s => s.classList.remove('active-tab')); document.querySelectorAll('.nav-link').forEach(n => n.classList.remove('active')); document.getElementById(tabId).classList.add('active-tab'); if (el) el.classList.add('active'); else { const btnId = tabId.replace('Tab', 'NavBtn'); if(document.getElementById(btnId)) document.getElementById(btnId).classList.add('active'); } }
        
        function startTickers() { 
            if(mainTimer) clearInterval(mainTimer); 
            const endCompetitionDate = Date.now() + (7 * 24 * 60 * 60 * 1000);
            mainTimer = setInterval(() => { 
                const remPiggy = (user.lastPiggy + 86400000) - Date.now(); 
                const piggyEl = document.getElementById('piggyTimer'); 
                if (remPiggy <= 0) { piggyEl.innerText = "جاهزة الآن!"; piggyEl.style.color = "green"; } else { const h = Math.floor(remPiggy / 3600000), m = Math.floor((remPiggy % 3600000) / 60000), s = Math.floor((remPiggy % 60000) / 1000); piggyEl.innerText = `${h}:${m}:${s}`; } 
                const remComp = endCompetitionDate - Date.now(); 
                const compEl = document.getElementById('compCountdown'); 
                if (compEl && remComp > 0) { const days = Math.floor(remComp / (1000 * 60 * 60 * 24)); const hours = Math.floor((remComp % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)); const mins = Math.floor((remComp % (1000 * 60 * 60)) / (1000 * 60)); compEl.innerText = `${days} يوم و ${hours}:${mins}`; } 
            }, 1000); 
        }
        function collectPiggy() { const now = Date.now(); if (now - user.lastPiggy < 86400000) return showMessage("الحصالة ما زالت تجمع النقاط!"); const reward = 200; db.ref('users/' + user.firebaseKey).update({ balance: user.balance + reward, lastPiggy: now }); showMessage(`مبروك! حصلت على ${reward} عملة.`); }
        
        function renderStore() { db.ref('products').on('value', (snapshot) => { const prods = snapshot.val() || {}; const list = document.getElementById('storeItemsList'); let html = ''; for (let key in prods) { let p = prods[key]; html += `<div class="store-item" onclick="buyProduct('${key}', '${p.name}', ${p.price}, '${p.img}')"><img src="${p.img || 'https://via.placeholder.com/50'}" width="45"><div style="flex:1"><h4>${p.name}</h4></div><div class="price-tag">${p.price} 🪙</div></div>`; } list.innerHTML = html || '<p>المتجر فارغ</p>'; }); }
        
        function buyProduct(id, name, price, img) { 
            if (user.balance < price) return showMessage("عذراً، رصيدك غير كافٍ!"); 
            showMessage(`هل أنت متأكد من شراء ${name}؟`, 'confirm', (yes) => {
                if (yes) {
                    const newBalance = user.balance - price; 
                    db.ref('users/' + user.firebaseKey).update({ balance: newBalance }); 
                    db.ref('orders').push({ id: Date.now(), userEmail: user.email, userName: user.name, prodName: name, price: price, img: img || '', status: 'pending', giftCode: null }); 
                    showMessage("تم استلام طلبك!"); 
                }
            });
        }
        
        function renderHistory() {
            db.ref('orders').orderByChild('userEmail').equalTo(user.email).on('value', (snapshot) => {
                const orders = snapshot.val() || {};
                const list = document.getElementById('historyListContainer');
                let html = '';
                Object.values(orders).reverse().forEach(o => {
                    let statusBadge = '';
                    if(o.status === 'approved') statusBadge = '<span class="status-badge-history status-approved">مقبول</span>';
                    else if(o.status === 'rejected') statusBadge = '<span class="status-badge-history status-rejected">مرفوض</span>';
                    else statusBadge = '<span class="status-badge-history status-pending">معلق</span>';

                    html += `<div class="store-item" style="display:block">
                        <div style="display:flex; justify-content:space-between; align-items:center;">
                            <h4>${o.prodName}</h4>
                            ${statusBadge}
                        </div>
                        ${o.giftCode ? `<div class="code-box" onclick="copyText('${o.giftCode}')">${o.giftCode} <i class="fas fa-copy" style="font-size:12px"></i></div>` : ''}
                    </div>`;
                });
                list.innerHTML = html || '<p>لا توجد طلبات</p>';
            });
        }

        function renderLeaderboard() { db.ref('users').orderByChild('balance').limitToLast(30).on('value', (snapshot) => { const users = []; snapshot.forEach(child => { users.push(child.val()); }); const sorted = users.reverse(); const top3 = sorted.slice(0, 3); const others = sorted.slice(3); let html = '<div class="podium-container">'; if(top3[1]) html += `<div class="podium-item rank-2"><div class="podium-avatar">${top3[1].name.charAt(0)}</div><div class="podium-name">${top3[1].name}</div><div class="podium-balance">${top3[1].balance}</div><div>2nd</div></div>`; if(top3[0]) html += `<div class="podium-item rank-1"><i class="fas fa-crown crown"></i><div class="podium-avatar">${top3[0].name.charAt(0)}</div><div class="podium-name">${top3[0].name}</div><div class="podium-balance">${top3[0].balance}</div><div>1st</div></div>`; if(top3[2]) html += `<div class="podium-item rank-3"><div class="podium-avatar">${top3[2].name.charAt(0)}</div><div class="podium-name">${top3[2].name}</div><div class="podium-balance">${top3[2].balance}</div><div>3rd</div></div>`; html += '</div>'; others.forEach((u, i) => { html += `<div class="rank-item ${u.email===user.email?'is-me':''}"><div class="rank-badge">${i+4}</div><div style="flex:1"><h5>${u.name}</h5></div><div>${u.balance} 🪙</div></div>`; }); document.getElementById('leaderboardList').innerHTML = html; }); }
        
        function copyText(text) {
            navigator.clipboard.writeText(text).then(() => {
                showMessage("تم النسخ بنجاح: " + text);
            });
        }

        function logout() { localStorage.removeItem('gp_active_session'); location.reload(); }

        // --- 1. Render Tasks with 10/10 Limit for ADS ONLY ---
        function renderTasks() { 
            db.ref('tasks').on('value', (snapshot) => { 
                const tasks = snapshot.val() || {}; 
                const list = document.getElementById('officialTasksList'); 
                if (!list) return;

                let html = ''; 
                const todayStr = new Date().toDateString();
                const maxDailyLimit = 10; // The 10/10 limit
                const progress = user.taskProgress || {};

                for (let key in tasks) { 
                    let t = tasks[key]; 
                    let duration = parseInt(t.timer) || 15; 
                    
                    // Fetch progress for this specific task today
                    let taskStats = progress[key] || { date: "", count: 0 };
                    let currentCount = (taskStats.date === todayStr) ? taskStats.count : 0;

                    // CHECK: Is this an Ad or a Regular Task?
                    const isAdTask = (t.type === 'ad' || t.title.includes("إعلان") || t.title.toLowerCase().includes("ad"));

                    // If it's an Ad and they hit the limit, don't show it
                    if (isAdTask && currentCount >= maxDailyLimit) continue; 

                    let iconHtml = t.img ? `<img src="${t.img}" class="item-image" alt="task">` : `<i class="fas fa-play-circle" style="color:var(--primary-purple); font-size:24px;"></i>`; 
                    
                    // UI for the Daily Limit Badge (Only for Ads)
                    let limitBadge = isAdTask ? `
                        <span style="font-size:10px; background:rgba(231, 76, 60, 0.1); color:#e74c3c; padding:2px 8px; border-radius:10px; font-weight:900; border: 1px solid #e74c3c;">
                            ${currentCount}/${maxDailyLimit} محاولات اليوم
                        </span>` : '';

                    html += `
                    <div class="store-item" onclick="startAdTask('${key}', '${t.link}', ${t.points}, ${duration}, ${isAdTask})">
                        ${iconHtml}
                        <div style="flex:1">
                            <h4 style="margin:0">${t.title}</h4>
                            <div style="display:flex; justify-content:space-between; align-items:center; margin-top:5px;">
                                <p style="font-size:11px; color:#888; margin:0;">شاهد ${duration} ثانية</p>
                                ${limitBadge}
                            </div>
                        </div>
                        <div class="price-tag" style="background:var(--kiwi-green)">+${t.points} 🪙</div>
                    </div>`; 
                } 
                list.innerHTML = html || '<p style="text-align:center; padding:20px; color:#aaa;">لا توجد مهام حالياً</p>'; 
            }); 
        }

        // --- 2. Handle Task Execution & Reward Tracking ---
        function startAdTask(taskId, link, reward, seconds, isAdTask) {
            if(!user) return showMessage("يرجى تسجيل الدخول أولاً!");

            const todayStr = new Date().toDateString();
            const maxLimit = 10;

            // Check limit only if it's an ad task
            if (isAdTask) {
                let taskStats = (user.taskProgress && user.taskProgress[taskId]) || { date: "", count: 0 };
                let currentCount = (taskStats.date === todayStr) ? taskStats.count : 0;
                if (currentCount >= maxLimit) return showMessage("لقد وصلت للحد الأقصى اليوم!");
            }

            // >> ✅ فتح الرابط في نافذة جديدة (متصفح خارجي) ✅
            window.open(link, '_blank');

            const overlay = document.getElementById('adTimerOverlay');
            const counter = document.getElementById('adCountDown');
            
            if (overlay && counter) {
                overlay.style.display = 'flex';
                let timeLeft = seconds;
                counter.innerText = timeLeft;

                const interval = setInterval(() => {
                    timeLeft--;
                    counter.innerText = timeLeft;

                    if (timeLeft <= 0) {
                        clearInterval(interval);
                        overlay.style.display = 'none';

                        const currentBalance = parseInt(user.balance) || 0;
                        const rewardAmount = parseInt(reward) || 0;
                        const newBalance = currentBalance + rewardAmount;
                        
                        let updates = {};
                        updates['/users/' + user.firebaseKey + '/balance'] = newBalance;

                        // Increment attempt count if it's an ad task
                        if (isAdTask) {
                            let taskStats = (user.taskProgress && user.taskProgress[taskId]) || { date: "", count: 0 };
                            let newCount = (taskStats.date === todayStr) ? (taskStats.count + 1) : 1;
                            updates['/users/' + user.firebaseKey + '/taskProgress/' + taskId] = {
                                date: todayStr,
                                count: newCount
                            };
                        }

                        db.ref().update(updates).then(() => {
                            // Local update is handled by the .on('value') listener in loadApp()
                            user.balance = newBalance;
                            if(document.getElementById('balanceDisplay')) {
                                document.getElementById('balanceDisplay').innerText = newBalance.toLocaleString();
                            }
                            showMessage(`🎉 أحسنت! حصلت على ${rewardAmount} عملة.`);
                            if(navigator.vibrate) navigator.vibrate([100, 50, 100]);
                        }).catch(err => {
                            showToast("حدث خطأ في الاتصال", "error");
                        });
                    }
                }, 1000);
            }
        }
    </script>
</body>
</html>
