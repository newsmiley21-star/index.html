<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SMILEY LOGISTIQUE +241</title>
    <style>
        :root {
            --gabon-vert: #009E60; --gabon-jaune: #FCD116; --gabon-bleu: #3A75C4;
            --danger: #e74c3c; --dark: #1a1a1a; --light: #f8f9fa;
        }
        body { font-family: 'Segoe UI', Tahoma, sans-serif; background: var(--light); margin: 0; padding: 10px; }
        
        /* ECRAN LOGIN */
        #auth-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: var(--dark); display: flex; align-items: center; justify-content: center; z-index: 9999;
        }
        .login-card {
            background: white; padding: 30px; border-radius: 20px; width: 85%; max-width: 350px;
            text-align: center; border-top: 10px solid var(--gabon-jaune); box-shadow: 0 10px 25px rgba(0,0,0,0.3);
        }
        .login-card input { width: 100%; padding: 14px; margin: 10px 0; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; font-size: 16px; }
        .btn-login { width: 100%; padding: 15px; background: var(--gabon-vert); color: white; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; font-size: 16px; }

        /* APP PRINCIPALE */
        #main-app { display: none; }
        .container { max-width: 1000px; margin: auto; background: white; padding: 15px; border-radius: 15px; box-shadow: 0 5px 15px rgba(0,0,0,0.1); border-top: 10px solid var(--gabon-vert); }
        header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; border-bottom: 1px solid #eee; padding-bottom: 10px; }
        
        nav { display: flex; gap: 8px; margin-bottom: 20px; position: sticky; top: 0; background: white; padding: 5px 0; z-index: 10; }
        nav button { padding: 12px 5px; cursor: pointer; border: none; background: #eee; border-radius: 8px; flex: 1; font-weight: bold; font-size: 11px; transition: 0.3s; }
        nav button.active { background: var(--gabon-vert); color: white; box-shadow: 0 3px 0 var(--gabon-jaune); }

        .form-section { background: #f1f3f5; padding: 15px; border-radius: 12px; margin-bottom: 20px; border: 1px solid #e0e0e0; }
        .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        input { padding: 12px; border: 1px solid #ddd; border-radius: 8px; width: 100%; box-sizing: border-box; font-size: 14px; }
        .btn-launch { width: 100%; padding: 16px; background: var(--gabon-bleu); color: white; border: none; border-radius: 8px; font-weight: bold; margin-top: 10px; cursor: pointer; text-transform: uppercase; }

        table { width: 100%; border-collapse: collapse; font-size: 12px; }
        th { background: #333; color: white; padding: 10px; text-align: left; text-transform: uppercase; font-size: 10px; }
        td { border-bottom: 1px solid #eee; padding: 10px; vertical-align: middle; }
        .id-badge { background: #333; color: var(--gabon-jaune); padding: 2px 5px; border-radius: 4px; font-family: monospace; font-weight: bold; }
        .phone-link { color: var(--gabon-bleu); font-weight: bold; text-decoration: none; display: block; margin-top: 2px; }
        
        .card { background: white; border: 1px solid #ddd; border-left: 8px solid var(--gabon-bleu); padding: 15px; margin-bottom: 10px; border-radius: 10px; display: flex; justify-content: space-between; align-items: center; }
        .card.done { border-left-color: var(--gabon-vert); background: #f0fff4; }
        
        .summary-box { margin-top: 20px; padding: 20px; background: var(--dark); color: var(--gabon-jaune); border-radius: 12px; text-align: right; }
        .summary-box b { font-size: 1.8em; color: white; display: block; }
        
        .section { display: none; }
        .active-sec { display: block; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color: var(--gabon-vert); margin:0;">SMILEY CLOUD 🇬🇦</h2>
            <p style="font-size: 12px; color: gray; margin-bottom: 20px;">Accès Administrateur</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnActionConnexion">SE CONNECTER</button>
            <p id="error-msg" style="color:red; font-size: 12px; margin-top: 15px; font-weight: bold;"></p>
        </div>
    </div>

    <div id="main-app">
        <div class="container">
            <header>
                <div>
                    <h1 style="font-size: 18px; color: var(--gabon-vert); margin:0;">SMILEY LOGISTIQUE</h1>
                    <small id="user-info" style="color: gray; font-size: 10px;"></small>
                </div>
                <span id="btnDeconnecter" style="color:red; cursor:pointer; font-size: 11px; font-weight: bold; text-decoration: underline;">Déconnexion</span>
            </header>

            <nav>
                <button onclick="ouvrir('saisie')" id="tab-saisie" class="active">📊 SAISIE</button>
                <button onclick="ouvrir('taches')" id="tab-taches">📦 TÂCHES</button>
                <button onclick="ouvrir('reçus')" id="tab-reçus">📜 REÇUS</button>
            </nav>

            <div id="sec-saisie" class="section active-sec">
                <div class="form-section">
                    <div class="form-grid">
                        <input type="text" id="mNom" placeholder="Nom Client">
                        <input type="tel" id="mTel" placeholder="N° Téléphone">
                        <input type="text" id="mLieu" placeholder="Quartier / Lieu">
                        <input type="number" id="mCom" placeholder="Commission (FCFA)">
                    </div>
                    <button class="btn-launch" id="btnCreer">🚀 Lancer la Mission</button>
                </div>
                <div style="overflow-x:auto;">
                    <table>
                        <thead><tr><th>N° Ordre</th><th>Client / Tel</th><th>Com.</th></tr></thead>
                        <tbody id="body-saisie"></tbody>
                    </table>
                </div>
            </div>

            <div id="sec-taches" class="section">
                <h3 style="font-size: 14px; color: var(--gabon-bleu);">📦 Missions sur le terrain</h3>
                <div id="liste-taches"></div>
            </div>

            <div id="sec-reçus" class="section">
                <h3 style="font-size: 14px;">📜 Journal des Encaissements</h3>
                <div id="liste-reçus"></div>
                <div class="summary-box">TOTAL ENCAISSÉ <b id="totalCom">0</b> FCFA</div>
            </div>
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

    // --- CONNEXION ---
    document.getElementById('btnActionConnexion').onclick = () => {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        signInWithEmailAndPassword(auth, email, pass).catch(err => {
            document.getElementById('error-msg').innerText = "Accès refusé : Vérifiez vos identifiants.";
        });
    };

    document.getElementById('btnDeconnecter').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (user) => {
        const authScreen = document.getElementById('auth-screen');
        const mainApp = document.getElementById('main-app');
        if (user) {
            authScreen.style.setProperty('display', 'none', 'important');
            mainApp.style.display = 'block';
            document.getElementById('user-info').innerText = user.email;
            ecouterMissions();
        } else {
            authScreen.style.display = 'flex';
            mainApp.style.display = 'none';
        }
    });

    // --- NAVIGATION ---
    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-' + id).classList.add('active-sec');
        document.getElementById('tab-' + id).classList.add('active');
    };

    // --- LOGIQUE METIER ---
    let dernierIndex = 0;
    document.getElementById('btnCreer').onclick = () => {
        const nom = document.getElementById('mNom').value;
        const com = document.getElementById('mCom').value;
        if(!nom || !com) return alert("Veuillez remplir les champs obligatoires.");

        const idOrdre = "OR-" + (dernierIndex + 1).toString().padStart(3, '0');
        push(ref(db, 'missions'), {
            id: idOrdre, nom, tel: document.getElementById('mTel').value || "N/A",
            lieu: document.getElementById('mLieu').value || "-",
            com: parseFloat(com), etape: 1,
            heure: new Date().toLocaleTimeString('fr-FR', {hour:'2-digit', minute:'2-digit'}),
            date: new Date().toLocaleDateString('fr-FR')
        });
        ['mNom', 'mTel', 'mLieu', 'mCom'].forEach(i => document.getElementById(i).value = "");
    };

    function ecouterMissions() {
        onValue(ref(db, 'missions'), (snap) => {
            const data = snap.val();
            const bS = document.getElementById('body-saisie');
            const lT = document.getElementById('liste-taches');
            const lR = document.getElementById('liste-reçus');
            let total = 0; let maxIdx = 0;
            bS.innerHTML = ""; lT.innerHTML = ""; lR.innerHTML = "";
            
            if(data) {
                Object.keys(data).forEach(key => {
                    const m = data[key];
                    const num = parseInt(m.id.split('-')[1]);
                    if(num > maxIdx) maxIdx = num;

                    bS.innerHTML += `<tr><td><span class="id-badge">${m.id}</span><br><small>${m.heure}</small></td><td><b>${m.nom}</b><a href="tel:${m.tel}" class="phone-link">📞 ${m.tel}</a></td><td>${m.com}</td></tr>`;
                    
                    if(m.etape === 1) {
                        lT.innerHTML += `<div class="card"><div><b>${m.nom}</b><br><small>📍 ${m.lieu} | 🕒 ${m.heure}</small></div><button onclick="validerLiv('${key}')" style="background:var(--gabon-vert); color:white; border:none; padding:10px; border-radius:8px; font-weight:bold;">LIVRER</button></div>`;
                    } else {
                        if(m.etape === 3) total += m.com;
                        lR.innerHTML += `<div class="card ${m.etape === 3 ? 'done' : ''}"><div><b>${m.nom}</b><br><small>${m.com} FCFA | ${m.etape === 3 ? '✅ Payé' : '⏳ Attente'}</small></div><div style="display:flex; gap:5px;">${m.etape === 2 ? `<button onclick="encaisser('${key}')" style="background:var(--gabon-bleu); color:white; border:none; padding:10px; border-radius:8px; font-weight:bold;">ENCAISSER</button>` : ''}<button onclick="supprimer('${key}')" style="background:var(--danger); color:white; border:none; padding:8px; border-radius:8px; font-weight:bold;">X</button></div></div>`;
                    }
                });
            }
            dernierIndex = maxIdx;
            document.getElementById('totalCom').innerText = total.toLocaleString();
        });
    }

    // --- ACTIONS EXPOSEES ---
    window.validerLiv = (key) => update(ref(db, 'missions/'+key), { etape: 2 });
    window.encaisser = (key) => update(ref(db, 'missions/'+key), { etape: 3 });
    window.supprimer = (key) => { if(confirm("Supprimer ?")) remove(ref(db, 'missions/'+key)); };

</script>
</body>
</html>
