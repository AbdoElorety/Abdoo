<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>DeepPlanner | تانية ثانوي - جدول + دروس متراكمة + DeepSeek + كاميرا</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Cairo', sans-serif; }
        body { background: #0a0c14; color: #edf2fb; overflow-x: hidden; }
        :root { --dark-card: #111520; --red-accent: #e63946; --blue-accent: #1e6091; --green-accent: #2a9d8f; --deepseek-teal: #0ea5e9; }
        .container { width: 100%; padding: 12px 16px; margin: 0 auto; }
        .login-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #0a0c14; z-index: 1000; display: flex; align-items: center; justify-content: center; }
        .login-card { background: #111520; border-radius: 48px; padding: 40px 30px; text-align: center; max-width: 400px; width: 90%; border: 1px solid #2a2f42; }
        .login-card i { font-size: 4rem; background: linear-gradient(135deg, #0ea5e9, #e63946); -webkit-background-clip: text; background-clip: text; color: transparent; }
        .google-login-btn { background: #1e6091; border: none; padding: 14px 28px; border-radius: 60px; color: white; font-weight: bold; font-size: 1.1rem; cursor: pointer; display: inline-flex; align-items: center; gap: 12px; width: 100%; justify-content: center; }
        .main-content { display: none; }
        .hero-header { background: linear-gradient(145deg, #0c0f18, #05070c); border-radius: 28px; padding: 16px 20px; margin-bottom: 20px; border-bottom: 3px solid var(--deepseek-teal); }
        .logo h1 { font-size: 1.5rem; background: linear-gradient(135deg, #0ea5e9, #e63946); -webkit-background-clip: text; background-clip: text; color: transparent; }
        .user-section { display: flex; align-items: center; gap: 12px; flex-wrap: wrap; justify-content: space-between; margin-top: 10px; }
        .user-info { display: flex; align-items: center; gap: 10px; background: #1a1f2c; padding: 5px 15px 5px 10px; border-radius: 50px; }
        .user-avatar { width: 36px; height: 36px; border-radius: 50%; object-fit: cover; border: 2px solid var(--deepseek-teal); }
        .logout-btn, .save-db-btn { background: #4a2e3a; border: none; padding: 8px 18px; border-radius: 40px; color: white; font-weight: bold; cursor: pointer; font-size: 0.8rem; }
        .save-db-btn { background: var(--green-accent); }
        .dashboard { display: flex; flex-direction: column; gap: 20px; }
        
        /* جدول المذاكرة */
        .schedule-panel, .chat-panel, .mindmap-panel, .lessons-panel {
            background: var(--dark-card); border-radius: 28px; padding: 18px;
            border: 1px solid #2a2f42;
        }
        .schedule-header { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 12px; margin-bottom: 16px; }
        .schedule-table { overflow-x: auto; margin-top: 12px; }
        .schedule-table table { width: 100%; border-collapse: collapse; background: #0f131e; border-radius: 20px; overflow: hidden; }
        .schedule-table th, .schedule-table td { border: 1px solid #2a2f42; padding: 12px 8px; text-align: center; font-size: 0.85rem; }
        .schedule-table th { background: #1a1f30; color: var(--deepseek-teal); }
        .create-schedule-btn, .capture-btn {
            background: var(--green-accent); border: none; padding: 10px 20px;
            border-radius: 40px; color: white; font-weight: bold; cursor: pointer;
            display: inline-flex; align-items: center; gap: 8px;
        }
        .capture-btn { background: var(--orange-accent, #f4a261); }
        
        /* دروس متراكمة */
        .lessons-input-area {
            background: #0c101c; border-radius: 20px; padding: 16px; margin-bottom: 16px;
        }
        .lesson-tag {
            display: inline-flex; align-items: center; gap: 8px;
            background: #1a1f30; padding: 6px 12px; border-radius: 40px;
            margin: 4px; font-size: 0.8rem; border-right: 2px solid var(--deepseek-teal);
        }
        .remove-lesson { background: none; border: none; color: var(--red-accent); cursor: pointer; font-size: 0.8rem; }
        .add-lesson-btn {
            background: var(--deepseek-teal); border: none; padding: 8px 16px;
            border-radius: 40px; color: #0a0c14; font-weight: bold; cursor: pointer;
        }
        
        .chat-window {
            background: #070b12; border-radius: 24px; padding: 12px;
            height: 380px; overflow-y: auto; margin: 16px 0; border: 1px solid #293042;
        }
        .message { margin-bottom: 18px; display: flex; gap: 8px; animation: fadeIn 0.3s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(8px);} to { opacity: 1; transform: translateY(0);} }
        .user-message { justify-content: flex-end; }
        .user-message .bubble { background: var(--blue-accent); border-radius: 22px 8px 22px 22px; }
        .ai-message .bubble { background: #1e253b; border-radius: 8px 22px 22px 22px; border-right: 3px solid var(--deepseek-teal); }
        .bubble { max-width: 88%; padding: 12px 16px; font-size: 0.9rem; line-height: 1.5; }
        .thinking-block { background: #0d1220; border-radius: 16px; padding: 10px; margin-bottom: 8px; font-size: 0.75rem; color: #a8b4e6; border-right: 2px solid var(--deepseek-teal); }
        .input-area { display: flex; gap: 10px; background: #0c101c; border-radius: 60px; padding: 5px; margin-top: 12px; flex-wrap: wrap; }
        .input-row { display: flex; gap: 8px; flex: 1; min-width: 0; }
        .input-area input { flex: 1; background: transparent; border: none; padding: 14px 16px; color: white; font-size: 0.9rem; outline: none; }
        .camera-btn, .send-btn { background: var(--red-accent); border: none; border-radius: 50px; padding: 0 18px; font-weight: bold; cursor: pointer; color: white; display: inline-flex; align-items: center; gap: 6px; }
        .camera-btn { background: #2a3a5a; }
        .quick-grid { display: flex; flex-wrap: wrap; gap: 8px; margin: 12px 0 8px; }
        .quick-chip { background: #1a1f30; border-radius: 40px; padding: 6px 14px; font-size: 0.7rem; cursor: pointer; border: 1px solid var(--deepseek-teal); }
        .mindmap-container { background: #0c0f18; border-radius: 24px; padding: 12px; min-height: 220px; margin-top: 16px; overflow-x: auto; }
        .subject-selector { display: flex; flex-wrap: wrap; gap: 10px; margin: 15px 0; }
        .sub-btn { background: #191f2e; border: none; padding: 8px 16px; border-radius: 36px; color: #cbd5ff; cursor: pointer; font-size: 0.8rem; }
        .error-rate { font-size: 0.65rem; background: #0ea5e922; display: inline-block; padding: 4px 12px; border-radius: 20px; margin-top: 8px; }
        footer { text-align: center; margin-top: 30px; padding: 16px; color: #6b789c; font-size: 0.7rem; }
        .toast-notify { position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%); background: #0ea5e9; color: white; padding: 12px 24px; border-radius: 50px; font-size: 0.85rem; z-index: 2000; animation: fadeOut 3s forwards; }
        @keyframes fadeOut { 0% { opacity: 1; } 70% { opacity: 1; } 100% { opacity: 0; visibility: hidden; } }
        @media (max-width: 650px) { .schedule-table th, .schedule-table td { padding: 8px 4px; font-size: 0.7rem; } .input-area { flex-direction: column; } .input-row { width: 100%; } }
    </style>
</head>
<body>
    <div id="loginOverlay" class="login-overlay">
        <div class="login-card">
            <i class="fas fa-calendar-alt"></i>
            <h2>DeepPlanner</h2>
            <p>📅 جدول مذاكرة أسبوعي • دروس متراكمة • DeepSeek • كاميرا • حفظ البيانات</p>
            <button id="googleLoginBtn" class="google-login-btn"><i class="fab fa-google"></i> تسجيل الدخول بجوجل (إلزامي)</button>
        </div>
    </div>

    <div id="mainApp" class="main-content">
        <div class="container">
            <div class="hero-header">
                <div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 10px;">
                    <div class="logo"><h1><i class="fas fa-table-list" style="color:#0ea5e9;"></i> DeepPlanner <i class="fas fa-image" style="color:#f4a261;"></i></h1><p style="font-size: 0.75rem;">جدول + دروس متراكمة + ذكاء DeepSeek</p></div>
                    <div class="badge-ai"><i class="fas fa-chart-line"></i> جدول بالصورة | توزيع ذكي</div>
                </div>
                <div class="user-section" id="userSection"></div>
            </div>

            <div class="dashboard">
                <!-- قسم الدروس المتراكمة والجدول -->
                <div class="schedule-panel">
                    <div class="schedule-header">
                        <div><i class="fas fa-list-check" style="color: #2a9d8f;"></i> <strong style="font-size: 1.2rem;">📝 الدروس المتراكمة + الجدول الأسبوعي</strong></div>
                        <div style="display: flex; gap: 8px; flex-wrap: wrap;">
                            <button class="create-schedule-btn" id="distributeBtn"><i class="fas fa-magic"></i> وزع الدروس على الأسبوع</button>
                            <button class="capture-btn" id="captureTableBtn"><i class="fas fa-camera"></i> 📸 تحويل الجدول إلى صورة</button>
                            <button class="save-db-btn" id="saveToDatabaseBtn"><i class="fas fa-cloud-upload-alt"></i> حفظ</button>
                        </div>
                    </div>
                    
                    <!-- إدخال الدروس المتراكمة -->
                    <div class="lessons-input-area">
                        <div style="display: flex; gap: 10px; flex-wrap: wrap; margin-bottom: 12px;">
                            <input type="text" id="lessonInput" placeholder="اكتب درس متراكم (مثال: الفصل الأول كيمياء, الوحدة الثانية فيزياء)" style="flex:1; background:#0f131e; border:1px solid #2a2f42; border-radius: 40px; padding: 10px 16px; color: white;">
                            <button class="add-lesson-btn" id="addLessonBtn"><i class="fas fa-plus"></i> أضف درس</button>
                        </div>
                        <div id="lessonsList" style="min-height: 60px;">
                            <div style="color:#6b789c; font-size:0.75rem;">📌 أضف الدروس اللي عليك، ثم اضغط "وزع الدروس على الأسبوع"</div>
                        </div>
                    </div>

                    <div id="scheduleContainer" class="schedule-table">
                        <div style="text-align: center; padding: 30px;"><i class="fas fa-spinner fa-pulse"></i> أضف دروسك ثم وزعها</div>
                    </div>
                    <div class="edit-note" style="margin-top: 10px;"><i class="fas fa-lightbulb"></i> سيتم توزيع الدروس تلقائيًا على أيام الأسبوع (كل يوم 2-3 دروس)</div>
                </div>

                <!-- شات DeepSeek -->
                <div class="chat-panel">
                    <div style="display: flex; gap: 8px; align-items: center;"><i class="fas fa-microchip" style="color:#0ea5e9;"></i><h2 style="font-size: 1.2rem;">🧠 DeepSeek - مساعدك الذكي</h2></div>
                    <div class="chat-window" id="chatWindow"></div>
                    <div class="input-area">
                        <div class="input-row" style="flex:1"><input type="text" id="userQuery" placeholder="اسأل ديب سيك: اشرحلي قانون نيوتن، أو خريطة ذهنية..."><button class="camera-btn" id="cameraBtn"><i class="fas fa-camera"></i></button><button class="camera-btn" id="uploadBtn"><i class="fas fa-image"></i></button></div>
                        <button class="send-btn" id="sendBtn"><i class="fas fa-paper-plane"></i> أرسل</button>
                    </div>
                    <div id="imagePreviewArea" style="margin-top: 8px;"></div>
                    <div class="quick-grid">
                        <div class="quick-chip" data-demo="اشرح قانون نيوتن الثاني بالتفكير المنطقي">⚡ شرح قانون نيوتن</div>
                        <div class="quick-chip" data-demo="خريطة ذهنية للكيمياء (الأحماض والقواعد)">🧪 خريطة كيمياء</div>
                        <div class="quick-chip" data-demo="حل مسألة: سيارة سرعتها 30 م/ث توقفت بعد 6 ثوان، أوجد التسارع">🚗 مسألة فيزياء</div>
                    </div>
                </div>

                <!-- خرائط ذهنية -->
                <div class="mindmap-panel">
                    <i class="fas fa-project-diagram" style="color:#0ea5e9;"></i>
                    <h3 style="font-size: 1.2rem;">🗺️ الخريطة الذهنية</h3>
                    <div class="subject-selector" id="subjectButtons">
                        <button class="sub-btn" data-sub="الرياضيات (تفاضل)">📐 رياضيات</button>
                        <button class="sub-btn" data-sub="الفيزياء (كهربية)">⚡ فيزياء</button>
                        <button class="sub-btn" data-sub="الكيمياء (الأحماض)">🧪 كيمياء</button>
                        <button class="sub-btn" data-sub="الأحياء (الوراثة)">🔬 أحياء</button>
                    </div>
                    <div class="mindmap-container" id="mindmapContainer"><div id="mermaidChart">🌟 اختر مادة لرسم خريطة ذهنية</div></div>
                </div>
            </div>
            <footer><i class="fas fa-image"></i> يمكنك تحويل الجدول إلى صورة | تسجيل دخول إلزامي | حفظ تلقائي للبيانات</footer>
        </div>
    </div>

    <script>
        mermaid.initialize({ startOnLoad: false, theme: 'dark', securityLevel: 'loose' });
        
        let currentUser = null;
        let lessonsList = [];      // الدروس المتراكمة
        let currentSchedule = null;
        let conversationMemory = [];
        let currentImageData = null;
        
        const daysOfWeek = ['السبت', 'الأحد', 'الإثنين', 'الثلاثاء', 'الأربعاء', 'الخميس', 'الجمعة'];
        
        // تحميل وحفظ البيانات
        function loadUserData() {
            if(!currentUser) return;
            const savedLessons = localStorage.getItem(`lessons_${currentUser.uid}`);
            if(savedLessons) lessonsList = JSON.parse(savedLessons);
            renderLessonsList();
            
            const savedSchedule = localStorage.getItem(`schedule_${currentUser.uid}`);
            if(savedSchedule) { currentSchedule = JSON.parse(savedSchedule); renderScheduleTable(currentSchedule); }
            
            const savedMemory = localStorage.getItem(`chat_memory_${currentUser.uid}`);
            if(savedMemory) { conversationMemory = JSON.parse(savedMemory); displayLastMessages(); }
            else { addAIMessage("✨ مرحبًا! أنا DeepSeek. أضف دروسك المتراكمة في القسم العلوي، واضغط 'وزع الدروس على الأسبوع' لتحصل على جدول منظم. واسألني أي شيء!", "تم تفعيل الذاكرة والجدول."); }
        }
        
        function saveAllToLocal() {
            if(!currentUser) return;
            localStorage.setItem(`lessons_${currentUser.uid}`, JSON.stringify(lessonsList));
            if(currentSchedule) localStorage.setItem(`schedule_${currentUser.uid}`, JSON.stringify(currentSchedule));
            localStorage.setItem(`chat_memory_${currentUser.uid}`, JSON.stringify(conversationMemory));
            showToast("✅ تم حفظ الدروس والجدول والمحادثات");
        }
        
        function saveToDatabaseHandler() { saveAllToLocal(); }
        
        function showToast(msg) {
            const toast = document.createElement('div'); toast.className = 'toast-notify'; toast.innerText = msg;
            document.body.appendChild(toast); setTimeout(() => toast.remove(), 3000);
        }
        
        // عرض الدروس المضافة
        function renderLessonsList() {
            const container = document.getElementById('lessonsList');
            if(lessonsList.length === 0) {
                container.innerHTML = '<div style="color:#6b789c; font-size:0.75rem;">📌 لا توجد دروس متراكمة. أضف الدروس التي تريد مذاكرتها.</div>';
                return;
            }
            let html = '<div style="display: flex; flex-wrap: wrap; gap: 8px;">';
            lessonsList.forEach((lesson, idx) => {
                html += `<div class="lesson-tag">📖 ${lesson} <button class="remove-lesson" data-idx="${idx}"><i class="fas fa-times-circle"></i></button></div>`;
            });
            html += '</div>';
            container.innerHTML = html;
            document.querySelectorAll('.remove-lesson').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const idx = parseInt(btn.getAttribute('data-idx'));
                    lessonsList.splice(idx, 1);
                    renderLessonsList();
                    saveAllToLocal();
                });
            });
        }
        
        // توزيع الدروس على الأسبوع
        function distributeLessonsOnWeek() {
            if(lessonsList.length === 0) {
                showToast("❌ أضف بعض الدروس أولاً!");
                return;
            }
            let schedule = {};
            let tempLessons = [...lessonsList];
            // خلط عشوائي بسيط لتوزيع عادل
            for(let i=tempLessons.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [tempLessons[i], tempLessons[j]] = [tempLessons[j], tempLessons[i]];
            }
            
            let lessonIndex = 0;
            for(let day of daysOfWeek) {
                let dayLessons = [];
                let count = (day === 'الجمعة' || day === 'الخميس') ? 2 : 3;
                for(let i=0; i<count && lessonIndex < tempLessons.length; i++) {
                    dayLessons.push(tempLessons[lessonIndex++]);
                }
                if(dayLessons.length === 0) dayLessons.push('مراجعة عامة / راحة');
                schedule[day] = dayLessons;
            }
            // إذا فضل دروس
            if(lessonIndex < tempLessons.length) {
                schedule['ملاحظة'] = `⚠️ لديك ${tempLessons.length - lessonIndex} درس إضافي، وزعها يدوياً أو أضف يوم السبت القادم.`;
            }
            schedule.notes = "📌 تم التوزيع بناءً على دروسك المتراكمة. راجع كل يوم الدرجات المحددة.";
            currentSchedule = schedule;
            renderScheduleTable(currentSchedule);
            saveAllToLocal();
            showToast(`✅ تم توزيع ${lessonsList.length} درس على أيام الأسبوع`);
        }
        
        function renderScheduleTable(schedule) {
            if (!schedule) return;
            let html = `<table id="scheduleTableForCapture">\n<thead><tr><th>اليوم</th><th>📚 الدروس المطلوب مذاكرتها</th></tr></thead><tbody>`;
            for(let day of daysOfWeek) {
                const subjects = schedule[day] || ['لا توجد دروس'];
                html += `<tr><td style="font-weight: bold; background: #1a1f30;">${day}</td><td style="text-align: right;">${subjects.map(s => `📖 ${s}`).join(' • ')}</td></tr>`;
            }
            if(schedule.notes) html += `<tr><td colspan="2" style="background:#1a1f30; text-align:right;">💡 ${schedule.notes}</td></tr>`;
            if(schedule.ملاحظة) html += `<tr><td colspan="2" style="background:#e6394622;">⚠️ ${schedule.ملاحظة}</td></tr>`;
            html += `</tbody></table><div class="edit-note" style="margin-top:8px;"><i class="fas fa-info-circle"></i> يمكنك إعادة التوزيع بعد إضافة دروس جديدة</div>`;
            scheduleContainer.innerHTML = html;
        }
        
        // تحويل الجدول إلى صورة
        async function captureScheduleAsImage() {
            const tableElement = document.getElementById('scheduleTableForCapture');
            if(!tableElement) { showToast("⚠️ لا يوجد جدول لتصويره، قم بتوزيع الدروس أولاً"); return; }
            try {
                showToast("📸 جاري تحويل الجدول إلى صورة...");
                const canvas = await html2canvas(tableElement, { scale: 2, backgroundColor: '#0f131e', logging: false });
                const link = document.createElement('a');
                link.download = `جدول_مذاكرتي_${new Date().toISOString().slice(0,10)}.png`;
                link.href = canvas.toDataURL();
                link.click();
                showToast("✅ تم حفظ الجدول كصورة في جهازك!");
            } catch(e) { showToast("❌ حدث خطأ في تحويل الصورة"); }
        }
        
        // ========== DeepSeek AI ==========
        function deepSeekResponse(userMessage, imageDesc=null) {
            let reasoning = "🧠 **تفكير DeepSeek المنطقي (Chain of Thought):**\n";
            const q = userMessage.toLowerCase();
            reasoning += `1️⃣ تحليل السؤال: "${userMessage.substring(0,80)}"\n2️⃣ السياق: طالب تانية ثانوي - منهج مصري.\n`;
            let answer = "";
            if(q.includes('نيوتن') && (q.includes('قانون') || q.includes('شرح'))) {
                reasoning += "3️⃣ استدعاء قانون نيوتن الثاني: F = m × a\n4️⃣ الشرح خطوة بخطوة: القوة تتناسب طردياً مع التسارع وعكسياً مع الكتلة.\n";
                answer = "⚡ **قانون نيوتن الثاني:**\n\nالقوة = الكتلة × التسارع (F = m × a)\n\n• القوة بوحدة نيوتن\n• الكتلة بوحدة كجم\n• التسارع بوحدة م/ث²\n\n**مثال:** سيارة كتلتها 1000 كجم، قوة 5000 نيوتن، التسارع = 5000/1000 = 5 م/ث²";
            }
            else if(q.includes('خريطة ذهنية') || q.includes('mind map')) {
                let map = `graph TD\n    A["${userMessage.substring(0,30)}"] --> B["مفاهيم أساسية"]\n    A --> C["تطبيقات"]`;
                if(q.includes('كيمياء')) map = `graph TD\n    K["الكيمياء"] --> A["أحماض وقواعد"]\n    K --> B["اتزان كيميائي"]\n    A --> A1["نظرية أرهينيوس"]\n    B --> B1["ثابت الاتزان Kc"]`;
                else if(q.includes('فيزياء')) map = `graph LR\n    P["الفيزياء"] --> Q["قوانين كيرشوف"]\n    Q --> Q1["∑I=0"]\n    Q --> Q2["∑V=0"]`;
                answer = `🗺️ **خريطة ذهنية:**\n\`\`\`mermaid\n${map}\n\`\`\``;
                setTimeout(() => renderMindMap(map), 100);
                reasoning += "3️⃣ تم إنشاء خريطة ذهنية بناءً على الطلب.";
            }
            else if(q.includes('تسارع') && q.includes('سيارة')) {
                reasoning += "3️⃣ تطبيق معادلة الحركة: a = (v-u)/t\n4️⃣ التعويض: السرعة النهائية 0، الابتدائية 30، الزمن 6 → a = -5 م/ث²\n";
                answer = "🚗 **حل المسألة:**\n\nالمعطيات:\nالسرعة الابتدائية (u) = 30 م/ث\nالسرعة النهائية (v) = 0 م/ث\nالزمن (t) = 6 ثوان\n\nالقانون: a = (v - u) / t\nالحساب: a = (0 - 30) / 6 = -5 م/ث²\n\nالإشارة السالبة تعني تباطؤ (تسارع سلبي).";
            }
            else {
                reasoning += "3️⃣ سؤال عام: أقدم المساعدة الشاملة.\n";
                answer = "💡 **DeepSeek هنا لمساعدتك!**\n\nيمكنك:\n• إضافة دروس متراكمة وتوزيعها على جدول أسبوعي\n• تحويل الجدول إلى صورة\n• سؤالي عن أي درس (فيزياء، رياضيات، كيمياء)\n• طلب خريطة ذهنية\n• رفع صورة لسؤال\n\nجرب تقول: 'اشرح قانون نيوتن' أو 'خريطة ذهنية للكيمياء'";
            }
            reasoning += "✅ إجابة مبنية على التفكير المنطقي العميق.";
            return { reasoning, answer };
        }
        
        async function renderMindMap(mermaidDef) {
            const el = document.getElementById('mermaidChart');
            if(!mermaidDef) return;
            try {
                const { svg } = await mermaid.render('mindMapGen', mermaidDef);
                el.innerHTML = svg;
            } catch(e) { el.innerHTML = '<div>✅ خريطة ذهنية جاهزة</div>'; }
        }
        
        // إدارة الشات
        function addMessageToDOM(role, reasoning, content, img) {
            const div = document.createElement('div'); div.className = `message ${role === 'user' ? 'user-message' : 'ai-message'}`;
            if(role === 'user') {
                let imgHtml = img ? `<div><img src="${img}" style="max-width:90px; border-radius:12px; margin-bottom:5px;"></div>` : '';
                div.innerHTML = `<div class="bubble">${imgHtml}<div>${content}</div></div>`;
            } else {
                div.innerHTML = `<div class="bubble"><div class="thinking-block"><div class="thinking-title"><i class="fas fa-brain"></i> DeepSeek تفكير عميق:</div><div>${reasoning}</div></div><div class="final-response">${content}</div></div>`;
            }
            chatWindow.appendChild(div); chatWindow.scrollTop = chatWindow.scrollHeight;
        }
        
        function addAIMessage(content, reasoning) {
            const msg = { role: 'ai', content, reasoning, timestamp: Date.now() };
            conversationMemory.push(msg);
            saveAllToLocal();
            addMessageToDOM('ai', reasoning, content, null);
        }
        
        function addUserMessage(content, img) {
            const msg = { role: 'user', content, image: img, timestamp: Date.now() };
            conversationMemory.push(msg);
            saveAllToLocal();
            addMessageToDOM('user', '', content, img);
        }
        
        function displayLastMessages() {
            chatWindow.innerHTML = '';
            const recent = conversationMemory.slice(-15);
            recent.forEach(m => addMessageToDOM(m.role, m.reasoning || '', m.content, m.image || null));
        }
        
        async function handleUserSend() {
            let text = userQueryInput.value.trim();
            if(!text && !currentImageData) return;
            const finalText = text || (currentImageData ? "📸 صورة مرفوعة للتحليل" : "");
            addUserMessage(finalText, currentImageData);
            userQueryInput.value = '';
            if(currentImageData) { document.getElementById('imagePreviewArea').innerHTML = ''; currentImageData = null; }
            setTimeout(() => {
                const { reasoning, answer } = deepSeekResponse(finalText, currentImageData);
                addAIMessage(answer, reasoning);
                if(answer.includes('mermaid')) {
                    const match = answer.match(/```mermaid\n([\s\S]*?)\n```/);
                    if(match) renderMindMap(match[1]);
                }
            }, 200);
        }
        
        // أحداث الكاميرا
        function setupImageUpload(captureMode = false) {
            const input = document.createElement('input'); input.type = 'file'; input.accept = 'image/*';
            if(captureMode) input.capture = 'environment';
            input.onchange = (e) => {
                const file = e.target.files[0];
                if(file) {
                    const reader = new FileReader();
                    reader.onload = (ev) => {
                        currentImageData = ev.target.result;
                        document.getElementById('imagePreviewArea').innerHTML = `<div class="image-preview"><img src="${currentImageData}" style="width:60px; border-radius:12px;"><button id="removeImg" style="background:none; border:none; color:#e63946;">❌</button></div>`;
                        document.getElementById('removeImg')?.addEventListener('click', () => { currentImageData = null; document.getElementById('imagePreviewArea').innerHTML = ''; });
                    };
                    reader.readAsDataURL(file);
                }
            };
            input.click();
        }
        
        // تسجيل الدخول
        function updateUIAfterLogin() {
            loginOverlay.style.display = 'none';
            mainApp.style.display = 'block';
            userSection.innerHTML = `<div class="user-info"><img src="${currentUser.photoURL}" class="user-avatar"><span class="user-name">${currentUser.displayName}</span><button class="logout-btn" id="logoutBtn">تسجيل خروج</button></div>`;
            document.getElementById('logoutBtn')?.addEventListener('click', () => { localStorage.removeItem('deepthink_user'); location.reload(); });
            loadUserData();
        }
        
        function tryAutoLogin() {
            const saved = localStorage.getItem('deepthink_user');
            if(saved) { currentUser = JSON.parse(saved); updateUIAfterLogin(); return true; }
            return false;
        }
        
        const loginOverlay = document.getElementById('loginOverlay');
        const mainApp = document.getElementById('mainApp');
        document.getElementById('googleLoginBtn')?.addEventListener('click', () => {
            const name = prompt("أدخل اسمك (محاكاة تسجيل دخول جوجل):", "طالب تانية ثانوي");
            if(name) {
                currentUser = { uid: 'user_'+Date.now(), displayName: name, photoURL: `https://ui-avatars.com/api/?background=0ea5e9&color=fff&name=${encodeURIComponent(name)}` };
                localStorage.setItem('deepthink_user', JSON.stringify(currentUser));
                updateUIAfterLogin();
            }
        });
        
        if(!tryAutoLogin()) { loginOverlay.style.display = 'flex'; mainApp.style.display = 'none'; }
        
        // ربط الأزرار
        document.getElementById('addLessonBtn')?.addEventListener('click', () => {
            const lesson = document.getElementById('lessonInput').value.trim();
            if(lesson) { lessonsList.push(lesson); renderLessonsList(); document.getElementById('lessonInput').value = ''; saveAllToLocal(); showToast(`✅ تم إضافة "${lesson}"`); }
        });
        document.getElementById('distributeBtn')?.addEventListener('click', distributeLessonsOnWeek);
        document.getElementById('captureTableBtn')?.addEventListener('click', captureScheduleAsImage);
        document.getElementById('saveToDatabaseBtn')?.addEventListener('click', saveToDatabaseHandler);
        document.getElementById('sendBtn')?.addEventListener('click', handleUserSend);
        document.getElementById('userQuery')?.addEventListener('keypress', (e) => { if(e.key === 'Enter') handleUserSend(); });
        document.getElementById('cameraBtn')?.addEventListener('click', () => setupImageUpload(true));
        document.getElementById('uploadBtn')?.addEventListener('click', () => setupImageUpload(false));
        
        document.querySelectorAll('.quick-chip').forEach(chip => {
            chip.addEventListener('click', () => { userQueryInput.value = chip.getAttribute('data-demo'); handleUserSend(); });
        });
        document.querySelectorAll('.sub-btn').forEach(btn => {
            btn.addEventListener('click', () => { userQueryInput.value = `خريطة ذهنية لـ ${btn.getAttribute('data-sub')}`; handleUserSend(); });
        });
        
        const scheduleContainer = document.getElementById('scheduleContainer');
        const chatWindow = document.getElementById('chatWindow');
    </script>
</body>
</html>
