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
        
        /* Auth Screen */
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
        .auth-logo { width: 100%; max-width: 180px; margin-bottom: 20px; }

        input, select { 
            width: 100%; padding: 12px; margin: 8px 0; border: 2px solid #edf2f7; 
            border-radius: 14px; box-sizing: border-box; font-size: 16px; transition: 0.3s;
            background: #f8fafc;
        }
        
        .btn-login { 
            width: 100%; padding: 16px; background: var(--gabon-vert); color: white; 
            border: none; border-radius: 14px; font-weight: 800; cursor: pointer; margin-top: 10px;
        }

        /* App Layout */
        #main-app { 
            display: none; max-width: 500px; margin: auto; background: var(--white); 
            border-radius: 30px; padding: 20px; box-shadow: 0 15px 35px rgba(0,0,0,0.06); 
            min-height: 90vh; padding-bottom: 40px;
        }

        header { 
            display: flex; justify-content: space-between; align-items: center; 
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

        /* Cards */
        .card { 
            background: var(--white); border: 1px solid #f1f5f9; padding: 15px; 
            border-radius: 18px; margin-bottom: 12px; border-left: 6px solid #cbd5e1;
            position: relative;
        }
        .card.etape-0 { border-left-color: var(--danger); } 
        .card.etape-1 { border-left-color: var(--gabon-jaune); } 
        .card.etape-2 { border-left-color: var(--gabon-vert); } 

        .btn-action { 
            width: 100%; padding: 12px; border: none; border-radius: 12px; 
            font-weight: 800; cursor: pointer; margin-top: 10px;
        }
        .btn-validate { background: var(--gabon-vert); color: white; }
        .btn-photo { background: var(--gabon-bleu); color: white; }
        .btn-delete-mini { 
            position: absolute; top: 10px; right: 10px; background: #fee2e2; 
            color: var(--danger); border: none; padding: 5px 8px; border-radius: 8px; 
            font-size: 10px; font-weight: bold; cursor: pointer;
        }
        
        .stats-banner {
            background: var(--dark); color: white; padding: 20px; border-radius: 20px; margin-bottom: 20px;
        }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 10px; }
        .stats-banner b { font-size: 18px; color: var(--gabon-jaune); }

        .label-mini { font-size: 10px; font-weight: 800; color: #64748b; text-transform: uppercase; margin-left: 5px; }
        .date-divider {
            padding: 8px 15px; background: #f1f5f9; border-radius: 10px;
            font-size: 11px; font-weight: 800; color: #64748b; margin: 20px 0 10px 0;
            border: 1px solid #e2e8f0;
        }

        .archive-item { border-bottom: 1px solid #eee; padding: 12px 0; cursor: pointer; position: relative; }
        .archive-header { display: flex; justify-content: space-between; font-weight: bold; font-size: 13px; padding-right: 30px; }

        /* Modal */
        #modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8); z-index: 10000;
            display: none; align-items: center; justify-content: center;
        }
        .modal-content {
            background: white; width: 90%; max-width: 450px; border-radius: 25px; padding: 20px;
            max-height: 85vh; overflow-y: auto;
        }

        .maintenance-card {
            background: #f8fafc; border: 1px solid #e2e8f0; padding: 15px; border-radius: 15px; margin-top: 20px;
        }
        canvas { display: none; }
    </style>
</head>
<body>

    <canvas id="canvas"></canvas>
    <input type="file" id="camInput" accept="image/*" capture="camera" style="display:none">

    <div id="auth-screen">
        <div class="login-card">
            <img src="https://i.ibb.co/xKY76DgR/Gemini-Generated-Image-1pvtp31pvtp31pvt-1.png" alt="Logo" class="auth-logo">
            <h2 style="margin:0; color:var(--gabon-vert)">CT241 OPS</h2>
            <p style="font-size:11px; margin-bottom:20px">GESTION LOGISTIQUE GABON</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="modal-overlay">
        <div class="modal-content">
            <h3 id="modal-title" style="margin-top:0">Détails de la Mission</h3>
            <div id="modal-body"></div>
            <button class="btn-action" onclick="fermerModal()" style="background:#f1f5f9; margin-top:15px">FERMER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-bleu)">CT241 OPS</h3>
                <span id="user-role" style="font-size:10px; background:var(--gabon-jaune); padding:2px 8px; border-radius:10px; font-weight:bold">...</span>
            </div>
            <button id="btnOut" style="border:1px solid var(--danger); color:var(--danger); background:none; border-radius:8px; padding:5px 10px; font-size:10px">SORTIR</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">FLUX</button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">MON BILAN</button>
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">DÉPLOYER</button>
            <button onclick="ouvrir('compta')" id="nav-compta" style="display:none">COMPTA</button>
            <button onclick="ouvrir('archives')" id="nav-archives" style="display:none">ARCHIVES</button>
        </nav>

        <div id="sec-missions" class="section active-sec">
            <input type="text" id="searchMissions" placeholder="Rechercher (Client ou livreur)..." onkeyup="renderUI()" style="margin-bottom:15px">
            <div id="list-active"></div>
        </div>

        <div id="sec-bilan" class="section">
            <div class="stats-banner">
                <small>Mon Historique</small>
                <div class="stats-grid">
                    <div><small>Bonus Cumulés</small><br><b id="Cpt-com">0 F</b></div>
                    <div><small>Missions Finies</small><br><b id="stat-count" style="color:white">0</b></div>
                </div>
            </div>
            <div id="list-bilan-today"></div>
        </div>

        <div id="sec-creer" class="section">
            <h4 style="margin:0 0 15px 0; color:var(--gabon-vert)">NOUVELLE MISSION</h4>
            <input type="text" id="mNom" placeholder="Nom du Client">
            <input type="tel" id="mTel" placeholder="Numéro de téléphone">
            <input type="text" id="mQuartier" placeholder="Quartier / Précisions">
            
            <div style="background: #f1f5f9; padding: 10px; border-radius: 15px; margin: 10px 0;">
                <span class="label-mini">Zone de livraison</span>
                <select id="mZoneSelect" onchange="updateFrais()">
                    <option value="2000">Libreville (2000 F)</option>
                    <option value="2500">Akanda / Owendo (2500 F)</option>
                    <option value="3000">Zones éloignées (3000 F)</option>
                </select>
            </div>

            <input type="number" id="mRetrait" placeholder="Valeur marchandise (F)">
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                <div><span class="label-mini">Frais</span><input type="number" id="mLiv" value="2000" readonly></div>
                <div><span class="label-mini">Bonus</span><input type="number" id="mCom" value="500"></div>
            </div>
            <button onclick="creerMission()" class="btn-action btn-validate">LANCER LE DÉPLOIEMENT</button>
        </div>

        <div id="sec-compta" class="section">
            <div class="stats-banner" style="background:var(--gabon-vert)">
                <small>Tableau Admin</small>
                <div class="stats-grid">
                    <div><small>C.A Livraison</small><br><b id="stat-total" style="color:white">0 F</b></div>
                    <div><small>Volume Colis</small><br><b id="cpt-vol" style="color:var(--gabon-jaune)">0 F</b></div>
                </div>
            </div>
            <div id="list-compta-daily"></div>

            <div class="maintenance-card" id="admin-maintenance" style="display:none">
                <h5 style="margin:0">OUTILS DE GESTION</h5>
                <div style="display:flex; gap:10px; margin-top:10px">
                    <button onclick="exporterDonnees()" class="btn-action" style="background:var(--gabon-bleu); color:white; font-size:10px">EXPORTER CSV</button>
                    <button onclick="nettoyerBase()" class="btn-action" style="background:var(--danger); color:white; font-size:10px">PURGE ANCIENS</button>
                </div>
            </div>
        </div>

        <div id="sec-archives" class="section">
            <input type="text" id="archiveSearchInput" placeholder="Chercher dans les archives..." onkeyup="renderUI()">
            <div id="list-archives-global"></div>
        </div>
    </div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import { getDatabase, ref, push, onValue, update, remove } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-database.js";

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
    let lastPhotoData = "";
    let currentKey = "";

    window.ouvrir = (sec) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById(`sec-${sec}`).classList.add('active-sec');
        document.getElementById(`nav-${sec}`).classList.add('active');
    };

    window.updateFrais = () => {
        document.getElementById('mLiv').value = document.getElementById('mZoneSelect').value;
    };

    document.getElementById('btnConnect').onclick = async () => {
        const e = document.getElementById('login-email').value;
        const p = document.getElementById('login-pass').value;
        try { await signInWithEmailAndPassword(auth, e, p); } catch(err) { alert("Accès refusé"); }
    };

    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (user) => {
        if(user) {
            const email = user.email.toLowerCase();
            userRole = (email.includes('admin') || email.includes('finance')) ? "admin" : "livreur";
            document.getElementById('user-role').innerText = userRole.toUpperCase();
            
            const isAdmin = (userRole === 'admin');
            ['nav-creer', 'nav-compta', 'nav-archives'].forEach(id => document.getElementById(id).style.display = isAdmin ? 'block' : 'none');
            if(isAdmin) document.getElementById('admin-maintenance').style.display = 'block';

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

    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const retrait = parseInt(document.getElementById('mRetrait').value) || 0;
        if(!nom || retrait <= 0) return alert("Nom et Montant requis");

        push(ref(db, 'missions'), {
            id: Math.floor(1000 + Math.random() * 9000),
            nom, tel: document.getElementById('mTel').value, 
            lieu: document.getElementById('mQuartier').value,
            retrait, frais: parseInt(document.getElementById('mLiv').value), 
            bonus: parseInt(document.getElementById('mCom').value),
            livreur: "Libre", etape: 0,
            timestamp: Date.now(),
            dateStr: new Date().toLocaleDateString('fr-FR')
        });
        alert("Déployée !");
        ['mNom', 'mRetrait', 'mTel', 'mQuartier'].forEach(id => document.getElementById(id).value = "");
    };

    window.accepter = (key) => {
        update(ref(db, `missions/${key}`), { livreur: auth.currentUser.email.split('@')[0].toUpperCase(), etape: 1 });
    };

    window.triggerCam = (key) => { currentKey = key; document.getElementById('camInput').click(); };
    document.getElementById('camInput').onchange = (e) => {
        const file = e.target.files[0];
        const reader = new FileReader();
        reader.onload = (re) => {
            const img = new Image();
            img.onload = () => {
                const canvas = document.getElementById('canvas');
                const ctx = canvas.getContext('2d');
                canvas.width = 600; canvas.height = (img.height/img.width)*600;
                ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                lastPhotoData = canvas.toDataURL('image/jpeg', 0.6);
                alert("Photo capturée !");
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.terminer = (key) => {
        const code = document.getElementById(`code-${key}`).value;
        if(!code || !lastPhotoData) return alert("Code + Photo obligatoires");
        update(ref(db, `missions/${key}`), { codeSMS: code, photo: lastPhotoData, etape: 2, clotureTs: Date.now(), dateStr: new Date().toLocaleDateString('fr-FR') });
        lastPhotoData = "";
    };

    // --- SUPPRESSION ---
    window.supprimerMission = (key) => {
        if(confirm("❗ ACTION IRREVERSIBLE : Supprimer cette mission de la base de données ?")) {
            remove(ref(db, `missions/${key}`)).then(() => {
                fermerModal();
                alert("Mission supprimée avec succès.");
            });
        }
    };

    window.voirDetails = (key) => {
        const m = allMissions.find(x => x.key === key);
        if(!m) return;
        const body = document.getElementById('modal-body');
        body.innerHTML = `
            <div style="font-size:13px; color:#444">
                <p><b>Statut :</b> ${m.etape === 2 ? 'LIVRÉ' : (m.etape === 1 ? 'EN COURS' : 'DISPONIBLE')}</p>
                <p><b>Client :</b> ${m.nom} (${m.tel || 'N/A'})</p>
                <p><b>Quartier :</b> ${m.lieu || 'N/A'}</p>
                <p><b>Montant :</b> ${m.retrait} F</p>
                <p><b>Livreur :</b> ${m.livreur}</p>
                <p><b>Date :</b> ${m.dateStr || 'Ancienne'}</p>
                ${m.photo ? `<img src="${m.photo}" style="width:100%; border-radius:15px; margin-top:10px">` : ''}
                
                ${userRole === 'admin' ? `
                    <div style="margin-top:20px; padding-top:15px; border-top:1px dashed #ccc">
                        <button onclick="supprimerMission('${m.key}')" style="background:var(--danger); color:white; border:none; padding:12px; border-radius:12px; width:100%; font-weight:bold">
                            🗑️ SUPPRIMER LA MISSION
                        </button>
                    </div>
                ` : ''}
            </div>
        `;
        document.getElementById('modal-overlay').style.display = 'flex';
    };

    window.fermerModal = () => document.getElementById('modal-overlay').style.display = 'none';

    window.renderUI = () => {
        const search = document.getElementById('searchMissions').value.toLowerCase();
        const archSearch = document.getElementById('archiveSearchInput').value.toLowerCase();
        const myName = auth.currentUser?.email.split('@')[0].toUpperCase();
        
        const listAct = document.getElementById('list-active');
        const listBilan = document.getElementById('list-bilan-today');
        const listCompta = document.getElementById('list-compta-daily');
        const listArch = document.getElementById('list-archives-global');
        
        [listAct, listBilan, listCompta, listArch].forEach(l => l.innerHTML = "");

        let bonusTotal = 0, countTotal = 0, caTotal = 0, volTotal = 0;
        const archMap = {};

        const sorted = [...allMissions].sort((a,b) => (b.clotureTs || b.timestamp || 0) - (a.clotureTs || a.timestamp || 0));

        sorted.forEach(m => {
            const isMatchAct = (m.nom + m.livreur).toLowerCase().includes(search);
            const isMatchArch = (m.nom + m.livreur).toLowerCase().includes(archSearch);

            // Flux Actif
            if(m.etape < 2 && isMatchAct) {
                let action = m.etape === 0 ? `<button onclick="accepter('${m.key}')" class="btn-action btn-validate">ACCEPTER</button>` :
                             (m.livreur === myName ? `<div style="background:#f8fafc; padding:10px; border-radius:12px; margin-top:10px"><button onclick="triggerCam('${m.key}')" class="btn-action btn-photo">📸 PHOTO</button><input type="text" id="code-${m.key}" placeholder="Code SMS" style="text-align:center"><button onclick="terminer('${m.key}')" class="btn-action btn-validate">VALIDER</button></div>` : `<div style="text-align:center; color:var(--gabon-bleu); font-size:10px; margin-top:10px">Par ${m.livreur}</div>`);

                listAct.innerHTML += `<div class="card etape-${m.etape}">
                    ${userRole === 'admin' ? `<button class="btn-delete-mini" onclick="supprimerMission('${m.key}')">Suppr.</button>` : ''}
                    <div style="display:flex; justify-content:space-between"><b>${m.nom}</b><small>#${m.id}</small></div>
                    <p style="font-size:11px; margin:5px 0">📍 ${m.lieu}</p>
                    <div style="display:flex; justify-content:space-between"><b>${m.retrait} F</b><span onclick="voirDetails('${m.key}')" style="color:var(--gabon-bleu); font-size:10px; text-decoration:underline">Détails</span></div>
                    ${action}
                </div>`;
            }

            // Bilan / Compta / Archives
            if(m.etape === 2) {
                const dateKey = m.dateStr || "Anciens";
                if(m.livreur === myName) { bonusTotal += (m.bonus || 0); countTotal++; listBilan.innerHTML += `<div class="archive-item" onclick="voirDetails('${m.key}')"><div class="archive-header"><span>${m.nom} <small>(${dateKey})</small></span><span style="color:var(--gabon-vert)">+${m.bonus} F</span></div></div>`; }
                if(userRole === 'admin') { caTotal += (m.frais || 0); volTotal += (m.retrait || 0); listCompta.innerHTML += `<div class="archive-item" onclick="voirDetails('${m.key}')"><div class="archive-header"><span>${m.nom} <small>(${m.livreur})</small></span><span>${m.frais} F</span></div></div>`; }
                if(userRole === 'admin' && isMatchArch) { if(!archMap[dateKey]) archMap[dateKey] = []; archMap[dateKey].push(m); }
            }
        });

        if(document.getElementById('Cpt-com')) document.getElementById('Cpt-com').innerText = bonusTotal.toLocaleString() + " F";
        if(document.getElementById('stat-count')) document.getElementById('stat-count').innerText = countTotal;
        if(document.getElementById('stat-total')) document.getElementById('stat-total').innerText = caTotal.toLocaleString() + " F";
        if(document.getElementById('cpt-vol')) document.getElementById('cpt-vol').innerText = volTotal.toLocaleString() + " F";

        Object.keys(archMap).forEach(d => {
            let html = `<div class="date-divider">${d}</div>`;
            archMap[d].forEach(m => html += `<div class="archive-item" onclick="voirDetails('${m.key}')"><div class="archive-header"><span><b>${m.nom}</b> <small>(${m.livreur})</small></span><span>${m.retrait} F</span></div></div>`);
            listArch.innerHTML += html;
        });
    };
</script>
</body>
</html>
