
<h2 class="sr-only">Bolão da Copa — sistema completo de palpites e ranking</h2>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  #app { font-family: var(--font-sans); min-height: 500px; padding: 0 0 2rem; }
  .header { background: #1a1a2e; color: #fff; padding: 1rem 1.25rem; border-radius: var(--border-radius-lg); margin-bottom: 1.25rem; display: flex; align-items: center; justify-content: space-between; }
  .header-title { font-size: 18px; font-weight: 500; letter-spacing: 0.5px; }
  .header-sub { font-size: 12px; opacity: 0.6; margin-top: 2px; }
  .tabs { display: flex; gap: 6px; margin-bottom: 1.25rem; flex-wrap: wrap; }
  .tab { padding: 7px 14px; border-radius: var(--border-radius-md); border: 0.5px solid var(--color-border-secondary); background: var(--color-background-primary); font-size: 13px; cursor: pointer; color: var(--color-text-secondary); transition: all 0.15s; }
  .tab.active { background: #1a1a2e; color: #fff; border-color: #1a1a2e; }
  .section { display: none; }
  .section.visible { display: block; }
  .card { background: var(--color-background-primary); border: 0.5px solid var(--color-border-tertiary); border-radius: var(--border-radius-lg); padding: 1rem 1.25rem; margin-bottom: 10px; }
  .game-card { display: flex; align-items: center; gap: 10px; }
  .team { flex: 1; text-align: center; }
  .team-flag { font-size: 28px; display: block; margin-bottom: 3px; }
  .team-name { font-size: 13px; color: var(--color-text-secondary); }
  .score-inputs { display: flex; align-items: center; gap: 8px; }
  .score-input { width: 44px; height: 44px; text-align: center; font-size: 20px; font-weight: 500; border-radius: var(--border-radius-md); border: 1px solid var(--color-border-secondary); background: var(--color-background-secondary); color: var(--color-text-primary); }
  .score-sep { font-size: 20px; color: var(--color-text-secondary); }
  .game-meta { font-size: 11px; color: var(--color-text-tertiary); text-align: center; margin-top: 6px; }
  .saved-badge { font-size: 11px; color: #1a7a4a; background: #eaf3de; padding: 2px 8px; border-radius: 99px; margin-left: auto; }
  .btn-primary { background: #1a1a2e; color: #fff; border: none; border-radius: var(--border-radius-md); padding: 9px 20px; font-size: 14px; cursor: pointer; width: 100%; margin-top: 10px; }
  .btn-primary:hover { opacity: 0.88; }
  .ranking-row { display: flex; align-items: center; gap: 10px; padding: 10px 0; border-bottom: 0.5px solid var(--color-border-tertiary); }
  .ranking-row:last-child { border-bottom: none; }
  .rank-pos { width: 28px; font-size: 15px; font-weight: 500; color: var(--color-text-secondary); text-align: center; }
  .rank-pos.gold { color: #ba7517; }
  .rank-pos.silver { color: #888780; }
  .rank-pos.bronze { color: #993c1d; }
  .rank-avatar { width: 36px; height: 36px; border-radius: 50%; background: #1a1a2e; color: #fff; display: flex; align-items: center; justify-content: center; font-size: 13px; font-weight: 500; flex-shrink: 0; }
  .rank-name { flex: 1; font-size: 14px; }
  .rank-pts { font-size: 15px; font-weight: 500; color: #1a1a2e; }
  .rank-detail { font-size: 11px; color: var(--color-text-secondary); }
  .admin-game { padding: 12px 0; border-bottom: 0.5px solid var(--color-border-tertiary); }
  .admin-game:last-child { border-bottom: none; }
  .admin-label { font-size: 12px; color: var(--color-text-secondary); margin-bottom: 6px; }
  .admin-teams { display: flex; align-items: center; gap: 8px; font-size: 14px; }
  .admin-score { display: flex; align-items: center; gap: 6px; margin-top: 8px; }
  .admin-score-in { width: 44px; height: 36px; text-align: center; font-size: 16px; font-weight: 500; border-radius: var(--border-radius-md); border: 1px solid var(--color-border-secondary); background: var(--color-background-secondary); color: var(--color-text-primary); }
  .btn-sm { padding: 6px 14px; font-size: 12px; border-radius: var(--border-radius-md); border: 0.5px solid var(--color-border-secondary); background: var(--color-background-secondary); cursor: pointer; color: var(--color-text-primary); }
  .btn-sm:hover { background: var(--color-background-tertiary); }
  .btn-confirm { background: #1a1a2e; color: #fff; border-color: #1a1a2e; }
  .input-full { width: 100%; padding: 9px 12px; border-radius: var(--border-radius-md); border: 0.5px solid var(--color-border-secondary); background: var(--color-background-secondary); font-size: 14px; color: var(--color-text-primary); margin-bottom: 8px; }
  .alert { padding: 10px 14px; border-radius: var(--border-radius-md); font-size: 13px; margin-bottom: 10px; }
  .alert-success { background: #eaf3de; color: #3b6d11; }
  .alert-error { background: #fcebeb; color: #a32d2d; }
  .pts-badge { display: inline-block; padding: 2px 8px; border-radius: 99px; font-size: 11px; font-weight: 500; }
  .pts-exact { background: #eaf3de; color: #3b6d11; }
  .pts-winner { background: #e6f1fb; color: #185fa5; }
  .pts-draw { background: #faeeda; color: #854f0b; }
  .pts-zero { background: var(--color-background-secondary); color: var(--color-text-secondary); }
  .palpite-detail { font-size: 12px; color: var(--color-text-secondary); margin-top: 4px; }
  .section-title { font-size: 15px; font-weight: 500; margin-bottom: 12px; color: var(--color-text-primary); }
  .user-select { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 12px; }
  .user-chip { padding: 6px 14px; border-radius: 99px; border: 1px solid var(--color-border-secondary); font-size: 13px; cursor: pointer; background: var(--color-background-primary); color: var(--color-text-secondary); }
  .user-chip.active { background: #1a1a2e; color: #fff; border-color: #1a1a2e; }
  .empty-state { text-align: center; padding: 2rem 1rem; color: var(--color-text-secondary); font-size: 14px; }
</style>

<div id="app">
  <div class="header">
    <div>
      <div class="header-title">🏆 Bolão da Adega</div>
      <div class="header-sub" id="header-sub">Copa do Mundo 2026</div>
    </div>
    <div id="user-indicator" style="font-size:13px;opacity:0.8;"></div>
  </div>

  <div id="login-screen">
    <div class="card">
      <p class="section-title">Entrar no bolão</p>
      <div id="existing-users" class="user-select"></div>
      <p style="font-size:12px;color:var(--color-text-secondary);margin-bottom:8px;">Ou cadastre um novo apelido:</p>
      <input class="input-full" id="new-username" placeholder="Seu apelido..." maxlength="20" />
      <div id="login-msg"></div>
      <button class="btn-primary" onclick="doLogin()">Entrar</button>
    </div>
  </div>

  <div id="main-screen" style="display:none;">
    <div class="tabs">
      <button class="tab active" onclick="switchTab('palpites')"><i class="ti ti-pencil" aria-hidden="true"></i> Palpites</button>
      <button class="tab" onclick="switchTab('ranking')"><i class="ti ti-trophy" aria-hidden="true"></i> Ranking</button>
      <button class="tab" onclick="switchTab('meus')"><i class="ti ti-user" aria-hidden="true"></i> Meus Palpites</button>
      <button class="tab" onclick="switchTab('admin')"><i class="ti ti-settings" aria-hidden="true"></i> Admin</button>
    </div>

    <div id="sec-palpites" class="section visible">
      <p class="section-title">Registrar palpites</p>
      <div id="games-list"></div>
      <button class="btn-primary" onclick="savePalpites()">💾 Salvar palpites</button>
      <div id="palpites-msg" style="margin-top:8px;"></div>
    </div>

    <div id="sec-ranking" class="section">
      <p class="section-title">Ranking geral</p>
      <div id="ranking-list"></div>
    </div>

    <div id="sec-meus" class="section">
      <p class="section-title">Meus palpites</p>
      <div id="meus-list"></div>
    </div>

    <div id="sec-admin" class="section">
      <p class="section-title">Admin — inserir resultados</p>
      <div class="card">
        <div id="admin-pass-area">
          <p style="font-size:13px;color:var(--color-text-secondary);margin-bottom:8px;">Senha admin:</p>
          <input class="input-full" id="admin-pass-in" type="password" placeholder="senha..." />
          <button class="btn-primary" onclick="checkAdminPass()">Entrar como admin</button>
          <div id="admin-msg" style="margin-top:8px;"></div>
        </div>
        <div id="admin-panel" style="display:none;">
          <div id="admin-games"></div>
          <div style="margin-top:12px;">
            <p class="section-title" style="margin-bottom:8px;">Gerenciar jogadores</p>
            <div id="admin-users-list"></div>
          </div>
          <div style="margin-top:12px;">
            <p class="section-title" style="margin-bottom:8px;">Adicionar jogo</p>
            <input class="input-full" id="new-home" placeholder="Time casa (ex: Brasil 🇧🇷)" />
            <input class="input-full" id="new-away" placeholder="Time visitante (ex: Argentina 🇦🇷)" />
            <input class="input-full" id="new-date" placeholder="Data (ex: 15/06)" />
            <button class="btn-primary" onclick="addGame()">Adicionar jogo</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
const ADMIN_PASS = "adega2026";
const STORAGE_KEY = "bolao_adega_v3";

function load() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (raw) return JSON.parse(raw);
  } catch(e) {}
  return {
    users: [],
    games: [
      { id: 1, home: "Brasil 🇧🇷", away: "México 🇲🇽", date: "12/06", result: null },
      { id: 2, home: "Argentina 🇦🇷", away: "Alemanha 🇩🇪", date: "13/06", result: null },
      { id: 3, home: "França 🇫🇷", away: "Inglaterra 🏴󠁧󠁢󠁥󠁮󠁧󠁿", date: "14/06", result: null },
      { id: 4, home: "Espanha 🇪🇸", away: "Portugal 🇵🇹", date: "15/06", result: null },
    ],
    palpites: {}
  };
}

function save(data) {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
}

let state = load();
let currentUser = null;
let adminUnlocked = false;

function initLogin() {
  const area = document.getElementById("existing-users");
  area.innerHTML = "";
  if (state.users.length > 0) {
    state.users.forEach(u => {
      const chip = document.createElement("button");
      chip.className = "user-chip";
      chip.textContent = u;
      chip.onclick = () => {
        document.querySelectorAll(".user-chip").forEach(c => c.classList.remove("active"));
        chip.classList.add("active");
        document.getElementById("new-username").value = u;
      };
      area.appendChild(chip);
    });
  }
}

function doLogin() {
  const val = document.getElementById("new-username").value.trim();
  const msg = document.getElementById("login-msg");
  if (!val) { msg.innerHTML = '<div class="alert alert-error">Digite um apelido!</div>'; return; }
  if (!state.users.includes(val)) {
    state.users.push(val);
    save(state);
  }
  currentUser = val;
  document.getElementById("login-screen").style.display = "none";
  document.getElementById("main-screen").style.display = "block";
  document.getElementById("user-indicator").textContent = "👤 " + currentUser;
  renderGames();
  renderRanking();
  renderMeus();
}

function switchTab(t) {
  document.querySelectorAll(".tab").forEach((el, i) => el.classList.remove("active"));
  document.querySelectorAll(".section").forEach(s => s.classList.remove("visible"));
  const tabs = ["palpites","ranking","meus","admin"];
  const idx = tabs.indexOf(t);
  document.querySelectorAll(".tab")[idx].classList.add("active");
  document.getElementById("sec-"+t).classList.add("visible");
  if (t === "ranking") renderRanking();
  if (t === "meus") renderMeus();
  if (t === "admin") renderAdmin();
}

function renderGames() {
  const el = document.getElementById("games-list");
  const myPalpites = state.palpites[currentUser] || {};
  el.innerHTML = "";
  if (state.games.length === 0) { el.innerHTML = '<div class="empty-state">Nenhum jogo cadastrado ainda.</div>'; return; }
  state.games.forEach(g => {
    const saved = myPalpites[g.id];
    const card = document.createElement("div");
    card.className = "card";
    const locked = !!g.result;
    card.innerHTML = `
      <div class="game-card">
        <div class="team">
          <span class="team-name">${g.home}</span>
        </div>
        <div class="score-inputs">
          <input class="score-input" type="number" min="0" max="20" id="ph_${g.id}" value="${saved ? saved[0] : ''}" placeholder="?" ${locked ? 'disabled' : ''} />
          <span class="score-sep">×</span>
          <input class="score-input" type="number" min="0" max="20" id="pa_${g.id}" value="${saved ? saved[1] : ''}" placeholder="?" ${locked ? 'disabled' : ''} />
        </div>
        <div class="team">
          <span class="team-name">${g.away}</span>
        </div>
        ${saved ? '<span class="saved-badge">✓</span>' : ''}
      </div>
      <div class="game-meta">${g.date}${g.result ? ' · Resultado: '+g.result[0]+'×'+g.result[1] : ''}${locked ? ' · <span style="color:#a32d2d;font-size:11px;">Encerrado</span>' : ''}</div>
    `;
    el.appendChild(card);
  });
}

function savePalpites() {
  const msg = document.getElementById("palpites-msg");
  if (!state.palpites[currentUser]) state.palpites[currentUser] = {};
  let saved = 0;
  state.games.forEach(g => {
    if (g.result) return;
    const h = document.getElementById("ph_"+g.id);
    const a = document.getElementById("pa_"+g.id);
    if (h && a && h.value !== "" && a.value !== "") {
      state.palpites[currentUser][g.id] = [parseInt(h.value), parseInt(a.value)];
      saved++;
    }
  });
  save(state);
  msg.innerHTML = saved > 0
    ? `<div class="alert alert-success">✓ ${saved} palpite(s) salvo(s)!</div>`
    : `<div class="alert alert-error">Preencha os placares antes de salvar.</div>`;
  renderGames();
}

function calcPoints(palpite, result) {
  if (!palpite || !result) return { pts: 0, type: "zero" };
  const [ph, pa] = palpite;
  const [rh, ra] = result;
  if (ph === rh && pa === ra) return { pts: 10, type: "exact" };
  const pWinner = ph > pa ? "h" : ph < pa ? "a" : "d";
  const rWinner = rh > ra ? "h" : rh < ra ? "a" : "d";
  if (pWinner === rWinner) {
    return pWinner === "d" ? { pts: 3, type: "draw" } : { pts: 5, type: "winner" };
  }
  return { pts: 0, type: "zero" };
}

function getUserScore(user) {
  let total = 0, exact = 0, winner = 0;
  const myP = state.palpites[user] || {};
  state.games.forEach(g => {
    if (!g.result) return;
    const r = calcPoints(myP[g.id], g.result);
    total += r.pts;
    if (r.type === "exact") exact++;
    if (r.type === "winner" || r.type === "draw") winner++;
  });
  return { total, exact, winner };
}

function renderRanking() {
  const el = document.getElementById("ranking-list");
  if (state.users.length === 0) { el.innerHTML = '<div class="empty-state">Nenhum participante ainda.</div>'; return; }
  const ranked = state.users.map(u => ({ user: u, ...getUserScore(u) }));
  ranked.sort((a,b) => b.total - a.total || b.exact - a.exact);
  const posClass = ["gold","silver","bronze"];
  const posEmoji = ["🥇","🥈","🥉"];
  el.innerHTML = "";
  const card = document.createElement("div");
  card.className = "card";
  ranked.forEach((r, i) => {
    const initials = r.user.substring(0,2).toUpperCase();
    const row = document.createElement("div");
    row.className = "ranking-row";
    row.innerHTML = `
      <span class="rank-pos ${posClass[i]||''}">${posEmoji[i] || (i+1)}</span>
      <div class="rank-avatar" style="background:${avatarColor(r.user)}">${initials}</div>
      <div style="flex:1">
        <div class="rank-name">${r.user}${r.user===currentUser?' <span style="font-size:11px;color:var(--color-text-secondary)">(você)</span>':''}</div>
        <div class="rank-detail">${r.exact} placares exatos · ${r.winner} vencedores</div>
      </div>
      <div>
        <div class="rank-pts">${r.total} pts</div>
      </div>
    `;
    card.appendChild(row);
  });
  el.appendChild(card);
}

function avatarColor(name) {
  const colors = ["#1a1a2e","#185fa5","#3b6d11","#854f0b","#993556","#0f6e56","#993c1d"];
  let h = 0;
  for(let c of name) h = (h*31 + c.charCodeAt(0)) % colors.length;
  return colors[h];
}

function renderMeus() {
  const el = document.getElementById("meus-list");
  el.innerHTML = "";
  const myP = state.palpites[currentUser] || {};
  const finishedGames = state.games.filter(g => g.result);
  const pendingGames = state.games.filter(g => !g.result);
  
  if (state.games.length === 0) { el.innerHTML = '<div class="empty-state">Nenhum jogo cadastrado.</div>'; return; }

  if (finishedGames.length > 0) {
    const card = document.createElement("div");
    card.className = "card";
    card.innerHTML = '<div style="font-size:13px;font-weight:500;margin-bottom:8px;color:var(--color-text-secondary);">Jogos finalizados</div>';
    finishedGames.forEach(g => {
      const r = calcPoints(myP[g.id], g.result);
      const palStr = myP[g.id] ? myP[g.id][0]+"×"+myP[g.id][1] : "—";
      const badgeMap = { exact:"pts-exact", winner:"pts-winner", draw:"pts-draw", zero:"pts-zero" };
      const labMap = { exact:"Placar exato!", winner:"Acertou o vencedor", draw:"Acertou o empate", zero:"Errou" };
      const div = document.createElement("div");
      div.style = "padding:10px 0;border-bottom:0.5px solid var(--color-border-tertiary);";
      div.innerHTML = `
        <div style="display:flex;align-items:center;gap:8px;">
          <span style="flex:1;font-size:13px;">${g.home} × ${g.away}</span>
          <span class="pts-badge ${badgeMap[r.type]}">+${r.pts}pts</span>
        </div>
        <div class="palpite-detail">Palpite: <b>${palStr}</b> · Real: <b>${g.result[0]}×${g.result[1]}</b> · ${labMap[r.type]}</div>
      `;
      card.appendChild(div);
    });
    const score = getUserScore(currentUser);
    const summary = document.createElement("div");
    summary.style = "margin-top:10px;display:flex;gap:12px;flex-wrap:wrap;";
    summary.innerHTML = `
      <div style="background:var(--color-background-secondary);padding:8px 14px;border-radius:var(--border-radius-md);text-align:center;">
        <div style="font-size:11px;color:var(--color-text-secondary);">Total</div>
        <div style="font-size:20px;font-weight:500;">${score.total} pts</div>
      </div>
      <div style="background:var(--color-background-secondary);padding:8px 14px;border-radius:var(--border-radius-md);text-align:center;">
        <div style="font-size:11px;color:var(--color-text-secondary);">Placares exatos</div>
        <div style="font-size:20px;font-weight:500;">${score.exact}</div>
      </div>
    `;
    card.appendChild(summary);
    el.appendChild(card);
  }

  if (pendingGames.length > 0) {
    const card2 = document.createElement("div");
    card2.className = "card";
    card2.innerHTML = '<div style="font-size:13px;font-weight:500;margin-bottom:8px;color:var(--color-text-secondary);">Próximos jogos</div>';
    pendingGames.forEach(g => {
      const palStr = myP[g.id] ? myP[g.id][0]+"×"+myP[g.id][1] : "sem palpite";
      const div = document.createElement("div");
      div.style = "padding:8px 0;border-bottom:0.5px solid var(--color-border-tertiary);font-size:13px;";
      div.innerHTML = `<span>${g.home} × ${g.away}</span> <span style="color:var(--color-text-secondary);float:right;">${palStr}</span>`;
      card2.appendChild(div);
    });
    el.appendChild(card2);
  }
}

function checkAdminPass() {
  const p = document.getElementById("admin-pass-in").value;
  const msg = document.getElementById("admin-msg");
  if (p === ADMIN_PASS) {
    adminUnlocked = true;
    document.getElementById("admin-pass-area").style.display = "none";
    document.getElementById("admin-panel").style.display = "block";
    renderAdmin();
  } else {
    msg.innerHTML = '<div class="alert alert-error">Senha incorreta.</div>';
  }
}

function renderAdmin() {
  if (!adminUnlocked) return;
  const el = document.getElementById("admin-games");
  el.innerHTML = '<div style="font-size:13px;font-weight:500;margin-bottom:8px;color:var(--color-text-secondary);">Inserir/editar resultados</div>';
  state.games.forEach(g => {
    const div = document.createElement("div");
    div.className = "admin-game";
    div.innerHTML = `
      <div class="admin-teams">${g.home} × ${g.away} <span style="font-size:11px;color:var(--color-text-secondary);margin-left:4px;">${g.date}</span></div>
      <div class="admin-score">
        <input class="admin-score-in" type="number" id="ar_h_${g.id}" min="0" max="20" value="${g.result ? g.result[0] : ''}" placeholder="?" />
        <span style="font-size:16px;color:var(--color-text-secondary);">×</span>
        <input class="admin-score-in" type="number" id="ar_a_${g.id}" min="0" max="20" value="${g.result ? g.result[1] : ''}" placeholder="?" />
        <button class="btn-sm btn-confirm" onclick="setResult(${g.id})">Confirmar</button>
        ${g.result ? '<button class="btn-sm" onclick="clearResult('+g.id+')">Limpar</button>' : ''}
      </div>
    `;
    el.appendChild(div);
  });

  const usersEl = document.getElementById("admin-users-list");
  usersEl.innerHTML = "";
  state.users.forEach(u => {
    const row = document.createElement("div");
    row.style = "display:flex;align-items:center;gap:8px;padding:6px 0;border-bottom:0.5px solid var(--color-border-tertiary);font-size:13px;";
    row.innerHTML = `<span style="flex:1;">${u}</span><button class="btn-sm" onclick="removeUser('${u}')">Remover</button>`;
    usersEl.appendChild(row);
  });
}

function setResult(id) {
  const h = document.getElementById("ar_h_"+id);
  const a = document.getElementById("ar_a_"+id);
  if (!h || !a || h.value === "" || a.value === "") return;
  const g = state.games.find(x => x.id === id);
  if (g) { g.result = [parseInt(h.value), parseInt(a.value)]; save(state); renderAdmin(); renderGames(); }
}

function clearResult(id) {
  const g = state.games.find(x => x.id === id);
  if (g) { g.result = null; save(state); renderAdmin(); renderGames(); }
}

function addGame() {
  const home = document.getElementById("new-home").value.trim();
  const away = document.getElementById("new-away").value.trim();
  const date = document.getElementById("new-date").value.trim();
  if (!home || !away) return;
  const id = Date.now();
  state.games.push({ id, home, away, date: date || "—", result: null });
  save(state);
  document.getElementById("new-home").value = "";
  document.getElementById("new-away").value = "";
  document.getElementById("new-date").value = "";
  renderAdmin();
  renderGames();
}

function removeUser(u) {
  if (u === currentUser) { alert("Não pode remover o usuário logado!"); return; }
  state.users = state.users.filter(x => x !== u);
  delete state.palpites[u];
  save(state);
  renderAdmin();
  renderRanking();
}

initLogin();
</script>
