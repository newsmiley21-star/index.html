<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - GESTION SÉCURISÉE</title>
    <style>
        :root {
            --gabon-vert: #009E60; --gabon-jaune: #FCD116; --gabon-bleu: #3A75C4;
            --danger: #e74c3c; --dark: #1a1a1a; --light: #f8f9fa;
        }
        body { font-family: 'Segoe UI', system-ui, sans-serif; background: var(--light); margin: 0; padding: 10px; }
        
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
        .btn-login { width: 100%; padding: 14px; background: var(--gabon-vert); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }

        /* INTERFACE */
        #main-app { display: none; max-width: 600px; margin: auto; background: white; border-radius: 15px; padding: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); min-height: 90vh; }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        
        .role-badge { font-size: 10px; padding: 2px 8px; border-radius: 10px; background: #eee; color: #666; font-weight: bold; }

        nav { display: flex; gap: 5px; margin-bottom: 15px; background: #eee; padding: 5px; border-radius: 10px; }
        nav button { flex: 1; padding: 10px; border: none; border-radius: 7px; background: transparent; font-weight: bold; font-size: 11px; color: #666; }
        nav button.active { background: white; color: var(--gabon-vert); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }

        .section { display: none; }
        .active-sec { display: block; }

        .form-box { background: #fdfdfd; padding: 15px; border-radius: 12px; margin-bottom: 20px; border: 1px solid #eee; }
        #mComDisplay { background: #e8f5e9; font-weight: bold; padding: 10px; border-radius: 8px; text-align: center; color: var(--gabon-vert); margin-bottom: 10px; border: 1px dashed var(--gabon-vert); font-size: 12px; }

        /* CARTES */
        .card { border: 1px solid #eee; padding: 15px; border-radius: 12px; margin-bottom: 12px; border-left: 6px solid var(--gabon-bleu); position: relative; }
        .card.step2 { border-left-color: var(--gabon-jaune); }
        .card.step3 { border-left-color: var(--gabon-vert); background: #f0fff4; }
        
        .card-title { font-weight: bold; font-size: 14px; color: var(--dark); }
        .card-info { font-size: 11px; color: #666; margin: 5px 0; }
        .badge-id { position: absolute; top: 10px; right: 10px; background: #f1f2f6; color: #2f3542; padding: 2px 5px; border-radius: 4px; font-size: 9px; font-weight: bold; }

        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 8px; }
        .btn-val { background: var(--gabon-vert); color: white; }
        .btn-enc { background: var(--gabon-bleu); color: white; }

        .footer-stats { background: var(--dark); color: white; padding: 15px; border-radius: 12px; margin-top: 20px; }
        .stat-row { display: flex; justify-content: space-between; font-size: 13px; margin-bottom: 5px; }
        
        /* PHOTO PREUVE */
        .photo-container { margin: 10px 0; }
        .photo-preview { width: 100%; height: auto; max-height: 200px; object-fit: contain; border-radius: 8px; display: none; border: 2px solid var(--gabon-vert); margin-top: 5px; }
        .btn-cam { background: #eee; border: 1px solid #ddd; padding: 10px; border-radius: 8px; display: flex; align-items: center; justify-content: center; cursor: pointer; flex: 0 0 50px; }
        .status-photo { font-size: 10px; color: var(--gabon-vert); font-weight: bold; margin-top: 5px; display: none; }

        .admin-only { display: none; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666; margin-bottom: 15px;">Accès sécurisé par profil</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-vert)">CT241 OPS</h3>
                <span id="user-role" class="role-badge">Chargement...</span>
            </div>
            <button id="btnOut" style="font-size:10px; color:var(--danger); background:none; border:none; font-weight:bold; cursor:pointer">SORTIR</button>
        </header>

        <nav id="main-nav">
            <button onclick="ouvrir('saisie')" id="t-saisie" class="admin-only active">CRÉER</button>
            <button onclick="ouvrir('taches')" id="t-taches">MISSIONS</button>
            <button onclick="ouvrir('reçus')" id="t-reçus">BILAN</button>
        </nav>

        <div id="sec-saisie" class="section admin-only active-sec">
            <div class="form-box">
                <div id="mComDisplay">Saisissez un montant</div>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="Téléphone">
                <input type="text" id="mLieu" placeholder="Quartier">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)" oninput="calcAuto()">
                <input type="hidden" id="mComCalculated">
                <button onclick="lancerMission()" class="btn-action" style="background:var(--dark); color:white">CRÉER MISSION</button>
            </div>
        </div>

        <div id="sec-taches" class="section">
            <div id="list-taches"></div>
        </div>

        <div id="sec-reçus" class="section">
            <div id="list-reçus"></div>
            <div class="footer-stats">
                <div class="stat-row"><span>Missions terminées</span><b id="countDone">0</b></div>
                <div class="stat-row"><span id="label-total">Total Bilan</span><b id="totalCom" style="color: var(--gabon-jaune); font-size: 16px;">0 F</b></div>
                <button id="btn-wa-share" class="btn-action" onclick="shareWA()" style="background:#25D366; color:white">📲 PARTAGER SUR WHATSAPP</button>
            </div>
        </div>
    </div>

    <!-- ÉLÉMENTS TECHNIQUES CACHÉS -->
    <input type="file" id="photoInput" accept="image/*" capture="camera" style="display:none">
    <canvas id="compressCanvas" style="display:none"></canvas>

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
    let missions = [];
    let currentTaskKey = null;
    let tempPhotoData = "";

    window.calcAuto = () => {
        const val = parseFloat(document.getElementById('mRetrait').value) || 0;
        let c = val >= 15000 ? 390 : (val > 0 ? 190 : 0);
        document.getElementById('mComCalculated').value = c;
        document.getElementById('mComDisplay').innerHTML = `Com Direction : <b>${c} F</b>`;
    };

    document.getElementById('btnConnect').onclick = () => {
        const e = document.getElementById('login-email').value;
        const p = document.getElementById('login-pass').value;
        signInWithEmailAndPassword(auth, e, p).catch(() => alert("Accès refusé"));
    };
    document.getElementById('btnOut').onclick = () => signOut(auth);

    onAuthStateChanged(auth, (u) => {
        if(u) {
            const email = u.email.toLowerCase();
            userRole = email.includes('admin') ? 'admin' : (email.includes('finance') ? 'finance' : 'livreur');
            document.getElementById('user-role').innerText = userRole.toUpperCase();
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
            
            const adminEls = document.querySelectorAll('.admin-only');
            adminEls.forEach(el => el.style.display = (userRole === 'admin') ? 'block' : 'none');
            
            if(userRole === 'livreur') ouvrir('taches');
            ecouterMissions();
        } else {
            document.getElementById('auth-screen').style.display = 'flex';
            document.getElementById('main-app').style.display = 'none';
        }
    });

    // COMPRESSION ET CAPTURE PHOTO
    window.ouvrirCamera = (key) => {
        currentTaskKey = key;
        document.getElementById('photoInput').click();
    };

    document.getElementById('photoInput').onchange = (e) => {
        const file = e.target.files[0];
        if(!file) return;

        const reader = new FileReader();
        reader.onload = (event) => {
            const img = new Image();
            img.onload = () => {
                const canvas = document.getElementById('compressCanvas');
                const ctx = canvas.getContext('2d');
                
                // Redimensionnement intelligent (Max 800px)
                let width = img.width;
                let height = img.height;
                const maxSize = 800;
                
                if (width > height) {
                    if (width > maxSize) { height *= maxSize / width; width = maxSize; }
                } else {
                    if (height > maxSize) { width *= maxSize / height; height = maxSize; }
                }
                
                canvas.width = width;
                canvas.height = height;
                ctx.drawImage(img, 0, 0, width, height);
                
                // Compression JPG à 60%
                tempPhotoData = canvas.toDataURL('image/jpeg', 0.6);
                
                // Mise à jour interface
                const preview = document.getElementById('img-'+currentTaskKey);
                const status = document.getElementById('status-'+currentTaskKey);
                if(preview) {
                    preview.src = tempPhotoData;
                    preview.style.display = 'block';
                    status.style.display = 'block';
                    status.innerText = "✅ Image capturée et prête";
                }
            };
            img.src = event.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.lancerMission = () => {
        const nom = document.getElementById('mNom').value;
        const tel = document.getElementById('mTel').value;
        const retrait = document.getElementById('mRetrait').value;
        if(!nom || !retrait) return alert("Champs vides");

        push(ref(db, 'missions'), {
            id: "OR" + Math.floor(100 + Math.random() * 899),
            nom, tel, lieu: document.getElementById('mLieu').value || "-",
            retrait: parseFloat(retrait), com: parseFloat(document.getElementById('mComCalculated').value),
            etape: 1, livreur: "Libre", transaction: "",
            date: new Date().toLocaleDateString('fr-FR'),
            heure: new Date().toLocaleTimeString('fr-FR', {hour:'2-digit', minute:'2-digit'}),
            photo: ""
        });
        ['mNom', 'mTel', 'mLieu', 'mRetrait'].forEach(i => document.getElementById(i).value = "");
        ouvrir('taches');
    };

    function ecouterMissions() {
        onValue(ref(db, 'missions'), (s) => {
            const data = s.val();
            missions = data ? Object.keys(data).map(k => ({...data[k], key: k})) : [];
            afficherMissions();
        });
    }

    function afficherMissions() {
        const listT = document.getElementById('list-taches');
        const listR = document.getElementById('list-reçus');
        listT.innerHTML = ""; listR.innerHTML = "";
        let tot = 0, count = 0;
        const me = auth.currentUser?.email?.split('@')[0].toUpperCase() || "USER";

        missions.slice().reverse().forEach(m => {
            const estAdmin = (userRole === 'admin' || userRole === 'finance');
            const estMaMission = (m.livreur === me || m.livreur === "Libre");

            if(m.etape < 3 && (estAdmin || estMaMission)) {
                listT.innerHTML += `
                    <div class="card step${m.etape}">
                        <span class="badge-id">${m.id}</span>
                        <div class="card-title">${m.nom}</div>
                        <div class="card-info">📍 ${m.lieu} | 💰 ${m.retrait} F</div>
                        
                        <div style="display:flex; gap:5px; margin-top:10px;">
                            <a href="tel:${m.tel}" style="text-decoration:none; background:#eee; padding:8px; border-radius:5px; font-size:11px; color:var(--gabon-bleu); font-weight:bold; flex:1; text-align:center;">📞 Appeler</a>
                            ${m.etape === 1 ? `
                                <input type="text" id="tx-${m.key}" placeholder="Code SMS..." style="flex:2; margin:0; padding:5px;">
                                <button class="btn-cam" onclick="ouvrirCamera('${m.key}')">📸</button>
                            ` : ""}
                        </div>
                        
                        ${m.etape === 1 ? `
                            <div class="photo-container">
                                <div id="status-${m.key}" class="status-photo"></div>
                                <img id="img-${m.key}" class="photo-preview">
                            </div>
                            <button class="btn-action btn-val" onclick="valider('${m.key}')">VALIDER LA LIVRAISON</button>
                        ` : ""}
                        
                        ${m.etape === 2 && estAdmin ? `
                            ${m.photo ? `<button onclick="window.open().document.write('<img src=\\'${m.photo}\\'>')" style="width:100%; border:1px solid var(--gabon-vert); background:none; color:var(--gabon-vert); padding:5px; margin:5px 0; border-radius:5px; font-size:10px; cursor:pointer;">🖼️ Voir la preuve image</button>` : ""}
                            <button class="btn-action btn-enc" onclick="encaisser('${m.key}')">ENCAISSER (${m.com}F)</button>
                        ` : ""}
                    </div>
                `;
            }

            if(m.etape === 3 && (estAdmin || m.livreur === me)) {
                listR.innerHTML += `
                    <div class="card step3">
                        <div class="card-title" style="font-size:12px;">${m.nom} <small>(${m.date})</small></div>
                        <div style="font-size:10px; color:#666">Code: ${m.transaction} | Livreur: ${m.livreur}</div>
                    </div>
                `;
                tot += (userRole === 'livreur') ? (m.retrait >= 15000 ? 800 : 400) : m.com;
                count++;
            }
        });

        document.getElementById('totalCom').innerText = tot.toLocaleString() + " F";
        document.getElementById('countDone').innerText = count;
    }

    window.valider = (k) => {
        const tx = document.getElementById('tx-'+k).value;
        if(!tx) return alert("Code SMS requis");
        if(!tempPhotoData) return alert("📸 Prenez d'abord une photo de preuve !");
        
        const me = auth.currentUser.email.split('@')[0].toUpperCase();
        update(ref(db, 'missions/'+k), { 
            etape: 2, livreur: me, transaction: tx, photo: tempPhotoData 
        }).then(() => {
            tempPhotoData = "";
        }).catch(err => alert("Erreur d'envoi. Essayez une autre photo."));
    };

    window.encaisser = (k) => update(ref(db, 'missions/'+k), { etape: 3 });

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('t-'+id).classList.add('active');
    };

    window.shareWA = () => {
        const me = auth.currentUser?.email?.split('@')[0].toUpperCase() || "USER";
        let txt = `*BILAN CT241 - ${me}*\nMissions : ${document.getElementById('countDone').innerText}\nTotal : ${document.getElementById('totalCom').innerText}`;
        window.open(`https://wa.me/?text=${encodeURIComponent(txt)}`);
    };
</script>
</body>
</html>
