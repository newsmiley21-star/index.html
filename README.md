<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - LOGISTIQUE GABON</title>
    
    <!-- CONFIGURATION PWA & MOBILE -->
    <meta name="theme-color" content="#009E60">
    <meta name="apple-mobile-web-app-capable" content="yes"> 
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="CT241 OPS">
    
    <style>
        :root {
            --gabon-vert: #009E60; 
            --gabon-jaune: #FCD116; 
            --gabon-bleu: #3A75C4;
            --gabon-vert-light: rgba(0, 158, 96, 0.1);
            --gabon-bleu-light: rgba(58, 117, 196, 0.1);
            --danger: #e74c3c; 
            --dark: #1a252f; 
            --white: #ffffff;
        }
        
        body { 
            font-family: 'Inter', system-ui, -apple-system, sans-serif; 
            background: linear-gradient(135deg, #f4f7f6 0%, #eef2f3 100%);
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
            backdrop-filter: blur(10px);
        }
        .login-card h2 { color: var(--gabon-vert); font-weight: 900; margin-bottom: 5px; font-size: 26px; }

        input, select { 
            width: 100%; padding: 15px; margin: 8px 0; border: 2px solid #edf2f7; 
            border-radius: 14px; box-sizing: border-box; font-size: 16px; transition: 0.3s;
            background: #f8fafc;
        }
        input:focus, select:focus { border-color: var(--gabon-bleu); outline: none; background: white; box-shadow: 0 0 0 4px var(--gabon-bleu-light); }
        
        .btn-login { 
            width: 100%; padding: 16px; background: var(--gabon-vert); color: white; 
            border: none; border-radius: 14px; font-weight: 800; cursor: pointer; 
            font-size: 16px; margin-top: 15px; box-shadow: 0 6px 15px rgba(0, 158, 96, 0.3);
        }

        #main-app { 
            display: none; max-width: 500px; margin: auto; background: var(--white); 
            border-radius: 30px; padding: 25px; box-shadow: 0 15px 35px rgba(0,0,0,0.06); 
            min-height: 90vh; position: relative;
        }

        header { 
            display: flex; justify-content: space-between; align-items: flex-start; 
            margin-bottom: 25px; border-bottom: 1px solid #f1f5f9; padding-bottom: 15px;
        }
        header h3 { margin: 0; font-size: 22px; font-weight: 900; color: var(--gabon-bleu); }
        .role-badge { font-size: 10px; padding: 5px 12px; border-radius: 50px; background: var(--gabon-jaune); color: #744210; font-weight: 800; text-transform: uppercase; }

        nav { 
            display: flex; overflow-x: auto; gap: 10px; margin-bottom: 25px; 
            padding: 8px; background: #f1f5f9; border-radius: 18px; scrollbar-width: none;
            -ms-overflow-style: none;
        }
        nav::-webkit-scrollbar { display: none; }
        nav button { 
            flex: 0 0 auto; padding: 12px 20px; border: none; border-radius: 14px; 
            background: transparent; font-weight: 700; font-size: 12px; color: #64748b; 
            cursor: pointer; transition: 0.2s;
        }
        nav button.active { background: var(--white); color: var(--gabon-bleu); box-shadow: 0 4px 12px rgba(0,0,0,0.08); }

        .section { display: none; animation: slideUp 0.4s ease-out; }
        .active-sec { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        .card { 
            background: var(--white); border: 1px solid #f1f5f9; padding: 20px; 
            border-radius: 22px; margin-bottom: 15px; position: relative; 
            box-shadow: 0 5px 15px rgba(0,0,0,0.02); border-left: 10px solid #cbd5e1;
        }
        .card.etape-0 { border-left-color: #94a3b8; background: #f8fafc; }
        .card.etape-1 { border-left-color: var(--gabon-bleu); background: var(--gabon-bleu-light); }
        .card.etape-2 { border-left-color: var(--gabon-jaune); background: rgba(252, 209, 22, 0.08); }
        .card.etape-3 { border-left-color: var(--gabon-vert); background: var(--gabon-vert-light); }

        .card-title { font-weight: 800; font-size: 16px; color: var(--dark); margin-bottom: 8px; display: flex; align-items: center; justify-content: space-between; }
        .card-info { font-size: 14px; color: #475569; line-height: 1.6; }
        .badge-id { background: #e2e8f0; padding: 4px 10px; border-radius: 8px; font-size: 10px; font-weight: 900; color: var(--gabon-bleu); }

        .btn-action { 
            width: 100%; padding: 16px; border: none; border-radius: 14px; 
            font-weight: 800; cursor: pointer; margin-top: 15px; font-size: 14px; transition: 0.2s;
        }
        .btn-validate { background: var(--gabon-vert); color: white; }
        .btn-accept { background: var(--gabon-bleu); color: white; }
        .btn-photo { background: var(--dark); color: white; }
        .btn-delete { 
            background: rgba(231, 76, 60, 0.1); color: var(--danger); 
            border: none; padding: 6px 10px; border-radius: 8px; font-weight: 800; 
            font-size: 10px; cursor: pointer; margin-left: 5px;
        }

        .stats-footer { 
            background: var(--dark); color: white; padding: 25px; 
            border-radius: 25px; margin-top: 25px;
        }
        .stats-footer div { display: flex; justify-content: space-between; margin-bottom: 10px; font-size: 13px; font-weight: 600; }
        .stats-footer b { color: var(--gabon-jaune); font-size: 18px; }

        .table-compta { width: 100%; border-collapse: collapse; font-size: 13px; }
        .table-compta th { text-align: left; padding: 15px 10px; color: #64748b; border-bottom: 2px solid #f1f5f9; }
        .table-compta td { padding: 15px 10px; border-bottom: 1px solid #f8fafc; }

        .preview-img { width: 100%; margin-top: 15px; border-radius: 15px; display: none; }
        .search-area { background: #f8fafc; padding: 20px; border-radius: 22px; margin-bottom: 25px; border: 1px solid #e2e8f0; }
        
        .zone-highlight { 
            background: var(--gabon-bleu-light); 
            padding: 12px; border-radius: 12px; 
            border: 1px dashed var(--gabon-bleu);
            margin: 10px 0;
        }
        label.input-label { font-size: 11px; font-weight: 800; color: #64748b; margin-left: 5px; text-transform: uppercase; }
        
        .search-hint { font-size: 10px; color: #94a3b8; margin-top: 8px; font-weight: 600; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2>CT241 OPS</h2>
            <p style="font-size: 12px; color: #64748b; margin-bottom: 25px; font-weight: 600;">PORTAIL LOGISTIQUE GABONAIS</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3>CT241 OPS</h3>
                <div style="margin-top:5px">
                    <span id="user-role" class="role-badge">...</span>
                    <span id="user-display" style="font-size:11px; color:#94a3b8; margin-left:8px; font-weight:700"></span>
                </div>
            </div>
            <button id="btnOut" style="background:rgba(231, 76, 60, 0.1); color:var(--danger); border:none; padding:8px 15px; border-radius:10px; font-weight:800; font-size:11px; cursor:pointer">DECONNEXION</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">NOUVEAU</button>
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">MISSIONS <span id="count-missions"></span></button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">MON BILAN</button>
            <button onclick="ouvrir('compta')" id="nav-compta" style="display:none">ADMIN COMPTA</button>
            <button onclick="ouvrir('archives')" id="nav-archives" style="display:none">ARCHIVES</button>
        </nav>

        <!-- SECTION CREER -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 10px;">
                <h4 style="margin:0 0 20px 0; font-weight:900; color:var(--gabon-vert)">CRÉER UNE MISSION</h4>
                
                <label class="input-label">Bénéficiaire</label>
                <input type="text" id="mNom" placeholder="Nom complet">
                <input type="tel" id="mTel" placeholder="Téléphone (ex: 077...)">
                
                <div class="zone-highlight">
                    <label class="input-label">Localisation précise</label>
                    <input type="text" id="mQuartier" list="liste-quartiers" placeholder="Saisir le quartier..." oninput="autoDetectZone()">
                    <datalist id="liste-quartiers">
                        <option value="Nzeng-Ayong">Libreville</option><option value="Lalala">Libreville</option>
                        <option value="Akébé">Libreville</option><option value="Glass">Libreville</option>
                        <option value="Louis">Libreville</option><option value="Batterie IV">Libreville</option>
                        <option value="Montagne Sainte">Libreville</option><option value="Awendjé">Libreville</option>
                        <option value="Oloumi">Libreville</option><option value="Charbonnages">Libreville</option>
                        <option value="Mindoubé">Libreville</option><option value="Plaine Orety">Libreville</option>
                        <option value="Sodogo">Libreville</option>
                        <option value="Angondjé">Zone 2000 F</option><option value="Cap Estérias">Akanda</option>
                        <option value="Okala">Akanda</option><option value="Sablière">Akanda</option>
                        <option value="Alénakiri">Owendo</option><option value="Awoungou">Owendo</option>
                        <option value="Cité SNI">Owendo</option><option value="Barracuda">Owendo</option>
                        <option value="Ntoum">Zone Ntoum</option><option value="Essassa">Zone Ntoum</option>
                    </datalist>

                    <label class="input-label">Zone de Facturation (Obligatoire)</label>
                    <select id="mZoneSelect" onchange="updateFraisByZone()">
                        <option value="">-- Sélectionner la zone --</option>
                        <option value="libreville">Libreville (1000 F)</option>
                        <option value="peripherie">Owendo / Akanda Proche (1500 F)</option>
                        <option value="eloignee">Angondjé / Ntoum / Essassa (2000 F)</option>
                    </select>
                </div>

                <label class="input-label">Finance</label>
                <input type="number" id="mRetrait" placeholder="Montant à retirer (FCFA)">
                
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px">
                    <div>
                        <label class="input-label">Commission (F)</label>
                        <input type="number" id="mCom" value="390">
                    </div>
                    <div>
                        <label class="input-label">Frais Liv. (F)</label>
                        <input type="number" id="mLiv" value="1000" readonly style="background:#f1f5f9; color:#64748b">
                    </div>
                </div>
                
                <button onclick="creerMission()" class="btn-action btn-validate">DÉPLOYER LA MISSION</button>
            </div>
        </div>

        <!-- SECTION MISSIONS -->
        <div id="sec-missions" class="section active-sec">
            <div id="div-validation" style="display:none; margin-bottom:30px;">
                <h5 style="color:var(--danger); margin:0 0 15px 0; font-weight:900; font-size:12px; letter-spacing:1px">⚠️ EN ATTENTE DE VALIDATION</h5>
                <div id="list-validation"></div>
            </div>
            <h5 style="color:var(--gabon-bleu); margin:0 0 15px 0; font-weight:900; font-size:12px; letter-spacing:1px">🚀 MISSIONS EN COURS</h5>
            <div id="list-active"></div>
        </div>

        <!-- SECTION BILAN -->
        <div id="sec-bilan" class="section">
            <h4 style="margin:0 0 20px 0; font-weight:900">SUIVI DES GAINS</h4>
            <div id="list-bilan"></div>
            <div class="stats-footer">
                <div><span>REVENUS CUMULÉS :</span><b id="stat-total">0 F</b></div>
            </div>
        </div>

        <!-- SECTION COMPTA -->
        <div id="sec-compta" class="section">
            <h4 style="margin:0 0 20px 0; font-weight:900">CONTRÔLE FINANCIER</h4>
            <div style="background:white; border-radius:22px; border:1px solid #f1f5f9; overflow:hidden">
                <table class="table-compta">
                    <thead><tr><th>CLIENT</th><th>RETRAIT</th><th>LIV.</th><th>COM.</th></tr></thead>
                    <tbody id="list-compta"></tbody>
                </table>
            </div>
            <div class="stats-footer" style="background:var(--gabon-vert)">
                <div><span>TOTAL COMMISSIONS :</span><b id="compta-total-com" style="color:white">0 F</b></div>
                <div><span>VOLUME DE RETRAIT :</span><b id="compta-total-retrait" style="color:var(--gabon-jaune)">0 F</b></div>
            </div>
        </div>

        <!-- SECTION ARCHIVES -->
        <div id="sec-archives" class="section">
            <div class="search-area">
                <input type="text" id="archSearch" placeholder="Recherche par Nom, ID ou Code SMS..." oninput="renderUI()" style="margin:0">
                <div class="search-hint">💡 Astuce : Tapez le code SMS reçu pour retrouver une mission précise.</div>
            </div>
            <div id="list-archives"></div>
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

    const DATABASE_ZONES = {
        libreville: ["nzeng-ayong", "lalala", "akebe", "akébé", "glass", "louis", "batterie iv", "batterie 4", "montagne sainte", "awendje", "awendjé", "oloumi", "charbonnages", "mindoube", "mindoubé", "plaine orety", "sodogo"],
        peripherie: ["cap esterias", "cap estérias", "okala", "mikolongo", "carriere", "sabliere", "sablière", "gendarmerie", "alenakiri", "alénakiri", "awoungou", "sni", "barracuda", "pointe clair", "owendo"],
        eloignee: ["angondje", "angondjé", "ntoum", "essassa", "bikele", "bikelé", "meyang"]
    };

    window.autoDetectZone = () => {
        const quartier = document.getElementById('mQuartier').value.toLowerCase();
        const selectZone = document.getElementById('mZoneSelect');
        if (!quartier) return;
        for (const [zoneKey, keywords] of Object.entries(DATABASE_ZONES)) {
            if (keywords.some(k => quartier.includes(k))) {
                selectZone.value = zoneKey;
                updateFraisByZone();
                break;
            }
        }
    };

    window.updateFraisByZone = () => {
        const zone = document.getElementById('mZoneSelect').value;
        const inputLiv = document.getElementById('mLiv');
        const tarifs = { "libreville": 1000, "peripherie": 1500, "eloignee": 2000 };
        inputLiv.value = tarifs[zone] || 1000;
    };

    document.getElementById('btnConnect').onclick = async () => {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        try { await signInWithEmailAndPassword(auth, email, pass); } catch(e) { alert("Accès refusé"); }
    };

    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        if(u) {
            const email = u.email.toLowerCase();
            userRole = email.includes('admin') ? "admin" : (email.includes('finance') ? "finance" : "livreur");
            
            document.getElementById('user-role').innerText = userRole;
            document.getElementById('user-display').innerText = u.email.split('@')[0].toUpperCase();
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            
            document.getElementById('nav-creer').style.display = (userRole !== 'livreur') ? 'block' : 'none';
            document.getElementById('nav-compta').style.display = (userRole === 'admin') ? 'block' : 'none';
            document.getElementById('div-validation').style.display = (userRole === 'admin') ? 'block' : 'none';
            document.getElementById('nav-archives').style.display = (userRole === 'admin') ? 'block' : 'none';

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

    window.renderUI = () => {
        const listVal = document.getElementById('list-validation');
        const listAct = document.getElementById('list-active');
        const listBil = document.getElementById('list-bilan');
        const listCpt = document.getElementById('list-compta');
        const listArc = document.getElementById('list-archives');
        const archSearch = document.getElementById('archSearch').value.toLowerCase();
        
        listVal.innerHTML = ""; listAct.innerHTML = ""; listBil.innerHTML = ""; 
        listCpt.innerHTML = ""; listArc.innerHTML = "";

        let totalGainsLivreur = 0;
        let totalComAdmin = 0;
        let totalRetraitsAdmin = 0;
        const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "";

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            const isFinished = (m.etape === 3);

            if(isFinished) {
                if(userRole === 'admin') {
                    total
