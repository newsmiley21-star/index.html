// Remplace la fonction ecouterMissions dans ton script par celle-ci :

function ecouterMissions() {
    onValue(ref(db, 'missions'), (snap) => {
        const data = snap.val();
        const bS = document.getElementById('body-saisie');
        const lT = document.getElementById('liste-taches');
        const lR = document.getElementById('liste-reçus');
        let total = 0; let maxIdx = 0;
        bS.innerHTML = ""; lT.innerHTML = ""; lR.innerHTML = "";
        
        if(data) {
            Object.keys(data).forEach(key => {
                const m = data[key];
                const num = parseInt(m.id.split('-')[1]);
                if(num > maxIdx) maxIdx = num;

                bS.innerHTML += `<tr><td><span class="id-badge">${m.id}</span><br><small>${m.heure}</small></td><td><b>${m.nom}</b><a href="tel:${m.tel}" class="phone-link">📞 ${m.tel}</a></td><td>${m.com}</td></tr>`;
                
                if(m.etape === 1) {
                    lT.innerHTML += `<div class="card"><div><b>${m.nom}</b><br><small>📍 ${m.lieu} | 🕒 ${m.heure}</small></div><button onclick="validerLiv('${key}')" style="background:var(--gabon-vert); color:white; border:none; padding:10px; border-radius:8px; font-weight:bold;">LIVRER</button></div>`;
                } else {
                    if(m.etape === 3) total += m.com;
                    lR.innerHTML += `
                        <div class="card ${m.etape === 3 ? 'done' : ''}">
                            <div>
                                <b>${m.nom}</b><br>
                                <small>${m.com.toLocaleString()} FCFA | ${m.etape === 3 ? '✅ Payé' : '⏳ Attente'}</small>
                            </div>
                            <div style="display:flex; gap:5px;">
                                ${m.etape === 2 ? 
                                    `<button onclick="encaisser('${key}')" style="background:var(--gabon-bleu); color:white; border:none; padding:10px; border-radius:8px; font-weight:bold;">ENCAISSER</button>` : 
                                    `<button onclick="annulerEncaissement('${key}')" style="background:#f39c12; color:white; border:none; padding:10px; border-radius:8px; font-weight:bold;" title="Remettre en attente">↩️</button>`
                                }
                                <button onclick="supprimer('${key}')" style="background:var(--danger); color:white; border:none; padding:8px; border-radius:8px; font-weight:bold;">X</button>
                            </div>
                        </div>`;
                }
            });
        }
        dernierIndex = maxIdx;
        document.getElementById('totalCom').innerText = total.toLocaleString();
    });
}

// AJOUTE CETTE NOUVELLE FONCTION EN BAS AVEC LES AUTRES :
window.annulerEncaissement = (key) => {
    if(confirm("Remettre ce reçu en attente de paiement ?")) {
        update(ref(db, 'missions/'+key), { etape: 2 });
    }
};
