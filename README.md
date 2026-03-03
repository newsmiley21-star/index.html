<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - LOGISTIQUE GABON</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --gabon-vert: #009E60; 
            --gabon-jaune: #FCD116; 
            --gabon-bleu: #3A75C4;
        }
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; -webkit-tap-highlight-color: transparent; }
        .gabon-gradient { background: linear-gradient(135deg, var(--gabon-vert) 0%, var(--gabon-jaune) 50%, var(--gabon-bleu) 100%); }
        .active-nav { background: white; color: var(--gabon-bleu); shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1); }
        .section { display: none; }
        .section.active { display: block; animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        nav::-webkit-scrollbar { display: none; }
        nav { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="antialiased text-slate-900">

    <!-- ECRAN AUTH -->
    <div id="auth-screen" class="fixed inset-0 z-[9999] flex items-center justify-center gabon-gradient p-6">
        <div class="bg-white/95 backdrop-blur-md p-8 rounded-[2rem] w-full max-w-sm shadow-2xl text-center">
            <h1 class="text-3xl font-black text-[#009E60] mb-1">CT241 OPS</h1>
            <p class="text-[10px] font-bold text-slate-400 tracking-[0.2em] mb-8">LOGISTIQUE & CASH GABON</p>
            
            <div class="space-y-4">
                <input type="email" id="login-email" placeholder="Email professionnel" class="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-[#3A75C4] outline-none transition-all">
                <input type="password" id="login-pass" placeholder="Mot de passe" class="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-[#3A75C4] outline-none transition-all">
                <button id="btnConnect" class="w-full py-4 bg-[#009E60] text-white font-black rounded-2xl shadow-lg active:scale-95 transition-transform">
                    SE CONNECTER
                </button>
            </div>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="main-app" class="hidden min-h-screen pb-20">
        <header class="bg-white px-6 pt-6 pb-4 sticky top-0 z-40 shadow-sm">
            <div class="flex justify-between items-start">
                <div>
                    <h2 class="text-xl font-black text-[#3A75C4]">CT241 OPS</h2>
                    <div class="flex items-center gap-2 mt-1">
                        <span id="user-role" class="text-[10px] px-2 py-0.5 bg-[#FCD116] text-amber-900 font-extrabold rounded-full uppercase">...</span>
                        <span id="user-display" class="text-[11px] font-bold text-slate-400"></span>
                    </div>
                </div>
                <button id="btnOut" class="p-2 bg-red-50 text-red-500 rounded-xl">DECONNEXION</button>
            </div>

            <nav class="flex overflow-x-auto gap-2 mt-6 p-1.5 bg-slate-100 rounded-2xl">
                <button onclick="switchTab('missions')" id="nav-missions" class="active-nav whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold transition-all">
                    MISSIONS <span id="count-badge" class="hidden ml-1 px-1.5 py-0.5 bg-red-500 text-white rounded-full text-[9px]">0</span>
                </button>
                <button onclick="switchTab('creer')" id="nav-creer" class="hidden whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold transition-all">
                    NOUVEAU
                </button>
                <button onclick="switchTab('bilan')" id="nav-bilan" class="whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold transition-all">
                    MON BILAN
                </button>
                <button onclick="switchTab('compta')" id="nav-compta" class="hidden whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold transition-all">
                    COMPTA
                </button>
            </nav>
        </header>

        <main class="p-6 max-w-lg mx-auto">
            <div id="sec-missions" class="section active space-y-6">
                <div id="admin-pending-box" class="hidden">
                    <h3 class="text-[11px] font-black text-red-500 tracking-widest uppercase mb-3">⚠️ À valider (Admin)</h3>
                    <div id="list-pending" class="space-y-4"></div>
                </div>
                <div>
                    <h3 class="text-[11px] font-black text-[#3A75C4] tracking-widest uppercase mb-3">🚀 En cours d'exécution</h3>
                    <div id="list-active" class="space-y-4"></div>
                </div>
            </div>

            <div id="sec-creer" class="section">
                <div class="bg-white p-6 rounded-[2rem] shadow-sm border border-slate-100">
                    <h3 class="text-xl font-black text-[#009E60] mb-6 text-center">Nouvelle Mission</h3>
                    <div class="space-y-4">
                        <input type="text" id="mNom" placeholder="Nom du client" class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent outline-none">
                        <input type="tel" id="mTel" placeholder="Téléphone" class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent outline-none">
                        <input type="text" id="mQuartier" placeholder="Quartier..." class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent outline-none" oninput="autoDetectZone()">
                        <select id="mZoneSelect" class="w-full p-4 bg-slate-50 rounded-2xl font-bold" onchange="updateFrais()">
                            <option value="libreville">Libreville (1000 F)</option>
                            <option value="peripherie">Akanda / Owendo (1500 F)</option>
                            <option value="eloignee">Zone Éloignée (2000 F)</option>
                        </select>
                        <input type="number" id="mRetrait" placeholder="Montant retrait" class="w-full p-4 bg-slate-50 rounded-2xl font-black text-lg">
                        <button onclick="createMission()" class="w-full py-5 bg-[#009E60] text-white font-black rounded-2xl">DÉPLOYER</button>
                    </div>
                </div>
            </div>

            <div id="sec-bilan" class="section">
                <div id="list-bilan" class="space-y-2 mb-6"></div>
                <div class="bg-slate-900 text-white p-8 rounded-[2rem] shadow-xl">
                    <p class="text-[11px] font-bold opacity-50 uppercase tracking-widest mb-2">Total Revenus</p>
                    <h2 id="stat-total" class="text-4xl font-black text-[#FCD116]">0 F</h2>
                </div>
            </div>

            <div id="sec-compta" class="section">
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden mb-6">
                    <table class="w-full text-left text-sm">
                        <thead class="bg-slate-50 text-[10px] font-black uppercase text-slate-400">
                            <tr><th class="p-4">Client</th><th class="p-4 text-right">Com.</th><th class="p-4 text-right">Retrait</th></tr>
                        </thead>
                        <tbody id="list-compta"></tbody>
                    </table>
                </div>
                <div class="bg-[#009E60] p-6 rounded-3xl text-white font-black text-lg flex justify-between">
                    <span>TOTAL COM.</span><span id="compta-total-com">0 F</span>
                </div>
            </div>
        </main>
    </div>

    <!-- LOGIQUE IMAGE CACHEE -->
    <input type="file" id="camInput" accept="image/*" capture="camera" class="hidden">
    <canvas id="offscreenCanvas" class="hidden"></canvas>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-auth.js";
        import { getDatabase, ref, push, onValue, update, remove } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAPCKRy9NTo4X8nn8YpxAbPtX8SlKj-7sQ",
            authDomain: "cashtransfert-21.firebaseapp.com",
            databaseURL: "https://cashtransfert-21-default-rtdb.firebaseio.com",
            projectId: "cashtransfert-21",
            storageBucket: "cashtransfert-21.firebasestorage.app",
            messagingSenderId: "564831743134",
            appId: "1:564831743134:web:c22a1f53707f0f1dd9df8f"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getDatabase(app);

        let userRole = "livreur";
        let allMissions = [];
        let activeKeyForPhoto = null;
        let lastBase64Image = null;

        // AUTH
        document.getElementById('btnConnect').onclick = async () => {
            const email = document.getElementById('login-email').value;
            const pass = document.getElementById('login-pass').value;
            try { await signInWithEmailAndPassword(auth, email, pass); } catch(e) { alert("Accès refusé."); }
        };
        document.getElementById('btnOut').onclick = () => signOut(auth);

        onAuthStateChanged(auth, (u) => {
            if (u) {
                const email = u.email.toLowerCase();
                userRole = email.includes('admin') ? "admin" : (email.includes('finance') ? "finance" : "livreur");
                document.getElementById('user-role').innerText = userRole;
                document.getElementById('user-display').innerText = u.email.split('@')[0].toUpperCase();
                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('main-app').classList.remove('hidden');
                
                if (userRole !== 'livreur') {
                    document.getElementById('nav-creer').classList.remove('hidden');
                    document.getElementById('nav-compta').classList.remove('hidden');
                    document.getElementById('admin-pending-box').classList.remove('hidden');
                }
                listenMissions();
            } else {
                document.getElementById('auth-screen').classList.remove('hidden');
                document.getElementById('main-app').classList.add('hidden');
            }
        });

        function listenMissions() {
            onValue(ref(db, 'missions'), (snap) => {
                const data = snap.val();
                allMissions = data ? Object.keys(data).map(k => ({...data[k], key:k})) : [];
                render();
            });
        }

        // PHOTO PROCESSING
        window.triggerPhoto = (key) => {
            activeKeyForPhoto = key;
            lastBase64Image = null; // Reset
            document.getElementById('camInput').click();
        };

        document.getElementById('camInput').onchange = function(e) {
            const file = e.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(event) {
                const img = new Image();
                img.onload = function() {
                    const canvas = document.getElementById('offscreenCanvas');
                    const ctx = canvas.getContext('2d');
                    
                    // Redimensionnement pour alléger la DB (max 600px)
                    const maxW = 600;
                    const scale = maxW / img.width;
                    canvas.width = maxW;
                    canvas.height = img.height * scale;
                    
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    lastBase64Image = canvas.toDataURL('image/jpeg', 0.7);
                    
                    // Affichage immédiat de l'aperçu
                    const preview = document.getElementById('preview-' + activeKeyForPhoto);
                    if (preview) {
                        preview.src = lastBase64Image;
                        preview.classList.remove('hidden');
                    }
                };
                img.src = event.target.result;
            };
            reader.readAsDataURL(file);
        };

        window.sendToFinance = (key) => {
            const code = document.getElementById('code-' + key).value;
            if (!code || !lastBase64Image) return alert("Photo et Code SMS obligatoires !");
            
            update(ref(db, `missions/${key}`), {
                etape: 2,
                codeSMS: code,
                photoPreuve: lastBase64Image,
                timestamp: Date.now()
            }).then(() => alert("Mission envoyée !"));
        };

        // RENDERING
        function render() {
            const pnd = document.getElementById('list-pending');
            const act = document.getElementById('list-active');
            const bil = document.getElementById('list-bilan');
            const cpt = document.getElementById('list-compta');
            const myName = auth.currentUser.email.split('@')[0].toUpperCase();

            pnd.innerHTML = ""; act.innerHTML = ""; bil.innerHTML = ""; cpt.innerHTML = "";
            let tGains = 0, tCom = 0, tVol = 0;

            allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
                if (m.etape === 3) {
                    if (m.livreur === myName) {
                        tGains += m.fraisLivraison;
                        bil.innerHTML += `<div class="bg-white p-4 rounded-2xl flex justify-between shadow-sm"><b>${m.nom}</b><span class="text-green-600">+${m.fraisLivraison}</span></div>`;
                    }
                    if (userRole === 'admin') {
                        tCom += m.com; tVol += m.retrait;
                        cpt.innerHTML += `<tr class="border-b"><td class="p-4 font-bold">${m.nom}</td><td class="p-4 text-right text-green-600">${m.com}</td><td class="p-4 text-right">${m.retrait}</td></tr>`;
                    }
                    return;
                }

                const card = `
                <div class="bg-white p-5 rounded-[1.8rem] shadow-sm border border-slate-100 mb-4">
                    <div class="flex justify-between items-start mb-2">
                        <h4 class="font-black text-slate-800">${m.nom}</h4>
                        <span class="text-[9px] bg-blue-50 text-blue-600 px-2 py-1 rounded font-bold">${m.id}</span>
                    </div>
                    <p class="text-[11px] font-bold text-slate-400">📍 ${m.lieu} | 📞 ${m.tel}</p>
                    <div class="mt-4 flex justify-between items-end">
                        <div><span class="text-[9px] uppercase font-black opacity-30">Retrait</span><br><span class="text-xl font-black">${m.retrait} F</span></div>
                        <div class="text-right text-[10px] font-bold text-slate-400">🛵 ${m.livreur}</div>
                    </div>

                    ${m.etape === 0 && userRole === 'admin' ? `<button onclick="updateStatus('${m.key}', 1)" class="w-full mt-4 py-3 bg-blue-600 text-white rounded-xl font-black text-xs">VALIDER & PUBLIER</button>` : ''}
                    
                    ${m.etape === 1 && m.livreur === "À pourvoir" && userRole === 'livreur' ? `<button onclick="assignMe('${m.key}')" class="w-full mt-4 py-3 bg-green-600 text-white rounded-xl font-black text-xs">ACCEPTER</button>` : ''}

                    ${m.etape === 1 && m.livreur === myName ? `
                        <div class="mt-4 pt-4 border-t border-dashed">
                            <img id="preview-${m.key}" class="hidden w-full h-32 object-cover rounded-xl mb-2 border">
                            <button onclick="triggerPhoto('${m.key}')" class="w-full py-3 bg-slate-900 text-white rounded-xl font-black text-xs mb-2">📸 PRENDRE PHOTO SMS</button>
                            <input type="text" id="code-${m.key}" placeholder="CODE SMS" class="w-full p-3 bg-slate-50 rounded-xl mb-2 text-center font-black outline-none border focus:border-blue-500">
                            <button onclick="sendToFinance('${m.key}')" class="w-full py-3 bg-blue-500 text-white rounded-xl font-black text-xs">TRANSMETTRE PROUVE</button>
                        </div>
                    ` : ''}

                    ${m.etape === 2 && (userRole === 'admin' || userRole === 'finance') ? `
                        <div class="mt-4 p-4 bg-amber-50 rounded-2xl">
                            <img src="${m.photoPreuve}" class="w-full rounded-xl mb-3 shadow-sm" onclick="window.open(this.src)">
                            <div class="text-center">
                                <span class="text-[10px] font-black opacity-40 uppercase">Code de retrait</span>
                                <h5 class="text-2xl font-black tracking-widest text-amber-900">${m.codeSMS}</h5>
                                <button onclick="updateStatus('${m.key}', 3)" class="w-full mt-3 py-3 bg-green-600 text-white rounded-xl font-black text-xs uppercase">Encaissement Confirmé ✅</button>
                            </div>
                        </div>
                    ` : ''}
                </div>`;

                if (m.etape === 0) pnd.innerHTML += card;
                else act.innerHTML += card;
            });

            document.getElementById('stat-total').innerText = tGains.toLocaleString() + " F";
            document.getElementById('compta-total-com').innerText = tCom.toLocaleString() + " F";
        }

        // GLOBALS
        window.switchTab = (id) => {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('active-nav'));
            document.getElementById('sec-'+id).classList.add('active');
            document.getElementById('nav-'+id).classList.add('active-nav');
        };
        window.updateStatus = (k, s) => update(ref(db, `missions/${k}`), { etape: s });
        window.assignMe = (k) => update(ref(db, `missions/${k}`), { livreur: auth.currentUser.email.split('@')[0].toUpperCase() });
        
        window.createMission = () => {
            const m = {
                id: "CT" + Math.floor(1000 + Math.random()*9000),
                nom: document.getElementById('mNom').value,
                tel: document.getElementById('mTel').value,
                lieu: document.getElementById('mQuartier').value,
                retrait: parseInt(document.getElementById('mRetrait').value),
                com: 390,
                fraisLivraison: 1000,
                livreur: "À pourvoir",
                etape: 0,
                timestamp: Date.now()
            };
            if(!m.nom || !m.retrait) return alert("Nom et Montant requis");
            push(ref(db, 'missions'), m);
            switchTab('missions');
        };
    </script>
</body>
</html>
