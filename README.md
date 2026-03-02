<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - SYSTÈME DE DISTRIBUTION</title>
    <style>
        :root {
            --gabon-vert: #009E60; --gabon-jaune: #FCD116; --gabon-bleu: #3A75C4;
            --danger: #e74c3c; --dark: #1a1a1a; --light: #f8f9fa;
        }
        body { font-family: 'Segoe UI', system-ui, sans-serif; background: var(--light); margin: 0; padding: 10px; }
        
        /* AUTH */
        #auth-screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #1e272e; display: flex; align-items: center; justify-content: center; z-index: 9999; }
        .login-card { background: white; padding: 25px; border-radius: 20px; width: 85%; max-width: 320px; text-align: center; border-top: 8px solid var(--gabon-jaune); }
        input { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; font-size: 16px; }
        .btn-login { width: 100%; padding: 14px; background: var(--gabon-vert); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }

        /* APP */
        #main-app { display: none; max-width: 600px; margin: auto; background: white; border-radius: 15px; padding: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); min-height: 95vh; }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        .role-badge { font-size: 10px; padding: 3px 10px; border-radius: 12px; background: var(--dark); color: white; }

        nav { display: flex; gap: 5px; margin-bottom: 15px; background: #eee; padding: 5px; border-radius: 10px; }
        nav button { flex: 1; padding: 12px 5px; border: none; border-radius: 7px; background: transparent; font-weight: bold; font-size: 11px; color: #666; cursor: pointer; }
        nav button.active { background: white; color: var(--gabon-vert); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        
        .section { display: none; }
        .active-sec { display: block; }

        /* CARDS */
        .card { border: 1px solid #eee; padding: 15px; border-radius: 12px; margin-bottom: 12px; position: relative; border-left: 6px solid #ccc; transition: 0.3s; }
        .card.etape-0 { border-left-color: #95a5a6; background: #fdfdfd; border-style: dashed; }
        .card.etape-1 { border-left-color: var(--gabon-bleu); animation: flash 2s infinite; }
        .card.etape-2 { border-left-color: var(--gabon-jaune); }
        
        @keyframes flash { 0% { opacity: 1; } 50% { opacity: 0.7; } 100% { opacity: 1; } }

        .card-title { font-weight: bold; font-size: 14px; margin-bottom: 5px; }
        .card-info { font-size: 12px; color: #555; line-height: 1.4; }
        .badge-id { position: absolute; top: 10px; right: 10px; background: #f1f2f6; padding: 2px 6px; border-radius: 4px; font-size: 10px; font-weight: bold; color: var(--gabon-bleu); }

        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; font-size: 13px; }
        .btn-validate { background: var(--gabon-vert); color: white; }
        .btn-accept { background: var(--gabon-bleu); color: white; }
        .btn-photo { background: var(--dark); color: white; }

        .preview-img { width: 100%; margin-top: 10px; border-radius: 8px; display: none; border: 1px solid #ddd; }
        
        .stats-footer { background: var(--dark); color: white; padding: 15px; border-radius: 12px; margin-top: 15px; }
        .time-badge { font-size: 9px; color: #999; display: block; margin-top: 4px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666; margin-bottom: 15px;">Système de Distribution en Temps Réel</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-vert)">CT241 OPS</h3>
                <span id="user-display" style="font-size:10px; color:#888"></span>
                <span id="user-role" class="role-badge">...</span>
            </div>
            <button id="btnOut" style="font-size:10px; color:var(--danger); background:none; border:none; font-weight:bold; cursor:pointer;">DÉCONNEXION</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">NOUVEAU</button>
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">MISSIONS <span id="count-missions"></span></button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">BILAN</button>
        </nav>

        <!-- CRÉATION -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 15px; border: 1px solid #eee; border-radius: 12px;">
                <h4 style="margin-top:0">Nouvelle Commande</h4>
                <div style="font-size: 10px; color: #888; margin-bottom: 5px;">Infos client et calculs automatiques activés</div>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="N° de Téléphone Client">
                <input type="text" id="mLieu" placeholder="Destination (Quartier/Ville)">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)">
                <button onclick="creerMission()" class="btn-action" style="background:var(--dark); color:white">ENVOYER POUR VALIDATION</button>
            </div>
        </div>

        <!-- LISTE MISSIONS -->
        <div id="sec-missions" class="section active-sec">
            <div id="div-validation" style="display:none; margin-bottom:20px;">
                <h5 style="color:var(--danger); margin:0 0 10px 0">🛡️ ATTENTE VALIDATION (ADMIN)</h5>
                <div id="list-validation"></div>
            </div>

            <h5 style="color:var(--gabon-bleu); margin:0 0 10px 0">🛵 MISSIONS DISPONIBLES / EN COURS</h5>
            <div id="list-active"></div>
        </div>

        <!-- BILAN -->
        <div id="sec-bilan" class="section">
            <input type="text" id="bilanSearch" placeholder="Rechercher Client, ID ou N°..." oninput="renderUI()" style="margin-bottom:10px">
            <div id="list-bilan"></div>
            <div class="stats-footer">
                <div style="display:flex; justify-content:space-between; align-items:center">
                    <span>SOLDE DU JOUR :</span>
                    <b id="stat-total" style="color:var(--gabon-jaune); font-size:1.2em">0 F</b>
                </div>
            </div>
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
            document.getElementById('user-display').innerText = u.email;
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            
            document.getElementById('nav-creer').style.display = (userRole === 'admin' || userRole === 'finance') ? 'block' : 'none';
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
        const search = document.getElementById('bilanSearch').value.toLowerCase();
        
        listVal.innerHTML = ""; listAct.innerHTML = ""; listBil.innerHTML = "";
        let totalGain = 0;
        const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "INVITÉ";

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            if(m.etape === 3) {
                if((userRole === 'admin' || userRole === 'finance' || m.livreur === myName) && 
                   (m.nom.toLowerCase().includes(search) || m.id.toLowerCase().includes(search) || (m.tel && m.tel.includes(search)))) {
                    
                    const gain = (userRole === 'livreur') ? m.fraisLivraison : m.com;
                    totalGain += gain;
                    
                    listBil.innerHTML += `<div style="padding:10px; border-bottom:1px solid #eee; font-size:12px; display:flex; justify-content:space-between">
                            <span><b>${m.nom}</b> [${m.tel || 'N/A'}]<br><small>${m.id} | ${m.livreur} | ${m.dateHeure}</small></span>
                            <span style="color:var(--gabon-vert); font-weight:bold">+${gain.toLocaleString()} F</span>
                        </div>`;
                }
                return;
            }

            const cardHtml = `<div class="card etape-${m.etape}">
                    <span class="badge-id">${m.id}</span>
                    <div class="card-title">${m.nom} <span style="font-weight:normal; font-size:11px; color:var(--gabon-bleu)">(${m.tel || 'N/A'})</span></div>
                    <div class="card-info">
                        📍 <b>${m.lieu}</b><br>💰 Retrait : <b>${m.retrait.toLocaleString()} F</b><br>
                        🛵 Livreur : <b>${m.livreur}</b>
                        <span class="time-badge">Créé le : ${m.dateHeure}</span>
                    </div>`;

            if(m.etape === 0 && userRole === 'admin') {
                listVal.innerHTML += cardHtml + `<button class="btn-action btn-validate" onclick="valider('${m.key}')">VALIDER LA MISSION</button></div>`;
            }

            if(m.etape > 0) {
                let controls = "";
                if(m.etape === 1 && m.livreur === "Libre" && userRole === 'livreur') {
                    controls = `<button class="btn-action btn-accept" onclick="accepter('${m.key}')">ACCEPTER LA COURSE</button>`;
                } 
                else if(m.etape === 1 && m.livreur === myName) {
                    controls = `<button class="btn-action btn-photo" onclick="triggerCam('${m.key}')">📸 PHOTO SMS</button>
                        <img id="pre-${m.key}" class="preview-img">
                        <input type="text" id="code-${m.key}" placeholder="Code SMS" style="margin-top:10px">
                        <button class="btn-action btn-validate" onclick="terminer('${m.key}')">ENVOYER À LA FINANCE</button>`;
                }
                else if(m.etape === 2 && (userRole === 'admin' || userRole === 'finance')) {
                    controls = `<div style="background:#f9f9f9; padding:10px; border-radius:10px; margin-top:10px; border:1px solid #ddd; text-align:center">
                            <img src="${m.photo}" style="width:100%; border-radius:5px">
                            <p style="color:red; font-size:20px; font-weight:bold; margin:10px 0">${m.codeSMS}</p>
                            <button class="btn-action btn-validate" onclick="cloturer('${m.key}')">CONFIRMER ENCAISSEMENT</button>
                        </div>`;
                }
                listAct.innerHTML += cardHtml + controls + `</div>`;
            }
        });

        document.getElementById('stat-total').innerText = totalGain.toLocaleString() + " F";
        const count = allMissions.filter(x => x.etape === 1 && x.livreur === "Libre").length;
        document.getElementById('count-missions').innerHTML = count > 0 ? `<b style="background:red; color:white; padding:1px 6px; border-radius:10px; font-size:9px; vertical-align:middle">${count}</b>` : "";
    }

    // ACTIONS
    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const tel = document.getElementById('mTel').value;
        const mnt = parseInt(document.getElementById('mRetrait').value);
        const lieu = document.getElementById('mLieu').value;

        if(!nom || !mnt || !tel) return alert("Nom, Téléphone et Montant obligatoires !");

        // AUTOMATISMES
        const now = new Date();
        const dateHeure = now.toLocaleDateString('fr-FR') + " à " + now.toLocaleTimeString('fr-FR', {hour: '2-digit', minute:'2-digit'});
        
        const randomNum = Math.floor(10000 + Math.random() * 89999);
        const orderID = "CT" + randomNum;

        // Calculs automatiques des frais
        const commission = (mnt >= 15000 ? 390 : 190);
        const fraisLivraison = 1000; // Frais fixe pour le livreur

        push(ref(db, 'missions'), {
            id: orderID,
            nom, 
            tel,
            lieu: lieu || "Non spécifié",
            retrait: mnt, 
            com: commission,
            fraisLivraison: fraisLivraison,
            livreur: "Libre", 
            etape: 0, 
            dateHeure: dateHeure,
            timestamp: Date.now()
        });

        // Reset formulaire
        document.getElementById('mNom').value = ""; 
        document.getElementById('mTel').value = "";
        document.getElementById('mRetrait').value = "";
        document.getElementById('mLieu').value = "";
        
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
        if(!code || !lastPhotoData) return alert("Photo et Code requis !");
        update(ref(db, `missions/${k}`), { etape: 2, codeSMS: code, photo: lastPhotoData });
    };

    window.cloturer = (k) => confirm("Confirmer l'encaissement ?") && update(ref(db, `missions/${k}`), { etape: 3 });

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('nav-'+id).classList.add('active');
    };
</script>
</body>
</html>
