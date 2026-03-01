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

        /* MAIN APP */
        #main-app { display: none; max-width: 600px; margin: auto; background: white; border-radius: 15px; padding: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); min-height: 90vh; }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        .role-badge { font-size: 10px; padding: 3px 10px; border-radius: 12px; background: var(--dark); color: white; font-weight: bold; }

        nav { display: flex; gap: 5px; margin-bottom: 15px; background: #eee; padding: 5px; border-radius: 10px; position: relative; }
        nav button { flex: 1; padding: 12px 5px; border: none; border-radius: 7px; background: transparent; font-weight: bold; font-size: 11px; color: #666; cursor: pointer; position: relative; }
        nav button.active { background: white; color: var(--gabon-vert); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        
        .dot { position: absolute; top: 5px; right: 5px; width: 10px; height: 10px; background: var(--danger); border-radius: 50%; border: 2px solid white; display: none; animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.3); } 100% { transform: scale(1); } }

        .section { display: none; }
        .active-sec { display: block; }

        /* MISSION CARDS */
        .card { border: 1px solid #eee; padding: 15px; border-radius: 12px; margin-bottom: 12px; position: relative; border-left: 6px solid #ccc; }
        .card.step-1 { border-left-color: var(--gabon-bleu); }
        .card.step-2 { border-left-color: var(--gabon-jaune); }
        
        .card-title { font-weight: bold; font-size: 14px; margin-bottom: 5px; display: flex; align-items: center; }
        .card-info { font-size: 12px; color: #555; line-height: 1.4; }
        .badge-id { position: absolute; top: 10px; right: 10px; background: #f1f2f6; padding: 2px 6px; border-radius: 4px; font-size: 10px; font-weight: bold; }

        .btn-action { width: 100%; padding: 12px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; font-size: 13px; }
        .btn-accept { background: var(--gabon-bleu); color: white; }
        .btn-photo { background: var(--dark); color: white; }
        .btn-confirm { background: var(--gabon-vert); color: white; }

        /* BILAN STYLING */
        .bilan-item { padding: 15px 0; border-bottom: 1px solid #eee; }
        .bilan-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 8px; }
        .bilan-client { font-weight: 800; font-size: 14px; color: var(--dark); }
        .bilan-meta { font-size: 11px; color: #777; margin-top: 4px; }
        .bilan-gain { color: var(--gabon-vert); font-weight: 900; font-size: 15px; }
        .bilan-role-info { font-size: 9px; text-transform: uppercase; color: #aaa; margin-top: 2px; }
        
        .btn-view-proof { background: #f1f2f6; border: none; padding: 8px 12px; border-radius: 6px; font-size: 10px; font-weight: bold; color: var(--gabon-bleu); cursor: pointer; margin-top: 10px; display: inline-flex; align-items: center; gap: 5px; }
        .proof-img-container { display: none; margin-top: 10px; border-radius: 8px; overflow: hidden; border: 1px solid #ddd; background: #eee; }
        .proof-img-container img { width: 100%; display: block; height: auto; }

        .photo-box { margin-top: 10px; border: 2px dashed #ddd; border-radius: 8px; padding: 10px; text-align: center; }
        .preview-img { width: 100%; border-radius: 5px; display: none; margin-top: 8px; border: 2px solid var(--gabon-vert); }
    </style>
</head>
<body>

    <div id="auth-screen">
        <div class="login-card">
            <h2 style="color:var(--gabon-vert); margin:0">CT241 OPS</h2>
            <p style="font-size: 11px; color: #666; margin-bottom: 15px;">Interface de Gestion</p>
            <input type="email" id="login-email" placeholder="Email">
            <input type="password" id="login-pass" placeholder="Mot de passe">
            <button class="btn-login" id="btnConnect">SE CONNECTER</button>
        </div>
    </div>

    <div id="main-app">
        <header>
            <div>
                <h3 style="margin:0; color:var(--gabon-vert)">CT241 OPS</h3>
                <span id="user-display" style="font-size:10px; color:#888"></span>
                <span id="user-role" class="role-badge">CHARGEMENT...</span>
            </div>
            <button id="btnOut" style="font-size:10px; color:var(--danger); background:none; border:none; font-weight:bold; cursor:pointer;">DECONNEXION</button>
        </header>

        <nav id="navbar">
            <button onclick="ouvrir('creer')" id="nav-creer" class="role-hide finance-only admin-only">CRÉER</button>
            <button onclick="ouvrir('missions')" id="nav-missions">MISSIONS <span id="mission-dot" class="dot"></span></button>
            <button onclick="ouvrir('bilan')" id="nav-bilan">BILAN</button>
        </nav>

        <!-- SECTION CREATION -->
        <div id="sec-creer" class="section">
            <div style="background: #fff; padding: 10px; border: 1px solid #eee; border-radius: 12px;">
                <h4 style="margin-top:0">Nouvelle Mission</h4>
                <input type="text" id="mNom" placeholder="Nom du Client">
                <input type="tel" id="mTel" placeholder="Téléphone Client">
                <input type="text" id="mLieu" placeholder="Quartier / Destination">
                <input type="number" id="mRetrait" placeholder="Montant Retrait (FCFA)" oninput="calculerCom()">
                <div id="comLabel" style="font-size:11px; color:var(--gabon-vert); font-weight:bold; margin: 5px 0;">Com Direction: 0 F</div>
                <button onclick="creerMission()" class="btn-action btn-confirm">LANCER LA MISSION</button>
            </div>
        </div>

        <!-- SECTION MISSIONS EN COURS -->
        <div id="sec-missions" class="section active-sec">
            <div id="container-missions"></div>
        </div>

        <!-- SECTION BILAN (POUR TOUS) -->
        <div id="sec-bilan" class="section">
            <h4 style="margin:0 0 10px 0; color:var(--dark); font-size: 14px; text-transform: uppercase;">Historique & Gains</h4>
            <div id="container-bilan" style="background:white; border-radius:12px; border:1px solid #eee; padding:0 15px; max-height:480px; overflow-y:auto;"></div>
            
            <div style="background:var(--dark); color:white; padding:15px; border-radius:1
