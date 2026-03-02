<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - LOGISTIQUE GABON</title>
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
        
        /* AUTH SCREEN */
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
        input:focus { border-color: var(--gabon-bleu); outline: none; background: white; box-shadow: 0 0 0 4px var(--gabon-bleu-light); }
        
        .btn-login { 
            width: 100%; padding: 16px; background: var(--gabon-vert); color: white; 
            border: none; border-radius: 14px; font-weight: 800; cursor: pointer; 
            font-size: 16px; margin-top: 15px; box-shadow: 0 6px 15px rgba(0, 158, 96, 0.3);
        }

        /* MAIN APP */
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

        /* NAV */
        nav { 
            display: flex; overflow-x: auto; gap: 10px; margin-bottom: 25px; 
            padding: 8px; background: #f1f5f9; border-radius: 18px; scrollbar-width: none;
        }
        nav button { 
            flex: 0 0 auto; padding: 12px 20px; border: none; border-radius: 14px; 
            background: transparent; font-weight: 700; font-size: 12px; color: #64748b; 
            cursor: pointer; transition: 0.2s;
        }
        nav button.active { background: var(--white); color: var(--gabon-bleu); box-shadow: 0 4px 12px rgba(0,0,0,0.08); }

        /* SECTIONS */
        .section { display: none; animation: slideUp 0.4s ease-out; }
        .active-sec { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        /* CARDS */
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

        /* BUTTONS */
        .btn-action { 
            width: 100%; padding: 16px; border: none; border-radius: 14px; 
            font-weight: 800; cursor: pointer; margin-top: 15px; font-size: 14px; transition: 0.2s;
        }
        .btn-validate { background: var(--gabon-vert); color: white; }
        .btn-accept { background: var(--gabon-bleu); color: white; }
        .btn-photo { background: var(--dark); color: white; }

        /* COMPTA GRID */
        .compta-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 25px; }
        .stat-box { 
            background: var(--white); border: 1px solid #f1f5f9; padding: 20px; 
            border-radius: 22px; text-align: center; box-shadow: 0 4px 10px rgba(0,0,0,0.02);
        }
        .stat-box span { font-size: 10px; font-weight: 800; color: #94a3b8; text-transform: uppercase; display: block; margin-bottom: 5px; }
        .stat-box b { font-size: 26px; color: var(--dark); }

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
        .sms-badge { background: var(--danger); color: white; padding: 2px 8px; border-radius: 6px; font-size: 10px; font-weight: 900; }
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

        <!-- CRÉATION -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 10px;">
                <h4 style="margin:0 0 20px 0; font-weight:900; color:var(--gabon-vert)">CRÉER UNE MISSION</h4>
                <input type="text" id="mNom" placeholder="Nom du bénéficiaire">
                <input type="tel" id="mTel" placeholder="Numéro de téléphone">
                <input type="text" id="mLieu" placeholder="Quartier / Lieu de livraison">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)">
                
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px">
                    <div>
                        <label style="font-size:10px; font-weight:800; color:#94a3b8; margin-left:5px">COMMISSION (F)</label>
                        <input type="number" id="mCom" value="390">
                    </div>
                    <div>
                        <label style="font-size:10px; font-weight:800; color:#94a3b8; margin-left:5px">LIVRAISON (F)</label>
                        <input type="number" id="mLiv" value="1000">
                    </div>
                </div>
                
                <button onclick="creerMission()" class="btn-action btn-validate">DÉPLOYER LA MISSION</button>
            </div>
        </div>

        <!-- MISSIONS -->
        <div id="sec-missions" class="section active-sec">
            <div id="div-validation" style="display:none; margin-bottom:30px;">
                <h5 style="color:var(--danger); margin:0 0 15px 0; font-weight:900; font-size:12px; letter-spacing:1px">⚠️ EN ATTENTE DE VALIDATION</h5>
                <div id="list-validation"></div>
            </div>

            <h5 style="color:var(--gabon-bleu); margin:0 0 15px 0; font-weight:900; font-size:12px; letter-spacing:1px">🚀 MISSIONS EN COURS</h5>
            <div id="list-active"></div>
        </div>

        <!-- BILAN LIVREUR -->
        <div id="sec-bilan" class="section">
            <h4 style="margin:0 0 20px 0; font-weight:900">SUIVI DES GAINS</h4>
            <div id="list-bilan"></div>
            <div class="stats-footer">
                <div>
                    <span>REVENUS CUMULÉS :</span>
                    <b id="stat-total">0 F</b>
                </div>
            </div>
        </div>

        <!-- COMPTA ADMIN -->
        <div id="sec-compta" class="section">
            <h4 style="margin:0 0 20px 0; font-weight:900">CONTRÔLE FINANCIER</h4>
            
            <div class="compta-grid">
                <div class="stat-box">
                    <span>RECOUVREMENT</span>
                    <b id="compta-count-finish" style="color:var(--gabon-vert)">0</b>
                </div>
                <div class="stat-box">
                    <span>OPS ACTIVES</span>
                    <b id="compta-count-encours" style="color:var(--gabon-bleu)">0</b>
                </div>
            </div>

            <div style="background:white; border-radius:22px; border:1px solid #f1f5f9; overflow:hidden">
                <table class="table-compta">
                    <thead>
                        <tr>
                            <th>CLIENT</th>
                            <th>RETRAIT</th>
                            <th>LIV.</th>
                            <th>COM.</th>
                        </tr>
                    </thead>
                    <tbody id="list-compta"></tbody>
                </table>
            </div>

            <div class="stats-footer" style="background:var(--gabon-vert)">
                <div>
                    <span>TOTAL COMMISSIONS :</span>
                    <b id="compta-total-com" style="color:white">0 F</b>
                </div>
                <div>
                    <span>VOLUME DE RETRAIT :</span>
                    <b id="compta-total-retrait" style="color:var(--gabon-jaune)">0 F</b>
                </div>
            </div>
        </div>

        <!-- ARCHIVES -->
        <div id="sec-archives" class="section">
            <div class="search-area">
                <input type="text" id="archSearch" placeholder="Rechercher par Nom, ID, SMS, Date..." oninput="renderUI()" style="margin:0">
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
        try { await signInWithEmailAndPassword(auth, email, pass); } catch(e) { alert("Accès non autorisé"); }
    };

    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        if(u) {
            const email = u.email.toLowerCase();
            if(email.includes('admin')) {
                userRole = "admin";
            } else if(email.includes('finance')) {
                userRole = "finance";
            } else {
                userRole = "livreur";
            }

            document.getElementById('user-role').innerText = userRole;
            document.getElementById('user-display').innerText = u.email.split('@')[0].toUpperCase();
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            
            const canManage = (userRole === 'admin' || userRole === 'finance');
            document.getElementById('nav-creer').style.display = canManage ? 'block' : 'none';
            document.getElementById('nav-archives').style.display = canManage ? 'block' : 'none';
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
        let totalRetraitsAdmin = 0;
        let countEnCours = 0;
        let countTerminees = 0;

        const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "LIBRE";

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            
            if(m.etape === 3) countTerminees++;
            else countEnCours++;

            // Archives (Admin et Finance) - Ajout du Code SMS dans la recherche
            if(userRole === 'admin' || userRole === 'finance') {
                const codeSms = m.codeSMS ? m.codeSMS.toString().toLowerCase() : "";
                const searchStr = `${m.id} ${m.nom} ${m.tel} ${m.dateHeure} ${m.livreur} ${codeSms}`.toLowerCase();
                
                if(searchStr.includes(archSearch)) {
                    listArc.innerHTML += `<div class="card" style="border-left:5px solid var(--dark)">
                        <div class="card-title">
                            <span>${m.nom}</span> 
                            <span class="badge-id">${m.id}</span>
                        </div>
                        <div class="card-info">
                            💰 ${m.retrait.toLocaleString()} F | 🛵 ${m.livreur}<br>
                            📅 ${m.dateHeure} ${m.codeSMS ? `| <span class="sms-badge">SMS: ${m.codeSMS}</span>` : ""}
                        </div>
                    </div>`;
                }
            }

            // Compta Admin
            if(m.etape === 3 && userRole === 'admin') {
                totalComAdmin += m.com;
                totalRetraitsAdmin += m.retrait;
                listCpt.innerHTML += `<tr>
                    <td><b>${m.nom}</b></td>
                    <td>${m.retrait.toLocaleString()} F</td>
                    <td>${m.fraisLivraison} F</td>
                    <td style="color:var(--gabon-vert); font-weight:800">${m.com} F</td>
                </tr>`;
            }

            // Bilan Livreur
            if(m.etape === 3) {
                if(m.livreur === myName) {
                    totalGainsLivreur += m.fraisLivraison;
                    listBil.innerHTML += `<div style="padding:15px; border-bottom:1px solid #f1f5f9; display:flex; justify-content:space-between">
                            <div><b>${m.nom}</b><br><small style="color:#94a3b8">${m.dateHeure}</small></div>
                            <b style="color:var(--gabon-vert)">+${m.fraisLivraison.toLocaleString()} F</b>
                        </div>`;
                }
                return;
            }

            // Flux Actif
            const cardHtml = `<div class="card etape-${m.etape}">
                    <div class="card-title"><span>${m.nom}</span> <span class="badge-id">${m.id}</span></div>
                    <div class="card-info">
                        📞 <b>${m.tel}</b> | 📍 <b>${m.lieu}</b><br>
                        💰 Retrait : <b style="color:var(--dark)">${m.retrait.toLocaleString()} F</b><br>
                        🛵 Livreur : <span style="background:var(--dark); color:white; padding:2px 8px; border-radius:6px; font-size:10px; font-weight:800">${m.livreur}</span><br>
                        <small style="color:#94a3b8">🕒 ${m.dateHeure}</small>
                    </div>`;

            if(m.etape === 0 && userRole === 'admin') {
                listVal.innerHTML += cardHtml + `<button class="btn-action btn-validate" onclick="valider('${m.key}')">VALIDER & PUBLIER</button></div>`;
            } else if(m.etape > 0) {
                let ctrls = "";
                if(m.etape === 1 && m.livreur === "Libre" && userRole === 'livreur') {
                    ctrls = `<button class="btn-action btn-accept" onclick="accepter('${m.key}')">ACCEPTER LA COURSE</button>`;
                } else if(m.etape === 1 && m.livreur === myName) {
                    ctrls = `<button class="btn-action btn-photo" onclick="triggerCam('${m.key}')">📸 PHOTO DU SMS</button>
                             <img id="pre-${m.key}" class="preview-img">
                             <input type="text" id="code-${m.key}" placeholder="Code SMS reçu" style="margin-top:15px; border:2px solid var(--gabon-bleu)">
                             <button class="btn-action btn-validate" onclick="terminer('${m.key}')">ENVOYER À LA FINANCE</button>`;
                } else if(m.etape === 2 && (userRole === 'admin' || userRole === 'finance')) {
                    ctrls = `<div style="background:white; padding:15px; border-radius:18px; margin-top:15px; text-align:center">
                                <img src="${m.photo}" style="width:100%; border-radius:12px">
                                <p style="color:var(--danger); font-size:28px; font-weight:900; margin:15px 0">${m.codeSMS}</p>
                                <button class="btn-action btn-validate" onclick="cloturer('${m.key}')">ENCAISSEMENT TERMINÉ ✅</button>
                             </div>`;
                }
                listAct.innerHTML += cardHtml + ctrls + `</div>`;
            }
        });

        document.getElementById('stat-total').innerText = totalGainsLivreur.toLocaleString() + " F";
        if(userRole === 'admin') {
            document.getElementById('compta-total-com').innerText = totalComAdmin.toLocaleString() + " F";
            document.getElementById('compta-total-retrait').innerText = totalRetraitsAdmin.toLocaleString() + " F";
            document.getElementById('compta-count-encours').innerText = countEnCours;
            document.getElementById('compta-count-finish').innerText = countTerminees;
        }

        const openM = allMissions.filter(x => x.etape === 1 && x.livreur === "Libre").length;
        document.getElementById('count-missions').innerHTML = openM > 0 ? `<b style="background:var(--danger); color:white; padding:2px 8px; border-radius:10px; font-size:10px">${openM}</b>` : "";
    }

    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const tel = document.getElementById('mTel').value;
        const mnt = parseInt(document.getElementById('mRetrait').value);
        const lieu = document.getElementById('mLieu').value;
        const com = parseInt(document.getElementById('mCom').value);
        const liv = parseInt(document.getElementById('mLiv').value);

        if(!nom || !mnt || !tel) return alert("Champs obligatoires manquants !");
        
        const now = new Date();
        const dateStr = now.toLocaleDateString('fr-FR') + " " + now.toLocaleTimeString('fr-FR', {hour:'2-digit', minute:'2-digit'});
        
        push(ref(db, 'missions'), {
            id: "CT" + Math.floor(Math.random()*9999), 
            nom, tel, lieu: lieu || "Lbv",
            retrait: mnt, com, fraisLivraison: liv,
            livreur: "Libre", etape: 0, dateHeure: dateStr, timestamp: Date.now()
        });
        
        document.getElementById('mNom').value=""; document.getElementById('mTel').value="";
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
                lastPhotoData = can.toDataURL('image/jpeg', 0.7);
                const pre = document.getElementById('pre-'+currentKey);
                if(pre) { pre.src = lastPhotoData; pre.style.display = 'block'; }
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.terminer = (k) => {
        const code = document.getElementById('code-'+k).value;
        if(!code || !lastPhotoData) return alert("Code et Photo requis !");
        update(ref(db, `missions/${k}`), { etape: 2, codeSMS: code, photo: lastPhotoData });
    };

    window.cloturer = (k) => confirm("Confirmer la fin d'opération ?") && update(ref(db, `missions/${k}`), { etape: 3 });

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('nav-'+id).classList.add('active');
    };
</script>
</body>
</html>
