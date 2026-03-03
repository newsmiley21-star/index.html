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
        body { 
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc;
            -webkit-tap-highlight-color: transparent;
        }
        .bg-gabon-gradient {
            background: linear-gradient(135deg, var(--gabon-vert) 0%, var(--gabon-jaune) 50%, var(--gabon-bleu) 100%);
        }
        .tab-active {
            background: white;
            color: var(--gabon-bleu);
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        .card-step-0 { border-left: 6px solid #94a3b8; } /* Attente Validation */
        .card-step-1 { border-left: 6px solid var(--gabon-bleu); } /* Disponible */
        .card-step-2 { border-left: 6px solid var(--gabon-jaune); } /* En cours */
        .card-step-3 { border-left: 6px solid #8b5cf6; } /* Photo soumise */
        .card-step-4 { border-left: 6px solid var(--gabon-vert); } /* Archivé/Payé */
        
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="min-h-screen pb-20">

    <!-- Auth Screen -->
    <div id="auth-screen" class="fixed inset-0 z-[9999] bg-gabon-gradient flex items-center justify-center p-6">
        <div class="bg-white/95 backdrop-blur-md p-8 rounded-[2rem] w-full max-w-sm shadow-2xl text-center">
            <h1 class="text-3xl font-900 text-[#009E60] mb-1">CT241 OPS</h1>
            <p class="text-xs font-semibold text-slate-500 mb-8 tracking-widest uppercase">Portail Logistique Gabon</p>
            
            <div class="space-y-4">
                <input type="email" id="login-email" placeholder="Email professionnel" class="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-[#3A75C4] outline-none transition-all">
                <input type="password" id="login-pass" placeholder="Mot de passe" class="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-[#3A75C4] outline-none transition-all">
                <button id="btnConnect" class="w-full py-4 bg-[#009E60] text-white rounded-2xl font-bold shadow-lg shadow-green-600/30 active:scale-95 transition-transform">
                    SE CONNECTER
                </button>
            </div>
            <p id="auth-error" class="text-red-500 text-[10px] mt-4 hidden"></p>
        </div>
    </div>

    <!-- Main App -->
    <div id="main-app" class="hidden max-w-md mx-auto px-4 pt-6">
        <header class="flex justify-between items-start mb-6">
            <div>
                <h2 class="text-2xl font-900 text-[#3A75C4] leading-tight">CT241 OPS</h2>
                <div class="flex items-center mt-1">
                    <span id="user-role" class="px-2 py-0.5 bg-[#FCD116] text-[#744210] text-[10px] font-bold rounded-full uppercase">...</span>
                    <span id="user-display" class="ml-2 text-[11px] font-bold text-slate-400 uppercase tracking-tight"></span>
                </div>
            </div>
            <button id="btnOut" class="p-2 bg-red-50 text-red-500 rounded-xl text-[10px] font-black active:bg-red-100 transition-colors">DÉCONNEXION</button>
        </header>

        <!-- Navigation -->
        <nav class="flex overflow-x-auto gap-2 p-1.5 bg-slate-100 rounded-2xl mb-6 no-scrollbar">
            <button onclick="switchTab('missions')" id="tab-missions" class="tab-active whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold transition-all">MISSIONS <span id="count-badge" class="hidden ml-1 px-1.5 py-0.5 bg-red-500 text-white rounded-full text-[9px]">0</span></button>
            <button onclick="switchTab('creer')" id="tab-creer" class="hidden whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold text-slate-500 transition-all">NOUVEAU</button>
            <button onclick="switchTab('bilan')" id="tab-bilan" class="whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold text-slate-500 transition-all">MON BILAN</button>
            <button onclick="switchTab('archives')" id="tab-archives" class="whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold text-slate-500 transition-all">ARCHIVES</button>
            <button onclick="switchTab('admin')" id="tab-admin" class="hidden whitespace-nowrap px-5 py-2.5 rounded-xl text-xs font-bold text-slate-500 transition-all">COMPTA</button>
        </nav>

        <main>
            <!-- Section Missions -->
            <section id="sec-missions" class="space-y-4">
                <div id="pending-validation-area" class="hidden">
                    <h3 class="text-[10px] font-black text-red-500 tracking-widest uppercase mb-3 px-1">⚠️ En attente de validation</h3>
                    <div id="list-pending" class="space-y-3"></div>
                    <div class="h-px bg-slate-200 my-6"></div>
                </div>
                
                <h3 class="text-[10px] font-black text-[#3A75C4] tracking-widest uppercase mb-3 px-1">🚀 Missions actives</h3>
                <div id="container-active" class="space-y-4"></div>
            </section>

            <!-- Section Créer -->
            <section id="sec-creer" class="hidden space-y-4">
                <div class="bg-white p-6 rounded-[2rem] shadow-sm border border-slate-100">
                    <h3 class="text-lg font-900 text-[#009E60] mb-6 uppercase tracking-tight">Nouvelle Mission</h3>
                    <div class="space-y-4">
                        <div>
                            <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Bénéficiaire</label>
                            <input type="text" id="mNom" placeholder="Nom complet" class="w-full p-4 bg-slate-50 rounded-xl mt-1 border border-transparent focus:border-[#009E60] outline-none">
                        </div>
                        <div>
                            <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Téléphone</label>
                            <input type="tel" id="mTel" placeholder="077 XX XX XX" class="w-full p-4 bg-slate-50 rounded-xl mt-1 outline-none">
                        </div>
                        <div class="bg-blue-50/50 p-4 rounded-2xl border border-blue-100">
                            <label class="text-[10px] font-bold text-[#3A75C4] uppercase ml-1">Quartier & Zone</label>
                            <input type="text" id="mQuartier" list="quartiers" placeholder="Ex: Nzeng-Ayong" oninput="detectZone()" class="w-full p-4 bg-white rounded-xl mt-1 outline-none border border-blue-100">
                            <select id="mZone" onchange="applyTarif()" class="w-full p-4 bg-white rounded-xl mt-2 outline-none border border-blue-100 text-sm font-semibold">
                                <option value="">Choisir la zone...</option>
                                <option value="lbv">Libreville (1000 F)</option>
                                <option value="peri">Akanda / Owendo (1500 F)</option>
                                <option value="far">Ntoum / Essassa (2000 F)</option>
                            </select>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Montant Retrait</label>
                                <input type="number" id="mRetrait" placeholder="FCFA" class="w-full p-4 bg-slate-50 rounded-xl mt-1 outline-none">
                            </div>
                            <div>
                                <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Commission Fixe</label>
                                <input type="text" value="400 F" readonly class="w-full p-4 bg-amber-50 text-amber-600 rounded-xl mt-1 outline-none font-bold text-center">
                            </div>
                        </div>
                        <div>
                             <label class="text-[10px] font-bold text-slate-400 uppercase ml-1">Frais Livraison</label>
                             <input type="number" id="mLiv" readonly class="w-full p-4 bg-slate-200 text-slate-500 rounded-xl mt-1 outline-none font-bold">
                        </div>
                        <button id="btnSubmitMission" class="w-full py-4 bg-[#009E60] text-white rounded-2xl font-black shadow-lg shadow-green-600/20 mt-4 active:scale-95 transition-all">DÉPLOYER LA MISSION</button>
                    </div>
                </div>
            </section>

            <!-- Section Bilan (Pour Livreur) -->
            <section id="sec-bilan" class="hidden space-y-4">
                <div class="bg-[#1a252f] p-6 rounded-[2rem] text-white mb-6">
                    <p class="text-[10px] font-black text-slate-400 tracking-widest uppercase mb-1">Mes Gains Totaux</p>
                    <h4 id="stat-total" class="text-3xl font-900 text-[#FCD116]">0 F</h4>
                </div>
                <div id="list-bilan" class="bg-white rounded-[2rem] divide-y divide-slate-50 overflow-hidden shadow-sm border border-slate-100"></div>
            </section>

            <!-- Section Archives -->
            <section id="sec-archives" class="hidden space-y-4">
                <h3 class="text-[10px] font-black text-slate-400 tracking-widest uppercase px-1">Historique des missions terminées</h3>
                <div id="list-archives" class="space-y-3"></div>
            </section>

            <!-- Section Admin (Comptabilité) -->
            <section id="sec-admin" class="hidden space-y-4">
                 <div class="bg-[#009E60] p-6 rounded-[2rem] text-white mb-6 grid grid-cols-2 gap-4">
                    <div>
                        <p class="text-[9px] font-black text-white/70 uppercase">Total Commissions</p>
                        <h4 id="adm-total-com" class="text-xl font-900">0 F</h4>
                    </div>
                    <div>
                        <p class="text-[9px] font-black text-white/70 uppercase">Volume Retraits</p>
                        <h4 id="adm-total-ret" class="text-xl font-900 text-[#FCD116]">0 F</h4>
                    </div>
                </div>
                <div class="overflow-x-auto bg-white rounded-[2rem] border border-slate-100 shadow-sm">
                    <table class="w-full text-left text-xs">
                        <thead class="bg-slate-50 text-slate-400 font-bold uppercase">
                            <tr>
                                <th class="p-4">Bénéficiaire</th>
                                <th class="p-4">Retrait</th>
                                <th class="p-4">Com.</th>
                                <th class="p-4">Livreur</th>
                            </tr>
                        </thead>
                        <tbody id="list-admin-rows" class="divide-y divide-slate-50"></tbody>
                    </table>
                </div>
            </section>
        </main>
    </div>

    <!-- Modals & Utilities -->
    <input type="file" id="camInput" accept="image/*" capture="camera" class="hidden">
    <canvas id="canvas" class="hidden"></canvas>
    
    <datalist id="quartiers">
        <option value="Nzeng-Ayong">Libreville</option>
        <option value="Akébé">Libreville</option>
        <option value="Angondjé">Akanda</option>
        <option value="Owendo">Peripherie</option>
        <option value="Ntoum">Zone Eloignée</option>
    </datalist>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, updateDoc, doc, deleteDoc, serverTimestamp, orderBy } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAPCKRy9NTo4X8nn8YpxAbPtX8SlKj-7sQ",
            authDomain: "cashtransfert-21.firebaseapp.com",
            projectId: "cashtransfert-21",
            storageBucket: "cashtransfert-21.firebasestorage.app",
            messagingSenderId: "564831743134",
            appId: "1:564831743134:web:c22a1f53707f0f1dd9df8f"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = "ct241-logistics-v1";

        let userState = { role: 'livreur', email: '', uid: '', shortName: '' };
        let currentPhotoMissionId = null;

        // AUTH
        document.getElementById('btnConnect').onclick = async () => {
            const email = document.getElementById('login-email').value;
            const pass = document.getElementById('login-pass').value;
            const errEl = document.getElementById('auth-error');
            errEl.classList.add('hidden');
            
            try {
                await signInWithEmailAndPassword(auth, email, pass);
            } catch (e) { 
                errEl.innerText = "Email ou mot de passe incorrect";
                errEl.classList.remove('hidden');
            }
        };

        document.getElementById('btnOut').onclick = () => signOut(auth);

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                const email = user.email ? user.email.toLowerCase() : 'invite@ct241.com';
                userState = {
                    uid: user.uid,
                    email: email,
                    role: email.includes('admin') ? 'admin' : (email.includes('finance') ? 'finance' : 'livreur'),
                    shortName: email.split('@')[0].toUpperCase()
                };
                
                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('main-app').classList.remove('hidden');
                document.getElementById('user-role').innerText = userState.role;
                document.getElementById('user-display').innerText = userState.shortName;

                // Affichage conditionnel des onglets
                if (userState.role !== 'livreur') {
                    document.getElementById('tab-creer').classList.remove('hidden');
                    document.getElementById('pending-validation-area').classList.remove('hidden');
                }
                if (userState.role === 'admin') {
                    document.getElementById('tab-admin').classList.remove('hidden');
                }
                
                initDataSync();
            } else {
                document.getElementById('auth-screen').classList.remove('hidden');
                document.getElementById('main-app').classList.add('hidden');
            }
        });

        function initDataSync() {
            // Utilisation du chemin sécurisé requis par les instructions
            const missionsCol = collection(db, "artifacts", appId, "public", "data", "missions");
            const q = query(missionsCol, orderBy("createdAt", "desc"));
            
            onSnapshot(q, (snapshot) => {
                const missions = [];
                snapshot.forEach(doc => missions.push({ id: doc.id, ...doc.data() }));
                renderUI(missions);
            }, (err) => console.error("Sync Error:", err));
        }

        function renderUI(data) {
            const listPending = document.getElementById('list-pending');
            const listActive = document.getElementById('container-active');
            const listBilan = document.getElementById('list-bilan');
            const listArchives = document.getElementById('list-archives');
            const listAdminRows = document.getElementById('list-admin-rows');
            
            // Stats
            let stats = { myGains: 0, admCom: 0, admRet: 0, openMissions: 0 };
            
            // Clear all
            [listPending, listActive, listBilan, listArchives, listAdminRows].forEach(el => el.innerHTML = "");

            data.forEach(m => {
                const isFinished = m.step === 4;
                
                // Logic pour BILAN & ADMIN (Missions terminées)
                if (isFinished) {
                    if (m.livreurId === userState.uid) {
                        stats.myGains += (m.fraisLiv || 0);
                        listBilan.innerHTML += `
                            <div class="p-4 flex justify-between items-center">
                                <div><p class="font-bold text-slate-800">${m.nom}</p><p class="text-[9px] text-slate-400">${m.lieu}</p></div>
                                <span class="font-black text-[#009E60]">+${(m.fraisLiv || 0).toLocaleString()} F</span>
                            </div>`;
                    }
                    if (userState.role === 'admin') {
                        stats.admCom += (m.com || 0);
                        stats.admRet += (m.retrait || 0);
                        listAdminRows.innerHTML += `
                            <tr>
                                <td class="p-4 font-bold">${m.nom}</td>
                                <td class="p-4">${(m.retrait || 0).toLocaleString()} F</td>
                                <td class="p-4 font-black text-[#009E60]">${(m.com || 0)} F</td>
                                <td class="p-4 text-slate-400 uppercase font-bold">${m.livreur || '-'}</td>
                            </tr>`;
                    }
                    
                    // Rubrique ARCHIVES
                    listArchives.innerHTML += `
                        <div class="bg-white p-4 rounded-2xl border border-slate-100 flex justify-between items-center">
                            <div>
                                <p class="text-[11px] font-black text-slate-800">${m.nom}</p>
                                <p class="text-[9px] text-slate-400 uppercase tracking-tighter">${m.lieu} • ${m.idCT}</p>
                            </div>
                            <div class="text-right">
                                <p class="text-[10px] font-bold text-green-600">COMPLÉTÉ</p>
                                <p class="text-[9px] text-slate-300">${m.livreur}</p>
                            </div>
                        </div>`;
                }

                // Logic pour MISSIONS ACTIVES
                const cardBase = `
                    <div class="bg-white p-5 rounded-[2rem] shadow-sm border border-slate-100 card-step-${m.step} relative text-left">
                        <div class="flex justify-between items-start mb-2">
                            <h4 class="font-900 text-slate-800">${m.nom}</h4>
                            <span class="text-[9px] font-black bg-slate-100 px-2 py-1 rounded-lg text-slate-400">${m.idCT}</span>
                        </div>
                        <div class="text-xs space-y-1">
                            <p>📍 ${m.lieu}</p>
                            <p class="font-bold text-[#3A75C4]">💰 Retrait: ${(m.retrait || 0).toLocaleString()} F</p>
                            <p class="text-[10px] text-amber-600 font-bold">💎 Commission CT: ${m.com || 400} F</p>
                        </div>
                `;

                // STEP 0 : En attente de validation admin
                if (m.step === 0 && userState.role !== 'livreur') {
                    listPending.innerHTML += cardBase + `
                        <div class="flex gap-2 mt-4">
                            <button onclick="updateStep('${m.id}', 1)" class="flex-1 py-3 bg-[#3A75C4] text-white text-[10px] font-black rounded-xl">VALIDER</button>
                            <button onclick="deleteM('${m.id}')" class="px-4 py-3 bg-red-50 text-red-500 text-[10px] font-black rounded-xl">SUPPR</button>
                        </div></div>`;
                } 
                // STEP 1 : Disponible pour les livreurs
                else if (m.step === 1) {
                    stats.openMissions++;
                    const action = userState.role === 'livreur' ? 
                        `<button onclick="assignMe('${m.id}')" class="w-full mt-4 py-3 bg-[#3A75C4] text-white text-[10px] font-black rounded-xl shadow-lg shadow-blue-500/20">ACCEPTER LA MISSION</button>` : 
                        `<p class="mt-4 text-[9px] text-slate-300 italic text-center">Disponible pour les livreurs...</p>`;
                    listActive.innerHTML += cardBase + action + `</div>`;
                } 
                // STEP 2 : En cours (Livreur a accepté)
                else if (m.step === 2 && (m.livreurId === userState.uid || userState.role !== 'livreur')) {
                    const action = m.livreurId === userState.uid ? 
                        `<button onclick="triggerCamera('${m.id}')" class="w-full mt-4 py-3 bg-slate-800 text-white text-[10px] font-black rounded-xl">📸 PHOTO SMS</button>
                         <div id="upload-${m.id}" class="hidden mt-3 space-y-2">
                            <img id="img-${m.id}" class="w-full rounded-xl border-2 border-slate-100">
                            <input id="code-${m.id}" type="text" placeholder="CODE SMS REÇU" class="w-full p-3 bg-slate-50 rounded-xl text-center font-black text-lg border focus:border-green-500 outline-none">
                            <button onclick="submitCompletion('${m.id}')" class="w-full py-3 bg-[#009E60] text-white text-[10px] font-black rounded-xl">ENVOYER POUR PAIEMENT</button>
                         </div>` : 
                        `<div class="mt-4 flex items-center justify-center gap-2 py-2 bg-amber-50 rounded-xl">
                            <span class="w-2 h-2 bg-amber-500 rounded-full animate-pulse"></span>
                            <p class="text-[9px] text-amber-500 font-bold uppercase">En cours par ${m.livreur}</p>
                         </div>`;
                    listActive.innerHTML += cardBase + action + `</div>`;
                } 
                // STEP 3 : Photo soumise, en attente encaissement final
                else if (m.step === 3 && userState.role !== 'livreur') {
                    listActive.innerHTML += cardBase + `
                        <div class="mt-4 p-3 bg-green-50 rounded-2xl border border-green-100 text-center">
                            <img src="${m.photo}" class="w-full rounded-xl mb-3 border border-green-200">
                            <p class="text-[9px] font-bold text-slate-400 uppercase">Code SMS</p>
                            <p class="text-2xl font-black text-[#009E60] mb-3 tracking-widest">${m.codeSMS}</p>
                            <button onclick="updateStep('${m.id}', 4)" class="w-full py-3 bg-[#009E60] text-white text-[10px] font-black rounded-xl">VALIDER L'ENCAISSEMENT ✅</button>
                        </div></div>`;
                }
            });

            // Update Global Displays
            document.getElementById('stat-total').innerText = stats.myGains.toLocaleString() + " F";
            document.getElementById('adm-total-com').innerText = stats.admCom.toLocaleString() + " F";
            document.getElementById('adm-total-ret').innerText = stats.admRet.toLocaleString() + " F";
            
            const badge = document.getElementById('count-badge');
            if(stats.openMissions > 0) { 
                badge.innerText = stats.openMissions; 
                badge.classList.remove('hidden'); 
            } else { 
                badge.classList.add('hidden'); 
            }
        }

        // NAVIGATION LOGIC
        window.switchTab = (id) => {
            ['missions', 'creer', 'bilan', 'archives', 'admin'].forEach(s => {
                const sec = document.getElementById('sec-' + s);
                const tab = document.getElementById('tab-' + s);
                if(sec) sec.classList.toggle('hidden', s !== id);
                if(tab) {
                    tab.classList.toggle('tab-active', s === id);
                    tab.classList.toggle('text-slate-500', s !== id);
                }
            });
        };

        // BUSINESS LOGIC
        window.detectZone = () => {
            const v = document.getElementById('mQuartier').value.toLowerCase();
            const z = document.getElementById('mZone');
            if(v.includes('nzeng') || v.includes('akebe') || v.includes('loué')) z.value = 'lbv';
            else if(v.includes('angondje') || v.includes('ntoum') || v.includes('essassa')) z.value = 'far';
            else if(v.includes('owendo') || v.includes('okala')) z.value = 'peri';
            applyTarif();
        };

        window.applyTarif = () => {
            const tarifs = { lbv: 1000, peri: 1500, far: 2000 };
            document.getElementById('mLiv').value = tarifs[document.getElementById('mZone').value] || 1000;
        };

        document.getElementById('btnSubmitMission').onclick = async () => {
            if(!auth.currentUser) return;
            const nom = document.getElementById('mNom').value;
            const ret = parseInt(document.getElementById('mRetrait').value);
            const quartier = document.getElementById('mQuartier').value;
            const liv = parseInt(document.getElementById('mLiv').value);
            
            if(!nom || !ret || !quartier) return alert("Champs obligatoires !");

            try {
                const missionsCol = collection(db, "artifacts", appId, "public", "data", "missions");
                await addDoc(missionsCol, {
                    idCT: "CT" + Math.floor(1000 + Math.random() * 9000),
                    nom,
                    tel: document.getElementById('mTel').value,
                    lieu: quartier,
                    retrait: ret,
                    fraisLiv: liv,
                    com: 400,
                    livreur: "Libre",
                    livreurId: "",
                    step: 0,
                    createdAt: serverTimestamp()
                });
                alert("Mission déployée avec succès !");
                switchTab('missions');
                // Reset form
                document.getElementById('mNom').value = "";
                document.getElementById('mRetrait').value = "";
            } catch(e) { console.error(e); }
        };

        // FIRESTORE ACTIONS
        window.updateStep = async (id, step) => {
            if(!auth.currentUser) return;
            const docRef = doc(db, "artifacts", appId, "public", "data", "missions", id);
            await updateDoc(docRef, { step });
        };

        window.assignMe = async (id) => {
            if(!auth.currentUser) return;
            const docRef = doc(db, "artifacts", appId, "public", "data", "missions", id);
            await updateDoc(docRef, { 
                step: 2, 
                livreur: userState.shortName, 
                livreurId: userState.uid 
            });
        };

        window.deleteM = async (id) => {
            if(!auth.currentUser) return;
            if(confirm("Confirmer la suppression ?")) {
                const docRef = doc(db, "artifacts", appId, "public", "data", "missions", id);
                await deleteDoc(docRef);
            }
        };

        // CAMERA HANDLING
        window.triggerCamera = (id) => {
            currentPhotoMissionId = id;
            document.getElementById('camInput').click();
        };

        document.getElementById('camInput').onchange = (e) => {
            const file = e.target.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = (event) => {
                const img = new Image();
                img.onload = () => {
                    const canvas = document.getElementById('canvas');
                    const ctx = canvas.getContext('2d');
                    // Resize for performance/storage
                    canvas.width = 600;
                    canvas.height = img.height * (600 / img.width);
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    
                    const dataUrl = canvas.toDataURL('image/jpeg', 0.5);
                    document.getElementById('img-' + currentPhotoMissionId).src = dataUrl;
                    document.getElementById('upload-' + currentPhotoMissionId).classList.remove('hidden');
                };
                img.src = event.target.result;
            };
            reader.readAsDataURL(file);
        };

        window.submitCompletion = async (id) => {
            if(!auth.currentUser) return;
            const code = document.getElementById('code-' + id).value;
            const photo = document.getElementById('img-' + id).src;
            if(!code) return alert("Le code SMS est obligatoire !");
            
            const docRef = doc(db, "artifacts", appId, "public", "data", "missions", id);
            await updateDoc(docRef, { 
                step: 3, 
                codeSMS: code, 
                photo: photo 
            });
        };

    </script>
</body>
</html>
