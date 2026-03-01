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

        /* AUTOMATION BOX */
        .auto-box { background: #f0fdf4; padding: 12px; border-radius: 10px; margin: 15px 0; border: 1px solid #bbf7d0; display: none; animation: fadeIn 0.3s; }
        .auto-item { display: flex; justify-content: space-between; font-size: 13px; margin: 5px 0; color: #166534; }
        .auto-val { font-weight: bold; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(-5px); } to { opacity: 1; transform: translateY(0); } }

        /* MISSION CARDS */
        .card { border: 1px solid #eee; padding: 15px; border-radius: 12px; margin-bottom: 12px; position: relative; border-left: 6px solid #ccc; transition: 0.3s; }
        .card.step-1 { border-left-color: var(--gabon-bleu); }
        
        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; font-size: 13px; }
        .btn-confirm { background: var(--gabon-vert); color: white; }

        .stats-footer { background: var(--dark); color: white; padding: 15px; border-radius: 12px; margin-top: 15px; }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666; margin-bottom: 15px;">Portail Administratif & Logistique</p>
            <input type="text" id="login-email" placeholder="Identifiant (ex: admin)">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">ACCÉDER AU SYSTÈME</button>
            <p style="font-size: 10px; color: #999; margin-top: 10px;">Identifiant test : admin / 1234</p>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-vert)">CT241 OPS</h3>
                <span id="user-display" style="font-size:10px; color:#888"></span>
                <span id="user-role" class="role-badge">...</span>
            </div>
            <button id="btnOut" style="font-size:10px; color:var(--danger); background:none; border:none; font-weight:bold; cursor:pointer">SORTIR</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('creer')" id="nav-creer">CRÉER</button>
            <button onclick="ouvrir('missions')" id="nav-missions" class="active">MISSIONS</button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">BILAN</button>
        </nav>

        <!-- SECTION CREATION -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 15px; border: 1px solid #eee; border-radius: 12px;">
                <h4 style="margin-top:0">Nouvelle Mission</h4>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="text" id="mLieu" placeholder="Destination (Quartier)">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)" oninput="calculerAutomatisme(this.value)">
                
                <!-- AUTOMATISATION POUR ADMIN -->
                <div id="admin-auto-box" class="auto-box">
                    <div class="auto-item">
                        <span>Commission CT241 à percevoir :</span>
                        <span id="auto-com" class="auto-val">0 F</span>
                    </div>
                    <div class="auto-item">
                        <span>Frais de Livraison :</span>
                        <span id="auto-liv" class="auto-val">1 000 F</span>
                    </div>
                    <div style="border-top: 1px solid #bbf7d0; margin-top:8px; padding-top:8px; display:flex; justify-content:space-between; font-weight:bold; color:var(--gabon-vert)">
                        <span>Total Frais :</span>
                        <span id="auto-total">0 F</span>
                    </div>
                </div>

                <button onclick="creerMission()" class="btn-action btn-confirm">LANCER LA MISSION</button>
            </div>
        </div>

        <!-- MISSIONS -->
        <div id="sec-missions" class="section active-sec">
            <div id="container-missions">
                <p style="text-align:center; color:#999; font-size:12px; margin-top:50px">Aucune mission en cours.</p>
            </div>
        </div>

        <!-- BILAN -->
        <div id="sec-bilan" class="section">
            <div id="container-bilan"></div>
            <div class="stats-footer">
                <div style="display:flex; justify-content:space-between; align-items:center;">
                    <span id="label-total">SOLDE TOTAL</span>
                    <b id="stat-total" style="color:var(--gabon-jaune); font-size:18px">0 F</b>
                </div>
            </div>
        </div>
    </div>

<script>
    // Variables d'état
    let userRole = "livreur";
    let currentUser = "";
    let missions = [];

    // Login logic (Mode Maintenance + Firebase ready)
    document.getElementById('btnConnect').onclick = () => {
        const id = document.getElementById('login-email').value.toLowerCase();
        const pass = document.getElementById('login-pass').value;

        if (id.includes('admin') && pass === '1234') {
            userRole = "admin";
            currentUser = id;
            initApp();
        } else {
            alert("Identifiant ou mot de passe incorrect.");
        }
    };

    document.getElementById('btnOut').onclick = () => {
        location.reload();
    };

    function initApp() {
        document.getElementById('auth-screen').style.display = 'none';
        document.getElementById('main-app').style.display = 'block';
        document.getElementById('user-role').innerText = userRole.toUpperCase();
        document.getElementById('user-display').innerText = currentUser;
        
        // Seul l'admin voit l'onglet Créer par défaut
        if(userRole !== 'admin') {
            document.getElementById('nav-creer').style.display = 'none';
        }
        renderMissions();
    }

    // AUTOMATISATION DES CALCULS
    window.calculerAutomatisme = (val) => {
        const mnt = parseInt(val) || 0;
        const box = document.getElementById('admin-auto-box');
        
        if (mnt > 0 && userRole === 'admin') {
            box.style.display = 'block';
            // Règle : < 15k = 190, >= 15k = 390
            const commission = mnt >= 15000 ? 390 : 190;
            const livraison = 1000;
            
            document.getElementById('auto-com').innerText = commission.toLocaleString() + " F";
            document.getElementById('auto-liv').innerText = livraison.toLocaleString() + " F";
            document.getElementById('auto-total').innerText = (commission + livraison).toLocaleString() + " F";
        } else {
            box.style.display = 'none';
        }
    };

    window.creerMission = () => {
        const nom = document.getElementById('mNom').value;
        const mnt = parseInt(document.getElementById('mRetrait').value);
        const lieu = document.getElementById('mLieu').value;

        if(!nom || !mnt) return alert("Veuillez saisir au moins le nom et le montant.");

        const com = mnt >= 15000 ? 390 : 190;

        const newMission = {
            id: "CT" + Math.floor(100 + Math.random() * 899),
            nom, lieu, retrait: mnt, com: com,
            etape: 1,
            timestamp: Date.now()
        };

        missions.push(newMission);
        alert("Mission créée avec succès !");
        
        // Reset
        document.getElementById('mNom').value = "";
        document.getElementById('mRetrait').value = "";
        document.getElementById('mLieu').value = "";
        document.getElementById('admin-auto-box').style.display = 'none';
        
        ouvrir('missions');
        renderMissions();
    };

    function renderMissions() {
        const cont = document.getElementById('container-missions');
        cont.innerHTML = "";

        if(missions.length === 0) {
            cont.innerHTML = `<p style="text-align:center; color:#999; font-size:12px; margin-top:50px">Aucune mission en cours.</p>`;
            return;
        }

        missions.forEach(m => {
            cont.innerHTML += `
                <div class="card step-1">
                    <span style="position:absolute; top:10px; right:10px; font-size:10px; font-weight:bold; background:#eee; padding:2px 5px; border-radius:4px">${m.id}</span>
                    <div style="font-weight:bold; color:var(--dark)">${m.nom}</div>
                    <div style="font-size:12px; color:#666; margin-top:5px">
                        📍 <b>${m.lieu || 'Non spécifié'}</b><br>
                        💰 Retrait : <b>${m.retrait.toLocaleString()} F</b>
                    </div>
                    ${userRole === 'admin' ? `
                    <div style="margin-top:10px; padding-top:10px; border-top:1px dashed #eee; font-size:11px; color:var(--gabon-vert)">
                        L'admin percevra : <b>${m.com} F</b> de commission
                    </div>` : ''}
                </div>
            `;
        });
    }

    window.ouvrir = (id) => {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active-sec'));
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        document.getElementById('sec-'+id).classList.add('active-sec');
        document.getElementById('nav-'+id).classList.add('active');
    };

</script>
</body>
</html>
