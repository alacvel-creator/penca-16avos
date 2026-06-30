<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Penca 16avos - Prode</title>
    <!-- Tailwind CSS para diseño visual premium -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .podium-1 { height: 110px; background: linear-gradient(135deg, #f59e0b, #d97706); }
        .podium-2 { height: 85px; background: linear-gradient(135deg, #94a3b8, #64748b); }
        .podium-3 { height: 65px; background: linear-gradient(135deg, #b45309, #78350f); }
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen pb-12">

    <!-- HEADER MÓVIL -->
    <header class="sticky top-0 z-40 bg-slate-900/90 backdrop-blur-md border-b border-slate-800 px-4 py-3 flex justify-between items-center">
        <div class="flex items-center gap-2">
            <span class="text-xl">🏆</span>
            <h1 class="text-lg font-bold tracking-tight bg-gradient-to-r from-amber-400 to-orange-500 bg-clip-text text-transparent">
                PRODE 16AVOS DE FINAL
            </h1>
        </div>
        <button onclick="toggleAdmin()" class="text-slate-400 hover:text-amber-400 transition p-2 text-lg">
            ⚙️
        </button>
    </header>

    <main class="max-w-md mx-auto px-4 mt-4 space-y-6">

        <!-- SECCIÓN 1: EL PODIO VISUAL -->
        <section class="bg-slate-900/50 border border-slate-800 rounded-2xl p-4 shadow-xl">
            <h2 class="text-center text-xs font-semibold uppercase tracking-widest text-slate-400 mb-4 flex items-center justify-center gap-1">
                🏅 Podio en Vivo
            </h2>
            <div id="podium-container" class="flex items-end justify-center gap-3 pt-6 pb-2">
                <!-- Se renderiza dinámicamente -->
            </div>
        </section>

        <!-- SECCIÓN 2: TABLA DE POSICIONES COMPLETA -->
        <section class="bg-slate-900 border border-slate-800 rounded-2xl p-4 shadow-xl">
            <div class="flex justify-between items-center mb-3">
                <h3 class="text-sm font-bold uppercase tracking-wider text-amber-400">Tabla General</h3>
                <span class="text-[10px] bg-slate-800 text-slate-400 px-2 py-0.5 rounded-full" id="live-indicator">
                    ● Auto-Sync Activo
                </span>
            </div>
            <div class="overflow-hidden rounded-xl border border-slate-800 bg-slate-950">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-slate-900 text-slate-400 text-xs font-semibold uppercase border-b border-slate-800">
                            <th class="py-2.5 px-3 text-center w-12">Pos</th>
                            <th class="py-2.5 px-2">Jugador</th>
                            <th class="py-2.5 px-2 text-center">Pleno</th>
                            <th class="py-2.5 px-3 text-right">Pts</th>
                        </tr>
                    </thead>
                    <tbody id="leaderboard-body" class="text-sm divide-y divide-slate-800/50">
                        <!-- Se renderiza dinámicamente -->
                    </tbody>
                </table>
            </div>
        </section>

        <!-- PANEL DE ADMINISTRADOR MANUAL -->
        <section id="admin-panel" class="hidden bg-amber-950/40 border border-amber-800/60 rounded-2xl p-4 space-y-3">
            <div class="flex justify-between items-center">
                <h3 class="text-xs font-bold uppercase text-amber-400 flex items-center gap-1">
                    ⚠️ Consola Marcadores Reales
                </h3>
                <button onclick="fetchLiveScores()" class="text-xs bg-amber-600 hover:bg-amber-500 text-slate-950 font-bold px-2 py-1 rounded transition">
                    🔄 Forzar API Web
                </button>
            </div>
            <p class="text-[11px] text-amber-200/70">Ingresa los resultados finales para actualizar los puntos del grupo al instante.</p>
            <div id="admin-matches-list" class="space-y-2 max-h-48 overflow-y-auto pr-1 text-xs">
                <!-- Control manual de partidos -->
            </div>
        </section>

        <!-- SECCIÓN 3: PARTIDOS Y APUESTAS DEL GRUPO -->
        <section class="space-y-3">
            <h3 class="text-sm font-bold uppercase tracking-wider text-slate-400 flex items-center gap-1.5 px-1">
                📅 Partidos de 16avos
            </h3>
            <div id="matches-container" class="space-y-4">
                <!-- Se renderiza dinámicamente -->
            </div>
        </section>

    </main>

    <script>
        const participants = ['MARIELH', 'TADY', 'ALVARO', 'ALAN', 'CAROL', 'TIAGO', 'JOAQUIN', 'CHUCHO', 'LUCE', 'FLOR', 'ROGER', 'PIPITA', 'MAURICIO', 'GOGUITO', 'ANTUAN', 'CACHI'];
        
        let matches = [
          { "id": 1, "match": "BRASIL/JAPON", "predictions": { "MARIELH": "2-1", "TADY": "3-1", "ALVARO": "2-1", "ALAN": "1-1", "CAROL": "2-2", "TIAGO": "1-2", "JOAQUIN": "2-1", "CHUCHO": "2-0", "LUCE": "3-1", "FLOR": "2-1", "ROGER": "2-1", "PIPITA": "1-1", "MAURICIO": "1-1", "GOGUITO": "3-1", "ANTUAN": "2-0" }, "actual_score": "2-1" },
          { "id": 2, "match": "ALEMANIA/PARAGUAY", "predictions": { "MARIELH": "5-0", "TADY": "2-0", "ALVARO": "2-0", "ALAN": "2-1", "CAROL": "3-1", "TIAGO": "3-0", "JOAQUIN": "3-1", "CHUCHO": "3-0", "LUCE": "3-1", "FLOR": "3-0", "ROGER": "1-0", "PIPITA": "2-0", "MAURICIO": "2-0", "GOGUITO": "2-1", "ANTUAN": "3-1" }, "actual_score": "2-0" },
          { "id": 3, "match": "PAISES BAJOS/MARRUECOS", "predictions": { "MARIELH": "3-2", "TADY": "1-0", "ALVARO": "1-0", "ALAN": "2-1", "CAROL": "1-1", "TIAGO": "2-1", "JOAQUIN": "2-1", "CHUCHO": "2-1", "LUCE": "2-2", "FLOR": "1-2", "ROGER": "2-1", "PIPITA": "1-0", "MAURICIO": "1-1", "GOGUITO": "3-2", "ANTUAN": "2-1" }, "actual_score": "" },
          { "id": 4, "match": "COSTA DE MARFIL/NORUEGA", "predictions": {}, "actual_score": "" },
          { "id": 5, "match": "FRANCIA/SUECIA", "predictions": {}, "actual_score": "" },
          { "id": 6, "match": "MEXICO/ECUADOR", "predictions": {}, "actual_score": "" },
          { "id": 7, "match": "INGLATERRA/CONGO", "predictions": {}, "actual_score": "" },
          { "id": 8, "match": "BELGICA/SENEGAL", "predictions": {}, "actual_score": "" },
          { "id": 9, "match": "EEUU/BOSNIA", "predictions": {}, "actual_score": "" },
          { "id": 10, "match": "ESPANA/ AUSTRIA", "predictions": {}, "actual_score": "" },
          { "id": 11, "match": "PORTUGAL/CROACIA", "predictions": {}, "actual_score": "" },
          { "id": 12, "match": "SUIZA/ARGELIA", "predictions": {}, "actual_score": "" },
          { "id": 13, "match": "AUSTRALIA/EGIPTO", "predictions": {}, "actual_score": "" },
          { "id": 14, "match": "ARGENTINA/CABO VERDE", "predictions": {}, "actual_score": "" },
          { "id": 15, "match": "COLOMBIA/GHANA", "predictions": {}, "actual_score": "" }
        ];

        if (localStorage.getItem('prode_scores')) {
            const savedScores = JSON.parse(localStorage.getItem('prode_scores'));
            matches.forEach(m => {
                if (savedScores[m.id] !== undefined) m.actual_score = savedScores[m.id];
            });
        }

        function getPoints(predStr, actualStr) {
            if (!predStr || !actualStr) return 0;
            const pred = predStr.split('-').map(Number);
            const actual = actualStr.split('-').map(Number);
            if (pred.length !== 2 || actual.length !== 2) return 0;

            if (pred[0] === actual[0] && pred[1] === actual[1]) return 3;
            if (Math.sign(pred[0] - pred[1]) === Math.sign(actual[0] - actual[1])) return 1;
            return 0;
        }

        async function fetchLiveScores() {
            document.getElementById('live-indicator').innerText = "🔄 Sincronizando...";
            try {
                // Mock de actualización automática simulada de la web
                const webMockData = { 1: "2-1", 2: "2-0", 3: "1-1" };
                matches.forEach(m => {
                    if (webMockData[m.id] && !m.actual_score) m.actual_score = webMockData[m.id];
                });
                saveToLocal();
                calculateAndRender();
                document.getElementById('live-indicator').innerText = "● Web Sincronizada";
                setTimeout(() => { document.getElementById('live-indicator').innerText = "● Auto-Sync Activo"; }, 3000);
            } catch (e) {
                document.getElementById('live-indicator').innerText = "⚠ Error de Red";
            }
        }

        function updateMatchScore(id, val) {
            const match = matches.find(m => m.id === id);
            if (match) {
                match.actual_score = val.trim();
                saveToLocal();
                calculateAndRender();
            }
        }

        function saveToLocal() {
            const scoreObj = {};
            matches.forEach(m => scoreObj[m.id] = m.actual_score);
            localStorage.setItem('prode_scores', JSON.stringify(scoreObj));
        }

        function toggleAdmin() {
            const panel = document.getElementById('admin-panel');
            panel.classList.toggle('hidden');
        }

        function calculateAndRender() {
            let leaderboard = participants.map(name => ({ name: name, points: 0, plenos: 0 }));

            matches.forEach(m => {
                if (!m.actual_score) return;
                leaderboard.forEach(player => {
                    const pred = m.predictions[player.name];
                    const pts = getPoints(pred, m.actual_score);
                    player.points += pts;
                    if (pts === 3) player.plenos++;
                });
            });

            leaderboard.sort((a, b) => b.points - a.points || b.plenos - a.plenos);

            const p1 = leaderboard[0] || { name: "-", points: 0 };
            const p2 = leaderboard[1] || { name: "-", points: 0 };
            const p3 = leaderboard[2] || { name: "-", points: 0 };

            document.getElementById('podium-container').innerHTML = `
                <div class="flex flex-col items-center flex-1">
                    <span class="text-xs font-bold text-slate-300 truncate max-w-[80px]">${p2.name}</span>
                    <span class="text-[10px] text-slate-400 mb-1">${p2.points} pts</span>
                    <div class="podium-2 w-full rounded-t-lg flex items-center justify-center shadow-lg">
                        <span class="text-xl font-black text-slate-200">2</span>
                    </div>
                </div>
                <div class="flex flex-col items-center flex-1 z-10">
                    <span class="text-lg mb-0.5">👑</span>
                    <span class="text-sm font-black text-amber-400 truncate max-w-[90px]">${p1.name}</span>
                    <span class="text-xs font-bold text-amber-300 mb-1">${p1.points} pts</span>
                    <div class="podium-1 w-full rounded-t-xl flex items-center justify-center shadow-2xl border-t border-amber-300/30">
                        <span class="text-2xl font-black text-slate-950">1</span>
                    </div>
                </div>
                <div class="flex flex-col items-center flex-1">
                    <span class="text-xs font-bold text-orange-300 truncate max-w-[80px]">${p3.name}</span>
                    <span class="text-[10px] text-orange-400 mb-1">${p3.points} pts</span>
                    <div class="podium-3 w-full rounded-t-lg flex items-center justify-center shadow-lg">
                        <span class="text-lg font-black text-orange-950">3</span>
                    </div>
                </div>
            `;

            let tbodyHtml = "";
            leaderboard.forEach((player, index) => {
                let medal = index + 1;
                if (index === 0) medal = "🥇";
                else if (index === 1) medal = "🥈";
                else if (index === 2) medal = "🥉";
                
                tbodyHtml += `
                    <tr class="${player.name === 'ALAN' ? 'bg-amber-500/10 font-bold text-amber-300' : ''} hover:bg-slate-900 transition">
                        <td class="py-2.5 text-center font-semibold text-slate-400">${medal}</td>
                        <td class="py-2.5 px-2 font-medium">${player.name}</td>
                        <td class="py-2.5 text-center text-xs text-slate-400">${player.plenos} 🎯</td>
                        <td class="py-2.5 px-3 text-right font-bold text-slate-100">${player.points} pts</td>
                    </tr>
                `;
            });
            document.getElementById('leaderboard-body').innerHTML = tbodyHtml;

            let matchesHtml = "";
            let adminHtml = "";

            matches.forEach(m => {
                const teams = m.match.split('/');
                const homeTeam = teams[0] || "Por definir";
                const awayTeam = teams[1] || "Por definir";
                const scoreDisplay = m.actual_score ? m.actual_score : "VS";
                const isFinished = m.actual_score !== "";

                let playerBetsHtml = "";
                participants.forEach(pName => {
                    const prediction = m.predictions[pName] || "N/A";
                    let badgeColor = "bg-slate-800 text-slate-400 border-slate-700";
                    let pointsEarned = "";
                    
                    if (isFinished && prediction !== "N/A") {
                        const pts = getPoints(prediction, m.actual_score);
                        if (pts === 3) {
                            badgeColor = "bg-emerald-950/80 text-emerald-300 border-emerald-500/50 font-bold";
                            pointsEarned = " (+3)";
                        } else if (pts === 1) {
                            badgeColor = "bg-amber-950/60 text-amber-300 border-amber-600/40";
                            pointsEarned = " (+1)";
                        } else {
                            badgeColor = "bg-rose-950/30 text-rose-400/70 border-rose-900/30 line-through";
                        }
                    }

                    playerBetsHtml += `
                        <div class="flex items-center justify-between border border-slate-800/40 p-1.5 rounded-lg text-[11px] ${badgeColor}">
                            <span class="truncate max-w-[75px]">${pName}</span>
                            <span class="font-mono font-bold">${prediction}${pointsEarned}</span>
                        </div>
                    `;
                });

                matchesHtml += `
                    <div class="bg-slate-900 border border-slate-800 rounded-2xl p-3 shadow-md space-y-3">
                        <div class="flex justify-between items-center bg-slate-950/60 py-1.5 px-3 rounded-xl border border-slate-800/60">
                            <span class="text-[10px] font-bold text-slate-500 tracking-wider">PARTIDO #${m.id}</span>
                            <span class="${isFinished ? 'bg-emerald-500/10 text-emerald-400 border-emerald-500/20' : 'bg-amber-500/10 text-amber-400 border-amber-500/20 animate-pulse'} text-[9px] font-black uppercase px-2 py-0.5 rounded-md border">
                                ${isFinished ? 'Finalizado' : 'Pendiente'}
                            </span>
                        </div>
                        
                        <div class="grid grid-cols-3 items-center text-center py-1">
                            <div class="text-xs font-bold text-slate-200 uppercase tracking-tight truncate">${homeTeam}</div>
                            <div class="bg-slate-950 border-2 border-slate-800 px-3 py-1.5 rounded-xl font-mono font-black text-base text-amber-400 shadow-inner tracking-wider">
                                ${scoreDisplay}
                            </div>
                            <div class="text-xs font-bold text-slate-200 uppercase tracking-tight truncate">${awayTeam}</div>
                        </div>

                        <details class="group border-t border-slate-800/50 pt-2">
                            <summary class="list-none flex justify-between items-center text-[11px] text-slate-400 cursor-pointer font-medium select-none hover:text-slate-200">
                                <span>Ver apuestas del grupo</span>
                                <span class="transition group-open:rotate-180 text-slate-500 text-[10px]">🔽</span>
                            </summary>
                            <div class="grid grid-cols-2 gap-1.5 pt-2 max-h-40 overflow-y-auto">
                                ${playerBetsHtml}
                            </div>
                        </details>
                    </div>
                `;

                adminHtml += `
                    <div class="flex items-center justify-between bg-slate-900 p-2 rounded border border-slate-800">
                        <span class="truncate font-semibold text-slate-300">${m.id}. ${m.match}</span>
                        <input type="text" placeholder="Ej: 2-1" value="${m.actual_score}" 
                               onchange="updateMatchScore(${m.id}, this.value)" 
                               class="w-16 bg-slate-950 border border-amber-600/50 rounded p-1 text-center font-mono text-amber-400 focus:outline-none focus:border-amber-400">
                    </div>
                `;
            });

            document.getElementById('matches-container').innerHTML = matchesHtml;
            document.getElementById('admin-matches-list').innerHTML = adminHtml;
        }

        // Ejecución inicial
        calculateAndRender();
        setTimeout(fetchLiveScores, 1000);
    </script>
</body>
</html>
