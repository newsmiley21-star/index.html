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

        /* --- STYLES IMPRESSION BON --- */
        #receipt-print {
            display: none; /* Caché par défaut en mode écran */
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

    <!-- ZONE IMPRESSION (TICKET CAISSE) -->
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

        <!-- BILAN (LIVREUR) -->
        <div id="sec-bilan" class="section">
            <div class="stats-banner">
                <button class="btn-refresh-bilan" onclick="rafraichirBilan()">🔄 ACTUALISER</button>
                <small>Session Active (Aujourd'hui)</small>
                <div class="stats-grid">
                    <div><small>Commissions Totales</small><b id="cpt-com" style="color:white">0 F</b></div>
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
                <input type="text" id="mQuartier" placeholder="lien ITINERAIRE">
                <select id="mZoneSelect" onchange="updateFrais()">
                    <option value="2000">Libreville  (2000 F)</option>
                    <option value="2500">Owendo / Akanda (2500 F)</option>
                    <option value="2000">PK / Ntoum / Angondjé (2000 F)</option>
                </select>
            </div>

            <span class="label-mini">Détails financiers</span>
            <input type="number" id="mRetrait" placeholder="prix du colis (FCFA)">
            <div class="finance-row">
                <div>
                    <span class="label-mini">frais livraison (CFA)</span>
                    <input type="number" id="mLiv" value="2000" readonly style="background:#f1f5f9">
                </div>
                <div>
                    <span class="label-mini">Com LIVREUR (CFA)</span>
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
                    <div><small>Gains du Jour</small><b id="stat-total">0 F</b></div>
                    <div><small>C.A </small><b id="Stat-count" style="color:var(--gabon-jaune)">0 F</b></div>
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

    // --- NAVIGATION ---
    window.ouvrir = (sec) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById(`sec-${sec}`).classList.add('active-sec');
        document.getElementById(`nav-${sec}`).classList.add('active');
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
    
    document.getElementById('btnOut').onclick = () => {
        if(confirm("Se déconnecter ?")) signOut(auth);
    };

    onAuthStateChanged(auth, (u) => {
        toggleLoading(false);
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

    function toggleLoading(show) {
        document.getElementById('loading').style.display = show ? 'flex' : 'none';
    }

    // --- GESTION DES IMPRESSIONS ---
    window.imprimerBon = (key) => {
        const m = allMissions.find(x => x.key === key);
        if(!m) return;

        // Remplissage des données
        document.getElementById('pr-id').innerText = "ID: #" + m.id;
        document.getElementById('pr-date').innerText = "Date: " + (m.dateLong || new Date(m.timestamp).toLocaleDateString());
        document.getElementById('pr-nom').innerText = m.nom;
        document.getElementById('pr-tel').innerText = m.tel || "N/A";
        document.getElementById('pr-lieu').innerText = m.lieu || "N/A";
        document.getElementById('pr-liv').innerText = m.livreur;
        document.getElementById('pr-montant').innerText = m.retrait.toLocaleString() + " FCFA";

        // Petite pause pour garantir que le DOM est mis à jour avant l'appel système d'impression
        setTimeout(() => {
            window.print();
        }, 300);
    };

    window.filterMissions = (inputId) => {
        const term = document.getElementById(inputId).value.toLowerCase();
        renderUI(term);
    };

    window.rafraichirBilan = () => {
        toggleLoading(true);
        setTimeout(() => {
            renderUI();
            toggleLoading(false);
            alert("Bilan actualisé ! Les missions du jour précédent sont dans l'historique.");
        }, 800);
    };

    window.openMaps = (query) => {
        if(!query) return;
        window.open(`https://www.google.com/maps/search/${encodeURIComponent(query + " Libreville")}`, '_blank');
    };

    window.exportToCSV = () => {
        if(allMissions.length === 0) return;
        let csv = "ID;Date;Heure;Beneficiaire;Retrait;Livreur;Frais;Commission;Status\n";
        allMissions.forEach(m => {
            const status = m.etape === 3 ? "Terminee" : "En cours";
            csv += `${m.id};${m.dateLong || m.dateHeure};${m.heureSeule || ''};${m.nom};${m.retrait};${m.livreur};${m.fraisLivraison};${m.com};${status}\n`;
        });
        const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
        const link = document.createElement("a");
        link.href = URL.createObjectURL(blob);
        link.setAttribute("download", `Bilan_CT241_${new Date().toLocaleDateString()}.csv`);
        link.click();
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

    window.fermerModal = () => {
        document.getElementById('modal-overlay').style.display = 'none';
    };

    window.consulterMission = (key) => {
        const m = allMissions.find(x => x.key === key);
        if(!m) return;
        const body = document.getElementById('modal-body');
        body.innerHTML = `
            <div class="detail-row"><b>Bénéficiaire</b> <span>${m.nom}</span></div>
            <div class="detail-row"><b>ID Mission</b> <span style="color:var(--gabon-bleu); font-weight:800">#${m.id}</span></div>
            <div class="detail-row"><b>Date Création</b> <span>${m.dateLong || m.dateHeure}</span></div>
            <div class="detail-row"><b>Téléphone</b> <a href="tel:${m.tel}">${m.tel || 'N/A'}</a></div>
            <div class="detail-row"><b>Lieu</b> <span>${m.lieu || 'N/A'}</span></div>
            <div class="detail-row"><b>Montant Retrait</b> <b>${m.retrait.toLocaleString()} F</b></div>
            <div class="detail-row"><b>Commission</b> <span>${m.com} F</span></div>
            <div class="detail-row"><b>Livreur</b> <span>${m.livreur}</span></div>
            <div style="margin-top:15px; background:#f8fafc; padding:15px; border-radius:15px; text-align:center">
                <span class="label-mini">Code SMS Preuve</span>
                <h2 style="letter-spacing:4px; color:var(--dark); margin:10px 0">${m.codeSMS || 'N/A'}</h2>
                ${m.photo ? `<img src="${m.photo}" style="width:100%; border-radius:12px; border:2px solid white; box-shadow:0 5px 15px rgba(0,0,0,0.1)">` : '<p style="font-size:10px; color:red">Aucune photo</p>'}
            </div>
            <button class="btn-action btn-validate" style="background:#000" onclick="imprimerBon('${m.key}')">🖨️ IMPRIMER BON DE LIVRAISON</button>
        `;
        document.getElementById('modal-overlay').style.display = 'flex';
    };

    window.supprimerMission = (key, id) => {
        if(userRole !== 'admin') return;
        if(confirm(`Supprimer définitivement #${id} ?`)) {
            remove(ref(db, `missions/${key}`)).catch(() => alert("Erreur"));
        }
    };

    window.renderUI = (filter = "") => {
        const listVal = document.getElementById('list-validation');
        const listAct = document.getElementById('list-active');
        const listBilToday = document.getElementById('list-bilan-today');
        const listHistory = document.getElementById('archive-history');
        const listCpt = document.getElementById('list-compta-daily');
        const listArchivesGlobal = document.getElementById('list-archives-global');
        
        listVal.innerHTML = ""; listAct.innerHTML = ""; listBilToday.innerHTML = ""; 
        listHistory.innerHTML = ""; listCpt.innerHTML = ""; listArchivesGlobal.innerHTML = "";

        const todayStr = new Date().toLocaleDateString('fr-FR');
        const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "";

        let dailyGains = 0, dailyCount = 0, adminCom = 0, adminVol = 0;
        const historyGroups = {}, globalArchiveGroups = {};

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            const match = !filter || m.nom.toLowerCase().includes(filter) || String(m.id).includes(filter) || (m.lieu && m.lieu.toLowerCase().includes(filter));
            const mDateStr = new Date(m.timestamp).toLocaleDateString('fr-FR');
            const isToday = (mDateStr === todayStr);

            if(m.etape < 3) {
                if(!match) return;
                const card = createCard(m, myName);
                if(m.etape === 0 && userRole === 'admin') listVal.innerHTML += card;
                else if(m.etape > 0) listAct.innerHTML += card;
            } else {
                if(m.livreur === myName) {
                    if(isToday) { 
                        dailyGains += m.fraisLivraison; 
                        dailyCount++; 
                        listBilToday.innerHTML += createRow(m, "livreur"); 
                    } else {
                        if(!historyGroups[mDateStr]) historyGroups[mDateStr] = { sum: 0, items: [] };
                        historyGroups[mDateStr].sum += m.fraisLivraison;
                        historyGroups[mDateStr].items.push(m);
                    }
                }
                
                if(userRole === 'admin' && isToday) { 
                    adminCom += m.com; 
                    adminVol += m.retrait; 
                    listCpt.innerHTML += createRow(m, "admin"); 
                }
                
                if(userRole === 'admin' || userRole === 'finance') {
                    if(!match) return;
                    if(!globalArchiveGroups[mDateStr]) globalArchiveGroups[mDateStr] = [];
                    globalArchiveGroups[mDateStr].push(m);
                }
            }
        });

        Object.keys(historyGroups).sort((a,b) => {
            const dateA = a.split('/').reverse().join('');
            const dateB = b.split('/').reverse().join('');
            return dateB.localeCompare(dateA);
        }).forEach(date => {
            let html = `<div class="date-divider"><span>📅 Historique: ${date}</span> <span>${historyGroups[date].sum.toLocaleString()} F</span></div>`;
            historyGroups[date].items.forEach(item => html += createRow(item, "livreur", true));
            listHistory.innerHTML += html;
        });

        Object.keys(globalArchiveGroups).sort((a,b) => {
            const dateA = a.split('/').reverse().join('');
            const dateB = b.split('/').reverse().join('');
            return dateB.localeCompare(dateA);
        }).forEach(date => {
            let html = `<div class="date-divider"><span>📦 ARCHIVES DU ${date}</span></div>`;
            globalArchiveGroups[date].forEach(m => {
                const showDel = userRole === 'admin' ? `<button class="btn-delete-archive" onclick="supprimerMission('${m.key}', '${m.id}')">🗑️</button>` : '';
                html += `
                <div class="archive-item">
                    <div class="archive-header"><span>${m.nom} (#${m.id})</span><span style="color:var(--gabon-vert)">+ ${m.com.toLocaleString()} F</span></div>
                    <div style="color:#64748b; font-size:10px; display:flex; justify-content:space-between; align-items:flex-end">
                        <div>Livreur: <b>${m.livreur}</b> | ${m.heureSeule || m.dateHeure}</div>
                        <div style="display:flex; gap:8px">
                            ${showDel}
                            <button class="btn-print-receipt" onclick="imprimerBon('${m.key}')">BON</button>
                            <button class="btn-consult" onclick="consulterMission('${m.key}')">Détails</button>
                        </div>
                    </div>
                </div>`;
            });
            listArchivesGlobal.innerHTML += html;
        });

        document.getElementById('stat-total').innerText = dailyGains.toLocaleString() + " F";
        document.getElementById('stat-count').innerText = dailyCount;
        if(userRole === 'admin') {
            document.getElementById('cpt-com').innerText = adminCom.toLocaleString() + " F";
            document.getElementById('cpt-vol').innerText = adminVol.toLocaleString() + " F";
        }
        const countActive = allMissions.filter(m => m.etape > 0 && m.etape < 3).length;
        document.getElementById('count-missions').innerHTML = countActive > 0 ? `<span class="badge badge-blue">${countActive}</span>` : "";
    };

    function createCard(m, myName) {
        let btn = "", contactUI = "";
        
        if(m.tel) {
            contactUI = `
                <div class="contact-group">
                    <a href="tel:${m.tel}" class="btn-contact btn-call">📞 APPELER</a>
                    <a href="https://wa.me/241${m.tel.replace(/\s/g, '')}" target="_blank" class="btn-contact btn-whatsapp">💬 WHATSAPP</a>
                </div>
            `;
        }

        if(m.etape === 0 && userRole === 'admin') {
            btn = `<button class="btn-action btn-validate" onclick="valider('${m.key}')">VALIDER & PUBLIER</button>`;
        } 
        else if(m.etape === 1) {
            if(m.livreur === "Libre" && userRole === 'livreur') {
                btn = `<button class="btn-action btn-validate" style="background:var(--gabon-bleu)" onclick="accepter('${m.key}')">ACCEPTER LA COURSE</button>`;
            } 
            else if(m.livreur === myName) {
                btn = `
                    <button class="btn-action" style="background:#000; color:#fff" onclick="triggerCam('${m.key}')">📸 PRENDRE PHOTO SMS</button>
                    <input type="text" id="code-${m.key}" placeholder="Saisir Code SMS">
                    <button class="btn-action btn-validate" onclick="terminer('${m.key}')">ENVOYER POUR ENCAISSEMENT</button>
                `;
            } else {
                btn = `<p style="font-size:10px; color:#64748b; text-align:center">Course prise par <b>${m.livreur}</b></p>`;
            }
        } 
        else if(m.etape === 2 && (userRole === 'admin' || userRole === 'finance')) {
            btn = `<div style="text-align:center; padding:10px; background:#f8fafc; border-radius:10px">
                    <img src="${m.photo}" style="width:100%; border-radius:8px; border:1px solid #e2e8f0; margin-bottom:10px">
                    <h1 style="margin:5px 0; color:var(--dark); letter-spacing:2px">${m.codeSMS}</h1>
                    <button class="btn-action btn-validate" onclick="cloturer('${m.key}')">VALIDER ENCAISSEMENT ✅</button>
                   </div>`;
        }

        const mDate = new Date(m.timestamp);
        const displayDate = `${mDate.toLocaleDateString('fr-FR')} à ${mDate.toLocaleTimeString('fr-FR', {hour:'2-digit', minute:'2-digit'})}`;

        return `<div class="card etape-${m.etape}">
                    <div style="display:flex; justify-content:space-between; font-weight:800; font-size:13px">
                        <span>${m.nom} <span class="mission-time">${displayDate}</span></span> <span style="color:var(--gabon-bleu)">#${m.id}</span>
                    </div>
                    <div style="font-size:12px; color:#475569; margin:5px 0; line-height:1.4">
                        <div onclick="openMaps('${m.lieu}')" style="cursor:pointer; color:var(--gabon-bleu)">📍 <b>${m.lieu || 'Zone...'}</b> 🗺️</div>
                        💰 Retrait: <b>${m.retrait.toLocaleString()} F</b> | Gain: <b>${m.fraisLivraison.toLocaleString()} F</b>
                    </div>
                    ${contactUI}
                    ${btn}
                </div>`;
    }

    function createRow(m, type, isOld = false) {
        const val = type === 'admin' ? m.com : m.fraisLivraison;
        const sub = type === 'admin' ? `Liv: ${m.fraisLivraison}F | ${m.livreur}` : `Retrait: ${m.retrait.toLocaleString()}F`;
        const color = isOld ? '#94a3b8' : (type === 'admin' ? 'var(--gabon-bleu)' : 'var(--gabon-vert)');
        const displayDate = m.dateLong || `${new Date(m.timestamp).toLocaleDateString('fr-FR')} ${m.dateHeure}`;
        const showPrint = (userRole === 'admin' || userRole === 'finance') ? `<span onclick="imprimerBon('${m.key}')" style="font-size:8px; margin-right:8px; text-decoration:underline; cursor:pointer; color:var(--dark)">Bon</span>` : '';
        
        return `<div style="padding:12px; border-bottom:1px solid #f1f5f9; display:flex; justify-content:space-between; align-items:center; font-size:11px">
                    <div><b>${m.nom}</b> <span style="font-size:9px; color:#94a3b8">#${m.id}</span><br>
                    <small style="color:#94a3b8">${sub} | ${displayDate}</small></div>
                    <div style="text-align:right">
                        <b style="color:${color}; font-size:13px">+ ${val.toLocaleString()} F</b><br>
                        ${showPrint}
                        <span onclick="consulterMission('${m.key}')" style="font-size:8px; text-decoration:underline; cursor:pointer; color:var(--gabon-bleu)">Détails</span>
                    </div>
                </div>`;
    }

    // --- PHOTO / CAMERA ---
    window.triggerCam = (key) => { currentKey = key; document.getElementById('camInput').click(); };
    document.getElementById('camInput').onchange = (e) => {
        const file = e.target.files[0];
        if(!file) return;
        const reader = new FileReader();
        reader.onload = (re) => {
            const img = new Image();
            img.onload = () => {
                const canvas = document.getElementById('canvas');
                const ctx = canvas.getContext('2d');
                canvas.width = 600; canvas.height = (img.height/img.width)*600;
                ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                lastPhotoData = canvas.toDataURL('image/jpeg', 0.7);
                alert("Photo enregistrée ! Saisissez le code et validez.");
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.terminer = (key) => {
        const code = document.getElementById(`code-${key}`).value;
        if(!code || !lastPhotoData) { alert("Code + Photo requis"); return; }
        update(ref(db, `missions/${key}`), { codeSMS: code, photo: lastPhotoData, etape: 2 });
        lastPhotoData = "";
    };

    window.cloturer = (key) => { update(ref(db, `missions/${key}`), { etape: 3 }); };

</script>
</body>
</html>
