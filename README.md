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
            background: linear-gradient(135deg, var(--gabon-vert), var(--gabon-bleu));
            display: flex; align-items: center; justify-content: center; z-index: 1000;
        }
        
        .auth-card {
            background: var(--white); padding: 30px; border-radius: 20px;
            width: 90%; max-width: 400px; box-shadow: 0 10px 25px rgba(0,0,0,0.2);
            text-align: center;
        }

        .logo-placeholder {
            width: 80px; height: 80px; background: var(--gray-bg);
            border-radius: 50%; margin: 0 auto 20px; display: flex;
            align-items: center; justify-content: center; font-weight: bold;
            border: 3px solid var(--gabon-jaune); color: var(--gabon-vert);
        }

        input {
            width: 100%; padding: 12px; margin: 10px 0; border: 1px solid #ddd;
            border-radius: 8px; box-sizing: border-box; font-size: 16px;
        }

        button {
            width: 100%; padding: 12px; border: none; border-radius: 8px;
            font-weight: bold; cursor: pointer; transition: 0.3s; font-size: 16px;
        }

        .btn-primary { background: var(--gabon-vert); color: white; }
        .btn-primary:hover { opacity: 0.9; }

        header {
            background: var(--white); padding: 15px; border-radius: 15px;
            display: flex; justify-content: space-between; align-items: center;
            margin-bottom: 15px; box-shadow: 0 2px 10px rgba(0,0,0,0.05);
        }

        .user-info { font-size: 14px; font-weight: 500; }
        .logout-btn { background: #eee; padding: 5px 10px; border-radius: 5px; font-size: 12px; width: auto; }

        .mission-card {
            background: var(--white); border-radius: 15px; padding: 15px;
            margin-bottom: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            border-left: 5px solid var(--gabon-jaune); position: relative;
        }

        .mission-header { display: flex; justify-content: space-between; margin-bottom: 10px; }
        .client-name { font-weight: 800; font-size: 18px; color: var(--dark); }
        .status-badge { 
            padding: 4px 8px; border-radius: 6px; font-size: 11px; font-weight: bold; text-transform: uppercase;
        }

        .details-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; font-size: 13px; margin: 10px 0; }
        .label { color: #888; display: block; font-size: 11px; margin-bottom: 2px; }

        .btn-group { display: flex; gap: 8px; margin-top: 15px; }
        .btn-action { flex: 1; font-size: 13px; padding: 10px; border-radius: 8px; }
        .btn-call { background: var(--gabon-bleu); color: white; text-decoration: none; text-align: center; line-height: 20px; }
        .btn-cam { background: var(--gabon-jaune); color: var(--dark); }
        .btn-done { background: var(--gabon-vert); color: white; }
        .btn-archive { background: var(--dark); color: white; }

        #loading {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(255,255,255,0.8); display: none; align-items: center;
            justify-content: center; z-index: 2000; font-weight: bold;
        }

        .admin-panel {
            background: var(--dark); color: white; padding: 15px;
            border-radius: 15px; margin-bottom: 20px;
        }
    </style>
</head>
<body>

    <div id="loading">TRAITEMENT EN COURS...</div>

    <div id="auth-screen">
        <div class="auth-card">
            <div class="logo-placeholder">CT241</div>
            <h2 style="margin-bottom: 20px;">Logistique Gabon</h2>
            <input type="email" id="email" placeholder="Email professionnel">
            <input type="password" id="password" placeholder="Mot de passe">
            <button class="btn-primary" onclick="login()">Se connecter</button>
        </div>
    </div>

    <div id="main-screen" style="display: none;">
        <header>
            <div class="user-info">
                <div id="user-display">Utilisateur</div>
                <div id="role-display" style="font-size: 10px; color: var(--gabon-vert);">Chargement...</div>
            </div>
            <button class="logout-btn" onclick="logout()">Quitter</button>
        </header>

        <div id="admin-controls" class="admin-panel" style="display: none;">
            <h4 style="margin: 0 0 10px 0;">Nouvelle Mission</h4>
            <input type="text" id="new-client" placeholder="Nom du Client">
            <input type="text" id="new-tel" placeholder="Téléphone">
            <input type="text" id="new-quartier" placeholder="Quartier (ex: Akanda)">
            <input type="number" id="new-montant" placeholder="Montant CFA">
            <input type="text" id="new-livreur" placeholder="Email du Livreur">
            <button class="btn-primary" onclick="creerMission()" style="background: var(--gabon-jaune); color: var(--dark);">Lancer la course</button>
        </div>

        <div id="missions-container"></div>
    </div>

    <input type="file" id="camInput" accept="image/*" capture="environment" style="display: none;">
    <canvas id="canvas" style="display: none;"></canvas>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-auth.js";
        import { getDatabase, ref, push, onValue, update, remove } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAs_XXXXXXXXXXXX",
            authDomain: "ct241-logistique.firebaseapp.com",
            databaseURL: "https://ct241-logistique-default-rtdb.firebaseio.com",
            projectId: "ct241-logistique",
            storageBucket: "ct241-logistique.appspot.com",
            messagingSenderId: "XXXXXXXX",
            appId: "1:XXXXXXX:web:XXXXXX"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getDatabase(app);

        let userRole = "livreur";
        let allMissions = [];
        let currentKey = "";
        let lastPhotoData = "";

        window.login = () => {
            const e = document.getElementById('email').value;
            const p = document.getElementById('password').value;
            signInWithEmailAndPassword(auth, e, p).catch(err => alert("Erreur: " + err.message));
        };

        window.logout = () => signOut(auth);

        onAuthStateChanged(auth, (user) => {
            if (user) {
                document.getElementById('auth-screen').style.display = 'none';
                document.getElementById('main-screen').style.display = 'block';
                document.getElementById('user-display').innerText = user.email;
                
                if(user.email.includes('admin') || user.email.includes('finance')) {
                    userRole = "admin";
                    document.getElementById('admin-controls').style.display = 'block';
                    document.getElementById('role-display').innerText = "GESTIONNAIRE";
                } else {
                    userRole = "livreur";
                    document.getElementById('role-display').innerText = "LIVREUR SUR LE TERRAIN";
                }

                // ECOUTE UNIQUE SUR MISSIONS (Identique à votre original)
                onValue(ref(db, 'missions'), (snap) => {
                    const data = snap.val();
                    allMissions = data ? Object.keys(data).map(k => ({...data[k], key:k})) : [];
                    renderUI();
                });
            } else {
                document.getElementById('auth-screen').style.display = 'flex';
                document.getElementById('main-screen').style.display = 'none';
            }
        });

        window.creerMission = () => {
            const client = document.getElementById('new-client').value;
            const tel = document.getElementById('new-tel').value;
            const quartier = document.getElementById('new-quartier').value;
            const montant = document.getElementById('new-montant').value;
            const livreurEmail = document.getElementById('new-livreur').value;
            if(!client || !livreurEmail) return alert("Nom client et Livreur requis");

            push(ref(db, 'missions'), {
                client, tel, quartier, montant,
                livreur: livreurEmail,
                etape: 0,
                timestamp: Date.now()
            });
            document.getElementById('new-client').value = '';
            document.getElementById('new-tel').value = '';
            document.getElementById('new-montant').value = '';
        };

        window.recuperer = (key) => update(ref(db, `missions/${key}`), { etape: 1 });

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
                    // OPTIMISATION IDENTIQUE
                    canvas.width = 400; canvas.height = (img.height/img.width)*400;
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    lastPhotoData = canvas.toDataURL('image/jpeg', 0.3);
                    alert("Photo capturée !");
                };
                img.src = re.target.result;
            };
            reader.readAsDataURL(file);
        };

        window.terminer = (key) => {
            const code = document.getElementById(`code-${key}`).value;
            if(!code || !lastPhotoData) { alert("Photo + Code requis"); return; }
            toggleLoading(true);
            update(ref(db, `missions/${key}`), { 
                codeSMS: code, 
                photo: lastPhotoData, 
                etape: 2 
            }).then(() => {
                lastPhotoData = "";
                toggleLoading(false);
            });
        };

        // LA SEULE CORRECTION APPLIQUÉE (ARCHIVAGE SÉCURISÉ DANS LE MÊME CODE)
        window.cloturer = async (key) => {
            if(!confirm("Confirmer l'encaissement et l'archivage ?")) return;
            toggleLoading(true);
            try {
                const m = allMissions.find(x => x.key === key);
                if(m) {
                    // 1. On pousse dans archives
                    await push(ref(db, 'archives'), { ...m, etape: 3, dateFin: new Date().toLocaleString() });
                    // 2. On supprime de missions
                    await remove(ref(db, `missions/${key}`));
                    alert("Mission archivée.");
                }
            } catch (e) { alert("Erreur d'archivage"); }
            toggleLoading(false);
        };

        function renderUI() {
            const container = document.getElementById('missions-container');
            container.innerHTML = '';
            
            const filtered = userRole === "admin" ? allMissions : allMissions.filter(m => m.livreur === auth.currentUser.email);

            filtered.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
                const card = document.createElement('div');
                card.className = "mission-card";
                
                let statusText = "Attente";
                let statusColor = "#eee";
                if(m.etape === 1) { statusText = "En route"; statusColor = varColor('--gabon-jaune'); }
                if(m.etape === 2) { statusText = "Livré"; statusColor = varColor('--gabon-vert'); }

                card.innerHTML = `
                    <div class="mission-header">
                        <span class="client-name">${m.client}</span>
                        <span class="status-badge" style="background:${statusColor}">${statusText}</span>
                    </div>
                    <div class="details-grid">
                        <div><span class="label">QUARTIER</span>${m.quartier || '-'}</div>
                        <div><span class="label">MONTANT</span>${m.montant || '0'} CFA</div>
                        <div><span class="label">TÉLÉPHONE</span>${m.tel || '-'}</div>
                        <div><span class="label">LIVREUR</span>${m.livreur.split('@')[0]}</div>
                    </div>
                    ${renderButtons(m)}
                `;
                container.appendChild(card);
            });
        }

        function renderButtons(m) {
            if(m.etape === 0) {
                return `<div class="btn-group">
                    <a href="tel:${m.tel}" class="btn-action btn-call">Appeler</a>
                    <button class="btn-action btn-done" onclick="recuperer('${m.key}')">Récupérer</button>
                </div>`;
            }
            if(m.etape === 1) {
                return `
                    <div style="background:#f9f9f9; padding:10px; border-radius:8px;">
                        <input type="text" id="code-${m.key}" placeholder="Code SMS">
                        <div class="btn-group">
                            <button class="btn-action btn-cam" onclick="triggerCam('${m.key}')">📷 Photo</button>
                            <button class="btn-action btn-done" onclick="terminer('${m.key}')">Valider</button>
                        </div>
                    </div>`;
            }
            if(m.etape === 2 && userRole === "admin") {
                return `<button class="btn-action btn-archive" style="width:100%; margin-top:10px;" onclick="cloturer('${m.key}')">Confirmer Encaissement & Archiver</button>`;
            }
            return `<div style="font-size:11px; color:#888; text-align:center;">Attente encaissement...</div>`;
        }

        function toggleLoading(s) { document.getElementById('loading').style.display = s ? 'flex' : 'none'; }
        function varColor(name) { return getComputedStyle(document.documentElement).getPropertyValue(name).trim(); }
    </script>
</body>
</html>
