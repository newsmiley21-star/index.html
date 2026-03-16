window.renderUI = (filter = "") => {
    // ... (Sélecteurs d'éléments restants identiques)
    const listBilToday = document.getElementById('list-bilan-today');
    const todayStr = new Date().toLocaleDateString('fr-FR');
    const myName = auth.currentUser ? auth.currentUser.email.split('@')[0].toUpperCase() : "";

    // Variables pour les totaux Admin
    let globalRetraits = 0;
    let globalCourses = 0;
    let globalCommissions = 0;
    let globalGainsLivreurs = 0;

    // Variables pour le Livreur individuel (Section Compta)
    let persoGains = 0;
    let persoCount = 0;

    allMissions.sort((a,b) => b.timestamp - a.timestamp).forEach(m => {
        const mDateStr = new Date(m.timestamp).toLocaleDateString('fr-FR');
        const isToday = (mDateStr === todayStr);

        if(m.etape === 3) {
            // LOGIQUE ADMIN : Bilan de TOUS les livreurs
            if(userRole === 'admin' && isToday) {
                globalRetraits += m.retrait;
                globalCourses++;
                globalCommissions += m.com;
                globalGainsLivreurs += m.fraisLivraison;
                listBilToday.innerHTML += createRow(m, "admin"); 
            }

            // LOGIQUE LIVREUR : Uniquement ses propres comptes
            if(userRole === 'livreur' && m.livreur === myName) {
                persoGains += m.fraisLivraison;
                persoCount++;
                document.getElementById('list-compta-daily').innerHTML += createRow(m, "livreur");
            }
        }
        // ... (Logique des archives et missions actives reste identique)
    });

    // Mise à jour de l'affichage Admin
    if(userRole === 'admin') {
        document.getElementById('admin-count-total').innerText = globalCourses;
        document.getElementById('admin-vol-total').innerText = globalRetraits.toLocaleString() + " F";
        document.getElementById('admin-com-total').innerText = globalCommissions.toLocaleString() + " F";
        document.getElementById('admin-gains-livreurs').innerText = globalGainsLivreurs.toLocaleString() + " F";
    }

    // Mise à jour de l'affichage Livreur (Section Compta)
    if(userRole === 'livreur') {
        document.getElementById('livreur-gains').innerText = persoGains.toLocaleString() + " F";
        document.getElementById('livreur-count').innerText = persoCount;
    }
};
