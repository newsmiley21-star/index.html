<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - GESTION PRO</title>
    <style>
        :root {
            --gabon-vert: #009E60; --gabon-jaune: #FCD116; --gabon-bleu: #3A75C4;
            --danger: #e74c3c; --dark: #1a1a1a; --light: #f8f9fa;
        }
        body { font-family: sans-serif; background: var(--light); margin: 0; padding: 10px; }
        
        #auth-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: var(--dark); display: flex; align-items: center; justify-content: center; z-index: 9999;
        }
        .login-card {
            background: white; padding: 25px; border-radius: 15px; width: 85%; max-width: 320px;
            text-align: center; border-top: 8px solid var(--gabon-jaune);
        }
        input { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; font-size: 14px; }
        .btn-login { width: 100%; padding: 14px; background: var(--gabon-vert); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }

        #main-app { display: none; max-width: 800px; margin: auto; background: white; border-radius: 12px; padding: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        
        nav { display: flex; gap: 5px; margin-bottom: 15px; }
        nav button { flex: 1; padding: 10px; border: none; border-radius: 5px; background: #eee; font-weight: bold; font-size: 11px; }
        nav button.active { background: var(--gabon-vert); color: white; }

        .form-box { background: #f9f9f9; padding: 15px; border-radius: 10px; margin-bottom: 20px; border: 1px solid #ddd; }
        #mCom { background: #e8f5e9; font-weight: bold; border: 1px solid var(--gabon-vert); color: var(--gabon-vert); }
        
        /* RECHERCHE */
        .search-box { background: white; border: 2px solid var(--gabon-bleu); padding: 10px; border-radius: 8px; margin-bottom: 15px; width: 100%; box-sizing: border-box; font-weight: bold; }

        table { width: 100%; border-collapse: collapse; font-size: 11px; }
        th { background: #444; color: white; padding: 8px; text-align: left; }
        td { padding: 8px; border-bottom: 1px solid #eee; }
        
        .card { border: 1px solid #ddd; padding: 12px; border-radius: 8px; margin-bottom: 10px; display: block; border-left: 6px solid var(--gabon-bleu); transition: 0.3s; }
        .card.done { border-left-color: var(--gabon-vert); background: #f0fff4; }
        .flex-card { display: flex; justify-content: space-between; align-items: center; }
        
        .tx-badge { background: #e3f2fd; color: #1976d2; padding: 3px 6px; border-radius: 4px; font-size: 10px; font-weight: bold; display: inline-block; margin-top: 5px; }
        .btn-wa { background: #25D366; color: white; border: none; padding: 10px; border-radius: 8px; width: 100%; font-weight: bold; cursor: pointer; margin-top: 10px; }
        
        .section { display: none; }
        .active-sec { display: block; }
        .tel-link { color: var(--gabon-bleu); text-decoration: none; font-weight: bold; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <h3 style="margin:0; color:var(--gabon-vert)">CT241 +241</h3>
            <button id="btnOut" style="font-size:10px; color:red; background:none; border:none; font-weight:bold">SORTIR</button>
        </header>

        <nav>
            <button onclick="ouvrir('saisie')" id="t-saisie" class="active">SAISIE</button>
            <button onclick="ouvrir('taches')" id="t-taches">TÂCHES</button>
            <button onclick="ouvrir('reçus')" id="t-reçus">REÇUS</button>
        </nav>

        <div id="sec-saisie" class="section active-sec">
            <div class="form-box">
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="Numéro du Client (Ex: 077...)">
                <input type="text" id="mLieu" placeholder="Quartier">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)" oninput="calcAuto()">
                <input type="number" id="mCom" placeholder="Commission calculée" readonly>
                <button onclick="lancerMission()" style="width:100%; padding:12px; background:var(--gabon-bleu); color:white; border:none; border-radius:8px; font-weight:bold; margin-top:10px">CRÉER MISSION</button>
            </div>
            <table>
                <thead><tr><th>ID</th><th>Client / Tél</th><th>Comm.</th></tr></thead>
                <tbody id="body-saisie"></tbody>
            </table>
        </div>

        <div id="sec-taches" class="section">
            <div id="list-taches"></div>
        </div>

        <div id="sec-reçus" class="section">
            <input type="text" id="searchInput" class="search-box" placeholder="🔍 Rechercher un nom, n° ou transaction..." oninput="filtrerRecus()">
            <div id="list-reçus"></div>
            <div style="background:#1a1a1a; color:var(--gabon-jaune); padding:15px; border-radius:10px; text-align:right; margin-top:10px">
                TOTAL COMMISSIONS <br><b id="totalCom" style="font-size:20px; color:white">0</b> FCFA
                <button class="btn-wa" onclick="shareWA()">📲 ENVOYER BILAN WHATSAPP</button>
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
    let userNow = "";
    let missionsLocales = [];

    // CALCUL AUTO (1000 F dès 15000 F)
    window.calcAuto = () => {
        const val = parseFloat(document.getElementById('mRetrait').value);
        let c = 0;
        if (val >= 15000) {
            c = 1000;
            if (val >= 40000 && val <= 80000) c = Math.max(1000, val * 0.02);
            else if (val >= 90000) c = Math.max(1000, val * 0.03);
        } else if (val >= 10000) {
            c = val * 0.015;
        }
        document.getElementById('mCom').value = Math.round(c);
    };

    // CONNEXION
    document.getElementById('btnConnect').onclick = () => {
        signInWithEmailAndPassword(auth, document.getElementById('login-email').value, document.getElementById('login-pass').value).catch(e => alert(e.message));
    };
    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        if(u) {
            userNow = u.email;
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            if(userNow.includes('livreur')) document.getElementById('t-saisie').style.display = 'none';
            ecouter();
        } else {
            document.getElementById('auth-screen').style.display = 'flex';
            document.getElementById('main-app').style.display = 'none';
        }
    });

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('t-'+id).classList.add('active');
    };

    let idx = 0;
    window.lancerMission = () => {
        const n = document.getElementById('mNom').value;
        const t = document.getElementById('mTel').value;
        const r = document.getElementById('mRetrait').value;
        const c = document.getElementById('mCom').value;
        if(!n || !r || !t) return alert("Nom, Téléphone et Montant obligatoires");
        
        push(ref(db, 'missions'), {
            id: "OR-"+(idx+1).toString().padStart(3,'0'),
            nom: n, tel: t, lieu: document.getElementById('mLieu').value || "-",
            retrait: parseFloat(r), com: parseFloat(c) || 0,
            etape: 1, livreur: "Aucun", transaction: "",
            date: new Date().toLocaleDateString('fr-FR'),
            heure: new Date().toLocaleTimeString('fr-FR', {hour:'2-digit', minute:'2-digit'})
        });
        ['mNom', 'mTel', 'mLieu', 'mRetrait', 'mCom'].forEach(id => document.getElementById(id).value = "");
    };

    function ecouter() {
        onValue(ref(db, 'missions'), (s) => {
            const data = s.val();
            missionsLocales = [];
            if(data) {
                Object.keys(data).forEach(k => missionsLocales.push({...data[k], key: k}));
            }
            afficherTout();
        });
    }

    function afficherTout() {
        const bS = document.getElementById('body-saisie');
        const lT = document.getElementById('list-taches');
        let tot = 0; idx = 0;
        bS.innerHTML = ""; lT.innerHTML = "";

        missionsLocales.slice().reverse().forEach(m => {
            const n = parseInt(m.id.split('-')[1]); if(n > idx) idx = n;

            // Saisie
            bS.innerHTML += `<tr><td>${m.id}</td><td><b>${m.nom}</b><br><small>${m.tel}</small></td><td>${m.com} F</td></tr>`;

            // Tâches
            if(m.etape === 1) {
                lT.innerHTML += `<div class="card">
                    <div class="flex-card">
                        <div><b>${m.nom}</b> (${m.retrait} F)<br><a href="tel:${m.tel}" class="tel-link">📞 ${m.tel}</a></div>
                    </div>
                    <input type="text" id="tx-${m.key}" placeholder="ID Transaction SMS..." style="margin-top:10px">
                    <button onclick="valider('${m.key}')" style="width:100%; background:var(--gabon-vert); color:white; border:none; padding:10px; border-radius:5px; margin-top:5px; font-weight:bold">VALIDER LIVRAISON</button>
                </div>`;
            }
        });
        filtrerRecus(); // Met à jour l'onglet Reçus
    }

    window.filtrerRecus = () => {
        const query = document.getElementById('searchInput').value.toLowerCase();
        const lR = document.getElementById('list-reçus');
        let tot = 0;
        lR.innerHTML = "";

        missionsLocales.slice().reverse().forEach(m => {
            if(m.etape > 1) {
                const chaine = (m.nom + m.tel + m.transaction).toLowerCase();
                if(chaine.includes(query)) {
                    if(m.etape === 3) tot += m.com;
                    lR.innerHTML += `<div class="card ${m.etape === 3 ? 'done' : ''}">
                        <div class="flex-card">
                            <div><b>${m.nom}</b> <small>(${m.date})</small><br><small>👤 ${m.livreur} | 📞 ${m.tel}</small></div>
                            <div>${m.etape === 2 ? `<button onclick="encaisser('${m.key}')" style="background:var(--gabon-bleu); color:white; border:none; padding:8px; border-radius:5px">ENCAISSER</button>` : '✅'}</div>
                        </div>
                        <div class="tx-badge">TX: ${m.transaction || 'SANS CODE'}</div>
                        <div style="font-size:10px; margin-top:5px">Retrait: ${m.retrait} F | Commission: ${m.com} F</div>
                    </div>`;
                }
            }
        });
        document.getElementById('totalCom').innerText = tot.toLocaleString();
    };

    window.valider = (k) => {
        const tx = document.getElementById('tx-'+k).value;
        if(!tx) return alert("Saisir le code SMS !");
        update(ref(db, 'missions/'+k), { etape: 2, livreur: userNow.split('@')[0].toUpperCase(), transaction: tx });
    };

    window.encaisser = (k) => update(ref(db, 'missions/'+k), { etape: 3 });

    window.shareWA = () => {
        let texte = "*BILAN CT241*\n\n";
        let total = 0;
        missionsLocales.forEach(m => {
            if(m.etape === 3) {
                texte += `✅ ${m.nom} (${m.tel}) | ${m.com}F\n`;
                total += m.com;
            }
        });
        texte += `\n*TOTAL : ${total} FCFA*`;
        window.open(`https://wa.me/?text=${encodeURIComponent(texte)}`, '_blank');
    };
</script>
</body>
</html>
