# Skillswap
<!DOCTYPE html>
<html lang="de" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SkillSwap OS | Pro & Mobile</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root { --primary: #6366f1; --secondary: #a855f7; --dark: #0f172a; }
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background: #f8fafc; 
            color: var(--dark);
            -webkit-tap-highlight-color: transparent;
        }

        /* Glassmorphism & Gradients */
        .glass { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.4); }
        .grad-bg { background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%); }
        .grad-text { background: linear-gradient(90deg, var(--primary), var(--secondary)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        /* Animations */
        .page-view { display: none; animation: slideUp 0.4s ease-out forwards; }
        .page-view.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* Chat Bubbles */
        .bubble-me { background: var(--primary); color: white; border-radius: 20px 20px 4px 20px; box-shadow: 0 4px 15px rgba(99, 102, 241, 0.2); }
        .bubble-other { background: white; color: var(--dark); border-radius: 20px 20px 20px 4px; border: 1px solid #e2e8f0; }

        /* Responsive Layout Handling */
        @media (max-width: 1023px) {
            .sidebar { display: none; }
            .bottom-nav { display: flex; }
            main { margin-left: 0 !important; padding-bottom: 100px !important; }
            .chat-modal { width: 100% !important; height: 100% !important; border-radius: 0 !important; bottom: 0 !important; right: 0 !important; }
        }

        .nav-active { color: var(--primary) !important; background: rgba(99, 102, 241, 0.05); border-radius: 1rem; }
        
        /* Academy Hover */
        .info-card { transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        .info-card:hover { transform: translateY(-10px); box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.1); }
    </style>
</head>
<body class="flex flex-col min-h-screen">

    <aside class="sidebar fixed left-0 top-0 bottom-0 w-72 bg-white border-r border-slate-100 z-50 flex flex-col p-8">
        <div class="mb-12 flex items-center gap-4">
            <div class="w-12 h-12 grad-bg rounded-2xl flex items-center justify-center text-white shadow-xl rotate-3">
                <i class="fa-solid fa-bolt-lightning text-xl"></i>
            </div>
            <span class="text-2xl font-black tracking-tighter grad-text italic">SkillSwap</span>
        </div>

        <nav class="space-y-3">
            <button onclick="showPage('home')" id="d-btn-home" class="w-full flex items-center gap-4 p-4 font-bold text-slate-500 hover:text-primary transition-all">
                <i class="fa-solid fa-house-chimney text-xl"></i> Community
            </button>
            <button onclick="showPage('market')" id="d-btn-market" class="w-full flex items-center gap-4 p-4 font-bold text-slate-500 hover:text-primary transition-all">
                <i class="fa-solid fa-briefcase text-xl"></i> Marktplatz
            </button>
            <button onclick="showPage('academy')" id="d-btn-academy" class="w-full flex items-center gap-4 p-4 font-bold text-slate-500 hover:text-primary transition-all">
                <i class="fa-solid fa-graduation-cap text-xl"></i> Akademie
            </button>
        </nav>

        <div class="mt-auto pt-6 border-t border-slate-50">
            <div onclick="openProfileEdit()" class="bg-slate-900 p-4 rounded-[2rem] cursor-pointer hover:bg-indigo-600 transition flex items-center gap-4 shadow-xl">
                <img id="d-avatar" src="https://ui-avatars.com/api/?name=?&background=6366f1&color=fff" class="w-12 h-12 rounded-2xl object-cover ring-2 ring-white/20">
                <div class="overflow-hidden">
                    <p id="d-name" class="text-white text-sm font-black truncate">Profil laden...</p>
                    <span class="text-[10px] text-indigo-300 font-bold uppercase">Einstellungen</span>
                </div>
            </div>
        </div>
    </aside>

    <nav class="bottom-nav hidden fixed bottom-0 left-0 right-0 h-20 bg-white border-t border-slate-100 z-[100] px-8 items-center justify-between shadow-[0_-10px_30px_rgba(0,0,0,0.05)]">
        <button onclick="showPage('home')" id="m-btn-home" class="flex flex-col items-center gap-1 text-slate-400">
            <i class="fa-solid fa-house text-xl transition-all"></i>
            <span class="text-[9px] font-black uppercase tracking-tighter">Home</span>
        </button>
        <button onclick="showPage('market')" id="m-btn-market" class="flex flex-col items-center gap-1 text-slate-400">
            <i class="fa-solid fa-magnifying-glass text-xl transition-all"></i>
            <span class="text-[9px] font-black uppercase tracking-tighter">Markt</span>
        </button>
        <button onclick="openProfileEdit()" class="w-14 h-14 grad-bg rounded-[1.5rem] flex items-center justify-center text-white shadow-lg -translate-y-5 border-4 border-white active:scale-90 transition">
            <i class="fa-solid fa-user-plus"></i>
        </button>
        <button onclick="showPage('academy')" id="m-btn-academy" class="flex flex-col items-center gap-1 text-slate-400">
            <i class="fa-solid fa-graduation-cap text-xl transition-all"></i>
            <span class="text-[9px] font-black uppercase tracking-tighter">Wissen</span>
        </button>
        <button onclick="openChat('Self','')" class="flex flex-col items-center gap-1 text-slate-400">
            <i class="fa-solid fa-message text-xl transition-all"></i>
            <span class="text-[9px] font-black uppercase tracking-tighter">Chat</span>
        </button>
    </nav>

    <main class="ml-0 lg:ml-72 p-6 lg:p-12 flex-grow transition-all">
        
        <section id="page-home" class="page-view active">
            <header class="mb-10">
                <h1 class="text-4xl lg:text-6xl font-black tracking-tighter mb-4">Entdecke <span class="grad-text">Skills.</span></h1>
                <p class="text-slate-400 font-semibold text-lg">Keine Bots. Nur echte Menschen wie du.</p>
            </header>
            
            <div id="emptyState" class="hidden bg-white rounded-[3rem] p-12 text-center border border-slate-100 shadow-sm">
                <div class="w-20 h-20 bg-indigo-50 text-indigo-500 rounded-full flex items-center justify-center mx-auto mb-6 text-3xl">
                    <i class="fa-solid fa-user-plus"></i>
                </div>
                <h2 class="text-xl font-black mb-2">Dein Profil fehlt noch!</h2>
                <p class="text-slate-500 mb-6 text-sm">Erstelle dein Profil, um für andere sichtbar zu werden.</p>
                <button onclick="openProfileEdit()" class="grad-bg text-white px-8 py-4 rounded-2xl font-black shadow-lg">Profil anlegen</button>
            </div>

            <div id="userGrid" class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-6"></div>
        </section>

        <section id="page-academy" class="page-view">
            <div class="max-w-4xl mx-auto py-6">
                <div class="text-center mb-12">
                    <span class="bg-indigo-100 text-indigo-600 px-4 py-1 rounded-full text-[10px] font-black uppercase tracking-widest">SkillSwap Akademie</span>
                    <h2 class="text-4xl lg:text-5xl font-black mt-4">Wissen ist die neue <span class="grad-text">Währung.</span></h2>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-8">
                    <div class="info-card bg-white p-10 rounded-[3rem] shadow-sm border border-slate-100 relative overflow-hidden group">
                        <div class="absolute top-[-20px] right-[-20px] text-slate-50 text-9xl font-black group-hover:text-indigo-50 transition duration-500">01</div>
                        <div class="relative z-10">
                            <div class="w-16 h-16 bg-indigo-600 text-white rounded-2xl flex items-center justify-center text-2xl shadow-xl mb-6"><i class="fa-solid fa-fingerprint"></i></div>
                            <h3 class="text-2xl font-black mb-3">Authentizität</h3>
                            <p class="text-slate-500 leading-relaxed text-sm font-medium">Dein Profil ist dein Aushängeschild. Nutze echte Bilder und beschreibe deine Fähigkeiten präzise. Je ehrlicher du bist, desto besser die Tauschangebote.</p>
                        </div>
                    </div>

                    <div class="info-card bg-slate-900 p-10 rounded-[3rem] shadow-xl text-white relative overflow-hidden group">
                        <div class="absolute top-[-20px] right-[-20px] text-white/5 text-9xl font-black">02</div>
                        <div class="relative z-10">
                            <div class="w-16 h-16 bg-rose-500 text-white rounded-2xl flex items-center justify-center text-2xl shadow-xl mb-6"><i class="fa-solid fa-comments"></i></div>
                            <h3 class="text-2xl font-black mb-3">Der Private Chat</h3>
                            <p class="text-slate-300 leading-relaxed text-sm font-medium">Sende Bilder von deinen Arbeiten, vereinbare Termine und tausche Tipps aus. Unser Chat ist sicher und nur für euch beide einsehbar.</p>
                        </div>
                    </div>
                </div>

                <div class="grad-bg rounded-[3rem] p-10 text-white flex flex-col md:flex-row items-center justify-between gap-6 shadow-2xl">
                    <div class="max-w-md">
                        <h3 class="text-2xl font-black mb-2">Bereit für den ersten Skill?</h3>
                        <p class="text-white/80 text-sm font-medium">Starte jetzt und werde Teil einer wachsenden Community von Machern.</p>
                    </div>
                    <button onclick="showPage('home')" class="bg-white text-indigo-600 px-8 py-4 rounded-2xl font-black hover:scale-105 transition shadow-xl">Jetzt loslegen</button>
                </div>
            </div>
        </section>

        <section id="page-market" class="page-view">
            <div class="flex flex-col md:flex-row justify-between items-end gap-6 mb-10">
                <div>
                    <h2 class="text-4xl lg:text-5xl font-black tracking-tight">Markt<span class="grad-text">platz</span></h2>
                    <p class="text-slate-400 font-semibold">Aktuelle Gesuche der Community.</p>
                </div>
                <button onclick="openMarketModal()" class="w-full md:w-auto grad-bg text-white px-8 py-5 rounded-[1.5rem] font-black shadow-lg hover:scale-105 transition">
                    Anzeige schalten +
                </button>
            </div>
            <div id="marketGrid" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>
        </section>
    </main>

    <div id="chatWidget" class="hidden fixed inset-0 z-[300] bg-slate-900/40 backdrop-blur-md flex items-center justify-center p-0 lg:p-8">
        <div class="chat-modal w-full lg:max-w-2xl h-full lg:h-[85vh] bg-white lg:rounded-[3rem] shadow-2xl flex flex-col overflow-hidden">
            <div class="p-6 bg-slate-900 text-white flex justify-between items-center shrink-0">
                <div class="flex items-center gap-4">
                    <div class="relative">
                        <img id="chatUserImg" src="" class="w-12 h-12 rounded-2xl object-cover bg-slate-700">
                        <div class="absolute -bottom-1 -right-1 w-4 h-4 bg-green-500 border-4 border-slate-900 rounded-full"></div>
                    </div>
                    <div>
                        <h4 id="chatUserName" class="font-black text-sm lg:text-base">Unbekannt</h4>
                        <span class="text-[9px] text-indigo-400 font-black uppercase tracking-widest">Tausch-Raum aktiv</span>
                    </div>
                </div>
                <div class="flex gap-2">
                    <button onclick="clearChat()" class="w-10 h-10 bg-white/10 rounded-xl flex items-center justify-center hover:bg-rose-500/20 transition text-white/50 hover:text-rose-500"><i class="fa-solid fa-trash-can text-sm"></i></button>
                    <button onclick="closeChat()" class="w-10 h-10 bg-white/10 rounded-xl flex items-center justify-center hover:bg-white/20 transition"><i class="fa-solid fa-xmark text-lg"></i></button>
                </div>
            </div>
            
            <div id="chatContainer" class="flex-grow overflow-y-auto p-6 flex flex-col gap-4 bg-slate-50/50"></div>

            <div id="previewBox" class="hidden px-6 py-4 bg-indigo-50 flex items-center gap-4 border-t border-indigo-100">
                <div class="relative">
                    <img id="imgToUpload" src="" class="w-20 h-20 rounded-xl object-cover shadow-md border-2 border-white">
                    <button onclick="cancelImg()" class="absolute -top-2 -right-2 bg-rose-500 text-white w-6 h-6 rounded-full flex items-center justify-center text-[10px]"><i class="fa-solid fa-times"></i></button>
                </div>
                <p class="text-xs font-bold text-indigo-600">Bild bereit...</p>
            </div>

            <div class="p-6 bg-white border-t border-slate-100 pb-12 lg:pb-6">
                <form onsubmit="sendMsg(event)" class="flex gap-3">
                    <button type="button" onclick="document.getElementById('fileIn').click()" class="w-14 h-14 bg-slate-100 rounded-2xl flex items-center justify-center text-slate-400 hover:text-indigo-600 transition">
                        <i class="fa-solid fa-camera text-xl"></i>
                    </button>
                    <input type="file" id="fileIn" class="hidden" accept="image/*" onchange="previewImg(event)">
                    <input type="text" id="msgIn" placeholder="Nachricht schreiben..." class="flex-grow bg-slate-100 px-6 rounded-2xl outline-none font-medium focus:ring-2 focus:ring-indigo-500">
                    <button class="w-14 h-14 grad-bg text-white rounded-2xl flex items-center justify-center shadow-lg active:scale-90 transition">
                        <i class="fa-solid fa-paper-plane text-lg"></i>
                    </button>
                </form>
            </div>
        </div>
    </div>

    <div id="profileModal" class="hidden fixed inset-0 z-[400] bg-slate-900/90 backdrop-blur-xl flex items-center justify-center p-4">
        <div class="bg-white rounded-[3rem] w-full max-w-xl p-8 lg:p-12 shadow-2xl animate-slideUp">
            <h3 class="text-3xl font-black mb-8">Dein Profil</h3>
            <form onsubmit="saveProfile(event)" class="space-y-5">
                <div class="flex flex-col items-center mb-6">
                    <div class="relative group cursor-pointer" onclick="document.getElementById('pInput').click()">
                        <img id="pPreview" src="https://ui-avatars.com/api/?name=?&background=6366f1&color=fff" class="w-32 h-32 rounded-[2.5rem] object-cover border-4 border-slate-50 shadow-2xl group-hover:scale-105 transition">
                        <div class="absolute inset-0 bg-black/20 rounded-[2.5rem] flex items-center justify-center opacity-0 group-hover:opacity-100 transition text-white">
                            <i class="fa-solid fa-camera text-2xl"></i>
                        </div>
                    </div>
                    <input type="file" id="pInput" class="hidden" accept="image/*" onchange="prevAvatar(event)">
                </div>
                <input type="text" id="pName" placeholder="Voller Name" required class="w-full p-5 bg-slate-50 rounded-2xl outline-none font-black text-lg border-2 border-transparent focus:border-indigo-500 transition">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="space-y-1">
                        <label class="text-[10px] font-black text-indigo-500 uppercase ml-2">Ich biete</label>
                        <input type="text" id="pGive" placeholder="z.B. Webdesign" required class="w-full p-4 bg-indigo-50 text-indigo-900 rounded-2xl outline-none font-bold">
                    </div>
                    <div class="space-y-1">
                        <label class="text-[10px] font-black text-rose-500 uppercase ml-2">Ich suche</label>
                        <input type="text" id="pTake" placeholder="z.B. Klavier" required class="w-full p-4 bg-rose-50 text-rose-900 rounded-2xl outline-none font-bold">
                    </div>
                </div>
                <button type="submit" class="w-full py-6 grad-bg text-white font-black rounded-[1.5rem] shadow-xl hover:shadow-2xl transition">Profil speichern</button>
                <button type="button" onclick="closeModal('profileModal')" class="w-full text-slate-400 font-bold text-sm">Abbrechen</button>
            </form>
        </div>
    </div>

    <script>
        // DATA
        let profile = JSON.parse(localStorage.getItem('ss_os_profile')) || null;
        let ads = JSON.parse(localStorage.getItem('ss_os_ads')) || [];
        let chats = JSON.parse(localStorage.getItem('ss_os_chats')) || [];
        let tempImg = null;

        window.onload = () => {
            if(profile) {
                updateSidebar();
                renderUser();
            } else {
                document.getElementById('emptyState').classList.remove('hidden');
            }
            renderMarket();
            showPage('home');
        };

        // NAVIGATION
        function showPage(p) {
            document.querySelectorAll('.page-view').forEach(v => v.classList.remove('active'));
            document.getElementById('page-' + p).classList.add('active');
            
            // Mobile Nav
            document.querySelectorAll('.bottom-nav button').forEach(b => b.classList.remove('nav-active'));
            document.getElementById('m-btn-' + p)?.classList.add('nav-active');
            
            // Desktop Nav
            document.querySelectorAll('.sidebar button').forEach(b => b.classList.remove('nav-active'));
            document.getElementById('d-btn-' + p)?.classList.add('nav-active');
        }

        // PROFILE LOGIC
        function prevAvatar(e) {
            const r = new FileReader();
            r.onload = () => document.getElementById('pPreview').src = r.result;
            r.readAsDataURL(e.target.files[0]);
        }

        function saveProfile(e) {
            e.preventDefault();
            profile = {
                name: document.getElementById('pName').value,
                give: document.getElementById('pGive').value,
                take: document.getElementById('pTake').value,
                avatar: document.getElementById('pPreview').src
            };
            localStorage.setItem('ss_os_profile', JSON.stringify(profile));
            document.getElementById('emptyState').classList.add('hidden');
            updateSidebar(); renderUser(); closeModal('profileModal');
        }

        function updateSidebar() {
            document.getElementById('d-name').innerText = profile.name;
            document.getElementById('d-avatar').src = profile.avatar;
        }

        function renderUser() {
            const g = document.getElementById('userGrid');
            g.innerHTML = '';
            if(!profile) return;
            g.innerHTML = `
                <div class="bg-white p-8 rounded-[3rem] shadow-sm border border-slate-50 flex flex-col hover:shadow-2xl transition duration-500 group">
                    <div class="flex items-center gap-5 mb-8">
                        <img src="${profile.avatar}" class="w-20 h-20 rounded-[1.8rem] object-cover bg-slate-100 shadow-xl group-hover:rotate-3 transition duration-500">
                        <div>
                            <h4 class="text-2xl font-black">${profile.name}</h4>
                            <span class="text-[10px] bg-indigo-100 text-indigo-600 px-3 py-1 rounded-full font-black uppercase tracking-widest">Ich</span>
                        </div>
                    </div>
                    <div class="space-y-3 mb-10">
                        <div class="p-5 bg-indigo-50 rounded-2xl flex justify-between items-center"><span class="text-[10px] font-black text-indigo-400 uppercase tracking-widest">Bietet</span><span class="font-bold text-indigo-900">${profile.give}</span></div>
                        <div class="p-5 bg-rose-50 rounded-2xl flex justify-between items-center"><span class="text-[10px] font-black text-rose-400 uppercase tracking-widest">Sucht</span><span class="font-bold text-rose-900">${profile.take}</span></div>
                    </div>
                    <button onclick="openChat('${profile.name}', '${profile.avatar}')" class="w-full py-5 bg-slate-900 text-white rounded-2xl font-black shadow-lg hover:bg-indigo-600 transition">Privater Tausch-Raum</button>
                </div>`;
        }

        // CHAT LOGIC (Riesen Skript)
        function openChat(name, img) {
            document.getElementById('chatUserName').innerText = name || "Unbekannt";
            document.getElementById('chatUserImg').src = img || "https://ui-avatars.com/api/?name=User";
            document.getElementById('chatWidget').classList.remove('hidden');
            document.body.style.overflow = "hidden"; // Scroll Lock
            renderMsgs();
        }

        function previewImg(e) {
            const f = e.target.files[0];
            if(f) {
                const r = new FileReader();
                r.onload = () => {
                    tempImg = r.result;
                    document.getElementById('imgToUpload').src = r.result;
                    document.getElementById('previewBox').classList.remove('hidden');
                };
                r.readAsDataURL(f);
            }
        }

        function cancelImg() { tempImg = null; document.getElementById('previewBox').classList.add('hidden'); }

        function sendMsg(e) {
            e.preventDefault();
            const val = document.getElementById('msgIn').value;
            if(!val.trim() && !tempImg) return;

            const m = { 
                text: val.trim(), 
                img: tempImg, 
                time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}) 
            };
            
            chats.push(m);
            localStorage.setItem('ss_os_chats', JSON.stringify(chats));
            renderMsgs();
            
            document.getElementById('msgIn').value = '';
            cancelImg();
        }

        function renderMsgs() {
            const c = document.getElementById('chatContainer');
            c.innerHTML = '<p class="text-[10px] text-center text-slate-400 font-bold uppercase py-6 tracking-[0.2em] opacity-50 italic">Sicherer Austausch-Raum</p>';
            chats.forEach(m => {
                const d = document.createElement('div');
                d.className = "max-w-[85%] p-4 bubble-me self-end animate-slideUp";
                if(m.img) d.innerHTML += `<img src="${m.img}" class="rounded-xl mb-2 w-full max-w-[280px] shadow-lg border-2 border-white/20">`;
                if(m.text) d.innerHTML += `<p class="text-sm font-semibold leading-relaxed">${m.text}</p>`;
                d.innerHTML += `<span class="text-[9px] opacity-60 block text-right mt-2 font-black">${m.time}</span>`;
                c.appendChild(d);
            });
            c.scrollTop = c.scrollHeight;
        }

        function clearChat() {
            if(confirm("Den gesamten Verlauf unwiderruflich löschen?")) {
                chats = [];
                localStorage.removeItem('ss_os_chats');
                renderMsgs();
            }
        }

        // MARKET LOGIC
        function renderMarket() {
            const g = document.getElementById('marketGrid');
            g.innerHTML = ads.length ? '' : '<p class="text-slate-400 text-center py-20 col-span-2 font-bold italic">Noch keine Inserate aktiv...</p>';
            ads.forEach(ad => {
                g.innerHTML += `
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm border-l-8 border-indigo-600 hover:shadow-xl transition">
                        <h4 class="text-2xl font-black mb-2">${ad.title}</h4>
                        <div class="text-indigo-600 font-bold bg-indigo-50 p-4 rounded-xl text-sm mb-4 italic">Gegenleistung: ${ad.offer}</div>
                        <div class="flex justify-between items-center text-[10px] font-black uppercase text-slate-400">
                            <span>VON: ${ad.author}</span>
                            <button onclick="showPage('home')" class="text-indigo-500 hover:underline">Profil ansehen</button>
                        </div>
                    </div>`;
            });
        }

        function openMarketModal() { 
            if(!profile) return alert("Erstelle erst dein Profil!");
            const t = prompt("Was suchst du?");
            const o = prompt("Was bietest du?");
            if(t && o) {
                ads.unshift({title: t, offer: o, author: profile.name});
                localStorage.setItem('ss_os_ads', JSON.stringify(ads));
                renderMarket();
            }
        }

        // HELPERS
        function openProfileEdit() { document.getElementById('profileModal').classList.remove('hidden'); }
        function closeModal(id) { document.getElementById(id).classList.add('hidden'); }
        function closeChat() { 
            document.getElementById('chatWidget').classList.add('hidden'); 
            document.body.style.overflow = "auto";
        }
    </script>
</body>
</html>
