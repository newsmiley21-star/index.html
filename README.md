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
            display: flex; align-items: center; justify-content: center; z-index: 10000;
        }

        .auth-card {
            background: white; padding: 30px; border-radius: 25px; width: 90%; max-width: 350px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.2); text-align: center;
        }

        .auth-card h2 { margin-bottom: 20px; color: var(--dark); }
        .auth-card input {
            width: 100%; padding: 12px; margin: 10px 0; border: 1px solid #ddd;
            border-radius: 10px; box-sizing: border-box;
        }

        nav {
            position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
            background: rgba(255,255,255,0.9); backdrop-filter: blur(10px);
            padding: 8px; border-radius: 20px; display: flex; gap: 5px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1); z-index: 1000; width: 90%; max-width: 400px;
        }

        nav button {
            flex: 1; border: none; background: transparent; padding: 12px 5px;
            border-radius: 15px; font-weight: 600; font-size: 12px; color: #666;
            transition: 0.3s;
        }

        nav button.active { background: var(--dark); color: white; }

        .section { display: none; margin-bottom: 100px; animation: fadeIn 0.3s ease; }
        .active-sec { display: block; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding: 10px 5px; }
        .logo-area h1 { font-size: 20px; margin: 0; font-weight: 800; letter-spacing: -1px; }
        .logo-area span { font-size: 10px; color: var(--gabon-vert); font-weight: 700; text-transform: uppercase; }

        .card { 
            background: white; border-radius: 20px; padding: 15px; margin-bottom: 15px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.03); border: 1px solid rgba(0,0,0,0.05);
        }

        .btn-action {
            width: 100%; padding: 15px; border: none; border-radius: 15px;
            font-weight: 700; font-size: 14px; cursor: pointer; transition: 0.2s;
        }

        .btn-primary { background: var(--dark); color: white; }
        .btn-validate { background: var(--gabon-vert); color: white; margin-top: 10px; }

        .mission-item {
            background: white; border-radius: 20px; padding: 15px; margin-bottom: 12px;
            display: flex; justify-content: space-between; align-items: center;
            border-left: 5px solid var(--gabon-jaune);
        }

        .m-info h3 { margin: 0; font-size: 15px; }
        .m-info p { margin: 3px 0 0; font-size: 12px; color: #777; }
        .m-status { text-align: right; }
        .badge { font-size: 10px; padding: 4px 8px; border-radius: 8px; font-weight: 700; }
        .badge-wait { background: #fff3cd; color: #856404; }
        .badge-process { background: #cce5ff; color: #004085; }
        .badge-done { background: #d4edda; color: #155724; }

        input, select {
            width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #eee;
            border-radius: 12px; background: #fafafa; font-size: 14px;
        }

        .modal {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.6); display: none; align-items: center;
            justify-content: center; z-index: 2000; backdrop-filter: blur(5px);
        }

        .modal-content {
            background: white; width: 90%; max-width: 400px; border-radius: 30px;
            padding: 25px; position: relative; max-height: 80vh; overflow-y: auto;
        }

        .detail-row { display: flex; justify-content: space-between; margin: 10px 0; font-size: 14px; border-bottom: 1px solid #f0f0f0; padding-bottom: 8px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="auth-card">
            <div style="font-size: 40px; margin-bottom: 10px;">🇬🇦</div>
            <h2>CT241 LOGISTIQUE</h2>
            <p style="font-size: 12px; color: #666; margin-bottom: 20px;">Connectez-vous pour accéder au portail</p>
            <input type="password" id="auth-code" placeholder="Code d'accès">
            <button class="btn-action btn-primary" onclick="verifierAuth()">ENTRER</button>
        </div>
    </div>

    <div id="app" style="display:none;">
        <header>
            <div class="logo-area">
                <span>Libreville • Gabon</span>
                <h1>CT241 EXPRESS</h1>
            </div>
            <div style="text-align: right">
                <div id="user-tag" style="font-size: 10px; font-weight: 800; background: #eee; padding: 4px 8px; border-radius: 10px;">LIVREUR</div>
            </div>
        </header>

        <nav>
            <button id="nav-missions" onclick="ouvrir('missions')" class="active">Missions</button>
            <button id="nav-archives" onclick="ouvrir('archives')">Archives</button>
            <button id="nav-compta" onclick="ouvrir('compta')">Admin</button>
        </nav>

        <div id="sec-missions" class="section active-sec">
            <div id="admin-add" style="display:none;">
                <div class="card">
                    <h4 style="margin:0 0 10px">Nouvelle Mission</h4>
                    <input type="text" id="m-nom" placeholder="Nom du bénéficiaire">
                    <input type="number" id="m-retrait" placeholder="Montant à retirer (F)">
                    <button class="btn-action btn-primary" onclick="ajouterMission()">CRÉER LA MISSION</button>
                </div>
            </div>
            <div id="list-missions"></div>
        </div>

        <div id="sec-archives" class="section">
            <h3 style="padding-left:10px">Historique</h3>
            <div id="list-archives"></div>
        </div>

        <div id="sec-compta" class="section">
            <div class="card" style="background: var(--dark); color: white;">
                <p style="opacity: 0.7; font-size: 12px; margin: 0;">Total Commissions (F)</p>
                <h1 id="stats-com" style="margin: 5px 0 0; font-size: 32px;">0 F</h1>
            </div>
            <div id="list-compta"></div>
        </div>
    </div>

    <div id="modal-overlay" class="modal" onclick="this.style.display='none'">
        <div class="modal-content" onclick="event.stopPropagation()">
            <h3 style="margin-top:0">Détails Mission</h3>
            <div id="modal-body"></div>
            <button class="btn-action" style="margin-top:20px; background:#f0f0f0" onclick="document.getElementById('modal-overlay').style.display='none'">FERMER</button>
        </div>
    </div>

    <input type="file" id="camInput" accept="image/*" capture="camera" style="display:none">
    <canvas id="canvas" style="display:none"></canvas>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
        // AJOUT DES FONCTIONS DE REQUÊTE UNIQUEMENT
        import { getDatabase, ref, push, onValue, update, remove, query, orderByChild, endAt, get, equalTo } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-database.js";

        const firebaseConfig = {
            databaseURL: "https://votre-projet.firebaseio.com"
        };

        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        let allMissions = [];
        let userRole = 'livreur';
        let currentKey = "";
        let lastPhotoData = "";

        window.verifierAuth = () => {
            const code = document.getElementById('auth-code').value;
            if(code === "241") { 
                userRole = 'admin'; 
                document.getElementById('admin-add').style.display = 'block';
                document.getElementById('user-tag').innerText = 'ADMIN';
                entrer();
            } else if(code === "000") {
                userRole = 'livreur';
                entrer();
            } else { alert("Code incorrect"); }
        };

        function entrer() {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').style.display = 'block';
        }

        // --- FILTRAGE POUR RÉDUIRE LA CONSOMMATION ---
        const missionsActivesQuery = query(ref(db, 'missions'), orderByChild('etape'), endAt(2));

        onValue(missionsActivesQuery, (snap) => {
            const data = snap.val();
            const actives = data ? Object.keys(data).map(k => ({...data[k], key:k})) : [];
            const archivesExistantes = allMissions.filter(m => m.etape === 3);
            allMissions = [...actives, ...archivesExistantes];
            renderUI();
        });

        // --- CHARGEMENT ARCHIVES À LA DEMANDE ---
        window.chargerArchives = async () => {
            if (allMissions.some(m => m.etape === 3)) return;
            const snap = await get(query(ref(db, 'missions'), orderByChild('etape'), equalTo(3)));
            if(snap.exists()){
                const data = snap.val();
                const archives = Object.keys(data).map(k => ({...data[k], key:k}));
                allMissions = [...allMissions.filter(m => m.etape < 3), ...archives];
                renderUI();
            }
        };

        window.ouvrir = (sec) => {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
            document.getElementById(`sec-${sec}`).classList.add('active-sec');
            document.getElementById(`nav-${sec}`).classList.add('active');
            if(sec === 'archives' || sec === 'compta') window.chargerArchives();
        };

        window.ajouterMission = () => {
            const nom = document.getElementById('m-nom').value;
            const retrait = parseInt(document.getElementById('m-retrait').value);
            if(!nom || !retrait) return;
            const m = {
                id: Math.floor(1000 + Math.random() * 9000),
                nom, retrait,
                com: retrait * 0.05,
                etape: 0,
                dateHeure: new Date().toLocaleString(),
                timestamp: Date.now()
            };
            push(ref(db, 'missions'), m);
            document.getElementById('m-nom').value = "";
            document.getElementById('m-retrait').value = "";
        };

        function renderUI() {
            const listMissions = document.getElementById('list-missions');
            const listArchives = document.getElementById('list-archives');
            const listCompta = document.getElementById('list-compta');
            
            listMissions.innerHTML = "";
            listArchives.innerHTML = "";
            listCompta.innerHTML = "";
            
            let totalCom = 0;

            allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
                const badge = m.etape === 0 ? 'badge-wait' : (m.etape === 1 ? 'badge-process' : 'badge-done');
                const label = m.etape === 0 ? 'EN ATTENTE' : (m.etape === 1 ? 'EN COURS' : 'TERMINÉ');

                const html = `
                    <div class="mission-item">
                        <div class="m-info">
                            <h3>${m.nom}</h3>
                            <p>${m.retrait.toLocaleString()} F • <span onclick="consulterMission('${m.key}')" style="text-decoration:underline; cursor:pointer">Détails</span></p>
                        </div>
                        <div class="m-status">
                            ${m.etape === 0 ? `<button class="badge badge-wait" onclick="prendre('${m.key}')">PRENDRE</button>` : `<span class="badge ${badge}">${label}</span>`}
                            ${m.etape === 1 ? `<button class="badge badge-process" onclick="triggerCam('${m.key}')">FINIR</button>` : ''}
                        </div>
                    </div>
                `;

                if(m.etape < 3) listMissions.innerHTML += html;
                else listArchives.innerHTML += html;

                if(m.etape >= 2) {
                    totalCom += m.com;
                    listCompta.innerHTML += `<div class="detail-row"><span>${m.nom}</span><b>${m.com} F</b></div>`;
                }
            });
            document.getElementById('stats-com').innerText = totalCom.toLocaleString() + " F";
        }

        window.prendre = (key) => update(ref(db, `missions/${key}`), { etape: 1 });
        window.triggerCam = (key) => { currentKey = key; document.getElementById('camInput').click(); };

        document.getElementById('camInput').onchange = (e) => {
            const file = e.target.files[0];
            const reader = new FileReader();
            reader.onload = (re) => {
                const img = new Image();
                img.onload = () => {
                    const canvas = document.getElementById('canvas');
                    const ctx = canvas.getContext('2d');
                    canvas.width = 600; canvas.height = (img.height/img.width)*600;
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    lastPhotoData = canvas.toDataURL('image/jpeg', 0.6);
                    const code = prompt("Code SMS ?");
                    if(code) update(ref(db, `missions/${currentKey}`), { codeSMS: code, photo: lastPhotoData, etape: 3 });
                };
                img.src = re.target.result;
            };
            reader.readAsDataURL(file);
        };

        window.consulterMission = (key) => {
            const m = allMissions.find(x => x.key === key);
            if(!m) return;
            document.getElementById('modal-body').innerHTML = `
                <div class="detail-row"><b>Nom</b> <span>${m.nom}</span></div>
                <div class="detail-row"><b>Retrait</b> <span>${m.retrait} F</span></div>
                <div class="detail-row"><b>Code</b> <span>${m.codeSMS || '---'}</span></div>
                ${m.photo ? `<img src="${m.photo}" style="width:100%; border-radius:15px; margin-top:10px">` : ''}
            `;
            document.getElementById('modal-overlay').style.display = 'flex';
        };
    </script>
</body>
</html>
