<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SMILEY CLOUD +241</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        :root { --gabon-vert: #009E60; --gabon-jaune: #FCD116; --gabon-bleu: #3A75C4; --danger: #e74c3c; --dark: #1a1a1a; --light: #f8f9fa; }
        body { font-family: 'Segoe UI', sans-serif; background: var(--light); margin: 0; padding: 10px; }
        #auth-screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--dark); display: flex; align-items: center; justify-content: center; z-index: 9999; }
        .login-card { background: white; padding: 30px; border-radius: 20px; width: 85%; max-width: 350px; text-align: center; border-top: 10px solid var(--gabon-jaune); }
        .login-card input { width: 100%; padding: 14px; margin: 10px 0; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; font-size: 16px; }
        .btn-login { width: 100%; padding: 15px; background: var(--gabon-vert); color: white; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; }
        #main-app { display: none; }
        .container { max-width: 800px; margin: auto; background: white; padding: 15px; border-radius: 15px; border-top: 10px solid var(--gabon-vert); box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        nav { display: flex; gap: 8px; margin-bottom: 15px; }
        nav button { padding: 12px 5px; cursor: pointer; border: none; background: #eee; border-radius: 8px; flex: 1; font-weight: bold; font-size: 11px; }
        nav button.active { background: var(--gabon-vert); color: white; }
        .form-section { background: #f1f3f5; padding: 15px; border-radius: 12px; margin-bottom: 20px; }
        .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        input { padding: 12px; border: 1px solid #ddd; border-radius: 8px; width: 100%; box-sizing: border-box; }
        .btn-launch { width: 100%; padding: 16px; background: var(--gabon-bleu); color: white; border: none; border-radius: 8px; font-weight: bold; margin-top: 10px; cursor: pointer; }
        .card { background: white; border: 1px solid #ddd; border-left: 8px solid var(--gabon-bleu); padding: 15px; margin-bottom: 10px; border-radius: 10px; display: flex; justify-content: space-between; align-items: center; }
        .summary-box { margin-top: 20px; padding: 20px; background: var(--dark); color: var(--gabon-jaune); border-radius: 12px; text-align: right; }
        .section { display: none; }
        .active-sec { display: block; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color: var(--gabon-vert); margin:0;">SMILEY CLOUD 🇬🇦</h2>
            <input type="email" id="login-email" placeholder="Email Admin">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnGo">SE CONNECTER</button>
            <p id="error-msg" style="color:red; font-size:12px;"></p>
        </div>
    </div>

    <div id="main-app">
        <div class="container">
            <header style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
                <h1 style="font-size: 18px; color: var(--gabon-vert);">SMILEY LOGISTIQUE</h1>
                <span id="btnOut" style="color:red; cursor:pointer; font-size:11px; font-weight:bold;">Déconnexion</span>
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
                        <input type="tel" id="mTel" placeholder="Téléphone">
                        <input type="text" id="mLieu" placeholder="Destination">
                        <input type="number" id="mCom" placeholder="Commission">
                    </div>
                    <button class="btn-launch" id="btnCreer">🚀 LANCER MISSION</button>
                </div>
                <div id="liste-historique"></div>
            </div>

            <div id="sec-taches" class="section"><div id="liste-taches"></div></div>
            <div id="sec-reçus" class="section">
                <div id="liste-reçus"></div>
                <div class="summary-box">TOTAL <b id="totalCom" style="font-size:2em; color:white;">0</b> FCFA</div>
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

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-' + id).classList.add('active-sec');
        document.getElementById('tab-' + id).classList.add('active');
    };

    document.getElementById('btnGo').onclick = () => {
        signInWithEmailAndPassword(auth, document.getElementById('login-email').value, document.getElementById('login-pass').value)
        .catch(() => document.getElementById('error-msg').innerText = "Erreur d'accès");
    };

    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (user) => {
        if (user) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            ecouterMissions();
        } else {
            document.getElementById('auth-screen').style.display = 'flex';
            document.getElementById('main-app').style.display = 'none';
        }
    });

    document.getElementById('btnCreer').onclick = () => {
        const nom = document.getElementById('mNom').value;
        const com = document.getElementById('mCom').value;
        if(nom && com) {
            push(ref(db, 'missions'), {
                nom, com: parseFloat(com),
                tel: document.getElementById('mTel').value || "N/A",
                lieu: document.getElementById('mLieu').value || "N/A",
                etape: 1, date: new Date().toLocaleString()
            });
            document.querySelectorAll('input').forEach(i => i.value = "");
        }
    };

    function ecouterMissions() {
        onValue(ref(db, 'missions'), (snap) => {
            const data = snap.val();
            const lT = document.getElementById('liste-taches');
            const lR = document.getElementById('liste-reçus');
            const lH = document.getElementById('liste-historique');
            let total = 0;
            lT.innerHTML = ""; lR.innerHTML = ""; lH.innerHTML = "";
            if(data) {
                Object.entries(data).forEach(([key, m]) => {
                    const html = `<div class="card">
                        <div><b>${m.nom}</b><br><small>${m.com} FCFA</small></div>
                        <div>
                            ${m.etape === 1 ? `<button onclick="mod('${key}', 2)" style="background:var(--gabon-vert); color:white; border:none; padding:10px; border-radius:5px;">LIVRER</button>` : 
                            `<button onclick="pdf('${m.nom}','${m.com}')" style="background:#555; color:white; border:none; padding:10px; border-radius:5px;">📄</button>
                             <button onclick="mod('${key}', 3)" style="background:var(--gabon-bleu); color:white; border:none; padding:10px; border-radius:5px;">${m.etape==3?'✅':'OK'}</button>`}
                        </div>
                    </div>`;
                    if(m.etape === 1) lT.innerHTML += html;
                    if(m.etape >= 2) lR.innerHTML += html;
                    if(m.etape === 3) total += m.com;
                    lH.innerHTML += `<div style="font-size:11px; border-bottom:1px solid #eee; padding:5px;">${m.date} - ${m.nom}</div>`;
                });
            }
            document.getElementById('totalCom').innerText = total.toLocaleString();
        });
    }

    window.mod = (k, e) => update(ref(db, 'missions/'+k), {etape: e});
    window.pdf = (n, c) => {
        const doc = new jspdf.jsPDF({format:[80, 80], unit:'mm'});
        doc.text("SMILEY CLOUD\nREÇU CLIENT\nNom: "+n+"\nTotal: "+c+" FCFA", 10, 10);
        doc.save("recu.pdf");
    };
</script>
</body>
</html>
