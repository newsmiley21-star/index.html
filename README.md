<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CT241 - LOGISTIQUE GABON</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --gabon-vert: #009E60; 
            --gabon-jaune: #FCD116; 
            --gabon-bleu: #3A75C4;
        }
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; -webkit-tap-highlight-color: transparent; }
        .gabon-gradient { background: linear-gradient(135deg, var(--gabon-vert) 0%, var(--gabon-jaune) 50%, var(--gabon-bleu) 100%); }
        .active-nav { background: white !important; color: var(--gabon-bleu) !important; box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1); }
        .section { display: none; }
        .section.active { display: block; animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        nav::-webkit-scrollbar { display: none; }
        nav { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="antialiased text-slate-900">

    <!-- ECRAN D'AUTHENTIFICATION -->
    <div id="auth-screen" class="fixed inset-0 z-[9999] flex items-center justify-center gabon-gradient p-6">
        <div class="bg-white/95 backdrop-blur-md p-8 rounded-[2rem] w-full max-w-sm shadow-2xl text-center">
            <h1 class="text-3xl font-black text-[#009E60] mb-1">CT241 OPS</h1>
            <p class="text-[10px] font-bold text-slate-400 tracking-[0.2em] mb-8">LOGISTIQUE & CASH GABON</p>
            
            <div class="space-y-4">
                <input type="email" id="login-email" placeholder="Email professionnel" class="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-[#3A75C4] outline-none transition-all">
                <input type="password" id="login-pass" placeholder="Mot de passe" class="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-[#3A75C4] outline-none transition-all">
                <button id="btnConnect" class="w-full py-4 bg-[#009E60] text-white font-black rounded-2xl shadow-lg active:scale-95 transition-transform">
                    SE CONNECTER
                </button>
            </div>
        </div>
    </div>

    <!-- APPLICATION PRINCIPALE -->
    <div id="main-app" class="hidden min-h-screen pb-20">
        <header class="bg-white px-6 pt-6 pb-4 sticky top-0 z-40 shadow-sm">
            <div class="flex justify-between items-start">
                <div>
                    <h2 class="text-xl font-black text-[#3A75C4]">CT241 OPS</h2>
                    <div class="flex items-center gap-2 mt-1">
                        <span id="user-role" class="text-[10px] px-2 py-0.5 bg-[#FCD116] text-amber-900 font-extrabold rounded-full uppercase">...</span>
                        <span id="user-display" class="text-[11px] font-bold text-slate-400"></span>
                    </div>
                </div>
                <button id="btnOut" class="p-2 bg-red-50 text-red-500 rounded-xl font-bold text-xs">DÉCONNEXION</button>
            </div>

            <nav class="flex overflow-x-auto gap-2 mt-6 p-1.5 bg-slate-100 rounded-2xl">
                <button onclick="switchTab('missions')" id="nav-missions" class="active-nav whitespace-nowrap px-4 py-2.5 rounded-xl text-[11px] font-bold transition-all text-slate-500">
                    MISSIONS
                </button>
                <button onclick="switchTab('creer')" id="nav-creer" class="hidden whitespace-nowrap px-4 py-2.5 rounded-xl text-[11px] font-bold transition-all text-slate-500">
                    NOUVEAU
                </button>
                <button onclick="switchTab('archives')" id="nav-archives" class="whitespace-nowrap px-4 py-2.5 rounded-xl text-[11px] font-bold transition-all text-slate-500">
                    ARCHIVES
                </button>
                <button onclick="switchTab('bilan')" id="nav-bilan" class="whitespace-nowrap px-4 py-2.5 rounded-xl text-[11px] font-bold transition-all text-slate-500">
                    BILAN
                </button>
                <button onclick="switchTab('compta')" id="nav-compta" class="hidden whitespace-nowrap px-4 py-2.5 rounded-xl text-[11px] font-bold transition-all text-slate-500">
                    COMPTA
                </button>
            </nav>
        </header>

        <main class="p-6 max-w-lg mx-auto">
            <!-- SECTION MISSIONS ACTIVES -->
            <div id="sec-missions" class="section active space-y-6">
                <div id="admin-pending-box" class="hidden">
                    <h3 class="text-[11px] font-black text-red-500 tracking-widest uppercase mb-3">⚠️ À valider (Admin)</h3>
                    <div id="list-pending" class="space-y-4"></div>
                </div>
                <div>
                    <h3 class="text-[11px] font-black text-[#3A75C4] tracking-widest uppercase mb-3">🚀 En cours d'exécution</h3>
                    <div id="list-active" class="space-y-4"></div>
                </div>
            </div>

            <!-- SECTION NOUVEAU (CREATION) -->
            <div id="sec-creer" class="section">
                <div class="bg-white p-6 rounded-[2rem] shadow-sm border border-slate-100">
                    <div class="w-12 h-12 bg-green-50 rounded-2xl flex items-center justify-center mb-4 mx-auto">
                        <svg class="w-6 h-6 text-[#009E60]" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path></svg>
                    </div>
                    <h3 class="text-xl font-black text-slate-800 mb-6 text-center">Nouvelle Mission</h3>
                    <div class="space-y-4">
                        <div>
                            <label class="text-[10px] font-black uppercase text-slate-400 ml-2 mb-1 block">Identité Client</label>
                            <input type="text" id="mNom" placeholder="Nom complet" class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent outline-none focus:border-[#009E60] transition-all">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="text-[10px] font-black uppercase text-slate-400 ml-2 mb-1 block">Contact</label>
                                <input type="tel" id="mTel" placeholder="06x / 07x..." class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent outline-none focus:border-[#009E60] transition-all">
                            </div>
                            <div>
                                <label class="text-[10px] font-black uppercase text-slate-400 ml-2 mb-1 block">Quartier</label>
                                <input type="text" id="mQuartier" placeholder="Ex: Nzeng" class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent outline-none focus:border-[#009E60] transition-all">
                            </div>
                        </div>
                        
                        <!-- FRAIS ET COMMISSION -->
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="text-[10px] font-black uppercase text-slate-400 ml-2 mb-1 block">Frais Livraison (Livreur)</label>
                                <input type="number" id="mLivraisonInput" placeholder="1000" value="1000" class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent outline-none focus:border-[#009E60] transition-all font-bold">
                            </div>
                            <div>
                                <label class="text-[10px] font-black uppercase text-slate-400 ml-2 mb-1 block">Commission (CT241)</label>
                                <input type="number" id="mComInput" placeholder="500" value="500" class="w-full p-4 bg-slate-50 rounded-2xl border-2 border-transparent outline-none focus:border-[#009E60] transition-all font-bold">
                            </div>
                        </div>

                        <div>
                            <label class="text-[10px] font-black uppercase text-slate-400 ml-2 mb-1 block">Montant du retrait</label>
                            <div class="relative">
                                <input type="number" id="mRetrait" placeholder="0" class="w-full p-4 pr-12 bg-slate-50 rounded-2xl font-black text-2xl border-2 border-transparent outline-none focus:border-[#009E60] transition-all text-[#3A75C4]">
                                <span class="absolute right-4 top-1/2 -translate-y-1/2 font-black text-slate-300">FCFA</span>
                            </div>
                        </div>
                        <button onclick="createMission()" class="w-full py-5 bg-[#009E60] text-white font-black rounded-2xl shadow-lg shadow-green-100 active:scale-95 transition-transform mt-4">DÉPLOYER LA MISSION</button>
                    </div>
                </div>
            </div>

            <!-- SECTION ARCHIVES -->
            <div id="sec-archives" class="section">
                <h3 class="text-[11px] font-black text-slate-400 tracking-widest uppercase mb-3">📁 Historique par date</h3>
                <div id="list-archives" class="space-y-6"></div>
            </div>

            <!-- SECTION BILAN -->
            <div id="sec-bilan" class="section">
                <h3 class="text-[11px] font-black text-slate-400 tracking-widest uppercase mb-3">💰 Détail mes gains</h3>
                <div id="list-bilan" class="space-y-2 mb-6"></div>
                <div class="bg-slate-900 text-white p-8 rounded-[2rem] shadow-xl">
                    <p class="text-[11px] font-bold opacity-50 uppercase tracking-widest mb-2">Total Revenus (Livreur)</p>
                    <h2 id="stat-total" class="text-4xl font-black text-[#FCD116]">0 F</h2>
                </div>
            </div>

            <!-- SECTION COMPTA -->
            <div id="sec-compta" class="section">
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden mb-6">
                    <table class="w-full text-left text-sm">
                        <thead class="bg-slate-50 text-[10px] font-black uppercase text-slate-400">
                            <tr><th class="p-4">Client</th><th class="p-4 text-right">Com.</th><th class="p-4 text-right">Retrait</th></tr>
                        </thead>
                        <tbody id="list-compta"></tbody>
                    </table>
                </div>
                <div class="bg-[#009E60] p-6 rounded-3xl text-white font-black text-lg flex justify-between">
                    <span>TOTAL COM.</span><span id="compta-total-com">0 F</span>
                </div>
