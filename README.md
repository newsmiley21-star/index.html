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
        .btn-validate:active { transform: scale(0.98); opacity: 0.9; }
        
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
        .btn-refresh-bilan:active { background: var(--gabon-jaune); color: var(--dark); }

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
        .detail-row b { color: var(--dark); font-size: 13px; }
        .detail-row span { color: #64748b; font-size: 13px; }

        .archive-item {
            border-bottom: 1px solid #eee;
            padding: 12px 0;
        }
        .archive-header {
            font-size: 12px; font-weight: bold; color: var(--gabon-bleu);
            display: flex; justify-content: space-between; margin-bottom: 5px;
        }
        .btn-consult, .btn-print-receipt {
            background: #f1f5f9; color: var(--gabon-bleu); border: none; border-radius: 8px;
            padding: 6px 12px; font-size: 10px; font-weight: 800; cursor: pointer;
        }
        .btn-print-receipt { background: #e0f2fe; margin-left: 5px; }
        
        .btn-delete-archive {
            background: #fef2f2; color: var(--danger); border: none; border-radius: 8px;
            padding: 6px 10px; font-size: 10px; font-weight: 800; cursor: pointer;
        }

        .search-bar {
            margin-bottom: 15px; position: relative;
        }
        .search-bar input {
            padding-left: 40px; background: #fff; border: 1px solid #e2e8f0;
        }
        .contact-group {
            display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 10px;
        }
        .btn-contact {
            padding: 10px; border-radius: 10px; font-size: 10px; font-weight: 800;
            text-decoration: none; text-align: center; border: 1px solid #e2e8f0;
        }
        .btn-whatsapp { background: #25D366; color: white; border: none; }
        .btn-call { background: #f1f5f9; color: var(--dark); }
        
        .loading-overlay {
            display: none; position: fixed; inset: 0; background: rgba(255,255,255,0.7);
            z-index: 10001; align-items: center; justify-content: center; font-weight: 800;
        }
        .btn-export {
            background: #000; color: white; padding: 10px; border-radius: 10px; font-size: 11px;
            border: none; cursor: pointer; margin-top: 10px; width: 100%;
        }
        .mission-time {
            font-size: 9px;
            background: #f1f5f9;
            padding: 2px 6px;
            border-radius: 5px;
            color: #64748b;
            font-weight: 600;
        }

        /* Styles spécifiques Suivi Client */
        .client-card {
            background: #fff; border-radius: 20px; padding: 25px; 
            border: 2px solid var(--gabon-bleu); text-align: center;
        }
        .status-bubble {
            display: inline-block; padding: 10px 20px; border-radius: 50px;
            font-weight: 900; margin-top: 15px; font-size: 18px; border: 2px solid;
        }

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
            #receipt-print { 
                position: absolute; 
                left: 0; top: 0; 
                width: 100% !important; 
                margin: 0; padding: 10px; 
            }
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
        <div style="margin-top:40px; border-top:1px dashed #000; padding-top:10px">
            <p>Signature Client</p>
            <br><br><br>
        </div>
        <p style="font-size:10px; margin-top:20px">Merci de votre confiance.</p>
    </div>

    <div id="auth-screen">
        <div class="login-card">
            <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" alt="Logo CT241" class="auth-logo">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #64748b; margin-bottom: 20px;">PORTAIL LOGISTIQUE GABON</p>
            <div id="login-fields">
                <input type="email" id="login-email" placeholder="Email">
                <input type="password" id="login-pass" placeholder="Mot de passe">
                <button class="btn-login" id="btnConnect">SE CONNECTER</button>
                <!-- VOTRE AJOUT : Lien consulter mission -->
                <p style="margin-top:20px; font-size:12px">
                    <a href="#" onclick="ouvrirSuiviInvite()" style="color:var(--gabon-bleu); font-weight:800; text-decoration:none">Suivre mon colis (Sans compte)</a>
                </p>
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
            <button onclick="ouvrir('suivi')" id="nav-suivi" class="active">SUIVI COLIS</button>
            <button onclick="ouvrir('missions')" id="nav-missions">MISSIONS <span id="count-missions"></span></button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">MON BILAN</button>
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">NOUVEAU</button>
            <button onclick="ouvrir('compta')" id="nav-compta" style="display:none">ADMIN</button>
            <button onclick="ouvrir('archives')" id="nav-archives" style="display:none">ARCHIVES</button>
        </nav>

        <!-- NOUVEL ONGLET : SUIVI CLIENT -->
        <div id="sec-suivi" class="section active-sec">
            <h4 style="margin:0 0 15px 0; color:var(--gabon-bleu)">SUIVI DE VOTRE COLIS</h4>
            <div class="search-bar">
                <input type="number" id="clientSearchInput" placeholder="Entrez votre ID de mission (4 chiffres)">
                <button class="btn-action btn-validate" onclick="trackerMission()">VÉRIFIER LE STATUT</button>
            </div>
            <div id="client-result" style="margin-top:20px"></div>
        </div>

        <!-- MISSIONS -->
        <div id="sec-missions" class="section">
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

        <!-- BILAN (LIVREUR) -->
        <div id="sec-bilan" class="section">
            <div class="stats-banner">
                <button class="btn-refresh-bilan" onclick="rafraichirBilan()">🔄 ACTUALISER</button>
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

            <span class="label-mini">Détails financiers</span>
            <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)">
            <div class="finance-row">
                <div>
                    <span class="label-mini">Livreur (F)</span>
                    <input type="number" id="mLiv" value="1000" readonly style="background:#f1f5f9">
                </div>
                <div>
                    <span class="label-mini">Commission (F)</span>
                    <input type="number" id="mCom" value="390">
                </div>
            </div>
            <button id="btnLancer" onclick="creerMission()" class="btn-action btn-validate">LANCER LA MISSION</button>
        </div>

        <!-- COMPTA ADMIN -->
        <div id="sec-compta" class="section">
            <div class="stats-banner" style="background:var(--gabon-vert)">
                <small>Tableau de bord Admin</small>
                <div class="stats-grid">
                    <div><small>Commissions Totales</small><b id="cpt-com" style="color:white">0 F</b></div>
                    <div><small>Volume Retraits</small><b id="cpt-vol" style="color:var(--gabon-jaune)">0 F</b></div>
                </div>
                <button class="btn-export" onclick="exportToCSV()">📥 EXPORTER LE BILAN (CSV)</button>
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
    import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-auth.js";
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

    // --- NAVIGATION ---
    window.ouvrir = (sec) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        const el = document.getElementById(`sec-${sec}`);
        if(el) el.classList.add('active-sec');
        const btn = document.getElementById(`nav-${sec}`);
        if(btn) btn.classList.add('active');
    };

    // --- AUTH ---
    document.getElementById('btnConnect').onclick = async () => {
        toggleLoading(true);
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        if(!email || !pass) { toggleLoading(false); return; }
        try { 
            await signInWithEmailAndPassword(auth, email, pass); 
        } catch(e) { 
            toggleLoading(false);
            alert("Accès refusé. Vérifiez vos identifiants."); 
        }
    };

    window.ouvrirSuiviInvite = async () => {
        toggleLoading(true);
        try {
            await signInAnonymously(auth);
        } catch(e) {
            alert("Erreur de connexion invitée");
        }
        toggleLoading(false);
    };
    
    document.getElementById('btnOut').onclick = () => {
        if(confirm("Se déconnecter ?")) signOut(auth);
    };

    onAuthStateChanged(auth, (u) => {
        toggleLoading(false);
        if(u) {
            if(u.isAnonymous) {
                userRole = "client";
                document.getElementById('user-role').innerText = "CLIENT";
                document.getElementById('nav-missions').style.display = 'none';
                document.getElementById('nav-bilan').style.display = 'none';
                document.getElementById('nav-suivi').style.display = 'block';
            } else {
                const email = u.email.toLowerCase();
                userRole = email.includes('admin') ? "admin" : (email.includes('finance') ? "finance" : "livreur");
                document.getElementById('user-role').innerText = userRole.toUpperCase();
                document.getElementById('nav-missions').style.display = 'block';
                document.getElementById('nav-bilan').style.display = 'block';
                document.getElementById('nav-suivi').style.display = 'block';
                
                document.getElementById('nav-creer').style.display = (userRole !== 'livreur') ? 'block' : 'none';
                document.getElementById('nav-compta').style.display = (userRole === 'admin') ? 'block' : 'none';
                document.getElementById('nav-archives').style.display = (userRole === 'admin' || userRole === 'finance') ? 'block' : 'none';
                document.getElementById('div-validation').style.display = (userRole === 'admin') ? 'block' : 'none';
            }
            
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';

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

    // --- VOTRE FONCTION TRACKERMISSION ---
    window.trackerMission = () => {
        const idSaisi = document.getElementById('clientSearchInput').value;
        const resDiv = document.getElementById('client-result');
        if(!idSaisi) return;

        const m = allMissions.find(x => String(x.id) === String(idSaisi));
        if(!m) {
            resDiv.innerHTML = `<p style="color:red; font-weight:bold">Aucune mission trouvée pour l'ID #${idSaisi}</p>`;
            return;
        }

        let statusText = "";
        let color = "";
        switch(m.etape) {
            case 0: statusText = "EN ATTENTE"; color = "var(--danger)"; break;
            case 1: statusText = "EN COURS"; color = "var(--gabon-bleu)"; break;
            case 2: statusText = "ARRIVÉ SUR ZONE"; color = "var(--gabon-jaune)"; break;
            case 3: statusText = "LIVRÉ"; color = "var(--gabon-vert)"; break;
        }

        resDiv.innerHTML = `
            <div class="client-card">
                <small>STATUT DE VOTRE COLIS (#${m.id})</small>
                <div class="status-bubble" style="color:${color}; border-color:${color}">
                    ${statusText}
                </div>
                <div style="margin-top:20px; text-align:left; font-size:14px">
                    <p><b>Bénéficiaire:</b> ${m.nom}</p>
                    <p><b>Lieu:</b> ${m.lieu || 'N/A'}</p>
                    <p><b>Livreur:</b> ${m.livreur}</p>
                    <p><b>Dernière mise à jour:</b> ${m.heureSeule || 'N/A'}</p>
                </div>
            </div>
        `;
    };

    function toggleLoading(show) {
        document.getElementById('loading').style.display = show ? 'flex' : 'none';
    }

    // --- LOGIQUE METIER (RESTE INCHANGÉ) ---
    window.imprimerBon = (key) => {
        const m = allMissions.find(x => x.key === key);
        if(!m) return;
        document.getElementById('pr-id').innerText = "ID: #" + m.id;
        document.getElementById('pr-date').innerText = "Date: " + (m.dateLong || new Date(m.timestamp).toLocaleDateString());
        document.getElementById('pr-nom').innerText = m.nom;
        document.getElementById('pr-tel').innerText = m.tel || "N/A";
        document.getElementById('pr-lieu').innerText = m.lieu || "N/A";
        document.getElementById('pr-liv').innerText = m.livreur;
        document.getElementById('pr-montant').innerText = m.retrait.toLocaleString() + " FCFA";
        setTimeout(() => { window.print(); }, 300);
    };

    window.filterMissions = (inputId) => {
        const term = document.getElementById(inputId).value.toLowerCase();
        renderUI(term);
    };

    window.rafraichirBilan = () => {
        toggleLoading(true);
        setTimeout(() => { renderUI(); toggleLoading(false); alert("Bilan actualisé !"); }, 800);
    };

    window.updateFrais = () => {
        document.getElementById('mLiv').value = document.getElementById('mZoneSelect').value;
    };

    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const tel = document.getElementById('mTel').value;
        const quartier = document.getElementById('mQuartier').value;
        const retrait = parseInt(document.getElementById('mRetrait').value);
        const liv = parseInt(document.getElementById('mLiv').value);
        const com = parseInt(document.getElementById('mCom').value);
        if(!nom || !retrait) { alert("Nom et Montant requis"); return; }
        const now = new Date();
        const mission = {
            id: Math.floor(1000 + Math.random() * 9000),
            nom, tel, lieu: quartier, retrait, 
            fraisLivraison: liv, com, 
            livreur: "Libre", etape: 1, 
            timestamp: Date.now(),
            dateHeure: now.toLocaleString(),
            dateLong: now.toLocaleDateString('fr-FR'),
            heureSeule: now.toLocaleTimeString('fr-FR', {hour:'2-digit', minute:'2-digit'})
        };
        push(ref(db, 'missions'), mission);
        alert("Mission déployée !");
        ouvrir('missions');
    };

    window.valider = (key) => { update(ref(db, `missions/${key}`), { etape: 1 }); };
    window.accepter = (key) => { 
        const myName = auth.currentUser.email.split('@')[0].toUpperCase();
        update(ref(db, `missions/${key}`), { livreur: myName }); 
    };

    window.fermerModal = () => { document.getElementById('modal-overlay').style.display = 'none'; };

    window.consulterMission = (key) => {
        const m = allMissions.find(x => x.key === key);
        if(!m) return;
        const body = document.getElementById('modal-body');
        body.innerHTML = `
            <div class="detail-row"><b>Bénéficiaire</b> <span>${m.nom}</span></div>
            <div class="detail-row"><b>ID Mission</b> <span style="color:var(--gabon-bleu); font-weight:800">#${m.id}</span></div>
            <div class="detail-row"><b>Montant Retrait</b> <b>${m.retrait.toLocaleString()} F</b></div>
            <div class="detail-row"><b>Livreur</b> <span>${m.livreur}</span></div>
            <button class="btn-action btn-validate" style="background:#000" onclick="imprimerBon('${m.key}')">🖨️ IMPRIMER BON</button>
        `;
        document.getElementById('modal-overlay').style.display = 'flex';
    };

    window.renderUI = (filter = "") => {
        if(userRole === "client") return;
        
        const listVal = document.getElementById('list-validation');
        const listAct = document.getElementById('list-active');
        const listBilToday = document.getElementById('list-bilan-today');
        const listCpt = document.getElementById('list-compta-daily');
        const listArchivesGlobal = document.getElementById('list-archives-global');
        
        listVal.innerHTML = ""; listAct.innerHTML = ""; listBilToday.innerHTML = ""; 
        listCpt.innerHTML = ""; listArchivesGlobal.innerHTML = "";

        const todayStr = new Date().toLocaleDateString('fr-FR');
        const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "";

        let dailyGains = 0, dailyCount = 0, adminCom = 0, adminVol = 0;

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            const match = !filter || m.nom.toLowerCase().includes(filter) || String(m.id).includes(filter);
            const mDateStr = new Date(m.timestamp).toLocaleDateString('fr-FR');
            const isToday = (mDateStr === todayStr);

            if(m.etape < 3) {
                if(!match) return;
                const card = `
                    <div class="card etape-${m.etape}">
                        <b>#${m.id} - ${m.nom}</b><br>
                        <small>${m.lieu || 'N/A'}</small><br>
                        <small>Livreur: ${m.livreur}</small>
                        ${m.livreur === "Libre" ? `<button onclick="accepter('${m.key}')" class="btn-action btn-validate">ACCEPTER</button>` : ''}
                    </div>
                `;
                if(m.etape === 0 && userRole === 'admin') listVal.innerHTML += card;
                else if(m.etape > 0) listAct.innerHTML += card;
            } else {
                if(m.livreur === myName && isToday) { dailyGains += m.fraisLivraison; dailyCount++; }
                if(userRole === 'admin' && isToday) { adminCom += m.com; adminVol += m.retrait; }
            }
        });

        document.getElementById('stat-total').innerText = dailyGains.toLocaleString() + " F";
        document.getElementById('stat-count').innerText = dailyCount;
        if(userRole === 'admin') {
            document.getElementById('cpt-com').innerText = adminCom.toLocaleString() + " F";
            document.getElementById('cpt-vol').innerText = adminVol.toLocaleString() + " F";
        }
    };
</script>
</body>
</html>
