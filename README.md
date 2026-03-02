<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - SYSTÈME DE DISTRIBUTION</title>
    <style>
        :root {
            --gabon-vert: #009E60; 
            --gabon-jaune: #FCD116; 
            --gabon-bleu: #3A75C4;
            --gabon-vert-light: rgba(0, 158, 96, 0.1);
            --gabon-bleu-light: rgba(58, 117, 196, 0.1);
            --danger: #e74c3c; 
            --dark: #2c3e50; 
            --light: #f4f7f6;
            --white: #ffffff;
        }
        
        body { 
            font-family: 'Inter', 'Segoe UI', system-ui, sans-serif; 
            background: linear-gradient(135deg, #f4f7f6 0%, #e9eeee 100%);
            margin: 0; 
            padding: 10px; 
            color: var(--dark);
            min-height: 100vh;
        }
        
        /* AUTH SCREEN */
        #auth-screen { 
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: linear-gradient(45deg, var(--gabon-vert), var(--gabon-jaune), var(--gabon-bleu));
            display: flex; align-items: center; justify-content: center; z-index: 9999; 
        }
        .login-card { 
            background: rgba(255, 255, 255, 0.95); 
            padding: 30px; border-radius: 24px; width: 85%; max-width: 340px; 
            text-align: center; box-shadow: 0 20px 40px rgba(0,0,0,0.2);
            backdrop-filter: blur(10px);
        }
        .login-card h2 { color: var(--gabon-vert); font-weight: 800; letter-spacing: -1px; margin-bottom: 5px; }

        input { 
            width: 100%; padding: 14px; margin: 10px 0; border: 2px solid #eee; 
            border-radius: 12px; box-sizing: border-box; font-size: 16px; transition: 0.3s;
        }
        input:focus { border-color: var(--gabon-bleu); outline: none; box-shadow: 0 0 0 4px var(--gabon-bleu-light); }
        
        .btn-login { 
            width: 100%; padding: 16px; background: var(--gabon-vert); color: white; 
            border: none; border-radius: 12px; font-weight: bold; cursor: pointer; 
            font-size: 16px; margin-top: 10px; box-shadow: 0 4px 12px rgba(0, 158, 96, 0.3);
        }

        /* MAIN APP STRUCTURE */
        #main-app { 
            display: none; max-width: 500px; margin: auto; background: var(--white); 
            border-radius: 25px; padding: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.08); 
            min-height: 92vh; position: relative;
        }

        header { 
            display: flex; justify-content: space-between; align-items: center; 
            margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px solid #f0f0f0;
        }
        header h3 { margin: 0; font-size: 20px; font-weight: 800; background: linear-gradient(to right, var(--gabon-vert), var(--gabon-bleu)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }

        .role-badge { font-size: 9px; padding: 4px 10px; border-radius: 20px; background: var(--gabon-jaune); color: #856404; font-weight: bold; }

        /* NAVIGATION */
        nav { 
            display: flex; overflow-x: auto; gap: 8px; margin-bottom: 20px; 
            padding: 6px; background: #f8f9fa; border-radius: 16px; scrollbar-width: none;
        }
        nav::-webkit-scrollbar { display: none; }
        nav button { 
            flex: 0 0 auto; padding: 10px 16px; border: none; border-radius: 12px; 
            background: transparent; font-weight: 700; font-size: 11px; color: #7f8c8d; 
            cursor: pointer; transition: 0.2s; white-space: nowrap;
        }
        nav button.active { background: var(--white); color: var(--gabon-bleu); box-shadow: 0 4px 10px rgba(0,0,0,0.05); }

        /* SECTIONS */
        .section { display: none; animation: fadeIn 0.3s ease-out; }
        .active-sec { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* CARDS */
        .card { 
            background: var(--white); border: 1px solid #f0f0f0; padding: 18px; 
            border-radius: 20px; margin-bottom: 15px; position: relative; 
            box-shadow: 0 4px 12px rgba(0,0,0,0.03); border-left: 8px solid #ddd;
        }
        .card.etape-0 { border-left-color: #95a5a6; border-style: dashed; background: #fafafa; }
        .card.etape-1 { border-left-color: var(--gabon-bleu); background: var(--gabon-bleu-light); }
        .card.etape-2 { border-left-color: var(--gabon-jaune); background: rgba(252, 209, 22, 0.05); }
        .card.etape-3 { border-left-color: var(--gabon-vert); background: var(--gabon-vert-light); }

        .card-title { font-weight: 800; font-size: 15px; color: var(--dark); margin-bottom: 6px; }
        .card-info { font-size: 13px; color: #5d6d7e; line-height: 1.5; }
        .badge-id { position: absolute; top: 15px; right: 15px; background: #edf2f7; padding: 4px 8px; border-radius: 8px; font-size: 10px; font-weight: 900; color: var(--gabon-bleu); }

        /* BUTTONS */
        .btn-action { 
            width: 100%; padding: 14px; border: none; border-radius: 12px; 
            font-weight: 800; cursor: pointer; margin-top: 12px; font-size: 13px;
            transition: 0.2s;
        }
        .btn-action:active { transform: scale(0.98); }
        .btn-validate { background: var(--gabon-vert); color: white; box-shadow: 0 4px 10px rgba(0, 158, 96, 0.2); }
        .btn-accept { background: var(--gabon-bleu); color: white; box-shadow: 0 4px 10px rgba(58, 117, 196, 0.2); }
        .btn-photo { background: var(--dark); color: white; }

        /* COMPTA GRID */
        .compta-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 20px; }
        .compta-stat-box { 
            background: var(--white); border: 1px solid #f0f0f0; padding: 15px; 
            border-radius: 18px; text-align: center; box-shadow: 0 4px 10px rgba(0,0,0,0.02);
        }
        .compta-stat-box span { font-size: 10px; font-weight: 700; color: #95a5a6; text-transform: uppercase; letter-spacing: 1px; }
        .compta-stat-box b { font-size: 24px; display: block; margin-top: 5px; }

        /* FOOTER STATS */
        .stats-footer { 
            background: var(--dark); color: white; padding: 20px; 
            border-radius: 20px; margin-top: 20px; box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }
        .stats-footer span { font-size: 11px; opacity: 0.7; font-weight: 600; }
        .stats-footer b { font-size: 1.4em; color: var(--gabon-jaune); }

        .table-compta { width: 100%; border-collapse: collapse; font-size: 12px; margin-top: 10px; }
        .table-compta th { text-align: left; padding: 12px 8px; color: #95a5a6; font-weight: 600; border-bottom: 2px solid #f0f0f0; }
        .table-compta td { padding: 12px 8px; border-bottom: 1px solid #f9f9f9; }

        .preview-img { width: 100%; margin-top: 15px; border-radius: 12px; display: none; border: 2px solid #eee; }
        .time-badge { font-size: 10px; color: #bdc3c7; display: block; margin-top: 8px; font-weight: 500; }

        .search-box { background: var(--gabon-bleu-light); border: 1px solid var(--gabon-bleu); border-radius: 16px; padding: 15px; margin-bottom: 20px; }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2>CT241 OPS</h2>
            <p style="font-size: 11px; color: #7f8c8d; margin-bottom: 20px; font-weight: 600;">LOGISTIQUE & CASH GABON</p>
            <input type="email" id="login-email" placeholder="Email professionnel">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">OUVRIR SESSION</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3>CT241 OPS</h3>
                <div style="display:flex; align-items:center; gap:8px; margin-top:2px">
                    <span id="user-role" class="role-badge">...</span>
                    <span id="user-display" style="font-size:10px; color:#95a5a6; font-weight:600"></span>
                </div>
            </div>
            <button id="btnOut" style="font-size:11px; color:var(--danger); background:rgba(231, 76, 60, 0.1); border:none; font-weight:800; cursor:pointer; padding:6px 12px; border-radius:8px">QUITTER</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">NOUVEAU</button>
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">MISSIONS <span id="count-missions"></span></button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">BILAN</button>
            <button onclick="ouvrir('compta')" id="nav-compta" style="display:none">COMPTA</button>
            <button onclick="ouvrir('archives')" id="nav-archives" style="display:none">ARCHIVES</button>
        </nav>

        <!-- CRÉATION -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 20px; border-radius: 20px; border: 1px solid #eee">
                <h4 style="margin:0 0 15px 0; font-weight:800">Nouvelle Commande</h4>
                <input type="text" id="mNom" placeholder="Nom complet du client">
                <input type="tel" id="mTel" placeholder="Numéro de téléphone">
                <input type="text" id="mLieu" placeholder="Quartier de livraison">
                <input type="number" id="mRetrait" placeholder="Montant du retrait (FCFA)">
                <button onclick="creerMission()" class="btn-action btn-validate">LANCER LA MISSION</button>
            </div>
        </div>

        <!-- LISTE MISSIONS -->
        <div id="sec-missions" class="section active-sec">
            <div id="div-validation" style="display:none; margin-bottom:25px;">
                <h5 style="color:var(--danger); margin:0 0 12px 0; font-weight:800; display:flex; align-items:center; gap:5px">
                    <span style="width:8px; height:8px; background:var(--danger); border-radius:50%"></span> EN ATTENTE DE VALIDATION
                </h5>
                <div id="list-validation"></div>
            </div>

            <h5 style="color:var(--gabon-bleu); margin:0 0 12px 0; font-weight:800">FLUX OPÉRATIONNEL</h5>
            <div id="list-active"></div>
        </div>

        <!-- BILAN (VUE LIVREUR) -->
        <div id="sec-bilan" class="section">
            <h4 style="margin:0 0 15px 0; font-weight:800">Historique Personnel</h4>
            <div id="list-bilan"></div>
            <div class="stats-footer">
                <div style="display:flex; justify-content:space-between; align-items:center">
                    <span>MES REVENUS LIVRAISON :</span>
                    <b id="stat-total">0 F</b>
                </div>
            </div>
        </div>

        <!-- COMPTABILITÉ (ADMIN) -->
        <div id="sec-compta" class="section">
            <h4 style="margin:0 0 15px 0; font-weight:800">Tableau de Bord Finance</h4>
            
            <div class="compta-grid">
                <div class="compta-stat-box">
                    <span>EN COURS</span>
                    <b id="compta-count-encours" style="color:var(--gabon-bleu)">0</b>
                </div>
                <div class="compta-stat-box">
                    <span>TERMINÉES</span>
                    <b id="compta-count-finish" style="color:var(--gabon-vert)">0</b>
                </div>
            </div>

            <div style="overflow-x:auto; background:white; border-radius:18px; border:1px solid #eee">
                <table class="table-compta">
                    <thead>
                        <tr>
                            <th>CLIENT</th>
                            <th>RETRAIT</th>
                            <th>COMM.</th>
                        </tr>
                    </thead>
                    <tbody id="list-compta"></tbody>
                </table>
            </div>
            <div class="stats-footer">
                <div style="display:flex; justify-content:space-between; margin-bottom:8px">
                    <span>TOTAL COMMISSIONS :</span>
                    <b id="compta-total-com" style="color:var(--gabon-jaune)">0 F</b>
                </div>
                <div style="display:flex; justify-content:space-between">
                    <span>VOLUME RETRAITS :</span>
                    <b id="compta-total-retrait" style="color:var(--white); font-size:1.1em">0 F</b>
                </div>
            </div>
        </div>

        <!-- ARCHIVES -->
        <div id="sec-archives" class="section">
            <div class="search-box">
                <h5 style="margin:0 0 10px 0; color:var(--gabon-bleu); font-weight:800">MOTEUR DE RECHERCHE</h5>
                <input type="text" id="archSearch" placeholder="Nom, ID, Date, SMS..." oninput="renderUI()" style="margin:0">
            </div>
            <div id="list-archives"></div>
        </div>
    </div>

    <input type="file" id="camInput" accept="image/*" capture="camera" style="display:none">
    <canvas id="canvas" style="display:none"></canvas>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
    import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-auth.js";
    import { getDatabase, ref, push, onValue, update } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-database.js";

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

    document.getElementById('btnConnect').onclick = async () => {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        try { await signInWithEmailAndPassword(auth, email, pass); } catch(e) { alert("Accès refusé"); }
    };

    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        if(u) {
            const email = u.email.toLowerCase();
            if(email.includes('admin')) userRole = "admin";
            else if(email.includes('finance')) userRole = "finance";
            else userRole = "livreur";

            document.getElementById('user-role').innerText = userRole.toUpperCase();
            document.getElementById('user-display').innerText = u.email.split('@')[0];
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            
            document.getElementById('nav-creer').style.display = (userRole === 'admin' || userRole === 'finance') ? 'block' : 'none';
            document.getElementById('nav-compta').style.display = (userRole === 'admin') ? 'block' : 'none';
            document.getElementById('nav-archives').style.display = (userRole === 'admin' || userRole === 'finance') ? 'block' : 'none';
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

    function renderUI() {
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
        let totalRetraitsClient = 0;
        let countEnCours = 0;
        let countTerminees = 0;

        const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "INVITÉ";

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            
            if(m.etape === 3) countTerminees++;
            else countEnCours++;

            if(userRole === 'admin' || userRole === 'finance') {
                const searchStr = `${m.id} ${m.nom} ${m.tel} ${m.dateHeure} ${m.codeSMS || ''} ${m.livreur}`.toLowerCase();
                if(searchStr.includes(archSearch)) {
                    listArc.innerHTML += `<div class="card" style="border-left:4px solid var(--dark); opacity:0.8">
                        <span class="badge-id">${m.id}</span>
                        <div class="card-title">${m.nom}</div>
                        <div class="card-info" style="font-size:11px">
                            📞 ${m.tel || 'N/A'} | 📅 ${m.dateHeure}<br>
                            💰 ${m.retrait.toLocaleString()} F | Code: <b>${m.codeSMS || 'N/A'}</b><br>
                            🛵 Livreur: ${m.livreur} | Statut: ${m.etape === 3 ? '✅ Terminé' : '⏳ En cours'}
                        </div>
                    </div>`;
                }
            }

            if(m.etape === 3 && userRole === 'admin') {
                totalComAdmin += m.com;
                totalRetraitsClient += m.retrait;
                listCpt.innerHTML += `<tr>
                    <td><b style="color:var(--dark)">${m.nom}</b><br><small style="color:#95a5a6">${m.id}</small></td>
                    <td>${m.retrait.toLocaleString()} F</td>
                    <td style="color:var(--gabon-vert); font-weight:bold">${m.com.toLocaleString()} F</td>
                </tr>`;
            }

            if(m.etape === 3) {
                if(m.livreur === myName) {
                    totalGainsLivreur += m.fraisLivraison;
                    listBil.innerHTML += `<div style="padding:15px; border-bottom:1px solid #f9f9f9; display:flex; justify-content:space-between; align-items:center">
                            <div><b style="font-size:13px">${m.nom}</b><br><small style="color:#bdc3c7">${m.id} • ${m.dateHeure}</small></div>
                            <b style="color:var(--gabon-vert); font-weight:bold">+${m.fraisLivraison.toLocaleString()} F</b>
                        </div>`;
                }
                return;
            }

            const cardHtml = `<div class="card etape-${m.etape}">
                    <span class="badge-id">${m.id}</span>
                    <div class="card-title">${m.nom} <span style="font-weight:600; font-size:11px; color:var(--gabon-bleu); margin-left:5px">(${m.tel || 'N/A'})</span></div>
                    <div class="card-info">
                        📍 <b>${m.lieu}</b><br>💰 Retrait : <b style="color:var(--dark)">${m.retrait.toLocaleString()} F</b><br>
                        🛵 Livreur : <span style="background:var(--dark); color:white; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:800">${m.livreur}</span>
                        <span class="time-badge">🕒 ${m.dateHeure}</span>
                    </div>`;

            if(m.etape === 0 && userRole === 'admin') {
                listVal.innerHTML += cardHtml + `<button class="btn-action btn-validate" onclick="valider('${m.key}')">VALIDER & PUBLIER</button></div>`;
            }

            if(m.etape > 0) {
                let controls = "";
                if(m.etape === 1 && m.livreur === "Libre" && userRole === 'livreur') {
                    controls = `<button class="btn-action btn-accept" onclick="accepter('${m.key}')">ACCEPTER LA COURSE</button>`;
                } 
                else if(m.etape === 1 && m.livreur === myName) {
                    controls = `<button class="btn-action btn-photo" onclick="triggerCam('${m.key}')">📸 PHOTOGRAPHIER LE SMS</button>
                        <img id="pre-${m.key}" class="preview-img">
                        <input type="text" id="code-${m.key}" placeholder="Saisir Code SMS de retrait" style="margin-top:12px; border:2px solid var(--gabon-bleu)">
                        <button class="btn-action btn-validate" onclick="terminer('${m.key}')">ENVOYER À LA FINANCE</button>`;
                }
                else if(m.etape === 2 && (userRole === 'admin' || userRole === 'finance')) {
                    controls = `<div style="background:white; padding:15px; border-radius:15px; margin-top:12px; border:1px solid #eee; text-align:center">
                            <img src="${m.photo}" style="width:100%; border-radius:10px; box-shadow:0 4px 10px rgba(0,0,0,0.1)">
                            <p style="color:var(--danger); font-size:24px; font-weight:800; margin:15px 0; letter-spacing:2px">${m.codeSMS}</p>
                            <button class="btn-action btn-validate" onclick="cloturer('${m.key}')">CONFIRMER ENCAISSEMENT ✅</button>
                        </div>`;
                }
                listAct.innerHTML += cardHtml + controls + `</div>`;
            }
        });

        document.getElementById('stat-total').innerText = totalGainsLivreur.toLocaleString() + " F";
        if(userRole === 'admin') {
            document.getElementById('compta-total-com').innerText = totalComAdmin.toLocaleString() + " F";
            document.getElementById('compta-total-retrait').innerText = totalRetraitsClient.toLocaleString() + " F";
            document.getElementById('compta-count-encours').innerText = countEnCours;
            document.getElementById('compta-count-finish').innerText = countTerminees;
        }

        const count = allMissions.filter(x => x.etape === 1 && x.livreur === "Libre").length;
        document.getElementById('count-missions').innerHTML = count > 0 ? `<b style="background:var(--danger); color:white; padding:2px 8px; border-radius:20px; font-size:10px; vertical-align:middle; margin-left:4px">${count}</b>` : "";
    }

    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const tel = document.getElementById('mTel').value;
        const mnt = parseInt(document.getElementById('mRetrait').value);
        const lieu = document.getElementById('mLieu').value;
        if(!nom || !mnt || !tel) return alert("Veuillez remplir les champs obligatoires");
        const now = new Date();
        const dateHeure = now.toLocaleDateString('fr-FR') + " " + now.toLocaleTimeString('fr-FR', {hour: '2-digit', minute:'2-digit'});
        const orderID = "CT" + Math.floor(10000 + Math.random() * 89999);
        push(ref(db, 'missions'), {
            id: orderID, nom, tel, lieu: lieu || "Libreville",
            retrait: mnt, com: (mnt >= 15000 ? 390 : 190), fraisLivraison: 1000,
            livreur: "Libre", etape: 0, dateHeure: dateHeure, timestamp: Date.now()
        });
        document.getElementById('mNom').value = ""; document.getElementById('mTel').value = "";
        document.getElementById('mRetrait').value = ""; document.getElementById('mLieu').value = "";
        ouvrir('missions');
    };

    window.valider = (k) => update(ref(db, `missions/${k}`), { etape: 1 });
    window.accepter = (k) => update(ref(db, `missions/${k}`), { livreur: auth.currentUser.email.split('@')[0].toUpperCase() });
    window.triggerCam = (k) => { currentKey = k; document.getElementById('camInput').click(); };

    document.getElementById('camInput').onchange = (e) => {
        const file = e.target.files[0]; if(!file) return;
        const reader = new FileReader();
        reader.onload = (re) => {
            const img = new Image();
            img.onload = () => {
                const can = document.getElementById('canvas');
                const ctx = can.getContext('2d');
                can.width = 600; can.height = 600 * (img.height/img.width);
                ctx.drawImage(img, 0, 0, 600, can.height);
                lastPhotoData = can.toDataURL('image/jpeg', 0.6);
                const pre = document.getElementById('pre-'+currentKey);
                if(pre) { pre.src = lastPhotoData; pre.style.display = 'block'; }
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.terminer = (k) => {
        const code = document.getElementById('code-'+k).value;
        if(!code || !lastPhotoData) return alert("Photo et Code obligatoires !");
        update(ref(db, `missions/${k}`), { etape: 2, codeSMS: code, photo: lastPhotoData });
    };

    window.cloturer = (k) => confirm("Confirmer l'encaissement définitif ?") && update(ref(db, `missions/${k}`), { etape: 3 });

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('nav-'+id).classList.add('active');
    };
</script>
</body>
</html>
