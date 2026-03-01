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
        .card.step-1 { border-left-color: var(--gabon-bleu); }
        .card.step-2 { border-left-color: var(--gabon-jaune); background: #fffdf5; }
        
        .card-title { font-weight: bold; font-size: 14px; margin-bottom: 5px; color: var(--dark); }
        .card-info { font-size: 12px; color: #555; line-height: 1.4; }
        .badge-id { position: absolute; top: 10px; right: 10px; background: #f1f2f6; padding: 2px 6px; border-radius: 4px; font-size: 10px; font-weight: bold; }

        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; font-size: 13px; }
        .btn-accept { background: var(--gabon-bleu); color: white; }
        .btn-photo { background: var(--dark); color: white; }
        .btn-confirm { background: var(--gabon-vert); color: white; }
        .btn-verify { background: #6c5ce7; color: white; }

        /* SEARCH BAR */
        .search-box { position: sticky; top: 0; background: white; z-index: 10; padding-bottom: 10px; margin-bottom: 10px; border-bottom: 1px solid #eee; }

        /* PHOTO PREVIEW */
        .photo-box { margin-top: 10px; border: 2px dashed #ddd; border-radius: 8px; padding: 10px; text-align: center; }
        .preview-img { width: 100%; border-radius: 5px; display: none; margin-top: 8px; border: 2px solid var(--gabon-vert); }
        .finance-verify-img { width: 100%; border-radius: 8px; margin-top: 10px; border: 1px solid #ddd; }

        /* BILAN */
        .bilan-item { padding: 15px 0; border-bottom: 1px solid #eee; }
        .bilan-header { display: flex; justify-content: space-between; align-items: flex-start; }
        .bilan-client { font-weight: 800; font-size: 14px; }
        .bilan-gain { color: var(--gabon-vert); font-weight: 900; }
        .bilan-meta { font-size: 11px; color: #777; margin-top: 4px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666; margin-bottom: 15px;">Portail Administratif & Logistique</p>
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
            <button onclick="ouvrir('creer')" id="nav-creer" style="display:none">CRÉER</button>
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">MISSIONS</button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">BILAN</button>
        </nav>

        <!-- SECTION CREATION (ADMIN SEUL) -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 15px; border: 1px solid #eee; border-radius: 12px;">
                <h4 style="margin-top:0">Nouvelle Mission</h4>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="Téléphone Client">
                <input type="text" id="mLieu" placeholder="Destination (Quartier)">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)">
                <button onclick="creerMission()" class="btn-action btn-confirm">LANCER LA MISSION</button>
            </div>
        </div>

        <!-- SECTION MISSIONS -->
        <div id="sec-missions" class="section active-sec">
            <div id="container-missions"></div>
        </div>

        <!-- SECTION BILAN -->
        <div id="sec-bilan" class="section">
            <div id="search-container" class="search-box" style="display:none">
                <input type="text" id="bilanSearch" placeholder="Rechercher Client ou Code SMS..." oninput="renderUI()">
            </div>
            <div id="container-bilan"></div>
            <div style="background:var(--dark); color:white; padding:15px; border-radius:12px; margin-top:15px; display:flex; justify-content:space-between; align-items:center;">
                <span id="label-total">VOTRE GAIN</span>
                <b id="stat-total" style="color:var(--gabon-jaune); font-size:18px">0 F</b>
            </div>
        </div>
    </div>

    <input type="file" id="camInput" accept="image/*" capture="camera" style="display:none">
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

    let userRole = "livreur"; // admin, finance, livreur
    let allMissions = [];
    let currentPhotoKey = null;
    let currentPhotoData = "";

    document.getElementById('btnConnect').onclick = async () => {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        try {
            await signInWithEmailAndPassword(auth, email, pass);
        } catch (e) {
            alert("Accès refusé. Vérifiez vos identifiants.");
        }
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
            
            // Affichage conditionnel des outils admin/finance
            document.getElementById('nav-creer').style.display = (userRole === 'admin') ? 'block' : 'none';
            document.getElementById('search-container').style.display = (userRole === 'finance' || userRole === 'admin') ? 'block' : 'none';
            document.getElementById('label-total').innerText = (userRole === 'admin' || userRole === 'finance') ? "TOTAL CAISSE" : "MON GAIN";

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
        const searchTerm = document.getElementById('bilanSearch').value.toLowerCase();
        
        contM.innerHTML = ""; contB.innerHTML = "";
        let totalCaisse = 0;
        const myName = auth.currentUser?.email ? auth.currentUser.email.split('@')[0].toUpperCase() : "INVITÉ";

        allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
            
            // --- LOGIQUE ONGLET MISSIONS ---
            if(m.etape < 3) {
                const canSee = (userRole === 'admin' || userRole === 'finance' || m.livreur === "Libre" || m.livreur === myName);
                if(canSee) {
                    let actions = "";
                    
                    // Cas Livreur : Accepter ou Livrer
                    if(userRole === 'livreur') {
                        if(m.etape === 1 && m.livreur === "Libre") {
                            actions = `<button class="btn-action btn-accept" onclick="accepter('${m.key}')">ACCEPTER LA COURSE</button>`;
                        } else if(m.etape === 1 && m.livreur === myName) {
                            actions = `
                                <div class="photo-box">
                                    <button class="btn-action btn-photo" onclick="triggerCam('${m.key}')">📸 FILMER SMS</button>
                                    <img id="pre-${m.key}" class="preview-img">
                                    <input type="text" id="sms-${m.key}" placeholder="Code SMS du retrait">
                                    <button id="val-${m.key}" class="btn-action btn-confirm" onclick="livrer('${m.key}')" disabled>TERMINER LA LIVRAISON</button>
                                </div>
                            `;
                        }
                    }
                    
                    // Cas Finance/Admin : Vérifier et Encaisser
                    if((userRole === 'finance' || userRole === 'admin') && m.etape === 2) {
                        actions = `
                            <div style="background:#f1f2f6; padding:10px; border-radius:10px; margin-top:10px">
                                <p style="font-size:11px; margin:0; font-weight:bold; color:var(--gabon-bleu)">VÉRIFICATION FINANCE :</p>
                                <img src="${m.photo}" class="finance-verify-img">
                                <p style="font-size:14px; font-weight:bold; color:red; text-align:center; margin:5px 0">CODE : ${m.codeSMS}</p>
                                <button class="btn-action btn-confirm" onclick="encaisser('${m.key}')">VALIDER & ENCAISSER (${m.com} F)</button>
                            </div>
                        `;
                    }

                    contM.innerHTML += `
                        <div class="card step-${m.etape}">
                            <span class="badge-id">${m.id}</span>
                            <div class="card-title">${m.nom}</div>
                            <div class="card-info">
                                📍 <b>${m.lieu || 'Zone libre'}</b><br>
                                💰 Retrait : <b>${m.retrait.toLocaleString()} F</b><br>
                                🛵 Livreur : <b>${m.livreur}</b>
                            </div>
                            ${actions}
                        </div>
                    `;
                }
            }

            // --- LOGIQUE ONGLET BILAN (ARCHIVES) ---
            if(m.etape === 3) {
                // Filtre de recherche
                const matchSearch = m.nom.toLowerCase().includes(searchTerm) || (m.codeSMS && m.codeSMS.toLowerCase().includes(searchTerm));
                
                // Restriction par rôle
                const canViewBilan = (userRole === 'admin' || userRole === 'finance' || m.livreur === myName);
                
                if(canViewBilan && matchSearch) {
                    const gain = (userRole === 'livreur') ? 800 : m.com;
                    totalCaisse += gain;

                    contB.innerHTML += `
                        <div class="bilan-item">
                            <div class="bilan-header">
                                <span class="bilan-client">${m.nom} <small style="color:var(--gabon-bleu)">#${m.id}</small></span>
                                <span class="bilan-gain">+ ${gain.toLocaleString()} F</span>
                            </div>
                            <div class="bilan-meta">
                                📅 ${m.date || ''} à ${m.heure || ''}<br>
                                🔑 Code SMS : <b>${m.codeSMS || 'N/A'}</b><br>
                                🛵 Livreur : ${m.livreur}
                            </div>
                            ${(userRole === 'admin' || userRole === 'finance') ? 
                                `<button onclick="this.nextElementSibling.style.display='block'" style="background:none; border:none; color:var(--gabon-bleu); font-size:10px; padding:0; cursor:pointer; text-decoration:underline">Voir preuve photo</button>
                                <div style="display:none; margin-top:10px"><img src="${m.photo}" style="width:100%; border-radius:5px"></div>` : ''}
                        </div>
                    `;
                }
            }
        });
        document.getElementById('stat-total').innerText = totalCaisse.toLocaleString() + " F";
    }

    // --- FONCTIONS ACTIONS ---

    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const mnt = parseInt(document.getElementById('mRetrait').value);
        if(!nom || !mnt) return alert("Nom et Montant obligatoires");

        const d = new Date();
        push(ref(db, 'missions'), {
            id: "CT" + Math.floor(100 + Math.random() * 899),
            nom, tel: document.getElementById('mTel').value, lieu: document.getElementById('mLieu').value,
            retrait: mnt, com: (mnt >= 15000 ? 390 : 190),
            etape: 1, livreur: "Libre", codeSMS: "", photo: "",
            date: d.toLocaleDateString('fr-FR'),
            heure: d.toLocaleTimeString('fr-FR', {hour: '2-digit', minute:'2-digit'}),
            timestamp: Date.now()
        });
        alert("Mission lancée !");
        document.getElementById('mNom').value = ""; document.getElementById('mRetrait').value = "";
        ouvrir('missions');
    };

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
        if(!file) return;
        const reader = new FileReader();
        reader.onload = (re) => {
            const img = new Image();
            img.onload = () => {
                const can = document.getElementById('canvas');
                const ctx = can.getContext('2d');
                const ratio = img.height / img.width;
                can.width = 600; can.height = 600 * ratio;
                ctx.drawImage(img, 0, 0, 600, can.height);
                currentPhotoData = can.toDataURL('image/jpeg', 0.6);
                document.getElementById('pre-'+currentPhotoKey).src = currentPhotoData;
                document.getElementById('pre-'+currentPhotoKey).style.display = 'block';
                document.getElementById('val-'+currentPhotoKey).disabled = false;
            };
            img.src = re.target.result;
        };
        reader.readAsDataURL(file);
    };

    window.livrer = (k) => {
        const code = document.getElementById('sms-'+k).value;
        if(!code) return alert("Le code SMS est obligatoire pour justifier le retrait.");
        update(ref(db, `missions/${k}`), { etape: 2, codeSMS: code, photo: currentPhotoData });
    };

    window.encaisser = (k) => {
        if(confirm("Confirmez-vous que la photo et le code sont corrects ?")) {
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
