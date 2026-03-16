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
        .auth-logo { width: 100%; max-width: 180px; margin-bottom: 20px; }

        input, select { 
            width: 100%; padding: 12px; margin: 8px 0; border: 2px solid #edf2f7; 
            border-radius: 14px; box-sizing: border-box; font-size: 15px; transition: 0.3s;
            background: #f8fafc;
        }
        
        .btn-login { 
            width: 100%; padding: 16px; background: var(--gabon-vert); color: white; 
            border: none; border-radius: 14px; font-weight: 800; cursor: pointer; margin-top: 10px;
        }

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
        
        .stats-banner {
            background: var(--dark); color: white; padding: 20px; border-radius: 20px; margin-bottom: 20px;
            position: relative;
        }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 10px; }
        .stats-banner b { font-size: 18px; color: var(--gabon-jaune); }

        .label-mini { font-size: 10px; font-weight: 800; color: #64748b; text-transform: uppercase; margin-left: 5px; }
        .zone-highlight { background: #f1f5f9; padding: 10px; border-radius: 15px; margin: 10px 0; }
        .finance-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }

        .date-divider {
            padding: 8px 15px; background: #f8fafc; border-radius: 10px;
            font-size: 11px; font-weight: 800; color: #64748b; margin: 20px 0 10px 0;
            border: 1px solid #e2e8f0;
        }

        .archive-item { border-bottom: 1px solid #eee; padding: 12px 0; }
        .archive-header { display: flex; justify-content: space-between; font-weight: bold; font-size: 13px; }

        #modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8); z-index: 10000;
            display: none; align-items: center; justify-content: center;
        }
        .modal-content {
            background: white; width: 90%; max-width: 450px; border-radius: 25px; padding: 20px;
            max-height: 80vh; overflow-y: auto;
        }

        .btn-refresh-bilan, .btn-export {
            background: rgba(255,255,255,0.2); color: white; border: none; padding: 5px 10px;
            border-radius: 8px; font-size: 10px; cursor: pointer; margin-bottom: 10px;
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
            <h3 id="modal-title">Détails Mission</h3>
            <div id="modal-body"></div>
            <button class="btn-action" onclick="fermerModal()" style="background:#f1f5f9">FERMER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-bleu)">CT241 OPS</h3>
                <span id="user-role" style="font-size:10px; background:var(--gabon-jaune); padding:2px 8px; border-radius:10px; font-weight:bold">...</span>
            </div>
            <button id="btnOut" style="border:1px solid var(--danger); color:var(--danger); background:none; border-radius:8px; padding:5px 10px; font-size:10px">DECONNEXION</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">FLUX</button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">MON BILAN</button>
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">DÉPLOYER</button>
            <button onclick="ouvrir('compta')" id="nav-compta" style="display:none">COMPTA</button>
            <button onclick="ouvrir('archives')" id="nav-archives" style="display:none">ARCHIVES</button>
        </nav>

        <div id="sec-missions" class="section active-sec">
            <input type="text" id="searchMissions" placeholder="Rechercher une mission..." onkeyup="renderUI()" style="margin-bottom:15px">
            <div id="list-active"></div>
        </div>

        <!-- BILAN (LIVREUR) -->
        <div id="sec-bilan" class="section">
            <div class="stats-banner">
                <button class="btn-refresh-bilan" onclick="renderUI()">🔄 ACTUALISER</button>
                <br><small>Session Active (Aujourd'hui)</small>
                <div class="stats-grid">
                    <div><small>Bonus du Jour</small><br><b id="Cpt-com">0 F</b></div>
                    <div><small>Courses Faites</small><br><b id="stat-count" style="color:white">0</b></div>
                </div>
            </div>
            <h5 class="label-mini" style="margin-top:20px">HISTORIQUE RÉCENT</h5>
            <div id="list-bilan-today"></div>
        </div>

        <!-- CREER MISSION -->
        <div id="sec-creer" class="section">
            <h4 style="margin:0 0 15px 0; color:var(--gabon-vert)">DÉPLOYER UNE MISSION</h4>
            <input type="text" id="mNom" placeholder="Nom du bénéficiaire">
            <input type="tel" id="mTel" placeholder="Téléphone (ex: 077...)">
            <input type="text" id="mItineraire" placeholder="Lien Itinéraire ou Note">
            
            <div class="zone-highlight">
                <span class="label-mini">Zone & Localisation</span>
                <input type="text" id="mQuartier" placeholder="Quartier précis...">
                <select id="mZoneSelect" onchange="updateFrais()">
                    <option value="2000">Libreville Centre (2000 F)</option>
                    <option value="2500">Owendo / Akanda (2500 F)</option>
                    <option value="2000">PK / Ntoum / Angondjé (2000 F)</option>
                </select>
            </div>

            <span class="label-mini">Détails financiers</span>
            <input type="number" id="mRetrait" placeholder="Prix du colis (FCFA)">
            <div class="finance-row">
                <div>
                    <span class="label-mini">Frais de livraison(CFA)</span>
                    <input type="number" id="mLiv" value="2000" readonly style="background:#f1f5f9">
                </div>
                <div>
                    <span class="label-mini">Bonus Livreur (CFA)</span>
                    <input type="number" id="mCom" value="500">
                </div>
            </div>
            <button id="btnLancer" onclick="creerMission()" class="btn-action btn-validate">LANCER LA MISSION</button>
        </div>

        <!-- COMPTA ADMIN -->
        <div id="sec-compta" class="section">
            <div class="stats-banner" style="background:var(--gabon-vert)">
                <small>Tableau de bord Admin</small>
                <div class="stats-grid">
                    <div><small>C.A du jour</small><br><b id="stat-total" style="color:white">0 F</b></div>
                    <div><small>Valeur totale livrée</small><br><b id="cpt-vol" style="color:var(--gabon-jaune)">0 F</b></div>
                </div>
                <button class="btn-export" onclick="exportCSV()" style="margin-top:10px; width:100%">📥 EXPORTER LE BILAN (CSV)</button>
            </div>
            <div id="list-compta-daily"></div>
        </div>

        <!-- ARCHIVES -->
        <div id="sec-archives" class="section">
            <h4 style="margin:0 0 15px 0; color:var(--gabon-bleu)">ARCHIVES GÉNÉRALES</h4>
            <input type="text" id="archiveSearchInput" placeholder="Rechercher par nom ou livreur..." onkeyup="renderUI()">
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
    let lastPhotoData = "";
    let currentKey = "";

    window.ouvrir = (sec) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        const target = document.getElementById(`sec-${sec}`);
        if(target) target.classList.add('active-sec');
        const btn = document.getElementById(`nav-${sec}`);
        if(btn) btn.classList.add('active');
    };

    window.updateFrais = () => {
        const select = document.getElementById('mZoneSelect');
        document.getElementById('mLiv').value = select.value;
    };

    document.getElementById('btnConnect').onclick = async () => {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        if(!email || !pass) return;
        try { await signInWithEmailAndPassword(auth, email, pass); } 
        catch(e) { alert("Accès refusé"); }
    };

    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (user) => {
        if(user) {
            const email = user.email.toLowerCase();
            userRole = email.includes('admin') ? "admin" : (email.includes('finance') ? "finance" : "livreur");
            document.getElementById('user-role').innerText = userRole.toUpperCase();
            
            const isAdmin = (userRole === 'admin' || userRole === 'finance');
            document.getElementById('nav-creer').style.display = isAdmin ? 'block' : 'none';
            document.getElementById('nav-compta').style.display = isAdmin ? 'block' : 'none';
            document.getElementById('nav-archives').style.display = isAdmin ? 'block' : 'none';
            
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
        const tel = document.getElementById('mTel').value;
        const itineraire = document.getElementById('mItineraire').value;
        const lieu = document.getElementById('mQuartier').value;
        const retrait = parseInt(document.getElementById('mRetrait').value) || 0;
        const frais = parseInt(document.getElementById('mLiv').value) || 0;
        const bonus = parseInt(document.getElementById('mCom').value) || 0;

        if(!nom || retrait <= 0) return alert("Nom et Prix Colis requis");

        push(ref(db, 'missions'), {
            id: Math.floor(1000 + Math.random() * 9000),
            nom, tel, lieu, itineraire, retrait, frais, bonus,
            livreur: "Libre", etape: 0,
            timestamp: Date.now(),
            dateStr: new Date().toLocaleDateString('fr-FR')
        });
        alert("Mission lancée !");
        ouvrir('missions');
        document.getElementById('mNom').value = "";
        document.getElementById('mRetrait').value = "";
    };

    window.accepter = (key) => {
        const myName = auth.currentUser.email.split('@')[0].toUpperCase();
        update(ref(db, `missions/${key}`), { livreur: myName, etape: 1 });
    };

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
                alert("Photo Colis ok !");
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.terminer = (key) => {
        const code = document.getElementById(`code-${key}`).value;
        if(!code || !lastPhotoData) return alert("Photo + Code requis");
        update(ref(db, `missions/${key}`), { codeSMS: code, photo: lastPhotoData, etape: 2, clotureTs: Date.now() });
        lastPhotoData = "";
    };

    window.fermerModal = () => document.getElementById('modal-overlay').style.display = 'none';

    window.voirDetails = (key) => {
        const m = allMissions.find(x => x.key === key);
        if(!m) return;
        const body = document.getElementById('modal-body');
        body.innerHTML = `
            <p><b>Client :</b> ${m.nom}</p>
            <p><b>Tel :</b> ${m.tel || 'N/A'}</p>
            <p><b>Itinéraire :</b> ${m.itineraire || 'N/A'}</p>
            <p><b>Quartier :</b> ${m.lieu}</p>
            <p><b>Prix Colis :</b> ${m.retrait} F</p>
            <p><b>Livreur :</b> ${m.livreur}</p>
            <p><b>Code :</b> ${m.codeSMS || 'En attente'}</p>
            ${m.photo ? `<img src="${m.photo}" style="width:100%; border-radius:15px; margin-top:10px">` : ''}
            ${userRole === 'admin' ? `<button onclick="suppr('${m.key}')" style="color:red; border:1px solid red; background:none; padding:10px; border-radius:10px; width:100%; margin-top:15px">SUPPRIMER</button>` : ''}
        `;
        document.getElementById('modal-overlay').style.display = 'flex';
    };

    window.suppr = (key) => { if(confirm("Supprimer ?")) { remove(ref(db, `missions/${key}`)); fermerModal(); }};

    window.exportCSV = () => {
        let csv = "\uFEFFID;Date;Client;Montant;Livreur;Code\n";
        allMissions.forEach(m => csv += `${m.id};${m.dateStr};${m.nom};${m.retrait};${m.livreur};${m.codeSMS||''}\n`);
        const blob = new Blob([csv], {type: 'text/csv;charset=utf-8;'});
        const a = document.createElement('a');
        a.href = URL.createObjectURL(blob);
        a.download = `Archives_Logistique_${new Date().toLocaleDateString()}.csv`;
        a.click();
    };

    window.renderUI = () => {
        const searchMissions = document.getElementById('searchMissions').value.toLowerCase();
        const searchArchive = document.getElementById('archiveSearchInput').value.toLowerCase();
        
        const listAct = document.getElementById('list-active');
        const listBilLivreur = document.getElementById('list-bilan-today');
        const listCompta = document.getElementById('list-compta-daily');
        const listArch = document.getElementById('list-archives-global');
        
        listAct.innerHTML = ""; listBilLivreur.innerHTML = ""; listCompta.innerHTML = ""; listArch.innerHTML = "";
        
        const myName = auth.currentUser?.email.split('@')[0].toUpperCase();
        const todayStr = new Date().toLocaleDateString('fr-FR');
        
        let bonusDay = 0, countDay = 0, caDay = 0, volDay = 0;
        const archMap = {};

        // Tri par date décroissante
        const sorted = [...allMissions].sort((a,b) => (b.clotureTs || b.timestamp) - (a.clotureTs || a.timestamp));

        sorted.forEach(m => {
            const isMatchMission = m.nom.toLowerCase().includes(searchMissions) || m.livreur.toLowerCase().includes(searchMissions);
            const isMatchArchive = m.nom.toLowerCase().includes(searchArchive) || m.livreur.toLowerCase().includes(searchArchive);

            // --- 1. MISSIONS ACTIVES ---
            if(m.etape < 2 && isMatchMission) {
                let actionHtml = "";
                if(m.etape === 0) {
                    actionHtml = `<button class="btn-action btn-validate" onclick="accepter('${m.key}')">ACCEPTER</button>`;
                } else if(m.livreur === myName) {
                    actionHtml = `
                        <div style="background:#f8fafc; padding:10px; border-radius:15px; margin-top:10px">
                            <button class="btn-action btn-photo" onclick="triggerCam('${m.key}')">📸 PHOTO COLIS</button>
                            <input type="text" id="code-${m.key}" placeholder="Code SMS" style="text-align:center">
                            <button class="btn-action btn-validate" onclick="terminer('${m.key}')">VALIDER LIVRAISON</button>
                        </div>`;
                } else {
                    actionHtml = `<p style="text-align:center; color:var(--gabon-bleu); font-size:11px; font-weight:bold">En cours : ${m.livreur}</p>`;
                }

                listAct.innerHTML += `
                    <div class="card etape-${m.etape}">
                        <div style="display:flex; justify-content:space-between"><b>${m.nom}</b> <small>#${m.id}</small></div>
                        <p style="font-size:12px; margin:5px 0">📍 ${m.lieu}</p>
                        <div style="display:flex; justify-content:space-between; align-items:center">
                            <b style="color:var(--gabon-vert); font-size:16px">${m.retrait} F</b>
                            <span onclick="voirDetails('${m.key}')" style="font-size:10px; color:var(--gabon-bleu); cursor:pointer">Détails</span>
                        </div>
                        ${actionHtml}
                    </div>`;
            }

            // --- 2. MISSIONS TERMINÉES ---
            if(m.etape === 2) {
                // Pour le livreur (Bilan Personnel)
                if(m.livreur === myName) {
                    if(m.dateStr === todayStr) {
                        countDay++;
                        bonusDay += (m.bonus || 0);
                    }
                    // Historique récent du livreur (limité aux 10 derniers ou recherche)
                    if(m.nom.toLowerCase().includes(searchMissions)) {
                        listBilLivreur.innerHTML += `
                            <div class="archive-item" onclick="voirDetails('${m.key}')">
                                <div class="archive-header">
                                    <span>${m.nom} (${m.dateStr})</span>
                                    <span style="color:var(--gabon-vert)">+${m.bonus || 0} F</span>
                                </div>
                            </div>`;
                    }
                }

                // Pour l'Admin / Finance
                if(userRole === 'admin' || userRole === 'finance') {
                    if(m.dateStr === todayStr) {
                        caDay += (m.frais || 0);
                        volDay += (m.retrait || 0);
                        listCompta.innerHTML += `
                            <div class="archive-item" onclick="voirDetails('${m.key}')">
                                <div class="archive-header"><span>${m.nom} (${m.livreur})</span><span>${m.frais} F</span></div>
                            </div>`;
                    }
                    
                    if(isMatchArchive) {
                        if(!archMap[m.dateStr]) archMap[m.dateStr] = [];
                        archMap[m.dateStr].push(m);
                    }
                }
            }
        });

        // Mise à jour des compteurs visuels
        if(document.getElementById('Cpt-com')) document.getElementById('Cpt-com').innerText = bonusDay + " F";
        if(document.getElementById('stat-count')) document.getElementById('stat-count').innerText = countDay;
        if(document.getElementById('stat-total')) document.getElementById('stat-total').innerText = caDay.toLocaleString() + " F";
        if(document.getElementById('cpt-vol')) document.getElementById('cpt-vol').innerText = volDay.toLocaleString() + " F";

        // Construction HTML des Archives Globales (Groupées par Date)
        const dateKeys = Object.keys(archMap).sort((a,b) => {
             // Tri des clés de date string vers date obj
             const parse = (s) => new Date(s.split('/').reverse().join('-'));
             return parse(b) - parse(a);
        });

        dateKeys.forEach(date => {
            let sectionHtml = `<div class="date-divider">${date}</div>`;
            archMap[date].forEach(m => {
                sectionHtml += `
                    <div class="archive-item" onclick="voirDetails('${m.key}')" style="cursor:pointer">
                        <div class="archive-header">
                            <span><b>${m.nom}</b> • ${m.livreur}</span>
                            <span style="color:var(--gabon-bleu)">${m.retrait} F</span>
                        </div>
                        <small style="font-size:10px; color:#64748b">Frais: ${m.frais} F | Code: ${m.codeSMS || '?'}</small>
                    </div>`;
            });
            listArch.innerHTML += sectionHtml;
        });
    };
</script>
</body>
</html>
