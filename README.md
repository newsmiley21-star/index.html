<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - LOGISTIQUE GABON</title>
    
    <meta name="theme-color" content="#009E60">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    
    <style>
        :root {
            --gabon-vert: #009E60; 
            --gabon-jaune: #FCD116; 
            --gabon-bleu: #3A75C4;
            --danger: #e74c3c; 
            --dark: #1a252f; 
            --white: #ffffff;
            --gray-bg: #f4f7f6;
        }
        
        body { 
            font-family: 'Inter', system-ui, sans-serif; 
            background: var(--gray-bg);
            margin: 0; padding: 10px; color: var(--dark); min-height: 100vh;
        }
        
        #auth-screen { 
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: linear-gradient(45deg, var(--gabon-vert), var(--gabon-jaune), var(--gabon-bleu));
            display: flex; align-items: center; justify-content: center; z-index: 9999; 
        }
        .login-card { 
            background: rgba(255, 255, 255, 0.98); 
            padding: 40px 30px; border-radius: 28px; width: 85%; max-width: 350px; 
            text-align: center; box-shadow: 0 25px 50px rgba(0,0,0,0.2);
        }

        input, select { 
            width: 100%; padding: 15px; margin: 8px 0; border: 2px solid #edf2f7; 
            border-radius: 14px; box-sizing: border-box; font-size: 16px; transition: 0.3s;
            background: #f8fafc;
        }
        
        .btn-login { 
            width: 100%; padding: 16px; background: var(--gabon-vert); color: white; 
            border: none; border-radius: 14px; font-weight: 800; cursor: pointer; 
        }

        #main-app { 
            display: none; max-width: 500px; margin: auto; background: var(--white); 
            border-radius: 30px; padding: 20px; box-shadow: 0 15px 35px rgba(0,0,0,0.06); 
            min-height: 90vh;
        }

        header { 
            display: flex; justify-content: space-between; align-items: flex-start; 
            margin-bottom: 20px; border-bottom: 1px solid #f1f5f9; padding-bottom: 15px;
        }

        nav { 
            display: flex; overflow-x: auto; gap: 8px; margin-bottom: 20px; 
            padding: 5px; background: #f1f5f9; border-radius: 15px;
        }
        nav button { 
            flex: 1; padding: 10px; border: none; border-radius: 10px; 
            background: transparent; font-weight: 700; font-size: 11px; color: #64748b; 
        }
        nav button.active { background: var(--white); color: var(--gabon-bleu); box-shadow: 0 4px 10px rgba(0,0,0,0.05); }

        .section { display: none; }
        .active-sec { display: block; }

        .card { 
            background: var(--white); border: 1px solid #f1f5f9; padding: 15px; 
            border-radius: 18px; margin-bottom: 12px; border-left: 6px solid #cbd5e1;
        }
        .card.etape-1 { border-left-color: var(--gabon-bleu); }
        .card.etape-3 { border-left-color: var(--gabon-vert); }

        .btn-action { 
            width: 100%; padding: 14px; border: none; border-radius: 12px; 
            font-weight: 800; cursor: pointer; margin-top: 10px;
        }
        .btn-validate { background: var(--gabon-vert); color: white; }
        
        .stats-banner {
            background: var(--dark); color: white; padding: 20px; border-radius: 20px;
            margin-bottom: 20px;
        }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 10px; }
        .stats-banner small { color: #94a3b8; font-size: 10px; text-transform: uppercase; display: block; }
        .stats-banner b { font-size: 18px; color: var(--gabon-jaune); }

        .date-divider {
            padding: 8px 15px; background: #f8fafc; border-radius: 10px;
            font-size: 11px; font-weight: 800; color: #64748b; margin: 25px 0 10px 0;
            border: 1px solid #e2e8f0; display: flex; justify-content: space-between;
        }

        .zone-highlight { background: rgba(58, 117, 196, 0.05); padding: 12px; border-radius: 15px; margin: 10px 0; border: 1px dashed var(--gabon-bleu); }
        .finance-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .label-mini { font-size: 10px; font-weight: 800; color: #64748b; margin-left: 5px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #64748b; margin-bottom: 20px;">PORTAIL LOGISTIQUE</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-bleu)">CT241 OPS</h3>
                <span id="user-role" style="font-size:10px; padding:3px 8px; border-radius:10px; background:var(--gabon-jaune); font-weight:800">...</span>
            </div>
            <button id="btnOut" style="background:none; color:var(--danger); border:1px solid var(--danger); padding:5px 10px; border-radius:8px; font-size:10px;">DECONNEXION</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">EN COURS <span id="count-missions"></span></button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">MON BILAN</button>
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">NOUVEAU</button>
            <button onclick="ouvrir('compta')" id="nav-compta" style="display:none">ADMIN</button>
        </nav>

        <!-- MISSIONS -->
        <div id="sec-missions" class="section active-sec">
            <div id="div-validation" style="display:none; margin-bottom:20px;">
                <h6 style="color:var(--danger); margin:0 0 10px 0; font-size:10px">EN ATTENTE DE VALIDATION</h6>
                <div id="list-validation"></div>
            </div>
            <h6 style="color:var(--gabon-bleu); margin:0 0 10px 0; font-size:10px">MISSIONS SUR LE TERRAIN</h6>
            <div id="list-active"></div>
        </div>

        <!-- BILAN -->
        <div id="sec-bilan" class="section">
            <div class="stats-banner">
                <small>Session du jour (07h - 20h)</small>
                <div class="stats-grid">
                    <div><small>Gains Livreur</small><b id="stat-total">0 F</b></div>
                    <div><small>Missions</small><b id="stat-count" style="color:white">0</b></div>
                </div>
            </div>
            <div id="list-bilan-today"></div>
            <div id="archive-history"></div>
        </div>

        <!-- CREER -->
        <div id="sec-creer" class="section">
            <h4 style="margin:0 0-15px 0; color:var(--gabon-vert)">DÉPLOYER UNE MISSION</h4>
            <input type="text" id="mNom" placeholder="Nom du bénéficiaire">
            <input type="tel" id="mTel" placeholder="Téléphone">
            
            <div class="zone-highlight">
                <span class="label-mini">Zone & Localisation</span>
                <input type="text" id="mQuartier" placeholder="Quartier..." list="g-quartiers">
                <select id="mZoneSelect" onchange="updateFrais()">
                    <option value="1000">Libreville (1000 F)</option>
                    <option value="1500">Owendo/Akanda (1500 F)</option>
                    <option value="2000">Angondjé/Ntoum (2000 F)</option>
                </select>
            </div>

            <span class="label-mini">Détails financiers</span>
            <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)">
            <div class="finance-row">
                <div>
                    <span class="label-mini">Livraison (F)</span>
                    <input type="number" id="mLiv" value="1000" readonly style="background:#f1f5f9">
                </div>
                <div>
                    <span class="label-mini">Commission (F)</span>
                    <input type="number" id="mCom" value="390">
                </div>
            </div>
            <button onclick="creerMission()" class="btn-action btn-validate">LANCER LA MISSION</button>
        </div>

        <!-- COMPTA ADMIN -->
        <div id="sec-compta" class="section">
            <div class="stats-banner" style="background:var(--gabon-vert)">
                <small>Comptabilité Admin Aujourd'hui</small>
                <div class="stats-grid">
                    <div><small>Total Commissions</small><b id="cpt-com" style="color:white">0 F</b></div>
                    <div><small>Volume Retraits</small><b id="cpt-vol" style="color:var(--gabon-jaune)">0 F</b></div>
                </div>
            </div>
            <div id="list-compta-daily"></div>
        </div>
    </div>

    <input type="file" id="camInput" accept="image/*" capture="camera" style="display:none">
    <canvas id="canvas" style="display:none"></canvas>

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
    let currentKey = null;
    let lastPhotoData = "";

    const H_START = 7;
    const H_END = 20;

    // --- AUTH ---
    document.getElementById('btnConnect').onclick = async () => {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        try { await signInWithEmailAndPassword(auth, email, pass); } catch(e) { alert("Erreur d'accès"); }
    };
    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        if(u) {
            const email = u.email.toLowerCase();
            userRole = email.includes('admin') ? "admin" : (email.includes('finance') ? "finance" : "livreur");
            document.getElementById('user-role').innerText = userRole.toUpperCase();
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            
            document.getElementById('nav-creer').style.display = (userRole !== 'livreur') ? 'block' : 'none';
            document.getElementById('nav-compta').style.display = (userRole === 'admin') ? 'block' : 'none';
            document.getElementById('div-validation').style.display = (userRole === 'admin') ? 'block' : 'none';

            onValue(ref(db, 'missions'), (snap) => {
                const data = snap.val();
                allMissions = data ? Object.keys(data).map(k => ({...data[k], key:k})) : [];
                renderUI();
            });
        } else {
            document.getElementById('auth-screen').style.display = 'flex';
            document.getElementById('main-app').style.display = 'none';
        }
    });

    // --- LOGIQUE RENDU ---
    window.updateFrais = () => {
        document.getElementById('mLiv').value = document.getElementById('mZoneSelect').value;
    };

    window.renderUI = () => {
        const listVal = document.getElementById('list-validation');
        const listAct = document.getElementById('list-active');
        const listBilToday = document.getElementById('list-bilan-today');
        const listHistory = document.getElementById('archive-history');
        const listCpt = document.getElementById('list-compta-daily');
        
        listVal.innerHTML = ""; listAct.innerHTML = ""; listBilToday.innerHTML = ""; 
        listHistory.innerHTML = ""; listCpt.innerHTML = "";

        const now = new Date();
        const todayStr = now.toLocaleDateString('fr-FR');
        const currentHour = now.getHours();
        const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "";

        let dailyGains = 0;
        let dailyCount = 0;
        let adminCom = 0;
        let adminVol = 0;
        const historyGroups = {};

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            const mDate = new Date(m.timestamp);
            const mDateStr = mDate.toLocaleDateString('fr-FR');
            const mHour = mDate.getHours();

            const isToday = (mDateStr === todayStr);
            const isSessionActive = isToday && (mHour >= H_START && mHour < H_END);

            if(m.etape < 3) {
                const card = createCard(m, myName);
                if(m.etape === 0 && userRole === 'admin') listVal.innerHTML += card;
                else if(m.etape > 0) listAct.innerHTML += card;
            } else {
                // Pour le livreur
                if(m.livreur === myName) {
                    if(isSessionActive) {
                        dailyGains += m.fraisLivraison;
                        dailyCount++;
                        listBilToday.innerHTML += createRow(m, "livreur");
                    } else {
                        if(!historyGroups[mDateStr]) historyGroups[mDateStr] = { sum: 0, items: [] };
                        historyGroups[mDateStr].sum += m.fraisLivraison;
                        historyGroups[mDateStr].items.push(m);
                    }
                }
                // Pour l'admin
                if(userRole === 'admin' && isToday) {
                    adminCom += m.com;
                    adminVol += m.retrait;
                    listCpt.innerHTML += createRow(m, "admin");
                }
            }
        });

        Object.keys(historyGroups).forEach(date => {
            let html = `<div class="date-divider"><span>📅 ${date}</span> <span>${historyGroups[date].sum} F</span></div>`;
            historyGroups[date].items.forEach(item => html += createRow(item, "livreur", true));
            listHistory.innerHTML += html;
        });

        document.getElementById('stat-total').innerText = dailyGains.toLocaleString() + " F";
        document.getElementById('stat-count').innerText = dailyCount;
        if(userRole === 'admin') {
            document.getElementById('cpt-com').innerText = adminCom.toLocaleString() + " F";
            document.getElementById('cpt-vol').innerText = adminVol.toLocaleString() + " F";
        }
    };

    function createCard(m, myName) {
        let btn = "";
        if(m.etape === 0 && userRole === 'admin') btn = `<button class="btn-action btn-validate" onclick="valider('${m.key}')">VALIDER & PUBLIER</button>`;
        else if(m.etape === 1) {
            if(m.livreur === "Libre" && userRole === 'livreur') btn = `<button class="btn-action btn-validate" style="background:var(--gabon-bleu)" onclick="accepter('${m.key}')">ACCEPTER LA COURSE</button>`;
            else if(m.livreur === myName) btn = `<button class="btn-action" style="background:#000; color:#fff" onclick="triggerCam('${m.key}')">📸 PHOTO SMS</button>
                                                <input type="text" id="code-${m.key}" placeholder="Code SMS">
                                                <button class="btn-action btn-validate" onclick="terminer('${m.key}')">ENVOYER</button>`;
        } else if(m.etape === 2 && (userRole === 'admin' || userRole === 'finance')) {
            btn = `<div style="text-align:center; padding:10px; background:#f8fafc; border-radius:10px">
                    <img src="${m.photo}" style="width:100%; border-radius:8px">
                    <h2 style="margin:5px 0">${m.codeSMS}</h2>
                    <button class="btn-action btn-validate" onclick="cloturer('${m.key}')">ENCAISSÉ ✅</button>
                   </div>`;
        }

        return `<div class="card etape-${m.etape}">
                    <div style="display:flex; justify-content:space-between; font-weight:800; font-size:13px">
                        <span>${m.nom}</span> <span style="color:var(--gabon-bleu)">#${m.id}</span>
                    </div>
                    <div style="font-size:12px; color:#475569; margin:5px 0">
                        📍 ${m.lieu}<br>
                        💰 Retrait: <b>${m.retrait} F</b> | 🛵 Livraison: <b>${m.fraisLivraison} F</b>
                    </div>
                    ${btn}
                </div>`;
    }

    function createRow(m, type, isOld = false) {
        const val = type === 'admin' ? m.com : m.fraisLivraison;
        const sub = type === 'admin' ? `Liv: ${m.fraisLivraison}F` : `Com: ${m.com}F`;
        const color = isOld ? '#94a3b8' : (type === 'admin' ? 'var(--gabon-bleu)' : 'var(--gabon-vert)');
        return `<div style="padding:12px; border-bottom:1px solid #f1f5f9; display:flex; justify-content:space-between; align-items:center; font-size:11px">
                    <div><b>${m.nom}</b><br><small style="color:#94a3b8">${sub} | ${m.dateHeure}</small></div>
                    <b style="color:${color}; font-size:13px">+ ${val} F</b>
                </div>`;
    }

    // --- ACTIONS ---
    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const mnt = parseInt(document.getElementById('mRetrait').value);
        const liv = parseInt(document.getElementById('mLiv').value);
        const com = parseInt(document.getElementById('mCom').value);
        if(!nom || !mnt) return alert("Nom et Montant requis");
        
        push(ref(db, 'missions'), {
            id: Math.floor(1000 + Math.random()*9000),
            nom, tel: document.getElementById('mTel').value, 
            lieu: document.getElementById('mQuartier').value,
            retrait: mnt, fraisLivraison: liv, com: com,
            livreur: "Libre", etape: 0,
            timestamp: Date.now(),
            dateHeure: new Date().toLocaleTimeString('fr-FR', {hour:'2-digit', minute:'2-digit'})
        });
        ouvrir('missions');
    };

    window.valider = (k) => update(ref(db, `missions/${k}`), { etape: 1 });
    window.accepter = (k) => update(ref(db, `missions/${k}`), { livreur: auth.currentUser.email.split('@')[0].toUpperCase() });
    window.triggerCam = (k) => { currentKey = k; document.getElementById('camInput').click(); };
    window.terminer = (k) => {
        const c = document.getElementById('code-'+k).value;
        if(!c || !lastPhotoData) return alert("Code + Photo requis");
        update(ref(db, `missions/${k}`), { etape: 2, codeSMS: c, photo: lastPhotoData });
    };
    window.cloturer = (k) => update(ref(db, `missions/${k}`), { etape: 3 });
    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('nav-'+id).classList.add('active');
    };

    document.getElementById('camInput').onchange = (e) => {
        const file = e.target.files[0];
        const reader = new FileReader();
        reader.onload = (re) => {
            const img = new Image();
            img.onload = () => {
                const can = document.getElementById('canvas');
                can.width = 400; can.height = 400 * (img.height/img.width);
                can.getContext('2d').drawImage(img, 0, 0, 400, can.height);
                lastPhotoData = can.toDataURL('image/jpeg', 0.5);
                alert("Photo OK");
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };
</script>
</body>
</html>
