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
        
        /* AUTH */
        #auth-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #1e272e; display: flex; align-items: center; justify-content: center; z-index: 9999;
        }
        .login-card {
            background: white; padding: 25px; border-radius: 20px; width: 85%; max-width: 320px;
            text-align: center; border-top: 8px solid var(--gabon-jaune);
        }
        input { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; }
        .btn-login { width: 100%; padding: 14px; background: var(--gabon-vert); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }

        /* APP */
        #main-app { display: none; max-width: 600px; margin: auto; background: white; border-radius: 15px; padding: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); min-height: 90vh; }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        .role-badge { font-size: 10px; padding: 3px 10px; border-radius: 12px; background: var(--dark); color: white; font-weight: bold; }

        nav { display: flex; gap: 5px; margin-bottom: 15px; background: #eee; padding: 5px; border-radius: 10px; }
        nav button { flex: 1; padding: 10px; border: none; border-radius: 7px; background: transparent; font-weight: bold; font-size: 11px; color: #666; cursor: pointer; }
        nav button.active { background: white; color: var(--gabon-vert); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }

        .section { display: none; }
        .active-sec { display: block; }

        /* CARDS */
        .card { border: 1px solid #eee; padding: 15px; border-radius: 12px; margin-bottom: 12px; position: relative; border-left: 6px solid #ccc; }
        .card.step-1 { border-left-color: var(--gabon-bleu); } /* En attente / Libre */
        .card.step-2 { border-left-color: var(--gabon-jaune); } /* Livré, attente encaissement */
        .card.step-3 { border-left-color: var(--gabon-vert); background: #f0fff4; } /* Terminé */
        
        .card-title { font-weight: bold; font-size: 14px; margin-bottom: 5px; }
        .card-info { font-size: 12px; color: #555; line-height: 1.4; }
        .badge-id { position: absolute; top: 10px; right: 10px; background: #f1f2f6; padding: 2px 6px; border-radius: 4px; font-size: 10px; font-weight: bold; }

        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; font-size: 13px; }
        .btn-accept { background: var(--gabon-bleu); color: white; }
        .btn-photo { background: var(--dark); color: white; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-confirm { background: var(--gabon-vert); color: white; }
        .btn-confirm:disabled { opacity: 0.5; cursor: not-allowed; }

        /* PHOTO PREVIEW */
        .photo-box { margin-top: 10px; border: 2px dashed #ddd; border-radius: 8px; padding: 10px; text-align: center; }
        .preview-img { width: 100%; border-radius: 5px; display: none; margin-top: 8px; border: 2px solid var(--gabon-vert); }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666; margin-bottom: 15px;">Connectez-vous pour accéder au réseau</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">ACCÉDER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-vert)">CT241 OPS</h3>
                <span id="user-display" style="font-size:10px; color:#888"></span>
                <span id="user-role" class="role-badge">...</span>
            </div>
            <button id="btnOut" style="font-size:10px; color:var(--danger); background:none; border:none; font-weight:bold; cursor:pointer">DÉCONNEXION</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('creer')" id="nav-creer" class="role-hide finance-only admin-only">CRÉER</button>
            <button onclick="ouvrir('missions')" id="nav-missions">MISSIONS</button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">BILAN</button>
        </nav>

        <!-- SECTION CRÉATION (Finance & Admin) -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 10px; border: 1px solid #eee; border-radius: 12px;">
                <h4 style="margin-top:0">Nouvelle Mission de Retrait</h4>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="Téléphone Client">
                <input type="text" id="mLieu" placeholder="Quartier / Emplacement">
                <input type="number" id="mRetrait" placeholder="Montant à retirer (FCFA)" oninput="calculerCom()">
                <div id="comLabel" style="font-size:11px; color:var(--gabon-vert); font-weight:bold; margin: 5px 0;">Com Direction: 0 F</div>
                <button onclick="creerMission()" class="btn-action btn-confirm">LANCER LA MISSION</button>
            </div>
        </div>

        <!-- SECTION MISSIONS (Tout le monde) -->
        <div id="sec-missions" class="section active-sec">
            <div id="container-missions"></div>
        </div>

        <!-- SECTION BILAN (Tout le monde) -->
        <div id="sec-bilan" class="section">
            <div id="container-bilan"></div>
            <div style="background:var(--dark); color:white; padding:15px; border-radius:12px; margin-top:15px;">
                <div style="display:flex; justify-content:space-between"><span>Terminées</span> <b id="stat-count">0</b></div>
                <div style="display:flex; justify-content:space-between; margin-top:10px;"><span style="font-size:18px">TOTAL GAIN</span> <b id="stat-total" style="color:var(--gabon-jaune); font-size:20px">0 F</b></div>
            </div>
        </div>
    </div>

    <!-- Hidden Elements for Logic -->
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

    let userRole = "visiteur";
    let myMissions = [];
    let currentPhotoKey = null;
    let currentPhotoData = "";

    // --- AUTHENTIFICATION ---
    document.getElementById('btnConnect').onclick = () => {
        const e = document.getElementById('login-email').value;
        const p = document.getElementById('login-pass').value;
        signInWithEmailAndPassword(auth, e, p).catch(err => alert("Erreur d'accès : " + err.message));
    };
    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        if(u) {
            const email = u.email.toLowerCase();
            if(email.includes('admin')) userRole = "admin";
            else if(email.includes('finance')) userRole = "finance";
            else if(email.includes('livreur')) userRole = "livreur";

            document.getElementById('user-role').innerText = userRole.toUpperCase();
            document.getElementById('user-display').innerText = u.email;
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';

            // Visibilité des onglets
            document.querySelectorAll('.role-hide').forEach(el => el.style.display = 'none');
            if(userRole === 'admin') document.querySelectorAll('.admin-only').forEach(el => el.style.display = 'block');
            if(userRole === 'finance') document.querySelectorAll('.finance-only').forEach(el => el.style.display = 'block');

            ouvrir(userRole === 'livreur' ? 'missions' : (userRole === 'finance' ? 'creer' : 'missions'));
            chargerDonnees();
        } else {
            document.getElementById('auth-screen').style.display = 'flex';
            document.getElementById('main-app').style.display = 'none';
        }
    });

    // --- LOGIQUE METIER ---
    window.calculerCom = () => {
        const val = parseFloat(document.getElementById('mRetrait').value) || 0;
        const com = val >= 15000 ? 390 : (val > 0 ? 190 : 0);
        document.getElementById('comLabel').innerText = `Com Direction: ${com} F`;
        return com;
    };

    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const tel = document.getElementById('mTel').value;
        const lieu = document.getElementById('mLieu').value;
        const mnt = document.getElementById('mRetrait').value;
        if(!nom || !mnt) return alert("Nom et Montant requis");

        push(ref(db, 'missions'), {
            id: "CT" + Math.floor(100 + Math.random() * 899),
            nom, tel, lieu, retrait: parseFloat(mnt), com: calculerCom(),
            etape: 1, livreur: "Libre", codeSMS: "", photo: "",
            date: new Date().toLocaleDateString('fr-FR'),
            timestamp: Date.now()
        });

        alert("Mission lancée !");
        ['mNom','mTel','mLieu','mRetrait'].forEach(id => document.getElementById(id).value = "");
        ouvrir('missions');
    };

    function chargerDonnees() {
        onValue(ref(db, 'missions'), (snap) => {
            const data = snap.val();
            myMissions = data ? Object.keys(data).map(k => ({...data[k], key:k})) : [];
            renderMissions();
        });
    }

    function renderMissions() {
        const contM = document.getElementById('container-missions');
        const contB = document.getElementById('container-bilan');
        contM.innerHTML = ""; contB.innerHTML = "";
        
        let totalGain = 0; let countDone = 0;
        const myName = auth.currentUser.email.split('@')[0].toUpperCase();

        myMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            
            // --- ONGLET MISSIONS ---
            if(m.etape < 3) {
                // Filtrage : Admin voit tout, Finance voit tout, Livreur voit Libre OU ses missions
                const canSee = (userRole === 'admin' || userRole === 'finance' || m.livreur === "Libre" || m.livreur === myName);
                
                if(canSee) {
                    let actions = "";
                    
                    // CAS 1 : Mission Libre (Pour Livreur uniquement)
                    if(m.etape === 1 && m.livreur === "Libre" && userRole === 'livreur') {
                        actions = `<button class="btn-action btn-accept" onclick="accepterMission('${m.key}')">ACCEPTER LA MISSION</button>`;
                    }
                    
                    // CAS 2 : Mission en cours (Pour le livreur qui a accepté)
                    else if(m.etape === 1 && m.livreur === myName && userRole === 'livreur') {
                        actions = `
                            <div class="photo-box">
                                <button class="btn-action btn-photo" onclick="triggerCam('${m.key}')">📸 FILMER LE SMS</button>
                                <img id="pre-${m.key}" class="preview-img">
                                <input type="text" id="sms-${m.key}" placeholder="Code de transaction SMS" style="margin-top:10px">
                                <button id="val-${m.key}" class="btn-action btn-confirm" onclick="finaliserLivreur('${m.key}')" disabled>CONFIRMER LIVRAISON</button>
                            </div>
                        `;
                    }

                    // CAS 3 : Attente Encaissement (Pour Finance et Admin)
                    else if(m.etape === 2 && (userRole === 'admin' || userRole === 'finance')) {
                        actions = `
                            <div style="background:#f9f9f9; padding:10px; border-radius:8px; margin-top:10px; font-size:11px;">
                                <b>Livreur :</b> ${m.livreur}<br>
                                <b>Code SMS :</b> <span style="color:var(--gabon-bleu)">${m.codeSMS}</span>
                            </div>
                            <button class="btn-action btn-confirm" onclick="encaisser('${m.key}')">ENCAISSER (${m.com} F)</button>
                        `;
                    }

                    contM.innerHTML += `
                        <div class="card step-${m.etape}">
                            <span class="badge-id">${m.id}</span>
                            <div class="card-title">${m.nom}</div>
                            <div class="card-info">
                                📍 ${m.lieu || 'Non précisé'}<br>
                                💰 Montant : <b>${m.retrait.toLocaleString()} F</b><br>
                                👤 Statut : <b>${m.livreur === 'Libre' ? 'DISPONIBLE' : 'Prise par ' + m.livreur}</b>
                            </div>
                            ${actions}
                        </div>
                    `;
                }
            }

            // --- ONGLET BILAN ---
            if(m.etape === 3) {
                const canSeeBilan = (userRole === 'admin' || userRole === 'finance' || m.livreur === myName);
                if(canSeeBilan) {
                    contB.innerHTML += `
                        <div class="card step-3">
                            <div class="card-title" style="font-size:12px">${m.nom}</div>
                            <div style="font-size:10px; color:#666">${m.date} | Livré par ${m.livreur}</div>
                        </div>
                    `;
                    countDone++;
                    // Calcul gain : Livreur gagne sa prime fixe, Finance/Admin voient la commission direction
                    if(userRole === 'livreur') totalGain += (m.retrait >= 15000 ? 800 : 400);
                    else totalGain += m.com;
                }
            }
        });

        document.getElementById('stat-count').innerText = countDone;
        document.getElementById('stat-total').innerText = totalGain.toLocaleString() + " F";
    }

    // --- ACTIONS BOUTONS ---
    window.accepterMission = (k) => {
        const myName = auth.currentUser.email.split('@')[0].toUpperCase();
        update(ref(db, `missions/${k}`), { livreur: myName });
    };

    window.triggerCam = (k) => {
        currentPhotoKey = k;
        document.getElementById('camInput').click();
    };

    document.getElementById('camInput').onchange = (e) => {
        const file = e.target.files[0];
        if(!file) return;
        const reader = new FileReader();
        reader.onload = (re) => {
            const img = new Image();
            img.onload = () => {
                const can = document.getElementById('canvas');
                const ctx = can.getContext('2d');
                can.width = 600; can.height = (img.height / img.width) * 600;
                ctx.drawImage(img, 0, 0, can.width, can.height);
                currentPhotoData = can.toDataURL('image/jpeg', 0.7);
                
                const preview = document.getElementById('pre-'+currentPhotoKey);
                preview.src = currentPhotoData;
                preview.style.display = 'block';
                document.getElementById('val-'+currentPhotoKey).disabled = false;
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.finaliserLivreur = (k) => {
        const code = document.getElementById('sms-'+k).value;
        if(!code) return alert("Entrez le code SMS !");
        update(ref(db, `missions/${k}`), {
            etape: 2,
            codeSMS: code,
            photo: currentPhotoData
        }).then(() => {
            currentPhotoData = "";
            alert("Preuve envoyée ! En attente d'encaissement.");
        });
    };

    window.encaisser = (k) => {
        if(confirm("Confirmez-vous avoir reçu les fonds ?")) {
            update(ref(db, `missions/${k}`), { etape: 3 });
        }
    };

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('nav-'+id).classList.add('active');
    };

</script>
</body>
</html>
