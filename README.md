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
        body { font-family: 'Segoe UI', system-ui, sans-serif; background: var(--light); margin: 0; padding: 10px; overflow-x: hidden; }
        
        /* AUTH SCREEN */
        #auth-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #1e272e; display: flex; align-items: center; justify-content: center; z-index: 9999;
        }
        .login-card {
            background: white; padding: 25px; border-radius: 20px; width: 85%; max-width: 320px;
            text-align: center; border-top: 8px solid var(--gabon-jaune);
        }
        input { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; font-size: 16px; }
        .btn-login { width: 100%; padding: 14px; background: var(--gabon-vert); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }

        /* MAIN APP */
        #main-app { display: none; max-width: 600px; margin: auto; background: white; border-radius: 15px; padding: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); min-height: 90vh; }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        .role-badge { font-size: 10px; padding: 3px 10px; border-radius: 12px; background: var(--dark); color: white; font-weight: bold; }

        nav { display: flex; gap: 5px; margin-bottom: 15px; background: #eee; padding: 5px; border-radius: 10px; }
        nav button { flex: 1; padding: 12px 5px; border: none; border-radius: 7px; background: transparent; font-weight: bold; font-size: 11px; color: #666; cursor: pointer; }
        nav button.active { background: white; color: var(--gabon-vert); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        
        .section { display: none; }
        .active-sec { display: block; }

        /* MISSION CARDS */
        .card { border: 1px solid #eee; padding: 15px; border-radius: 12px; margin-bottom: 12px; position: relative; border-left: 6px solid #ccc; transition: 0.3s; }
        .card.available { border-left-color: var(--gabon-bleu); background: #f0f7ff; border-style: dashed; }
        .card.step-1 { border-left-color: var(--gabon-jaune); }
        .card.step-2 { border-left-color: var(--dark); background: #fffdf5; }
        
        .card-title { font-weight: bold; font-size: 14px; margin-bottom: 5px; color: var(--dark); }
        .card-info { font-size: 12px; color: #555; line-height: 1.4; }
        .badge-id { position: absolute; top: 10px; right: 10px; background: #f1f2f6; padding: 2px 6px; border-radius: 4px; font-size: 10px; font-weight: bold; }
        .status-pill { display: inline-block; padding: 2px 8px; border-radius: 10px; font-size: 9px; font-weight: bold; margin-bottom: 5px; text-transform: uppercase; }

        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; font-size: 13px; }
        .btn-accept { background: var(--gabon-bleu); color: white; animation: pulse 2s infinite; }
        .btn-photo { background: var(--dark); color: white; }
        .btn-confirm { background: var(--gabon-vert); color: white; }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.02); }
            100% { transform: scale(1); }
        }

        .preview-img { width: 100%; max-height: 200px; object-fit: contain; margin-top: 10px; display: none; border-radius: 10px; border: 1px solid #ddd; }

        /* STATS */
        .stats-footer { background: var(--dark); color: white; padding: 15px; border-radius: 12px; margin-top: 15px; }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 10px; border-top: 1px solid #333; padding-top: 10px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666; margin-bottom: 15px;">Portail Logistique Temps Réel</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">ACCÉDER AU SYSTÈME</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-vert)">CT241 OPS</h3>
                <span id="user-display" style="font-size:10px; color:#888"></span>
                <span id="user-role" class="role-badge">...</span>
            </div>
            <button id="btnOut" style="font-size:10px; color:var(--danger); background:none; border:none; font-weight:bold;">SORTIR</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">NOUVEAU</button>
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">MISSIONS</button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">BILAN</button>
        </nav>

        <!-- SECTION CREATION ADMIN -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 15px; border: 1px solid #eee; border-radius: 12px;">
                <h4 style="margin:0 0 10px 0">Créer une Mission</h4>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="Téléphone">
                <input type="text" id="mLieu" placeholder="Destination (Quartier)">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)" oninput="window.calculerAutomatisme(this.value)">
                
                <div id="admin-auto-box" style="display:none; background:#f9f9f9; padding:10px; border-radius:8px; margin:10px 0; font-size:12px; border:1px dashed var(--gabon-vert)">
                    <div style="display:flex; justify-content:space-between"><span>Commission :</span> <b id="auto-com">0 F</b></div>
                    <div style="display:flex; justify-content:space-between"><span>Livraison :</span> <b>1000 F</b></div>
                </div>

                <button id="btnLancerMission" class="btn-action btn-confirm">LANCER IMMÉDIATEMENT</button>
            </div>
        </div>

        <!-- SECTION MISSIONS (CORE) -->
        <div id="sec-missions" class="section active-sec">
            <div id="container-dispo" style="margin-bottom: 20px;">
                <!-- Les nouvelles missions non acceptées s'afficheront ici -->
            </div>
            <hr style="border:0; border-top:1px solid #eee; margin: 20px 0">
            <div id="container-en-cours">
                <!-- Les missions en cours de traitement s'afficheront ici -->
            </div>
        </div>

        <!-- SECTION BILAN -->
        <div id="sec-bilan" class="section">
            <input type="text" id="bilanSearch" placeholder="🔍 Rechercher client ou SMS..." style="margin-bottom:15px">
            <div id="container-bilan"></div>
            
            <div class="stats-footer">
                <div style="display:flex; justify-content:space-between; align-items:center;">
                    <span id="label-total">VOTRE GAIN</span>
                    <b id="stat-total" style="color:var(--gabon-jaune); font-size:18px">0 F</b>
                </div>
                <div id="admin-stats-grid" class="stats-grid" style="display:none">
                    <div><small>COMMISSIONS</small><br><span id="stat-com" style="color:var(--gabon-vert); font-weight:bold">0 F</span></div>
                    <div><small>LIVRAISONS</small><br><span id="stat-liv" style="color:var(--gabon-bleu); font-weight:bold">0 F</span></div>
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
    let currentPhotoKey = null;
    let currentPhotoData = "";

    window.calculerAutomatisme = (val) => {
        const mnt = parseInt(val) || 0;
        const box = document.getElementById('admin-auto-box');
        if (mnt > 0) {
            box.style.display = 'block';
            document.getElementById('auto-com').innerText = (mnt >= 15000 ? 390 : 190) + " F";
        } else { box.style.display = 'none'; }
    };

    document.getElementById('btnConnect').onclick = async () => {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        try { await signInWithEmailAndPassword(auth, email, pass); } 
        catch (e) { alert("Identifiants incorrects."); }
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
            document.getElementById('nav-creer').style.display = (userRole === 'admin') ? 'block' : 'none';
            document.getElementById('admin-stats-grid').style.display = (userRole === 'admin') ? 'grid' : 'none';
            
            // Écouteur de données TEMPS RÉEL
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
        const contDispo = document.getElementById('container-dispo');
        const contCours = document.getElementById('container-en-cours');
        const contBilan = document.getElementById('container-bilan');
        const search = document.getElementById('bilanSearch').value.toLowerCase();
        
        contDispo.innerHTML = "<h5 style='margin:0 0 10px 0; color:var(--gabon-bleu)'>🚨 NOUVELLES MISSIONS (DISPONIBLES)</h5>";
        contCours.innerHTML = "<h5 style='margin:0 0 10px 0; color:#666'>⏳ MISSIONS EN COURS</h5>";
        contBilan.innerHTML = "";
        
        let tRetrait = 0, tCom = 0, tLiv = 0;
        const myName = auth.currentUser.email.split('@')[0].toUpperCase();

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            if(m.etape < 3) {
                const isLibre = m.livreur === "Libre";
                const isMine = m.livreur === myName;
                const isAdmin = (userRole === 'admin' || userRole === 'finance');

                let cardHTML = `
                    <div class="card ${isLibre ? 'available' : 'step-'+m.etape}">
                        <span class="badge-id">${m.id}</span>
                        <div class="status-pill" style="background:${isLibre ? 'var(--gabon-bleu)' : 'var(--gabon-jaune)'}; color:white">
                            ${isLibre ? 'Disponible' : 'En cours'}
                        </div>
                        <div class="card-title">${m.nom}</div>
                        <div class="card-info">
                            📍 <b>${m.lieu || 'Non spécifié'}</b><br>
                            💰 Retrait : <b>${m.retrait.toLocaleString()} F</b><br>
                            🛵 Livreur : <b>${m.livreur}</b>
                        </div>
                `;

                // ACTIONS SELON LE ROLE ET L'ETAT
                if(isLibre) {
                    // Tout le monde voit les missions libres, mais seul le livreur peut l'accepter
                    if(userRole === 'livreur') {
                        cardHTML += `<button class="btn-action btn-accept" onclick="window.accepter('${m.key}')">ACCEPTER LA COURSE</button>`;
                    } else {
                        cardHTML += `<div style="font-size:10px; color:var(--gabon-bleu); margin-top:5px; font-weight:bold italic">En attente d'un livreur...</div>`;
                    }
                    cardHTML += `</div>`;
                    contDispo.innerHTML += cardHTML;
                } 
                else if(isMine || isAdmin) {
                    if(isMine && m.etape === 1) {
                        cardHTML += `
                            <button class="btn-action btn-photo" onclick="window.triggerCam('${m.key}')">📸 PHOTO SMS</button>
                            <img id="pre-${m.key}" class="preview-img">
                            <input type="text" id="sms-${m.key}" placeholder="Code SMS" style="margin-top:10px">
                            <button id="val-${m.key}" class="btn-action btn-confirm" onclick="window.livrer('${m.key}')" disabled>VALIDER LIVRAISON</button>
                        `;
                    } else if(isAdmin && m.etape === 2) {
                        cardHTML += `
                            <div style="background:#f1f2f6; padding:10px; border-radius:10px; margin-top:10px">
                                <img src="${m.photo}" style="width:100%; border-radius:8px">
                                <p style="font-weight:bold; color:red; text-align:center; margin:5px 0">CODE : ${m.codeSMS}</p>
                                <button class="btn-action btn-confirm" onclick="window.encaisser('${m.key}')">ENCAISSER & CLÔTURER</button>
                            </div>
                        `;
                    }
                    cardHTML += `</div>`;
                    contCours.innerHTML += cardHTML;
                }
            } else {
                // BILAN
                if(userRole === 'admin' || userRole === 'finance' || m.livreur === myName) {
                    if(m.nom.toLowerCase().includes(search) || (m.codeSMS && m.codeSMS.toLowerCase().includes(search))) {
                        tRetrait += m.retrait; tCom += m.com; tLiv += 1000;
                        contBilan.innerHTML += `
                            <div style="padding:10px; border-bottom:1px solid #eee">
                                <div style="display:flex; justify-content:space-between"><b>${m.nom}</b> <span>${m.retrait.toLocaleString()} F</span></div>
                                <div style="font-size:10px; color:#888">${m.date} | SMS: ${m.codeSMS} | 🛵 ${m.livreur}</div>
                            </div>`;
                    }
                }
            }
        });

        // Mise à jour des compteurs
        if(userRole === 'admin') {
            document.getElementById('stat-total').innerText = tRetrait.toLocaleString() + " F";
            document.getElementById('stat-com').innerText = tCom.toLocaleString() + " F";
            document.getElementById('stat-liv').innerText = tLiv.toLocaleString() + " F";
            document.getElementById('label-total').innerText = "CUMUL RETRAITS";
        } else if(userRole === 'finance') {
            document.getElementById('stat-total').innerText = tCom.toLocaleString() + " F";
            document.getElementById('label-total').innerText = "CUMUL COM.";
        } else {
            document.getElementById('stat-total').innerText = tLiv.toLocaleString() + " F";
            document.getElementById('label-total').innerText = "VOS GAINS";
        }
    }

    window.accepter = (k) => {
        const myName = auth.currentUser.email.split('@')[0].toUpperCase();
        update(ref(db, `missions/${k}`), { livreur: myName });
    };

    window.triggerCam = (k) => {
        currentPhotoKey = k;
        document.getElementById('camInput').click();
    };

    document.getElementById('camInput').onchange = (e) => {
        const file = e.target.files[0];
        const reader = new FileReader();
        reader.onload = (re) => {
            const img = new Image();
            img.onload = () => {
                const can = document.getElementById('canvas');
                const ctx = can.getContext('2d');
                can.width = 600; can.height = 600 * (img.height / img.width);
                ctx.drawImage(img, 0, 0, 600, can.height);
                currentPhotoData = can.toDataURL('image/jpeg', 0.6);
                const pre = document.getElementById('pre-'+currentPhotoKey);
                pre.src = currentPhotoData; pre.style.display = 'block';
                document.getElementById('val-'+currentPhotoKey).disabled = false;
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.livrer = (k) => {
        const code = document.getElementById('sms-'+k).value;
        if(!code) return alert("Code SMS requis");
        update(ref(db, `missions/${k}`), { etape: 2, codeSMS: code, photo: currentPhotoData });
    };

    window.encaisser = (k) => {
        if(confirm("Confirmer l'encaissement et fermer la mission ?")) {
            update(ref(db, `missions/${k}`), { etape: 3 });
        }
    };

    document.getElementById('btnLancerMission').onclick = () => {
        const nom = document.getElementById('mNom').value;
        const mnt = parseInt(document.getElementById('mRetrait').value);
        if(!nom || !mnt) return alert("Remplir les champs !");
        
        push(ref(db, 'missions'), {
            id: "CT" + Math.floor(100 + Math.random() * 899),
            nom, tel: document.getElementById('mTel').value, 
            lieu: document.getElementById('mLieu').value,
            retrait: mnt, com: (mnt >= 15000 ? 390 : 190), 
            etape: 1, livreur: "Libre",
            timestamp: Date.now(), date: new Date().toLocaleDateString('fr-FR')
        });
        
        document.getElementById('mNom').value = ""; document.getElementById('mRetrait').value = "";
        window.ouvrir('missions');
    };

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('nav-'+id).classList.add('active');
    };

    document.getElementById('bilanSearch').oninput = () => renderUI();
</script>
</body>
</html>
