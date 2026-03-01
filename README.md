 <!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - GESTION OPÉRATIONNELLE</title>
    <style>
        :root {
            --gabon-vert: #009E60; --gabon-jaune: #FCD116; --gabon-bleu: #3A75C4;
            --danger: #e74c3c; --dark: #1a1a1a; --light: #f8f9fa;
        }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: var(--light); margin: 0; padding: 10px; }
        
        /* AUTHENTIFICATION */
        #auth-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #1e272e; display: flex; align-items: center; justify-content: center; z-index: 9999;
        }
        .login-card {
            background: white; padding: 25px; border-radius: 20px; width: 85%; max-width: 320px;
            text-align: center; border-top: 8px solid var(--gabon-jaune); box-shadow: 0 10px 25px rgba(0,0,0,0.5);
        }
        input { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; font-size: 14px; outline: none; }
        input:focus { border-color: var(--gabon-bleu); }
        .btn-login { width: 100%; padding: 14px; background: var(--gabon-vert); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; }

        /* APP PRINCIPALE */
        #main-app { display: none; max-width: 600px; margin: auto; background: white; border-radius: 15px; padding: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); min-height: 90vh; }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        
        nav { display: flex; gap: 5px; margin-bottom: 15px; background: #eee; padding: 5px; border-radius: 10px; }
        nav button { flex: 1; padding: 10px; border: none; border-radius: 7px; background: transparent; font-weight: bold; font-size: 11px; color: #666; transition: 0.3s; }
        nav button.active { background: white; color: var(--gabon-vert); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }

        .form-box { background: #fdfdfd; padding: 15px; border-radius: 12px; margin-bottom: 20px; border: 1px solid #eee; }
        #mComDisplay { background: #e8f5e9; font-weight: bold; padding: 10px; border-radius: 8px; text-align: center; color: var(--gabon-vert); margin-bottom: 10px; border: 1px dashed var(--gabon-vert); font-size: 13px; }
        
        /* LISTES & CARTES */
        .section { display: none; animation: fadeIn 0.3s ease; }
        .active-sec { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .card { border: 1px solid #eee; padding: 15px; border-radius: 12px; margin-bottom: 12px; border-left: 6px solid var(--gabon-bleu); position: relative; }
        .card.step2 { border-left-color: var(--gabon-jaune); background: #fffdf0; }
        .card.step3 { border-left-color: var(--gabon-vert); background: #f0fff4; opacity: 0.8; }
        
        .card-title { font-weight: 800; font-size: 15px; margin-bottom: 5px; color: var(--dark); }
        .card-info { font-size: 12px; color: #636e72; margin-bottom: 10px; }
        .tel-link { color: var(--gabon-bleu); text-decoration: none; font-weight: bold; border: 1px solid var(--gabon-bleu); padding: 2px 8px; border-radius: 5px; font-size: 11px; }

        .badge-id { position: absolute; top: 10px; right: 10px; background: var(--dark); color: var(--gabon-jaune); font-family: monospace; padding: 2px 6px; border-radius: 4px; font-size: 10px; }
        
        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; transition: 0.2s; }
        .btn-val { background: var(--gabon-vert); color: white; }
        .btn-enc { background: var(--gabon-bleu); color: white; }
        
        .search-bar { width: 100%; padding: 12px; border-radius: 10px; border: 2px solid #eee; margin-bottom: 15px; box-sizing: border-box; }

        .footer-total { background: var(--dark); color: white; padding: 15px; border-radius: 12px; margin-top: 20px; }
        .total-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666;">Gestion des livraisons de proximité</p>
            <input type="email" id="login-email" placeholder="Email professionnel">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <h3 style="margin:0; color:var(--gabon-vert)">CT241 <span style="color:var(--dark)">GABON</span></h3>
            <button id="btnOut" style="font-size:10px; color:var(--danger); background:none; border:none; font-weight:bold; cursor:pointer">DÉCONNEXION</button>
        </header>

        <nav id="nav-bar">
            <button onclick="ouvrir('saisie')" id="t-saisie" class="active">CRÉER</button>
            <button onclick="ouvrir('taches')" id="t-taches">MISSIONS</button>
            <button onclick="ouvrir('reçus')" id="t-reçus">HISTORIQUE</button>
        </nav>

        <!-- ONGLET 1 : SAISIE (Réservé admin) -->
        <div id="sec-saisie" class="section active-sec">
            <div class="form-box">
                <div id="mComDisplay">Saisissez un montant pour voir la com</div>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="Téléphone (ex: 077...)">
                <input type="text" id="mLieu" placeholder="Quartier / Précisions">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)" oninput="calcAuto()">
                <input type="hidden" id="mComCalculated">
                <button onclick="lancerMission()" class="btn-action btn-enc" style="background:var(--dark)">CRÉER LA MISSION</button>
            </div>
            <div id="recent-saisie" style="font-size: 11px; color: #999; text-align: center;">Dernières saisies affichées dans l'historique</div>
        </div>

        <!-- ONGLET 2 : MISSIONS EN COURS -->
        <div id="sec-taches" class="section">
            <div id="list-taches"></div>
        </div>

        <!-- ONGLET 3 : HISTORIQUE -->
        <div id="sec-reçus" class="section">
            <input type="text" id="searchInput" class="search-bar" placeholder="🔍 Rechercher client ou n°..." oninput="afficherTout()">
            <div id="list-reçus"></div>
            
            <div class="footer-total">
                <div class="total-row">
                    <span>Missions clôturées</span>
                    <b id="countDone">0</b>
                </div>
                <div class="total-row">
                    <span>Commissions Direction</span>
                    <b id="totalCom" style="font-size: 18px; color: var(--gabon-jaune);">0 FCFA</b>
                </div>
                <button class="btn-action" onclick="shareWA()" style="background:#25D366; color:white">📲 PARTAGER LE BILAN WHATSAPP</button>
            </div>
        </div>
    </div>

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
    let currentUser = null;
    let missions = [];

    // CALCUL SELON LE MODÈLE DOUBLE MARGE (390 F)
    window.calcAuto = () => {
        const val = parseFloat(document.getElementById('mRetrait').value) || 0;
        const display = document.getElementById('mComDisplay');
        const hidden = document.getElementById('mComCalculated');
        
        let comDirection = 0;
        if (val >= 15000) {
            comDirection = 390; // Modèle Gain Client (190) + Redevance Livreur (200)
            display.innerHTML = `Com Direction: <b>${comDirection} F</b> | Livreur Net: <b>800 F</b>`;
        } else if (val > 0) {
            comDirection = 190;
            display.innerHTML = `Com Direction: <b>${comDirection} F</b> | Livreur Net: <b>400 F</b>`;
        } else {
            display.innerText = "Saisissez un montant...";
        }
        hidden.value = comDirection;
    };

    // AUTH
    document.getElementById('btnConnect').onclick = () => {
        const e = document.getElementById('login-email').value;
        const p = document.getElementById('login-pass').value;
        signInWithEmailAndPassword(auth, e, p).catch(err => alert("Erreur: " + err.message));
    };
    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        if(u) {
            currentUser = u;
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            
            // Si c'est un livreur, on cache l'onglet Saisie
            if(u.email.toLowerCase().includes('livreur')) {
                document.getElementById('t-saisie').style.display = 'none';
                ouvrir('taches');
            }
            ecouterMissions();
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

    // CRUD MISSIONS
    window.lancerMission = () => {
        const nom = document.getElementById('mNom').value;
        const tel = document.getElementById('mTel').value;
        const retrait = document.getElementById('mRetrait').value;
        const com = document.getElementById('mComCalculated').value;

        if(!nom || !retrait || !tel) return alert("Veuillez remplir les champs obligatoires");

        const idMission = "CT-" + Math.floor(1000 + Math.random() * 9000);

        push(ref(db, 'missions'), {
            id: idMission,
            nom: nom,
            tel: tel,
            lieu: document.getElementById('mLieu').value || "-",
            retrait: parseFloat(retrait),
            com: parseFloat(com),
            etape: 1, // 1: Nouveau, 2: Validé Livreur, 3: Encaissé Direction
            livreur: "En attente",
            transaction: "",
            date: new Date().toLocaleDateString('fr-FR'),
            heure: new Date().toLocaleTimeString('fr-FR', {hour:'2-digit', minute:'2-digit'})
        });

        ['mNom', 'mTel', 'mLieu', 'mRetrait'].forEach(i => document.getElementById(i).value = "");
        document.getElementById('mComDisplay').innerText = "Mission créée avec succès !";
        setTimeout(() => ouvrir('taches'), 800);
    };

    function ecouterMissions() {
        onValue(ref(db, 'missions'), (snapshot) => {
            const data = snapshot.val();
            missions = [];
            if(data) {
                Object.keys(data).forEach(key => missions.push({...data[key], key: key}));
            }
            afficherTout();
        });
    }

    window.afficherTout = () => {
        const query = document.getElementById('searchInput').value.toLowerCase();
        const listTaches = document.getElementById('list-taches');
        const listRecus = document.getElementById('list-reçus');
        
        listTaches.innerHTML = "";
        listRecus.innerHTML = "";
        
        let totalCom = 0;
        let countDone = 0;

        missions.slice().reverse().forEach(m => {
            const isLivreur = currentUser.email.toLowerCase().includes('livreur');
            const userName = currentUser.email.split('@')[0].toUpperCase();

            // Filtrage : Les livreurs ne voient que les missions "Etape 1" ou les leurs
            if(isLivreur && m.etape > 1 && m.livreur !== userName) return;

            const cardHTML = `
                <div class="card step${m.etape}">
                    <span class="badge-id">${m.id}</span>
                    <div class="card-title">${m.nom}</div>
                    <div class="card-info">
                        📍 ${m.lieu}<br>
                        💰 Retrait: <b>${m.retrait.toLocaleString()} F</b><br>
                        📅 ${m.date} à ${m.heure}
                    </div>
                    <a href="tel:${m.tel}" class="tel-link">📞 Appeler ${m.tel}</a>
                    
                    ${m.etape === 1 ? `
                        <div style="margin-top:15px;">
                            <input type="text" id="tx-${m.key}" placeholder="Code Transaction SMS..." style="margin-bottom:5px">
                            <button class="btn-action btn-val" onclick="validerLivreur('${m.key}')">VALIDER LA LIVRAISON</button>
                        </div>
                    ` : ""}

                    ${m.etape === 2 && !isLivreur ? `
                        <div style="margin-top:10px;">
                            <div style="font-size:10px; color:var(--gabon-bleu)">Code SMS: <b>${m.transaction}</b> | Livreur: <b>${m.livreur}</b></div>
                            <button class="btn-action btn-enc" onclick="encaisserDirection('${m.key}')">CONFIRMER L'ENCAISSEMENT</button>
                        </div>
                    ` : ""}

                    ${m.etape === 3 ? `
                        <div style="font-size:10px; color:var(--gabon-vert); font-weight:bold; margin-top:5px;">✅ CLÔTURÉ (TX: ${m.transaction})</div>
                    ` : ""}
                </div>
            `;

            // Répartition dans les onglets
            if(m.etape < 3) {
                listTaches.innerHTML += cardHTML;
            } else {
                if((m.nom + m.tel + m.id).toLowerCase().includes(query)) {
                    listRecus.innerHTML += cardHTML;
                    totalCom += m.com;
                    countDone++;
                }
            }
        });

        document.getElementById('totalCom').innerText = totalCom.toLocaleString() + " FCFA";
        document.getElementById('countDone').innerText = countDone;
    };

    window.validerLivreur = (key) => {
        const tx = document.getElementById('tx-'+key).value;
        if(!tx) return alert("Le code de transaction est obligatoire pour valider.");
        const name = currentUser.email.split('@')[0].toUpperCase();
        update(ref(db, 'missions/' + key), {
            etape: 2,
            livreur: name,
            transaction: tx
        });
    };

    window.encaisserDirection = (key) => {
        update(ref(db, 'missions/' + key), { etape: 3 });
    };

    window.shareWA = () => {
        let txt = "*BILAN CT241 GABON*\n------------------\n";
        let total = 0;
        missions.forEach(m => {
            if(m.etape === 3) {
                txt += `✅ ${m.nom} | ${m.com}F (via ${m.livreur})\n`;
                total += m.com;
            }
        });
        txt += `\n*TOTAL DIRECTION : ${total} FCFA*`;
        window.open(`https://wa.me/?text=${encodeURIComponent(txt)}`, '_blank');
    };

</script>
</body>
</html>
