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
            overflow-y: auto;
        }
        .login-card { 
            background: rgba(255, 255, 255, 0.98); 
            padding: 30px 25px; border-radius: 28px; width: 85%; max-width: 350px; 
            text-align: center; box-shadow: 0 25px 50px rgba(0,0,0,0.2);
            margin: 20px 0;
        }

        .auth-logo {
            width: 100%; max-width: 180px; height: auto;
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

        /* Styles Suivi de commande */
        .tracking-section {
            margin-top: 25px;
            padding-top: 20px;
            border-top: 1px dashed #cbd5e1;
        }
        .tracking-title {
            font-size: 12px;
            font-weight: 800;
            color: var(--gabon-bleu);
            margin-bottom: 10px;
            text-transform: uppercase;
        }
        .btn-track {
            width: 100%; padding: 12px; background: var(--white); color: var(--gabon-bleu); 
            border: 2px solid var(--gabon-bleu); border-radius: 14px; font-weight: 700; cursor: pointer; 
            font-size: 14px;
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

        .date-divider {
            padding: 8px 15px; background: #f8fafc; border-radius: 10px;
            font-size: 11px; font-weight: 800; color: #64748b; margin: 25px 0 10px 0;
            border: 1px solid #e2e8f0; display: flex; justify-content: space-between;
        }

        .zone-highlight { background: rgba(58, 117, 196, 0.05); padding: 12px; border-radius: 15px; margin: 10px 0; border: 1px dashed var(--gabon-bleu); }
        
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
        .detail-row b { color: var(--dark); font-size: 13px; }
        .detail-row span { color: #64748b; font-size: 13px; }

        .archive-item { border-bottom: 1px solid #eee; padding: 12px 0; }
        
        .search-bar { margin-bottom: 15px; position: relative; }
        .search-bar input { padding-left: 40px; background: #fff; border: 1px solid #e2e8f0; }

        .loading-overlay {
            display: none; position: fixed; inset: 0; background: rgba(255,255,255,0.7);
            z-index: 10001; align-items: center; justify-content: center; font-weight: 800;
        }

        #receipt-print {
            display: none;
            font-family: 'Courier New', Courier, monospace;
            width: 300px; padding: 20px; text-align: center; font-size: 12px; color: #000; background: white;
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

    <!-- ZONE IMPRESSION -->
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
        <div style="margin-top:40px; border-top:1px dashed #000; padding-top:10px">
            <p>Signature Client</p>
            <br><br><br>
        </div>
        <p style="font-size:10px; margin-top:20px">Merci de votre confiance.</p>
    </div>

    <!-- ECRAN AUTH ET SUIVI -->
    <div id="auth-screen">
        <div class="login-card">
            <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" alt="Logo CT241" class="auth-logo">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #64748b; margin-bottom: 20px;">PORTAIL LOGISTIQUE GABON</p>
            
            <!-- Connexion Admin/Livreur -->
            <input type="email" id="login-email" placeholder="Email professionnel">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>

            <!-- NOUVELLE SECTION : SUIVI DE COMMANDE CLIENT -->
            <div class="tracking-section">
                <div class="tracking-title">Suivi de commande client</div>
                <input type="number" id="tracking-id" placeholder="N° ID de votre commande (ex: 4582)">
                <button class="btn-track" onclick="clientTrack()">SUIVRE MON COLIS</button>
            </div>
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

        <!-- MISSIONS -->
        <div id="sec-missions" class="section active-sec">
            <div class="search-bar">
                <input type="text" id="searchInput" placeholder="Rechercher une mission active..." onkeyup="filterMissions('searchInput')">
            </div>
            <div id="div-validation" style="display:none; margin-bottom:20px;">
                <h6 style="color:var(--danger); margin:0 0 10px 0; font-size:10px; text-transform:uppercase">⚠️ À Valider par l'Admin</h6>
                <div id="list-validation"></div>
                <hr style="border:0; border-top:1px solid #f1f5f9; margin:20px 0">
            </div>
            <h6 style="color:var(--gabon-bleu); margin:0 0 10px 0; font-size:10px; text-transform:uppercase">Flux en temps réel</h6>
            <div id="list-active"></div>
        </div>

        <!-- BILAN -->
        <div id="sec-bilan" class="section">
            <div class="stats-banner">
                <button class="btn-refresh-bilan" onclick="rafraichirBilan()" style="position: absolute; top: 15px; right: 15px; background: rgba(255,255,255,0.15); color: white; border: 1px solid rgba(255,255,255,0.1); border-radius: 10px; padding: 8px 12px; font-size: 9px; font-weight: 800; cursor: pointer;">🔄 ACTUALISER</button>
                <small>Session Active (Aujourd'hui)</small>
                <div class="stats-grid">
                    <div><small>Gains du Jour</small><b id="stat-total">0 F</b></div>
                    <div><small>Courses Faites</small><b id="stat-count" style="color:white">0</b></div>
                </div>
            </div>
            <div id="list-bilan-today"></div>
            <div id="archive-history"></div>
        </div>

        <!-- CREER -->
        <div id="sec-creer" class="section">
            <h4 style="margin:0 0 15px 0; color:var(--gabon-vert)">DÉPLOYER UNE MISSION</h4>
            <input type="text" id="mNom" placeholder="Nom du bénéficiaire">
            <input type="tel" id="mTel" placeholder="Téléphone (ex: 077...)">
            <div class="zone-highlight">
                <span class="label-mini">Zone & Localisation</span>
                <input type="text" id="mQuartier" placeholder="Quartier précis...">
                <select id="mZoneSelect" onchange="updateFrais()">
                    <option value="1000">Libreville Centre (1000 F)</option>
                    <option value="1500">Owendo / Akanda (1500 F)</option>
                    <option value="2000">PK / Ntoum / Angondjé (2000 F)</option>
                </select>
            </div>
            <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)">
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                <div><span class="label-mini">Livreur (F)</span><input type="number" id="mLiv" value="1000" readonly style="background:#f1f5f9"></div>
                <div><span class="label-mini">Commission (F)</span><input type="number" id="mCom" value="390"></div>
            </div>
            <button id="btnLancer" onclick="creerMission()" class="btn-action btn-validate">LANCER LA MISSION</button>
        </div>

        <!-- ADMIN -->
        <div id="sec-compta" class="section">
            <div class="stats-banner" style="background:var(--gabon-vert)">
                <small>Tableau de bord Admin</small>
                <div class="stats-grid">
                    <div><small>Commissions Totales</small><b id="cpt-com" style="color:white">0 F</b></div>
                    <div><small>Volume Retraits</small><b id="cpt-vol" style="color:var(--gabon-jaune)">0 F</b></div>
                </div>
            </div>
            <div id="list-compta-daily"></div>
        </div>

        <!-- ARCHIVES -->
        <div id="sec-archives" class="section">
            <h4 style="margin:0 0 15px 0; color:var(--gabon-bleu)">ARCHIVES GÉNÉRALES</h4>
            <div class="search-bar">
                <input type="text" id="archiveSearchInput" placeholder="Rechercher dans l'historique..." onkeyup="filterMissions('archiveSearchInput')">
            </div>
            <div id="list-archives-global"></div>
        </div>
    </div>

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

    // --- FONCTION DE SUIVI CLIENT (NOUVEAU) ---
    window.clientTrack = () => {
        const idInput = document.getElementById('tracking-id').value;
        if(!idInput) { alert("Entrez un ID de commande"); return; }
        
        // Recherche de la mission par l'ID numérique (id: 4582)
        const mission = allMissions.find(m => String(m.id) === String(idInput));
        
        if(mission) {
            window.consulterMission(mission.key);
        } else {
            alert("Aucune commande trouvée avec cet ID. Vérifiez votre numéro.");
        }
    };

    // --- LOGIQUE EXISTANTE (INCHANGÉE) ---
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
        if(!email || !pass) { toggleLoading(false); return; }
        try { await signInWithEmailAndPassword(auth, email, pass); } catch(e) { 
            toggleLoading(false); alert("Accès refusé."); 
        }
    };
    
    document.getElementById('btnOut').onclick = () => { if(confirm("Se déconnecter ?")) signOut(auth); };

    onAuthStateChanged(auth, (u) => {
        toggleLoading(false);
        // On charge toujours les missions pour le suivi client même sans être loggé
        onValue(ref(db, 'missions'), (snap) => {
            const data = snap.val();
            allMissions = data ? Object.keys(data).map(k => ({...data[k], key:k})) : [];
            if(u) renderUI();
        });

        if(u) {
            const email = u.email.toLowerCase();
            userRole = email.includes('admin') ? "admin" : (email.includes('finance') ? "finance" : "livreur");
            document.getElementById('user-role').innerText = userRole.toUpperCase();
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            document.getElementById('nav-creer').style.display = (userRole !== 'livreur') ? 'block' : 'none';
            document.getElementById('nav-compta').style.display = (userRole === 'admin') ? 'block' : 'none';
            document.getElementById('nav-archives').style.display = (userRole === 'admin' || userRole === 'finance') ? 'block' : 'none';
            document.getElementById('div-validation').style.display = (userRole === 'admin') ? 'block' : 'none';
        } else {
            document.getElementById('auth-screen').style.display = 'flex';
            document.getElementById('main-app').style.display = 'none';
        }
    });

    function toggleLoading(show) { document.getElementById('loading').style.display = show ? 'flex' : 'none'; }

    window.consulterMission = (key) => {
        const m = allMissions.find(x => x.key === key);
        if(!m) return;
        
        let statutTxt = "En attente";
        let color = "#e74c3c";
        if(m.etape === 1) { statutTxt = "Accepté par livreur"; color = "#3A75C4"; }
        if(m.etape === 2) { statutTxt = "En cours de livraison"; color = "#FCD116"; }
        if(m.etape === 3) { statutTxt = "Livré avec succès"; color = "#009E60"; }

        const body = document.getElementById('modal-body');
        body.innerHTML = `
            <div style="text-align:center; padding:10px; background:${color}; color:white; border-radius:15px; margin-bottom:15px; font-weight:800; font-size:12px">
                STATUT : ${statutTxt.toUpperCase()}
            </div>
            <div class="detail-row"><b>ID Commande</b> <b style="color:var(--gabon-bleu)">#${m.id}</b></div>
            <div class="detail-row"><b>Bénéficiaire</b> <span>${m.nom}</span></div>
            <div class="detail-row"><b>Quartier</b> <span>${m.lieu || 'N/A'}</span></div>
            <div class="detail-row"><b>Montant à régler</b> <b>${m.retrait.toLocaleString()} FCFA</b></div>
            <div class="detail-row"><b>Livreur assigné</b> <span>${m.livreur}</span></div>
            ${m.photo ? `<div style="margin-top:15px"><span class="label-mini">Preuve de livraison</span><img src="${m.photo}" style="width:100%; border-radius:15px; margin-top:5px"></div>` : ''}
            ${auth.currentUser ? `<button class="btn-action btn-validate" style="background:#000" onclick="imprimerBon('${m.key}')">🖨️ IMPRIMER BON</button>` : ''}
        `;
        document.getElementById('modal-overlay').style.display = 'flex';
    };

    window.fermerModal = () => { document.getElementById('modal-overlay').style.display = 'none'; };

    // --- RESTE DES FONCTIONS DE RENDU (Missions, Bilan, etc.) ---
    window.renderUI = () => {
        const listAct = document.getElementById('list-active');
        const listVal = document.getElementById('list-validation');
        if(!listAct) return;
        listAct.innerHTML = "";
        listVal.innerHTML = "";
        
        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            if(m.etape < 3) {
                const card = `
                    <div class="card etape-${m.etape}">
                        <div style="display:flex; justify-content:space-between">
                            <b>${m.nom} (#${m.id})</b>
                            <span style="font-size:10px">${m.heureSeule || ''}</span>
                        </div>
                        <div style="font-size:12px; color:#64748b; margin:5px 0">${m.lieu || ''}</div>
                        <div style="font-weight:800; color:var(--gabon-vert)">${m.retrait.toLocaleString()} F</div>
                        <button class="btn-action" style="background:#f1f5f9; color:var(--dark); font-size:11px" onclick="consulterMission('${m.key}')">VOIR DETAILS</button>
                    </div>
                `;
                if(m.etape === 0 && userRole === 'admin') listVal.innerHTML += card;
                else listAct.innerHTML += card;
            }
        });
    };
</script>
</body>
</html>
