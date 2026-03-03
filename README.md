<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - LOGISTIQUE GABON</title>
    
    <!-- Tailwind CSS pour un design moderne et rapide -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --gabon-vert: #009E60; 
            --gabon-jaune: #FCD116; 
            --gabon-bleu: #3A75C4;
        }
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .gabon-gradient { background: linear-gradient(135deg, var(--gabon-vert) 0%, var(--gabon-jaune) 50%, var(--gabon-bleu) 100%); }
        .active-nav { background: white; color: var(--gabon-bleu); box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1); }
        .section { display: none; }
        .section.active { display: block; animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Masquer la scrollbar pour la nav */
        nav::-webkit-scrollbar { display: none; }
        nav { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="antialiased text-slate-900">

    <!-- ECRAN D'AUTH -->
    <div id="auth-screen" class="fixed inset-0 z-[9999] flex items-center justify-center gabon-gradient p-6">
        <div class="bg-white/95 backdrop-blur-md p-8 rounded-[2rem] w-full max-w-sm shadow-2xl text-center">
            <h1 class="text-3xl font-black text-[#009E60] mb-1">CT241 OPS</h1>
            <p class="text-[10px] font-bold text-slate-400 tracking-[0.2em] mb-8">LOGISTIQUE & CASH GABON</p>
            
            <div class="space-y-4">
                <input type="email" id="login-email" placeholder="Email professionnel" class="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-[#3A75C4] outline-none transition-all">
                <input type="password" id="login-pass" placeholder="Mot de passe" class="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-[#3A75C4] outline-none transition-all">
                <button id="btnConnect" class="w-full py-4 bg-[#009E60] text-white font-black rounded-2xl shadow-lg shadow-green-200 active:scale-95 transition-transform">
                    SE CONNECTER
                </button>
            </div>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="main-app" class="hidden min-h-screen pb-20">
        <!-- HEADER -->
        <header class="bg-white px-6 pt-6 pb-4 sticky top-0 z-40 shadow-sm">
            <div class="flex justify-between items-start">
                <div>
                    <h2 class="text-xl font-black text-[#3A75C4]">CT241 OPS</h2>
                    <div class="flex items-center gap-2 mt-1">
                        <span id="user-role" class="text-[10px] px-2 py-0.5 bg-[#FCD116] text-amber-900 font-extrabold rounded-full uppercase">...</span>
                        <span id="user-display" class="text-[11px] font-bold text-slate-400"></span>
                    </div>
                </div>
                <button id="btnOut" class="p-2 bg-red-50 text-red-500 rounded-xl">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg>
                </button>
            </div>

            <!-- NAVIGATION -->
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
            
            <!-- SECTION MISSIONS -->
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

            <!-- SECTION CREER -->
            <div id="sec-creer" class="section">
                <div class="bg-white p-6 rounded-[2rem] shadow-sm border border-slate-100">
                    <h3 class="text-xl font-black text-[#009E60] mb-6 text-center">Nouvelle Mission</h3>
                    
                    <div class="space-y-4">
                        <div>
                            <label class="text-[10px] font-black text-slate-400 uppercase ml-2">Bénéficiaire</label>
                            <input type="text" id="mNom" placeholder="Nom du client" class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent focus:border-[#009E60] outline-none">
                            <input type="tel" id="mTel" placeholder="077 / 066 ..." class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent focus:border-[#009E60] outline-none mt-2">
                        </div>

                        <div class="p-4 bg-blue-50/50 rounded-2xl border border-blue-100">
                            <label class="text-[10px] font-black text-blue-400 uppercase">Localisation & Zone</label>
                            <input type="text" id="mQuartier" list="list-q" placeholder="Quartier..." class="w-full p-3 bg-white rounded-xl mt-2 outline-none border border-blue-100" oninput="autoDetectZone()">
                            <datalist id="list-q">
                                <option value="Nzeng-Ayong">Libreville</option><option value="Louis">Libreville</option>
                                <option value="Angondjé">Zone 2000</option><option value="Okala">Akanda</option>
                                <option value="Ntoum">Ntoum</option><option value="Owendo">Owendo</option>
                            </datalist>
                            <select id="mZoneSelect" class="w-full p-3 bg-white rounded-xl mt-2 outline-none border border-blue-100 font-bold text-sm" onchange="updateFrais()">
                                <option value="libreville">Libreville (1000 F)</option>
                                <option value="peripherie">Akanda / Owendo (1500 F)</option>
                                <option value="eloignee">Zone Éloignée (2000 F)</option>
                            </select>
                        </div>

                        <div>
                            <label class="text-[10px] font-black text-slate-400 uppercase ml-2">Finances</label>
                            <input type="number" id="mRetrait" placeholder="Montant retrait (FCFA)" class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent focus:border-[#009E60] outline-none font-bold text-lg">
                            <div class="grid grid-cols-2 gap-3 mt-2">
                                <div class="bg-slate-50 p-3 rounded-2xl">
                                    <span class="text-[9px] font-bold text-slate-400 block uppercase">Com. CT241</span>
                                    <input type="number" id="mCom" value="390" class="bg-transparent font-bold outline-none w-full">
                                </div>
                                <div class="bg-slate-100 p-3 rounded-2xl">
                                    <span class="text-[9px] font-bold text-slate-400 block uppercase">Livreur</span>
                                    <input type="number" id="mLiv" value="1000" readonly class="bg-transparent font-bold text-slate-500 outline-none w-full">
                                </div>
                            </div>
                        </div>

                        <button onclick="createMission()" class="w-full py-5 bg-[#009E60] text-white font-black rounded-2xl shadow-xl shadow-green-100 mt-4 active:scale-95 transition-all">
                            DÉPLOYER LA MISSION
                        </button>
                    </div>
                </div>
            </div>

            <!-- SECTION BILAN -->
            <div id="sec-bilan" class="section">
                <h3 class="text-lg font-black mb-4">Mes Gains</h3>
                <div id="list-bilan" class="space-y-2 mb-6"></div>
                <div class="bg-slate-900 text-white p-8 rounded-[2rem] shadow-xl">
                    <p class="text-[11px] font-bold text-slate-400 uppercase tracking-widest mb-2">Total Revenus</p>
                    <h2 id="stat-total" class="text-4xl font-black text-[#FCD116]">0 F</h2>
                </div>
            </div>

            <!-- SECTION COMPTA -->
            <div id="sec-compta" class="section">
                <h3 class="text-lg font-black mb-4 uppercase text-[11px] text-slate-400">Journal des encaissements</h3>
                <div class="bg-white rounded-3xl shadow-sm border border-slate-100 overflow-hidden mb-6">
                    <table class="w-full text-left text-sm">
                        <thead class="bg-slate-50 text-[10px] font-black text-slate-400 uppercase">
                            <tr>
                                <th class="p-4">Client</th>
                                <th class="p-4 text-right">Com.</th>
                                <th class="p-4 text-right">Retrait</th>
                            </tr>
                        </thead>
                        <tbody id="list-compta"></tbody>
                    </table>
                </div>
                <div class="bg-[#009E60] p-6 rounded-3xl text-white">
                    <div class="flex justify-between items-center opacity-80 text-xs font-bold mb-1">
                        <span>TOTAL COMMISSIONS</span>
                        <span id="compta-total-com">0 F</span>
                    </div>
                    <div class="flex justify-between items-center font-black text-lg">
                        <span>VOLUME GLOBAL</span>
                        <span id="compta-total-retrait" class="text-[#FCD116]">0 F</span>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- UI ELEMENTS -->
    <input type="file" id="camInput" accept="image/*" capture="camera" class="hidden">
    <canvas id="canvas" class="hidden"></canvas>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-auth.js";
        import { getDatabase, ref, push, onValue, update, remove, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-database.js";

        // CONFIGURATION FIREBASE
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
        let activeKey = null;

        const ZONES = {
            libreville: ["nzeng", "lalala", "akebe", "akébé", "glass", "louis", "charbonnages", "oloumi"],
            peripherie: ["okala", "mikolongo", "sabliere", "owendo", "sni", "alénakiri"],
            eloignee: ["angondje", "angondjé", "ntoum", "essassa", "bikele"]
        };

        // --- AUTH LOGIC ---
        document.getElementById('btnConnect').onclick = async () => {
            const email = document.getElementById('login-email').value;
            const pass = document.getElementById('login-pass').value;
            try { await signInWithEmailAndPassword(auth, email, pass); } catch(e) { alert("Accès non autorisé."); }
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
                
                // Permission based UI
                if (userRole === 'admin' || userRole === 'finance') {
                    document.getElementById('nav-creer').classList.remove('hidden');
                    document.getElementById('nav-compta').classList.remove('hidden');
                    document.getElementById('admin-pending-box').classList.remove('hidden');
                }

                listenToMissions();
            } else {
                document.getElementById('auth-screen').classList.remove('hidden');
                document.getElementById('main-app').classList.add('hidden');
            }
        });

        // --- DATA LOGIC ---
        function listenToMissions() {
            onValue(ref(db, 'missions'), (snap) => {
                const data = snap.val();
                allMissions = data ? Object.keys(data).map(k => ({...data[k], key:k})) : [];
                render();
            });
        }

        window.autoDetectZone = () => {
            const q = document.getElementById('mQuartier').value.toLowerCase();
            const sel = document.getElementById('mZoneSelect');
            for(const [key, list] of Object.entries(ZONES)) {
                if(list.some(word => q.includes(word))) {
                    sel.value = key;
                    updateFrais();
                    break;
                }
            }
        }

        window.updateFrais = () => {
            const z = document.getElementById('mZoneSelect').value;
            const rates = { libreville: 1000, peripherie: 1500, eloignee: 2000 };
            document.getElementById('mLiv').value = rates[z];
        }

        window.createMission = () => {
            const m = {
                id: "CT" + Math.floor(1000 + Math.random()*9000),
                nom: document.getElementById('mNom').value,
                tel: document.getElementById('mTel').value,
                lieu: document.getElementById('mQuartier').value,
                retrait: parseInt(document.getElementById('mRetrait').value),
                com: parseInt(document.getElementById('mCom').value),
                fraisLivraison: parseInt(document.getElementById('mLiv').value),
                livreur: "À pourvoir",
                etape: 0, // 0: Attente Admin, 1: En cours, 2: SMS reçu, 3: Terminé
                timestamp: Date.now()
            };
            if(!m.nom || !m.retrait) return alert("Champs manquants");
            push(ref(db, 'missions'), m);
            switchTab('missions');
            // Reset form
            document.getElementById('mNom').value = ""; document.getElementById('mRetrait').value = "";
        }

        // --- RENDERING ---
        function render() {
            const pnd = document.getElementById('list-pending');
            const act = document.getElementById('list-active');
            const bil = document.getElementById('list-bilan');
            const cpt = document.getElementById('list-compta');
            
            pnd.innerHTML = ""; act.innerHTML = ""; bil.innerHTML = ""; cpt.innerHTML = "";
            
            let totalGains = 0;
            let totalCom = 0;
            let totalVolume = 0;
            const myName = auth.currentUser.email.split('@')[0].toUpperCase();

            allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
                
                // Logique Compta/Bilan
                if(m.etape === 3) {
                    if(m.livreur === myName) {
                        totalGains += m.fraisLivraison;
                        bil.innerHTML += `<div class="bg-white p-4 rounded-2xl flex justify-between items-center shadow-sm">
                            <span class="text-sm font-bold">${m.nom}</span>
                            <span class="text-green-600 font-black">+${m.fraisLivraison} F</span>
                        </div>`;
                    }
                    if(userRole === 'admin') {
                        totalCom += m.com;
                        totalVolume += m.retrait;
                        cpt.innerHTML += `<tr class="border-b border-slate-50 text-xs">
                            <td class="p-4 font-bold">${m.nom}</td>
                            <td class="p-4 text-right text-green-600 font-bold">${m.com}</td>
                            <td class="p-4 text-right">${m.retrait}</td>
                        </tr>`;
                    }
                    return;
                }

                // Cards UI
                const isMyMission = m.livreur === myName;
                const canAccept = m.etape === 1 && m.livreur === "À pourvoir";
                
                const card = `
                    <div class="bg-white p-5 rounded-[1.8rem] shadow-sm border border-slate-100 relative overflow-hidden">
                        <div class="absolute top-0 right-0 p-3">
                            <span class="text-[9px] font-black text-[#3A75C4] bg-blue-50 px-2 py-1 rounded-lg">${m.id}</span>
                        </div>
                        <h4 class="font-black text-slate-800">${m.nom}</h4>
                        <div class="text-[11px] font-bold text-slate-400 mt-1 flex flex-wrap gap-x-3">
                            <span>📍 ${m.lieu}</span>
                            <span>📞 ${m.tel}</span>
                        </div>
                        <div class="mt-4 flex items-end justify-between">
                            <div>
                                <span class="text-[9px] font-black text-slate-300 block uppercase">Cash à retirer</span>
                                <span class="text-xl font-black text-slate-900">${m.retrait.toLocaleString()} F</span>
                            </div>
                            <div class="text-right">
                                <span class="text-[9px] font-black text-slate-300 block uppercase">Livreur</span>
                                <span class="text-[11px] font-extrabold ${isMyMission?'text-green-600':'text-slate-500'}">${m.livreur}</span>
                            </div>
                        </div>

                        ${m.etape === 0 && userRole === 'admin' ? `
                            <button onclick="updateStatus('${m.key}', 1)" class="w-full mt-4 py-3 bg-[#3A75C4] text-white text-xs font-black rounded-xl uppercase">Valider & Publier</button>
                        ` : ''}

                        ${canAccept && userRole === 'livreur' ? `
                            <button onclick="assignMe('${m.key}')" class="w-full mt-4 py-3 bg-[#009E60] text-white text-xs font-black rounded-xl uppercase">Accepter la course</button>
                        ` : ''}

                        ${isMyMission && m.etape === 1 ? `
                            <div class="mt-4 pt-4 border-t border-dashed border-slate-100">
                                <button onclick="triggerPhoto('${m.key}')" class="w-full py-3 bg-slate-900 text-white text-xs font-black rounded-xl uppercase flex items-center justify-center gap-2">
                                    📸 Photo SMS
                                </button>
                                <input type="text" id="code-${m.key}" placeholder="Code de retrait" class="w-full mt-2 p-3 bg-slate-50 rounded-xl text-center font-black text-lg outline-none focus:bg-white border focus:border-blue-500">
                                <button onclick="sendToFinance('${m.key}')" class="w-full mt-2 py-3 bg-blue-500 text-white text-xs font-black rounded-xl uppercase">Envoyer à la Finance</button>
                            </div>
                        ` : ''}

                        ${m.etape === 2 && (userRole === 'admin' || userRole === 'finance') ? `
                            <div class="mt-4 p-3 bg-amber-50 rounded-2xl text-center">
                                <p class="text-[10px] font-bold text-amber-600 mb-2 uppercase">Confirmation de retrait</p>
                                <h5 class="text-2xl font-black text-amber-800 tracking-widest">${m.codeSMS}</h5>
                                <button onclick="updateStatus('${m.key}', 3)" class="w-full mt-3 py-3 bg-[#009E60] text-white text-xs font-black rounded-xl uppercase">Encaissé ✅</button>
                            </div>
                        ` : ''}
                    </div>
                `;

                if(m.etape === 0) pnd.innerHTML += card;
                else act.innerHTML += card;
            });

            document.getElementById('stat-total').innerText = totalGains.toLocaleString() + " F";
            document.getElementById('compta-total-com').innerText = totalCom.toLocaleString() + " F";
            document.getElementById('compta-total-retrait').innerText = totalVolume.toLocaleString() + " F";
            
            const openCount = allMissions.filter(x => x.etape === 1 && x.livreur === "À pourvoir").length;
            const badge = document.getElementById('count-badge');
            if(openCount > 0) { badge.innerText = openCount; badge.classList.remove('hidden'); }
            else { badge.classList.add('hidden'); }
        }

        // --- ACTIONS ---
        window.switchTab = (id) => {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('active-nav'));
            document.getElementById('sec-'+id).classList.add('active');
            document.getElementById('nav-'+id).classList.add('active-nav');
        }

        window.updateStatus = (k, s) => update(ref(db, `missions/${k}`), { etape: s });
        window.assignMe = (k) => update(ref(db, `missions/${k}`), { livreur: auth.currentUser.email.split('@')[0].toUpperCase() });
        
        window.triggerPhoto = (k) => {
            activeKey = k;
            document.getElementById('camInput').click();
        }

        document.getElementById('camInput').onchange = (e) => {
            const file = e.target.files[0];
            if(!file) return;
            // Note: En version réelle, on uploaderait sur Firebase Storage. 
            // Ici on simule par un message pour la démo Canvas.
            alert("Photo enregistrée localement.");
        }

        window.sendToFinance = (k) => {
            const code = document.getElementById('code-'+k).value;
            if(!code) return alert("Entrez le code SMS");
            update(ref(db, `missions/${k}`), { 
                etape: 2, 
                codeSMS: code,
                timestamp: Date.now()
            });
        }
    </script>
</body>
</html>
