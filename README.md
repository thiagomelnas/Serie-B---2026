# Serie-B---2026
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard · Série B 2025</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;600;700;900&family=Barlow:wght@300;400;500&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<style>
  :root {
    --bg: #0B0F1A;
    --bg2: #111827;
    --bg3: #1a2235;
    --border: rgba(255,255,255,0.06);
    --green: #22c55e;
    --green-dim: rgba(34,197,94,0.15);
    --red: #ef4444;
    --red-dim: rgba(239,68,68,0.15);
    --yellow: #eab308;
    --text: #f1f5f9;
    --muted: #64748b;
    --accent: #3b82f6;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Barlow', sans-serif;
    font-size: 14px;
    min-height: 100vh;
    padding: 24px;
  }

  /* ── HEADER ── */
  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 28px;
  }
  .header-left { display: flex; align-items: center; gap: 14px; }
  .badge {
    background: var(--green);
    color: #000;
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 900;
    font-size: 11px;
    letter-spacing: 1px;
    padding: 4px 10px;
    border-radius: 4px;
  }
  h1 {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 900;
    font-size: 26px;
    letter-spacing: 1px;
    text-transform: uppercase;
  }
  h1 span { color: var(--green); }
  .header-meta { color: var(--muted); font-size: 12px; }

  /* ── GRID SYSTEM ── */
  .grid { display: grid; gap: 16px; }
  .grid-4 { grid-template-columns: repeat(4, 1fr); }
  .grid-2 { grid-template-columns: 1fr 1fr; }
  .grid-3 { grid-template-columns: 2fr 1fr; }

  /* ── CARD ── */
  .card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 18px 20px;
    position: relative;
    overflow: hidden;
  }
  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
  }
  .card.green::before { background: var(--green); }
  .card.red::before { background: var(--red); }
  .card.blue::before { background: var(--accent); }
  .card.yellow::before { background: var(--yellow); }
  .card-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 600;
    font-size: 11px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 10px;
  }
  .card-value {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 900;
    font-size: 32px;
    line-height: 1;
  }
  .card-value.green { color: var(--green); }
  .card-value.red { color: var(--red); }
  .card-sub { color: var(--muted); font-size: 12px; margin-top: 4px; }

  /* ── TABLE ── */
  .table-wrap { overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; }
  thead th {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 11px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--muted);
    padding: 8px 10px;
    text-align: center;
    border-bottom: 1px solid var(--border);
  }
  thead th:nth-child(2) { text-align: left; }
  tbody tr {
    border-bottom: 1px solid var(--border);
    transition: background 0.15s;
  }
  tbody tr:hover { background: rgba(255,255,255,0.03); }
  tbody td {
    padding: 9px 10px;
    text-align: center;
    font-size: 13px;
  }
  tbody td:nth-child(2) { text-align: left; font-weight: 500; }
  .pos-badge {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 24px; height: 24px;
    border-radius: 5px;
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 700;
    font-size: 13px;
  }
  .pos-g4 { background: var(--green-dim); color: var(--green); }
  .pos-z4 { background: var(--red-dim); color: var(--red); }
  .pos-mid { background: rgba(255,255,255,0.05); color: var(--muted); }
  .pts { font-weight: 700; color: var(--text); }
  .aproveitamento-bar {
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .bar-track {
    flex: 1;
    height: 4px;
    background: var(--border);
    border-radius: 2px;
    overflow: hidden;
  }
  .bar-fill { height: 100%; border-radius: 2px; }

  /* ── CHART WRAPPER ── */
  .chart-container { position: relative; }

  /* ── ZONE ALERT ── */
  .z4-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
  .z4-card {
    background: var(--red-dim);
    border: 1px solid rgba(239,68,68,0.2);
    border-radius: 8px;
    padding: 12px 14px;
  }
  .z4-pos {
    font-family: 'Barlow Condensed', sans-serif;
    font-weight: 900;
    font-size: 28px;
    color: var(--red);
    line-height: 1;
  }
  .z4-team { font-weight: 500; font-size: 13px; margin: 4px 0 2px; }
  .z4-pts { font-size: 12px; color: var(--muted); }
  .alert-header {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 14px;
  }
  .alert-icon {
    width: 28px; height: 28px;
    background: var(--red-dim);
    border: 1px solid rgba(239,68,68,0.3);
    border-radius: 6px;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px;
  }

  @media (max-width: 900px) {
    .grid-4 { grid-template-columns: 1fr 1fr; }
    .grid-2, .grid-3 { grid-template-columns: 1fr; }
    .z4-grid { grid-template-columns: 1fr 1fr; }
  }
</style>
</head>
<body>

<!-- HEADER -->
<div class="header">
  <div class="header-left">
    <span class="badge">⚽ SÉRIE B</span>
    <h1>Classificação <span>2025</span></h1>
  </div>
  <div class="header-meta">4ª Rodada · 20 Times</div>
</div>

<!-- BLOCO 1: KPIs -->
<div class="grid grid-4" style="margin-bottom:16px">
  <div class="card green">
    <div class="card-title">Líder</div>
    <div class="card-value" style="font-size:22px">Ceará SC</div>
    <div class="card-sub">1º lugar · 8 pts</div>
  </div>
  <div class="card blue">
    <div class="card-title">Maior Pontuação</div>
    <div class="card-value">8</div>
    <div class="card-sub">5 times empatados</div>
  </div>
  <div class="card yellow">
    <div class="card-title">Média de Gols</div>
    <div class="card-value" id="mediaGols">—</div>
    <div class="card-sub">por time (4 rodadas)</div>
  </div>
  <div class="card" style="border-top:2px solid var(--muted)">
    <div class="card-title">Total de Times</div>
    <div class="card-value">20</div>
    <div class="card-sub">Campeonato Brasileiro</div>
  </div>
</div>

<!-- BLOCO 2: TABELA PRINCIPAL -->
<div class="card" style="margin-bottom:16px">
  <div class="card-title" style="margin-bottom:14px">Classificação</div>
  <div class="table-wrap">
    <table id="mainTable">
      <thead>
        <tr>
          <th>#</th>
          <th>Time</th>
          <th>Pts</th>
          <th>J</th>
          <th>V</th>
          <th>E</th>
          <th>D</th>
          <th>Gols</th>
          <th>Aproveito.</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>
</div>

<!-- BLOCO 3+4: Ataque + Scatter -->
<div class="grid grid-2" style="margin-bottom:16px">
  <div class="card">
    <div class="card-title" style="margin-bottom:14px">Ataque · Gols Marcados</div>
    <div class="chart-container" style="height:280px">
      <canvas id="barChart"></canvas>
    </div>
  </div>
  <div class="card">
    <div class="card-title" style="margin-bottom:14px">Eficiência · Gols × Pontos</div>
    <div class="chart-container" style="height:280px">
      <canvas id="scatterChart"></canvas>
    </div>
  </div>
</div>

<!-- BLOCO 5: CONSISTÊNCIA -->
<div class="card" style="margin-bottom:16px">
  <div class="card-title" style="margin-bottom:14px">Consistência · Resultado por Time</div>
  <div class="chart-container" style="height:260px">
    <canvas id="stackedChart"></canvas>
  </div>
</div>

<!-- BLOCO 6: ZONA DE RISCO -->
<div class="card red">
  <div class="alert-header">
    <div class="alert-icon">🚨</div>
    <div>
      <div style="font-family:'Barlow Condensed',sans-serif;font-weight:700;font-size:14px;letter-spacing:1px">ZONA DE REBAIXAMENTO · Z4</div>
      <div style="font-size:11px;color:var(--muted)">Posições 17 a 20</div>
    </div>
  </div>
  <div class="z4-grid" id="z4Grid"></div>
</div>

<script>
const data = [
  {pos:1,time:"Ceará SC",pts:8,j:4,v:2,e:2,d:0,gols:5},
  {pos:2,time:"Avaí",pts:8,j:4,v:2,e:2,d:0,gols:5},
  {pos:3,time:"Athletic",pts:8,j:4,v:2,e:2,d:0,gols:7},
  {pos:4,time:"Vila Nova",pts:8,j:4,v:2,e:2,d:0,gols:6},
  {pos:5,time:"Operário",pts:8,j:4,v:2,e:2,d:0,gols:3},
  {pos:6,time:"Goiás",pts:7,j:4,v:2,e:1,d:1,gols:6},
  {pos:7,time:"Criciúma",pts:7,j:4,v:2,e:1,d:1,gols:3},
  {pos:8,time:"Fortaleza",pts:7,j:4,v:2,e:1,d:1,gols:3},
  {pos:9,time:"Botafogo SP",pts:6,j:4,v:2,e:0,d:2,gols:7},
  {pos:10,time:"Náutico",pts:6,j:4,v:2,e:0,d:2,gols:3},
  {pos:11,time:"Sport Recife",pts:6,j:4,v:1,e:3,d:0,gols:5},
  {pos:12,time:"Novorizontino",pts:5,j:4,v:1,e:2,d:1,gols:5},
  {pos:13,time:"Londrina",pts:4,j:4,v:1,e:1,d:2,gols:7},
  {pos:14,time:"São Bernardo",pts:4,j:4,v:1,e:1,d:2,gols:4},
  {pos:15,time:"Juventude",pts:4,j:4,v:1,e:1,d:2,gols:3},
  {pos:16,time:"Atlético GO",pts:3,j:4,v:1,e:0,d:3,gols:4},
  {pos:17,time:"Cuiabá",pts:3,j:4,v:0,e:3,d:1,gols:0},
  {pos:18,time:"CRB",pts:2,j:4,v:0,e:2,d:2,gols:5},
  {pos:19,time:"Ponte Preta",pts:1,j:4,v:0,e:1,d:3,gols:2},
  {pos:20,time:"América MG",pts:1,j:4,v:0,e:1,d:3,gols:3},
];

const mediaGols = (data.reduce((s,d)=>s+d.gols,0)/data.length).toFixed(2);
document.getElementById('mediaGols').textContent = mediaGols;

// TABLE
const tbody = document.querySelector('#mainTable tbody');
data.forEach(r => {
  const aprov = (r.pts/(r.j*3)*100).toFixed(0);
  const color = aprov >= 60 ? '#22c55e' : aprov >= 40 ? '#eab308' : '#ef4444';
  const posClass = r.pos <= 4 ? 'pos-g4' : r.pos >= 17 ? 'pos-z4' : 'pos-mid';
  tbody.innerHTML += `
    <tr>
      <td><span class="pos-badge ${posClass}">${r.pos}</span></td>
      <td>${r.time}</td>
      <td class="pts">${r.pts}</td>
      <td>${r.j}</td>
      <td>${r.v}</td>
      <td>${r.e}</td>
      <td>${r.d}</td>
      <td>${r.gols}</td>
      <td>
        <div class="aproveitamento-bar">
          <div class="bar-track"><div class="bar-fill" style="width:${aprov}%;background:${color}"></div></div>
          <span style="font-size:11px;color:${color};min-width:28px">${aprov}%</span>
        </div>
      </td>
    </tr>`;
});

// Z4
const z4 = document.getElementById('z4Grid');
data.filter(r=>r.pos>=17).forEach(r=>{
  z4.innerHTML += `
    <div class="z4-card">
      <div class="z4-pos">${r.pos}º</div>
      <div class="z4-team">${r.time}</div>
      <div class="z4-pts">${r.pts} pts · ${r.gols} gols</div>
    </div>`;
});

const cfg = {
  plugins: { legend: { display: false } },
  scales: {
    x: { ticks: { color: '#64748b', font: { size: 10 } }, grid: { color: 'rgba(255,255,255,0.04)' } },
    y: { ticks: { color: '#64748b', font: { size: 10 } }, grid: { color: 'rgba(255,255,255,0.06)' } }
  }
};

// BAR CHART — sorted desc by gols
const sorted = [...data].sort((a,b)=>b.gols-a.gols);
new Chart(document.getElementById('barChart'), {
  type: 'bar',
  data: {
    labels: sorted.map(d=>d.time),
    datasets: [{
      data: sorted.map(d=>d.gols),
      backgroundColor: sorted.map(d => d.pos<=4?'rgba(34,197,94,0.7)': d.pos>=17?'rgba(239,68,68,0.7)':'rgba(59,130,246,0.55)'),
      borderRadius: 4,
      borderSkipped: false,
    }]
  },
  options: {
    indexAxis: 'y',
    plugins: { legend:{ display:false } },
    scales: {
      x: { ticks: { color:'#64748b', font:{size:10} }, grid:{ color:'rgba(255,255,255,0.05)' } },
      y: { ticks: { color:'#94a3b8', font:{size:10} }, grid:{ display:false } }
    }
  }
});

// SCATTER
const scatterColors = data.map(d => d.pos<=4?'#22c55e': d.pos>=17?'#ef4444':'#3b82f6');
new Chart(document.getElementById('scatterChart'), {
  type: 'scatter',
  data: {
    datasets: data.map((d,i) => ({
      label: d.time,
      data: [{ x: d.gols, y: d.pts }],
      backgroundColor: scatterColors[i] + 'cc',
      borderColor: scatterColors[i],
      borderWidth: 1,
      pointRadius: 5 + Math.round((d.pts / (d.j*3)) * 8),
    }))
  },
  options: {
    plugins: {
      legend: { display: false },
      tooltip: {
        callbacks: {
          label: ctx => ${ctx.dataset.label}: ${ctx.parsed.x} gols, ${ctx.parsed.y} pts
        }
      }
    },
    scales: {
      x: { title:{ display:true, text:'Gols Marcados', color:'#64748b', font:{size:11} }, ticks:{ color:'#64748b' }, grid:{ color:'rgba(255,255,255,0.05)' } },
      y: { title:{ display:true, text:'Pontos', color:'#64748b', font:{size:11} }, ticks:{ color:'#64748b' }, grid:{ color:'rgba(255,255,255,0.05)' } }
    }
  }
});

// STACKED BAR
new Chart(document.getElementById('stackedChart'), {
  type: 'bar',
  data: {
    labels: data.map(d=>d.time),
    datasets: [
      { label:'Vitórias', data: data.map(d=>d.v), backgroundColor:'rgba(34,197,94,0.75)', borderRadius:2 },
      { label:'Empates',  data: data.map(d=>d.e), backgroundColor:'rgba(234,179,8,0.65)', borderRadius:2 },
      { label:'Derrotas', data: data.map(d=>d.d), backgroundColor:'rgba(239,68,68,0.65)', borderRadius:2 },
    ]
  },
  options: {
    plugins: {
      legend: {
        display: true,
        position: 'top',
        labels: { color:'#94a3b8', font:{size:11}, boxWidth:12, padding:16 }
      }
    },
    scales: {
      x: { stacked:true, ticks:{ color:'#64748b', font:{size:10} }, grid:{ display:false } },
      y: { stacked:true, ticks:{ color:'#64748b', font:{size:10} }, grid:{ color:'rgba(255,255,255,0.05)' }, max:4 }
    }
  }
});
</script>
</body>
</html>
