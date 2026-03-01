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
        input { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; }
        .btn-login { width: 100%; padding: 14px; background: var(--gabon-vert); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }
        .error-msg { color: var(--danger); font-size: 11px; margin-top: 10px; display: none; }

        /* MAIN APP */
        #main-app { display: none; max-width: 600px; margin: auto; background: white; border-radius: 15px; padding: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); min-height: 90vh; }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        .role-badge { font-size: 10px; padding: 3px 10px; border-radius: 12px; background: var(--dark); color: white; font-weight: bold; }

        nav { display: flex; gap: 5px; margin-bottom: 15px; background: #eee; padding: 5px; border-radius: 10px; position: relative; }
        nav button { flex: 1; padding: 12px 5px; border: none; border-radius: 7px; background: transparent; font-weight: bold; font-size: 11px; color: #666; cursor: pointer; }
        nav button.active { background: white; color: var(--gabon-vert); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        
        .section { display: none; }
        .active-sec { display: block; }

        /* MISSION CARDS */
        .card { border: 1px solid #eee; padding: 15px; border-radius: 12px; margin-bottom: 12px; position: relative; border-left: 6px solid #ccc; }
        .card.step-1 { border-left-color: var(--gabon-bleu); }
        .card.step-2 { border-left-color: var(--gabon-jaune); }
        
        .card-title { font-weight: bold; font-size: 14px; margin-bottom: 5px; }
        .card-info { font-size: 12px; color: #555; line-height: 1.4; }
        .badge-id { position: absolute; top: 10px; right: 10px; background: #f1f2f6; padding: 2px 6px; border-radius: 4px; font-size: 10px; font-weight: bold; }

        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; font-size: 13px; }
        .btn-confirm { background: var(--gabon-vert); color: white; }

        /* BILAN STYLING */
        .bilan-item { padding: 15px 0; border-bottom: 1px solid #eee; }
        .bilan-header { display: flex; justify-content: space-between; align-items: flex-start; }
        .bilan-client { font-weight: 800; font-size: 14px; }
        .bilan-gain { color: var(--gabon-vert); font-weight: 900; }
        
        .btn-view-proof { background: #f1f2f6; border: none; padding: 8px 12px; border-radius: 6px; font-size: 10px; font-weight: bold; color: var(--gabon-bleu); cursor: pointer; margin-top: 10px; }
        .proof-img-container { display: none; margin-top: 10px; border-radius: 8px; overflow: hidden; border: 1px solid #ddd; }
        .proof-img-container img { width: 100%; height: auto; display: block; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666; margin-bottom: 15px;">Interface de Gestion</p>
            <input type="email" id="login-email" placeholder="Email (ex: admin@ct241.com)">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
            <div id="auth-error" class="error-msg">Identifiants incorrects ou problème réseau.</div>
            <p style="font-size: 10px; color: #999; margin-top: 15px;">Note: Utilisez un email contenant 'admin', 'finance' ou 'livreur'.</p>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-vert)">CT241 OPS</h3>
                <span id="user-display" style="font-size:10px; color:#888"></span>
                <span id="user-role" class="role-badge">CHARGEMENT...</span>
            </div>
            <button id="btnOut" style="font-size:10px; color:var(--danger); background:none; border:none; font-weight:bold;">DECONNEXION</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">CRÉER</button>
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">MISSIONS</button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">BILAN</button>
        </nav>

        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 15px; border: 1px solid #eee; border-radius: 12px;">
                <h4 style="margin-top:0">Nouvelle Mission</h4>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="Téléphone Client">
                <input type="text" id="mLieu" placeholder="Quartier / Destination">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)">
                <button onclick="creerMission()" class="btn-action btn-confirm">LANCER LA MISSION</button>
            </div>
        </div>

        <div id="sec-missions" class="section active-sec">
            <div id="container-missions"></div>
        </div>

        <div id="sec-bilan" class="section">
            <h4 style="margin:0 0 10px 0;">HISTORIQUE & GAINS</h4>
            <div id="container-bilan" style="background:white; border-radius:12px; border:1px solid #eee; padding:0 15px;"></div>
            <div style="background:var(--dark); color:white; padding:15px; border-radius:12px; margin-top:15px; display:flex; justify-content:space-between; align-items:center;">
                <span id="label-total">TOTAL</span>
                <b id="stat-total" style="color:var(--gabon-jaune); font-size:18px">0 F</b>
            </div>
        </div>
    </div>

    <canvas id="canvas" style="display:none"></canvas>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
    import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-auth.js";
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

    // Connexion
    document.getElementById('btnConnect').onclick = async () => {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        const errDiv = document.getElementById('auth-error');
        
        errDiv.style.display = 'none';

        try {
            await signInWithEmailAndPassword(auth, email, pass);
        } catch (error) {
            console.error(error);
            errDiv.innerText = "Erreur: " + error.message;
            errDiv.style.display = 'block';
            
            // Secours : Mode Démo/Anonyme si les identifiants échouent pour l'instant
            if(confirm("Échec de connexion. Voulez-vous entrer en mode 'Démo Invité' ?")) {
                await signInAnonymously(auth);
            }
        }
    };

    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (user) => {
        if(user) {
            const email = (user.email || "livreur-demo@ct241.com").toLowerCase();
            
            if(email.includes('admin')) userRole = "admin";
            else if(email.includes('finance')) userRole = "finance";
            else userRole = "livreur";

            document.getElementById('user-role').innerText = userRole.toUpperCase();
            document.getElementById('user-display').innerText = email;
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';

            // Visibilité Menu
            if(userRole === 'admin' || userRole === 'finance') {
                document.getElementById('nav-creer').style.display = 'block';
            }

            chargerDonnees();
        } else {
            document.getElementById('auth-screen').style.display = 'flex';
            document.getElementById('main-app').style.display = 'none';
        }
    });

    function chargerDonnees() {
        onValue(ref(db, 'missions'), (snap) => {
            const data = snap.val();
            allMissions = data ? Object.keys(data).map(k => ({...data[k], key:k})) : [];
            renderUI();
        });
    }

    function renderUI() {
        const contM = document.getElementById('container-missions');
        const contB = document.getElementById('container-bilan');
        contM.innerHTML = ""; contB.innerHTML = "";
        
        let total = 0;
        const myId = auth.currentUser.email ? auth.currentUser.email.split('@')[0].toUpperCase() : "DEMO";

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            // Liste active
            if(m.etape < 3) {
                if(userRole !== 'livreur' || m.livreur === "Libre" || m.livreur === myId) {
                    contM.innerHTML += `
                        <div class="card step-${m.etape}">
                            <span class="badge-id">${m.id}</span>
                            <div class="card-title">${m.nom}</div>
                            <div class="card-info">
                                📍 <b>${m.lieu || 'N/A'}</b> | 💰 <b>${m.retrait} F</b><br>
                                🛵 Livreur: ${m.livreur}
                            </div>
                        </div>
                    `;
                }
            }

            // Bilan (Missions terminées)
            if(m.etape === 3) {
                const gain = (userRole === 'livreur') ? 800 : m.com;
                total += gain;
                
                contB.innerHTML += `
                    <div class="bilan-item">
                        <div class="bilan-header">
                            <span class="bilan-client">${m.nom} <small>#${m.id}</small></span>
                            <span class="bilan-gain">+ ${gain} F</span>
                        </div>
                        <div style="font-size:11px; color:#666; margin-top:4px">
                            📍 Destination: <b>${m.lieu || 'N/A'}</b><br>
                            🛵 Livreur: ${m.livreur} | SMS: ${m.codeSMS}
                        </div>
                        ${m.photo ? `
                            <button class="btn-view-proof" onclick="this.nextElementSibling.style.display='block'">👁️ VOIR PREUVE</button>
                            <div class="proof-img-container"><img src="${m.photo}"></div>
                        ` : ''}
                    </div>
                `;
            }
        });
        document.getElementById('stat-total').innerText = total + " F";
    }

    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const mnt = document.getElementById('mRetrait').value;
        if(!nom || !mnt) return alert("Nom et Montant requis");
        
        push(ref(db, 'missions'), {
            id: "CT" + Math.floor(100+Math.random()*900),
            nom, lieu: document.getElementById('mLieu').value,
            retrait: parseInt(mnt), com: 390, etape: 1, 
            livreur: "Libre", timestamp: Date.now(), date: new Date().toLocaleDateString()
        });
        alert("Mission créée");
        ouvrir('missions');
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
