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
            background: linear-gradient(135deg, var(--gabon-vert), var(--gabon-jaune), var(--gabon-bleu));
            display: flex; align-items: center; justify-content: center; z-index: 9999; 
        }
        .login-card { 
            background: rgba(255, 255, 255, 0.98); 
            padding: 40px 30px; border-radius: 28px; width: 85%; max-width: 350px; 
            text-align: center; box-shadow: 0 25px 50px rgba(0,0,0,0.2);
        }

        .auth-logo {
            width: 100%; max-width: 220px; height: auto;
            margin-bottom: 10px; display: block; margin-left: auto; margin-right: auto;
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
            min-height: 90vh; padding-bottom: 40px;
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
            white-space: nowrap; cursor: pointer;
        }
        nav button.active { background: var(--white); color: var(--gabon-bleu); box-shadow: 0 4px 10px rgba(0,0,0,0.05); }

        .section { display: none; animation: fadeIn 0.3s ease; }
        .active-sec { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .card { 
            background: var(--white); border: 1px solid #f1f5f9; padding: 15px; 
            border-radius: 18px; margin-bottom: 12px; border-left: 6px solid #cbd5e1;
            position: relative;
        }
        .card.etape-0 { border-left-color: var(--danger); }
        .card.etape-1 { border-left-color: var(--gabon-bleu); }
        .card.etape-2 { border-left-color: var(--gabon-jaune); }
        .card.etape-3 { border-left-color: var(--gabon-vert); }

        .btn-action { 
            width: 100%; padding: 14px; border: none; border-radius: 12px; 
            font-weight: 800; cursor: pointer; margin-top: 10px; transition: 0.2s;
            display: flex; align-items: center; justify-content: center; gap: 8px;
        }
        .btn-validate { background: var(--gabon-vert); color: white; }
        
        .stats-banner {
            background: var(--dark); color: white; padding: 20px; border-radius: 20px;
            margin-bottom: 20px; position: relative;
        }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 10px; }
        .stats-banner small { color: #94a3b8; font-size: 10px; text-transform: uppercase; display: block; }
        .stats-banner b { font-size: 18px; color: var(--gabon-jaune); }

        .btn-refresh-bilan {
            position: absolute; top: 15px; right: 15px; background: rgba(255,255,255,0.15);
            color: white; border: none; border-radius: 10px; padding: 8px 12px;
            font-size: 9px; font-weight: 800; cursor: pointer; border: 1px solid rgba(255,255,255,0.1);
        }

        .date-divider {
            padding: 8px 15px; background: #f8fafc; border-radius: 10px;
            font-size: 11px; font-weight: 800; color: #64748b; margin: 25px 0 10px 0;
            border: 1px solid #e2e8f0; display: flex; justify-content: space-between;
        }

        .zone-highlight { background: rgba(58, 117, 196, 0.05); padding: 12px; border-radius: 15px; margin: 10px 0; border: 1px dashed var(--gabon-bleu); }
        .finance-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .label-mini { font-size: 10px; font-weight: 800; color: #64748b; margin-left: 5px; text-transform: uppercase; }
        
        .badge { font-size: 9px; padding: 2px 6px; border-radius: 6px; font-weight: bold; margin-left: 5px; vertical-align: middle; }
        .badge-blue { background: #dbeafe; color: #1e40af; }

        #modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8); z-index: 10000;
            display: none; align-items: center; justify-content: center;
        }
        .modal-content {
            background: white; width: 90%; max-width: 450px; border-radius: 25px;
            padding: 20px; max-height: 85vh; overflow-y: auto; position: relative;
        }
        .close-modal {
            position: absolute; top: 15px; right: 15px; font-size: 24px; color: var(--danger); font-weight: bold; cursor: pointer;
        }
        .detail-row {
            display: flex; justify-content: space-between; padding: 10px 0; border-bottom: 1px solid #f1f5f9;
        }

        .archive-item { border-bottom: 1px solid #eee; padding: 12px 0; }
        .archive-header { font-size: 12px; font-weight: bold; color: var(--gabon-bleu); display: flex; justify-content: space-between; margin-bottom: 5px; }

        .search-bar { margin-bottom: 15px; position: relative; }
        .search-bar input { padding-left: 40px; background: #fff; border: 1px solid #e2e8f0; }
        
        .loading-overlay {
            display: none; position: fixed; inset: 0; background: rgba(255,255,255,0.7);
            z-index: 10001; align-items: center; justify-content: center; font-weight: 800;
        }
        .btn-export {
            background: #000; color: white; padding: 10px; border-radius: 10px; font-size: 11px;
            border: none; cursor: pointer; margin-top: 10px; width: 100%;
        }

        /* --- STYLES IMPRESSION BON --- */
        #receipt-print {
            display: none;
            font-family: 'Courier New', Courier, monospace;
            width: 300px; padding: 20px;
            text-align: center; font-size: 12px; color: #000;
            background: white;
        }

        @media print {
            body * { visibility: hidden !important; }
            #receipt-print, #receipt-print * { visibility: visible !important; display: block !important; }
            #receipt-print { position: absolute; left: 0; top: 0; width: 100% !important; margin: 0; padding: 10px; }
            @page { margin: 0; size: auto; }
        }
    </style>
</head>
<body>

    <div id="loading" class="loading-overlay">TRAITEMENT EN COURS...</div>

    <div id="receipt-print">
        <h2 style="margin:5px 0">CT241 OPS</h2>
        <p>LOGISTIQUE GABON</p>
        <p>--------------------------------</p>
        <p><b>BON DE LIVRAISON</b></p>
        <p id="pr-id" style="font-weight:bold; font-size:18px; margin:10px 0"></p>
        <p id="pr-date"></p>
        <p>--------------------------------</p>
        <div style="text-align:left">
            <p>Bénéficiaire: <span id="pr-nom" style="font-weight:bold"></span></p>
            <p>Téléphone: <span id="pr-tel"></span></p>
            <p>Quartier: <span id="pr-lieu"></span></p>
            <p>Livreur: <span id="pr-liv"></span></p>
        </div>
        <p>--------------------------------</p>
        <h3 style="margin:5px 0">MONTANT RETRAIT</h3>
        <h2 id="pr-montant" style="margin:5px 0; font-size:24px"></h2>
        <p>--------------------------------</p>
        <p style="font-size:10px; margin-top:20px">Merci de votre confiance.</p>
    </div>

    <div id="auth-screen">
        <div class="login-card">
            <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" alt="Logo CT241" class="auth-logo">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #64748b; margin-bottom: 20px;">PORTAIL LOGISTIQUE GABON</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="modal-overlay">
        <div class="modal-content">
            <span class="close-modal" onclick="fermerModal()">×</span>
            <h3 style="color:var(--gabon-bleu); margin-top:0">Détails Mission</h3>
            <div id="modal-body"></div>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-bleu)">CT241 OPS</h3>
                <span id="user-role" style="font-size:10px; padding:3px 8px; border-radius:10px; background:var(--gabon-jaune); font-weight:800">...</span>
            </div>
            <button id="btnOut" style="background:none; color:var(--danger); border:1px solid var(--danger); padding:5px 10px; border-radius:8px; font-size:10px; cursor:pointer">DECONNEXION</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">MISSIONS <span id="count-missions"></span></button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">MON BILAN</button>
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">NOUVEAU</button>
            <button onclick="ouvrir('compta')" id="nav-compta" style="display:none">ADMIN</button>
            <button onclick="ouvrir('archives')" id="nav-archives" style="display:none">ARCHIVES</button>
        </nav>

        <div id="sec-missions" class="section active-sec">
            <div class="search-bar">
                <input type="text" id="searchInput" placeholder="Rechercher une mission..." onkeyup="filterMissions('searchInput')">
            </div>
            <div id="div-validation" style="display:none; margin-bottom:20px;">
                <h6 style="color:var(--danger); margin:0 0 10px 0; font-size:10px; text-transform:uppercase">⚠️ À Valider par l'Admin</h6>
                <div id="list-validation"></div>
            </div>
            <div id="list-active"></div>
        </div>

        <div id="sec-bilan" class="section">
            <div class="stats-banner">
                <button class="btn-refresh-bilan" onclick="rafraichirBilan()">🔄 ACTUALISER</button>
                <small>Session Active (Aujourd'hui)</small>
                <div class="stats-grid">
                    <div><small>Bonus du Jour</small><b id="stat-total">0 F</b></div>
                    <div><small>Courses Faites</small><b id="stat-count">0</b></div>
                </div>
            </div>
            <div id="list-bilan-today"></div>
            <div id="archive-history"></div>
        </div>

        <div id="sec-creer" class="section">
            <h4 style="margin:0 0 15px 0; color:var(--gabon-vert)">DÉPLOYER UNE MISSION</h4>
            <input type="text" id="mNom" placeholder="Nom du bénéficiaire">
            <input type="tel" id="mTel" placeholder="Téléphone">
            <input type="text" id="mQuartier" placeholder="Quartier précis...">
            <select id="mZoneSelect" onchange="updateFrais()">
                <option value="2000">Libreville (2000 F)</option>
                <option value="2500">Owendo / Akanda (2500 F)</option>
            </select>
            <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)">
            <div class="finance-row">
                <div><span class="label-mini">Frais</span><input type="number" id="mLiv" value="2000" readonly></div>
                <div><span class="label-mini">Com</span><input type="number" id="mCom" value="700"></div>
            </div>
            <button id="btnLancer" onclick="creerMission()" class="btn-action btn-validate">LANCER LA MISSION</button>
        </div>

        <div id="sec-compta" class="section">
            <div class="stats-banner" style="background:var(--gabon-vert)">
                <small>Tableau de bord Admin</small>
                <div class="stats-grid">
                    <div><small>Commissions Totales</small><b id="cpt-com">0 F</b></div>
                    <div><small>Volume Retraits</small><b id="cpt-vol">0 F</b></div>
                </div>
                <button class="btn-export" onclick="exportToCSV()">📥 EXPORTER CSV</button>
            </div>
            <div id="list-compta-daily"></div>
        </div>

        <div id="sec-archives" class="section">
            <div id="list-archives-global"></div>
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
        projectId: "cashtransfert-21"
    };

    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getDatabase(app);

    let userRole = "livreur";
    let allMissions = [];
    let currentKey = null;
    let lastPhotoData = "";

    window.ouvrir = (sec) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById(`sec-${sec}`).classList.add('active-sec');
        document.getElementById(`nav-${sec}`).classList.add('active');
    };

    document.getElementById('btnConnect').onclick = async () => {
        toggleLoading(true);
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        try { await signInWithEmailAndPassword(auth, email, pass); } 
        catch(e) { toggleLoading(false); alert("Erreur d'accès"); }
    };

    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        toggleLoading(false);
        if(u) {
            const email = u.email.toLowerCase();
            userRole = email.includes('admin') ? "admin" : "livreur";
            document.getElementById('user-role').innerText = userRole.toUpperCase();
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            
            document.getElementById('nav-creer').style.display = (userRole === 'admin') ? 'block' : 'none';
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

    function toggleLoading(s) { document.getElementById('loading').style.display = s ? 'flex' : 'none'; }

    window.renderUI = (filter = "") => {
        const listAct = document.getElementById('list-active');
        const listBilToday = document.getElementById('list-bilan-today');
        const listCpt = document.getElementById('list-compta-daily');
        
        listAct.innerHTML = ""; listBilToday.innerHTML = ""; listCpt.innerHTML = "";

        const todayStr = new Date().toLocaleDateString('fr-FR');
        const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "";

        let dailyGains = 0, dailyCount = 0, adminCom = 0, adminVol = 0;

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            const mDateStr = new Date(m.timestamp).toLocaleDateString('fr-FR');
            const isToday = (mDateStr === todayStr);

            if(m.etape < 3) {
                listAct.innerHTML += createCard(m, myName);
            } else {
                if(m.livreur === myName && isToday) {
                    dailyGains += m.fraisLivraison;
                    dailyCount++;
                    listBilToday.innerHTML += createRow(m, "livreur");
                }
                if(userRole === 'admin' && isToday) {
                    adminCom += m.com;
                    adminVol += m.retrait;
                    listCpt.innerHTML += createRow(m, "admin");
                }
            }
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
        if(m.etape === 1) {
            if(m.livreur === "Libre" && userRole === 'livreur') {
                btn = `<button class="btn-action btn-validate" onclick="accepter('${m.key}')">ACCEPTER</button>`;
            } else if(m.livreur === myName) {
                btn = `<button class="btn-action" onclick="triggerCam('${m.key}')">📸 PHOTO & CODE</button>
                       <input type="text" id="code-${m.key}" placeholder="Code SMS">
                       <button class="btn-action btn-validate" onclick="terminer('${m.key}')">VALIDER</button>`;
            }
        } else if(m.etape === 2 && userRole === 'admin') {
            btn = `<button class="btn-action btn-validate" onclick="cloturer('${m.key}')">CLÔTURER ✅</button>`;
        }

        return `<div class="card etape-${m.etape}">
            <b>${m.nom} (#${m.id})</b><br>
            Lieu: ${m.lieu}<br>
            Retrait: ${m.retrait} F | Gain: ${m.fraisLivraison} F<br>
            ${btn}
        </div>`;
    }

    function createRow(m, type) {
        const val = type === 'admin' ? m.com : m.fraisLivraison;
        return `<div style="padding:10px; border-bottom:1px solid #eee; display:flex; justify-content:space-between">
            <span>${m.nom}</span>
            <b style="color:var(--gabon-vert)">+ ${val.toLocaleString()} F</b>
        </div>`;
    }

    window.creerMission = () => {
        const mission = {
            id: Math.floor(1000 + Math.random() * 9000),
            nom: document.getElementById('mNom').value,
            lieu: document.getElementById('mQuartier').value,
            retrait: parseInt(document.getElementById('mRetrait').value),
            fraisLivraison: parseInt(document.getElementById('mLiv').value),
            com: parseInt(document.getElementById('mCom').value),
            livreur: "Libre", etape: 1, timestamp: Date.now()
        };
        push(ref(db, 'missions'), mission);
        alert("Mission créée !");
    };

    window.accepter = (k) => update(ref(db, `missions/${k}`), { livreur: auth.currentUser.email.split('@')[0].toUpperCase() });
    window.triggerCam = (k) => { currentKey = k; document.getElementById('camInput').click(); };
    document.getElementById('camInput').onchange = (e) => {
        const reader = new FileReader();
        reader.onload = (re) => lastPhotoData = re.target.result;
        reader.readAsDataURL(e.target.files[0]);
    };
    window.terminer = (k) => update(ref(db, `missions/${k}`), { codeSMS: document.getElementById(`code-${k}`).value, photo: lastPhotoData, etape: 2 });
    window.cloturer = (k) => update(ref(db, `missions/${k}`), { etape: 3 });

</script>
</body>
</html>
