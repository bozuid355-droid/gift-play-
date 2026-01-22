<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Loot Play - GiftPlay Pro</title>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-purple: #6a1b9a;
            --accent-gold: #ffc107;
            --bg-light: #f4f7f6;
            --white: #ffffff;
            --text-dark: #333;
        }

        body {
            font-family: 'Tajawal', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-dark);
            margin: 0;
            padding-bottom: 80px;
            direction: rtl;
            overflow-x: hidden;
        }

        /* --- Sidebar --- */
        .sidebar-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 3000; display: none; }
        .sidebar { position: fixed; top: 0; right: -280px; bottom: 0; width: 280px; background: white; z-index: 3001; transition: 0.3s ease; display: flex; flex-direction: column; box-shadow: -5px 0 15px rgba(0,0,0,0.1); }
        .sidebar.active { right: 0; }
        .sidebar-header { background: var(--primary-purple); color: white; padding: 30px 20px; position: relative; text-align: right; }
        .header-top-row { display: flex; align-items: center; justify-content: flex-start; gap: 15px; margin-bottom: 20px; }
        .avatar-box { width: 55px; height: 55px; background: #8e44ad; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 24px; font-weight: bold; border: 2px solid rgba(255,255,255,0.2); }
        .sidebar-menu { padding: 10px 0; flex: 1; overflow-y: auto; }
        .menu-link { display: flex; align-items: center; gap: 15px; padding: 15px 20px; color: #444; text-decoration: none; border-bottom: 1px solid #f9f9f9; transition: 0.2s; }
        .menu-link i { width: 25px; font-size: 18px; color: #555; text-align: center; }

        /* --- Auth Screen --- */
        #authScreen { position: fixed; inset: 0; background: var(--primary-purple); display: flex; align-items: center; justify-content: center; z-index: 4000; }
        .auth-card { background: white; width: 85%; padding: 30px; border-radius: 20px; text-align: center; }
        .auth-card h2 { color: var(--primary-purple); margin-bottom: 20px; }
        .auth-card input { width: 100%; padding: 12px; margin-bottom: 10px; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; font-family: 'Tajawal'; }
        .btn-auth { background: var(--primary-purple); color: white; border: none; width: 100%; padding: 12px; border-radius: 10px; font-weight: 900; cursor: pointer; }

        /* --- Header --- */
        .main-header { background-color: var(--primary-purple); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; color: white; position: sticky; top: 0; z-index: 1000; }
        .header-stats { display: flex; gap: 12px; }
        .stat-item { background: rgba(255,255,255,0.2); padding: 5px 10px; border-radius: 20px; font-weight: bold; font-size: 13px; display: flex; align-items: center; gap: 5px; }

        /* --- Tabs Sections --- */
        .tab-section { display: none; padding: 20px; animation: fadeIn 0.3s ease; }
        .active-tab { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* --- Components --- */
        .quick-actions { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 20px; }
        .action-card { background: white; padding: 15px; border-radius: 15px; text-align: center; box-shadow: 0 4px 6px rgba(0,0,0,0.05); cursor: pointer; }
        .action-card img { width: 50px; margin-bottom: 10px; }
        .comp-header { background: linear-gradient(135deg, #6a1b9a, #8e24aa); color: white; padding: 20px; border-radius: 15px; text-align: center; margin-bottom: 20px; }
        
        /* --- Tasks & Store Items --- */
        .store-item { background: white; border-radius: 15px; padding: 10px; margin-bottom: 12px; display: flex; align-items: center; gap: 15px; box-shadow: 0 2px 4px rgba(0,0,0,0.05); }
        .store-item .price-tag { background: var(--primary-purple); color: white; padding: 8px 15px; border-radius: 10px; min-width: 80px; text-align: center; font-weight: bold; border: none; cursor: pointer; }
        .store-item img { width: 50px; height: 50px; border-radius: 10px; object-fit: cover; }
        .store-item h4 { margin: 0; font-size: 14px; }

        /* --- Bottom Nav --- */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 10px 0; border-top: 1px solid #eee; box-shadow: 0 -2px 10px rgba(0,0,0,0.05); z-index: 1000; }
        .nav-link { text-align: center; color: #888; font-size: 11px; cursor: pointer; flex: 1; transition: 0.3s; }
        .nav-link.active { color: var(--primary-purple); font-weight: bold; transform: translateY(-3px); }
        .nav-link i { display: block; font-size: 20px; margin-bottom: 4px; }

        .code-box { background: #e8f5e9; border: 2px dashed #4caf50; padding: 10px; border-radius: 10px; margin-top: 10px; text-align: center; font-weight: bold; color: #2e7d32; }
    </style>
</head>
<body>

    <div class="sidebar-overlay" id="sideOverlay" onclick="closeSidebar()"></div>
    <div class="sidebar" id="sideMenu">
        <div class="sidebar-header">
            <div class="header-top-row">
                <div class="avatar-box" id="avatarLetter">B</div>
                <div class="user-info-side">
                    <h4 id="sideName">اسم المستخدم</h4>
                    <p id="sideID">ID: 0000000</p>
                </div>
            </div>
            <div class="header-buttons">
                <button class="profile-btn">ملف الشخصي</button>
                <button class="settings-btn" onclick="logout()"><i class="fas fa-sign-out-alt"></i></button>
            </div>
        </div>
        <div class="sidebar-menu">
            <a href="javascript:void(0)" class="menu-link" onclick="switchTab('earnTab'); closeSidebar();">
                <i class="fas fa-home"></i> <span>الرئيسية</span>
            </a>
            <a href="javascript:void(0)" class="menu-link" onclick="switchTab('tasksTabSection'); closeSidebar();">
                <i class="fas fa-check-circle"></i> <span>المهام</span>
            </a>
            <a href="javascript:void(0)" class="menu-link" onclick="switchTab('storeTab'); closeSidebar();">
                <i class="fas fa-store"></i> <span>المتجر</span>
            </a>
            <a href="javascript:void(0)" class="menu-link" onclick="switchTab('leaderboardTab'); closeSidebar();">
                <i class="fas fa-trophy"></i> <span>أول 100</span>
            </a>
        </div>
        <div class="sidebar-footer">Privacy Policy</div>
    </div>

    <div id="authScreen">
        <div class="auth-card">
            <h2>Loot Play</h2>
            <div id="loginBox">
                <input type="email" id="logEmail" placeholder="البريد الإلكتروني">
                <input type="password" id="logPass" placeholder="كلمة المرور">
                <button class="btn-auth" onclick="login()">تسجيل الدخول</button>
                <p onclick="toggleAuth(true)" style="font-size: 12px; color: #666; cursor: pointer; margin-top: 15px;">إنشاء حساب جديد</p>
            </div>
            <div id="signBox" style="display:none;">
                <input type="text" id="regName" placeholder="الاسم">
                <input type="email" id="regEmail" placeholder="البريد الإلكتروني">
                <input type="password" id="regPass" placeholder="كلمة المرور">
                <button class="btn-auth" onclick="signup()">إنشاء الحساب</button>
                <p onclick="toggleAuth(false)" style="font-size: 12px; color: #666; cursor: pointer; margin-top: 15px;">لديك حساب؟ سجل دخولك</p>
            </div>
        </div>
    </div>

    <div id="appContainer" style="display: none;">
        <header class="main-header">
            <div class="header-stats">
                <div class="stat-item">
                    <img src="https://cdn-icons-png.flaticon.com/512/272/272525.png" width="18">
                    <span id="balanceDisplay">0</span>
                </div>
            </div>
            <div style="font-weight: 900; font-size: 20px;">Loot Play</div>
            <i class="fas fa-bars" style="font-size: 22px; cursor: pointer;" onclick="openSidebar()"></i>
        </header>

        <section id="earnTab" class="tab-section active-tab">
            <div class="quick-actions">
                <div class="action-card" onclick="collectPiggy()">
                    <img src="https://cdn-icons-png.flaticon.com/512/2830/2830284.png">
                    <p style="margin: 5px 0; font-weight: bold;">الحصالة</p>
                    <div id="piggyTimer" style="background:#eee; border-radius:5px; padding:5px; font-size:12px;">جاهزة!</div>
                </div>
                <div class="action-card" onclick="switchTab('leaderboardTab')">
                    <img src="https://cdn-icons-png.flaticon.com/512/3112/3112946.png">
                    <p style="margin: 5px 0; font-weight: bold;">أول 100</p>
                    <div style="font-size: 10px; color: #666;">جائزة 10,000 نقطة</div>
                </div>
            </div>
            <div class="store-item" onclick="switchTab('tasksTabSection')" style="cursor:pointer">
                <i class="fas fa-tasks" style="font-size: 30px; color: var(--primary-purple); margin: 0 10px;"></i>
                <div style="flex:1"><h4>مركز المهام</h4><p style="margin:0; font-size:11px">نفذ المهام واربح النقاط فوراً</p></div>
                <i class="fas fa-chevron-left"></i>
            </div>
        </section>

        <section id="tasksTabSection" class="tab-section">
            <div class="comp-header">
                <h4>قائمة المهام</h4>
                <p style="font-size: 12px; opacity: 0.9;">أكمل المهام البسيطة لزيادة رصيدك</p>
            </div>
            <div id="officialTasksList">
                </div>
        </section>

        <section id="leaderboardTab" class="tab-section">
            <div class="comp-header">
                <h4>مسابقة المتصدرين</h4>
                <div id="compCountdown" style="font-size: 22px; font-weight: 900; color: var(--accent-gold);">00:00:00</div>
            </div>
            <div id="leaderboardList"></div>
        </section>

        <section id="storeTab" class="tab-section">
            <div id="storeItemsList"></div>
        </section>

        <section id="referralTab" class="tab-section">
            <div style="background:white; padding: 20px; border-radius: 15px; text-align: center;">
                <p>كود الدعوة الخاص بك:</p>
                <div id="myInviteCode" style="font-size: 24px; font-weight: 900; color: var(--primary-purple); margin-bottom: 20px;">------</div>
                <input type="text" id="friendCodeInput" placeholder="أدخل الكود هنا..." style="text-align: center; border: 1px solid #ddd; width: 100%; padding: 12px; border-radius: 10px;">
                <button class="btn-auth" style="margin-top:10px" onclick="applyReferral()">تفعيل الكود 🎁</button>
            </div>
        </section>

        <section id="historyTab" class="tab-section">
            <div id="historyListContainer"></div>
        </section>

        <nav class="bottom-nav">
            <div class="nav-link" id="historyNavBtn" onclick="switchTab('historyTab', this)"><i class="fas fa-shopping-bag"></i>مشترياتك</div>
            <div class="nav-link" id="tasksNavBtn" onclick="switchTab('tasksTabSection', this)"><i class="fas fa-check-circle"></i>المهام</div>
            <div class="nav-link" id="storeNavBtn" onclick="switchTab('storeTab', this)"><i class="fas fa-store"></i>المتجر</div>
            <div class="nav-link" id="referralNavBtn" onclick="switchTab('referralTab', this)"><i class="fas fa-plus-circle"></i>دعوة</div>
            <div class="nav-link active" id="earnNavBtn" onclick="switchTab('earnTab', this)"><i class="fas fa-coins"></i>اكسب</div>
        </nav>
    </div>

    <script>
        const DB = { 
            users: 'gp_users_pro', 
            prods: 'gp_prods_pro', 
            orders: 'gp_orders_pro', 
            tasks: 'gp_tasks_pro', // الربط مع الأدمن
            session: 'gp_active_session',
            compStart: 'gp_comp_start_time'
        };
        
        let user = null;
        let mainTimer;

        window.onload = () => {
            if (!localStorage.getItem(DB.compStart)) localStorage.setItem(DB.compStart, Date.now().toString());
            const savedEmail = localStorage.getItem(DB.session);
            if (savedEmail) {
                const users = JSON.parse(localStorage.getItem(DB.users)) || [];
                user = users.find(u => u.email === savedEmail);
                if (user) loadApp();
            }
        };

        function loadApp() {
            document.getElementById('authScreen').style.display = 'none';
            document.getElementById('appContainer').style.display = 'block';
            updateUI();
            startTickers();
        }

        function updateUI() {
            const users = JSON.parse(localStorage.getItem(DB.users)) || [];
            user = users.find(u => u.email === user.email);
            
            // التأكد من وجود سجل المهام
            if (!user.completedTasks) user.completedTasks = [];

            document.getElementById('balanceDisplay').innerText = user.balance.toLocaleString();
            document.getElementById('myInviteCode').innerText = user.inviteCode;
            document.getElementById('sideName').innerText = user.name;
            document.getElementById('sideID').innerText = "ID: " + (user.id || '---');
            document.getElementById('avatarLetter').innerText = user.name.charAt(0).toUpperCase();

            renderStore(); 
            renderHistory();
            renderLeaderboard();
            renderTasks(); // جلب المهام
        }

        // --- وظائف المهام ---
        function renderTasks() {
            const tasks = JSON.parse(localStorage.getItem(DB.tasks)) || [];
            const container = document.getElementById('officialTasksList');

            if (tasks.length === 0) {
                container.innerHTML = '<p style="text-align:center; padding:20px; color:#999;">لا توجد مهام متاحة حالياً</p>';
                return;
            }

            container.innerHTML = tasks.map(t => {
                const isDone = user.completedTasks.includes(t.id);
                return `
                    <div class="store-item" style="opacity: ${isDone ? '0.6' : '1'}">
                        <img src="${t.img || 'https://cdn-icons-png.flaticon.com/512/2098/2098402.png'}">
                        <div style="flex:1">
                            <h4>${t.title}</h4>
                            <p style="color:var(--primary-purple); font-weight:bold; margin:0">+${t.points} نقطة</p>
                        </div>
                        ${isDone ? '<span style="color:green; font-weight:bold">مكتملة ✅</span>' : 
                        `<button class="price-tag" onclick="doTask(${t.id}, '${t.link}', ${t.points})">تنفيذ</button>`}
                    </div>
                `;
            }).join('');
        }

        function doTask(id, link, points) {
            window.open(link, '_blank');
            if (!user.completedTasks.includes(id)) {
                user.completedTasks.push(id);
                user.balance += parseInt(points);
                saveUser();
                updateUI();
                alert(`أحسنت! حصلت على ${points} نقطة.`);
            }
        }

        // --- الوظائف الأساسية ---
        function login() {
            const email = document.getElementById('logEmail').value;
            const pass = document.getElementById('logPass').value;
            const users = JSON.parse(localStorage.getItem(DB.users)) || [];
            const found = users.find(u => u.email === email && u.pass === pass);
            if (found) {
                if (found.banned) return alert("⛔ محظور!");
                user = found; localStorage.setItem(DB.session, user.email); loadApp();
            } else { alert("خطأ في البيانات!"); }
        }

        function signup() {
            const name = document.getElementById('regName').value;
            const email = document.getElementById('regEmail').value;
            const pass = document.getElementById('regPass').value;
            if(!name || !email || !pass) return alert("اكمل البيانات");
            let users = JSON.parse(localStorage.getItem(DB.users)) || [];
            if(users.find(u => u.email === email)) return alert("البريد مستخدم!");

            const myRef = Math.random().toString(36).substring(2, 8).toUpperCase();
            users.push({ name, email, pass, balance: 0, banned: false, inviteCode: myRef, hasUsedRef: false, lastPiggy: 0, completedTasks: [], id: Math.floor(1000000 + Math.random() * 9000000) });
            localStorage.setItem(DB.users, JSON.stringify(users));
            alert("تم التسجيل!"); toggleAuth(false);
        }

        function switchTab(tabId, el) {
            document.querySelectorAll('.tab-section').forEach(s => s.classList.remove('active-tab'));
            document.querySelectorAll('.nav-link').forEach(n => n.classList.remove('active'));
            document.getElementById(tabId).classList.add('active-tab');
            if(el) el.classList.add('active');
            else {
                const btnId = tabId === 'tasksTabSection' ? 'tasksNavBtn' : (tabId.replace('Tab', 'NavBtn'));
                if(document.getElementById(btnId)) document.getElementById(btnId).classList.add('active');
            }
        }

        function startTickers() {
            if(mainTimer) clearInterval(mainTimer);
            mainTimer = setInterval(() => {
                const remPiggy = (user.lastPiggy + 86400000) - Date.now();
                const piggyEl = document.getElementById('piggyTimer');
                if (remPiggy <= 0) { piggyEl.innerText = "جاهزة!"; piggyEl.style.color = "green"; } 
                else {
                    const h = Math.floor(remPiggy / 3600000), m = Math.floor((remPiggy % 3600000) / 60000), s = Math.floor((remPiggy % 60000) / 1000);
                    piggyEl.innerText = `${h}:${m}:${s}`;
                }

                const startTime = parseInt(localStorage.getItem(DB.compStart));
                const remComp = (startTime + (30 * 86400000)) - Date.now();
                if (remComp > 0) {
                    const d = Math.floor(remComp / 86400000), h = Math.floor((remComp % 86400000) / 3600000), m = Math.floor((remComp % 3600000) / 60000), s = Math.floor((remComp % 60000) / 1000);
                    document.getElementById('compCountdown').innerText = `${d}d ${h}:${m}:${s}`;
                }
            }, 1000);
        }

        function collectPiggy() {
            const now = Date.now();
            if (now - user.lastPiggy < 86400000) return alert("الحصالة لم تمتلئ!");
            const reward = Math.floor(Math.random() * 900) + 100;
            user.balance += reward; user.lastPiggy = now;
            saveUser(); updateUI(); alert(`ربحت ${reward} عملة!`);
        }

        function saveUser() {
            let users = JSON.parse(localStorage.getItem(DB.users)) || [];
            const idx = users.findIndex(u => u.email === user.email);
            users[idx] = user; localStorage.setItem(DB.users, JSON.stringify(users));
        }

        function renderStore() {
            const prods = JSON.parse(localStorage.getItem(DB.prods)) || [];
            document.getElementById('storeItemsList').innerHTML = prods.map(p => `
                <div class="store-item" onclick="buyProduct(${p.id}, '${p.name}', ${p.price})">
                    <div class="price-tag">${p.price} 🪙</div>
                    <div style="flex:1"><h4>${p.name}</h4><p style="margin:0; font-size:11px">تسليم خلال 24 ساعة</p></div>
                    <img src="${p.img || 'https://via.placeholder.com/50'}">
                </div>
            `).join('');
        }

        function buyProduct(id, name, price) {
            if (user.balance < price) return alert("نقاطك لا تكفي!");
            if (confirm(`شراء ${name}?`)) {
                user.balance -= price; saveUser();
                let orders = JSON.parse(localStorage.getItem(DB.orders)) || [];
                orders.push({ id: Date.now(), userEmail: user.email, userName: user.name, prodName: name, status: 'pending', giftCode: null });
                localStorage.setItem(DB.orders, JSON.stringify(orders));
                updateUI(); alert("تم الطلب!");
            }
        }

        function renderLeaderboard() {
            const users = JSON.parse(localStorage.getItem(DB.users)) || [];
            const sorted = [...users].sort((a, b) => b.balance - a.balance).slice(0, 100);
            document.getElementById('leaderboardList').innerHTML = sorted.map((u, i) => `
                <div class="rank-item" style="display:flex; align-items:center; background:white; padding:10px; margin-bottom:5px; border-radius:10px;">
                    <b style="width:30px">${i+1}</b>
                    <div style="flex:1"><b>${u.name}</b></div>
                    <span style="color:var(--primary-purple)">${u.balance.toLocaleString()} 🪙</span>
                </div>
            `).join('');
        }

        function renderHistory() {
            const orders = JSON.parse(localStorage.getItem(DB.orders)) || [];
            const my = orders.filter(o => o.userEmail === user.email);
            document.getElementById('historyListContainer').innerHTML = my.reverse().map(o => `
                <div class="store-item" style="display:block">
                    <div style="display:flex; justify-content:space-between">
                        <h4>${o.prodName}</h4>
                        <span style="font-size:12px; color:${o.status==='approved'?'green':'orange'}">${o.status==='approved'?'تم ✅':'قيد المراجعة'}</span>
                    </div>
                    ${o.giftCode ? `<div class="code-box">${o.giftCode}</div>` : ''}
                </div>
            `).join('');
        }

        function logout() { localStorage.removeItem(DB.session); location.reload(); }
        function openSidebar() { document.getElementById('sideMenu').classList.add('active'); document.getElementById('sideOverlay').style.display = 'block'; }
        function closeSidebar() { document.getElementById('sideMenu').classList.remove('active'); document.getElementById('sideOverlay').style.display = 'none'; }
        function toggleAuth(showSign) { document.getElementById('loginBox').style.display = showSign ? 'none' : 'block'; document.getElementById('signBox').style.display = showSign ? 'block' : 'none'; }
        
        function applyReferral() {
            if (user.hasUsedRef) return alert("استخدمت كوداً!");
            const code = document.getElementById('friendCodeInput').value.trim().toUpperCase();
            let users = JSON.parse(localStorage.getItem(DB.users)) || [];
            if (users.find(u => u.inviteCode === code && code !== user.inviteCode)) {
                user.balance += 300; user.hasUsedRef = true; saveUser(); updateUI(); alert("ربحت 300 عملة!");
            } else { alert("كود خطأ!"); }
        }
    </script>
</body>
</html>
