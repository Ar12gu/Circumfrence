Circumfrence
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Circle Area & Circumference Calculator</title>
  <style>
    :root {
      --bg: #0f172a;          /* slate-900 */
      --panel: #111827;       /* gray-900 */
      --muted: #94a3b8;       /* slate-400 */
      --text: #e5e7eb;        /* gray-200 */
      --accent: #38bdf8;      /* sky-400 */
      --accent-2: #22d3ee;    /* cyan-400 */
      --danger: #ef4444;
      --ring: rgba(56,189,248,.4);
    }
    * { box-sizing: border-box; }
    html, body { height: 100%; }
    body {
      margin: 0;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Inter, Arial, sans-serif;
      background: radial-gradient(1200px 800px at 80% -10%, rgba(34,211,238,.08), transparent),
                  radial-gradient(900px 600px at -10% 120%, rgba(56,189,248,.08), transparent),
                  var(--bg);
      color: var(--text);
      display: grid;
      place-items: center;
      padding: 24px;
    }
    .card {
      width: min(680px, 100%);
      background: linear-gradient(180deg, rgba(255,255,255,.06), rgba(255,255,255,.02));
      border: 1px solid rgba(255,255,255,.08);
      border-radius: 20px;
      box-shadow: 0 30px 80px rgba(0,0,0,.35);
      overflow: hidden;
    }
    header {
      padding: 22px 26px;
      background: linear-gradient(135deg, rgba(34,211,238,.12), rgba(56,189,248,.12));
      border-bottom: 1px solid rgba(255,255,255,.08);
    }
    header h1 {
      margin: 0 0 6px 0;
      font-size: 1.35rem;
      letter-spacing: .3px;
    }
    header p { margin: 0; color: var(--muted); font-size: .95rem; }
    main { padding: 24px; display: grid; gap: 18px; }

    .row { display: grid; gap: 10px; }
    @media (min-width: 640px) {
      .grid-2 { grid-template-columns: 1fr 1fr; gap: 16px; }
    }

    label { font-size: .95rem; color: var(--muted); }

    .field { position: relative; }
    input[type="number"], select, input[type="text"] {
      width: 100%;
      padding: 12px 14px;
      border-radius: 12px;
      border: 1px solid rgba(255,255,255,.12);
      background: rgba(17,24,39,.6);
      color: var(--text);
      outline: none;
      transition: box-shadow .2s ease, border-color .2s ease, transform .06s ease;
    }
    input:focus, select:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 6px var(--ring);
    }
    .hint { font-size: .85rem; color: var(--muted); margin-top: 6px; }
    .error { color: var(--danger); font-size: .9rem; display: none; }

    .actions { display: flex; gap: 10px; flex-wrap: wrap; margin-top: 4px; }
    button {
      padding: 11px 14px;
      border-radius: 12px;
      border: 1px solid rgba(255,255,255,.12);
      background: linear-gradient(180deg, rgba(56,189,248,.18), rgba(56,189,248,.08));
      color: var(--text);
      cursor: pointer;
      font-weight: 600;
      transition: transform .05s ease, filter .2s ease, background .2s ease;
    }
    button:hover { filter: brightness(1.08); }
    button:active { transform: translateY(1px); }
    .ghost { background: transparent; }

    .results { display: grid; gap: 12px; margin-top: 8px; }
    .result {
      padding: 14px;
      border: 1px solid rgba(255,255,255,.1);
      border-radius: 14px;
      background: rgba(17,24,39,.5);
      display: grid;
      gap: 4px;
    }
    .kicker { font-size: .8rem; color: var(--muted); text-transform: uppercase; letter-spacing: .12em; }
    .value { font-size: 1.2rem; font-variant-numeric: tabular-nums; }
    .formula { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace; font-size: .95rem; color: var(--muted); }
    footer { padding: 0 24px 22px; color: var(--muted); font-size: .85rem; }
    .sep { opacity: .4; margin: 0 .4em; }
  </style>
</head>
<body>
  <div class="card" role="application">
    <header>
      <h1>Circle Area & Circumference</h1>
      <p>Enter a radius (or diameter) and get exact & rounded results. Uses <span class="formula">π = Math.PI</span>.</p>
    </header>

    <main>
      <div class="row grid-2">
        <div>
          <label for="mode">Input Mode</label>
          <div class="field">
            <select id="mode" aria-label="Choose whether you are entering radius or diameter">
              <option value="radius" selected>Radius (r)</option>
              <option value="diameter">Diameter (d)</option>
            </select>
          </div>
          <div class="hint">Switch to diameter if that's what you know.</div>
        </div>

        <div>
          <label for="unit">Units</label>
          <div class="field">
            <select id="unit" aria-label="Measurement units">
              <option value="cm" selected>cm</option>
              <option value="m">m</option>
              <option value="mm">mm</option>
              <option value="in">in</option>
              <option value="ft">ft</option>
            </select>
          </div>
        </div>
      </div>

      <div class="row grid-2">
        <div>
          <label for="value">Value</label>
          <div class="field">
            <input id="value" type="number" min="0" step="any" placeholder="e.g., 7.5" aria-describedby="err" />
          </div>
          <div id="err" class="error" role="alert">Enter a positive number.</div>
        </div>

        <div>
          <label for="precision">Decimals</label>
          <div class="field">
            <input id="precision" type="number" min="0" max="10" value="2" />
          </div>
          <div class="hint">Number of decimal places for rounded results.</div>
        </div>
      </div>

      <div class="actions">
        <button id="calc">Calculate</button>
        <button id="clear" class="ghost">Clear</button>
      </div>

      <section class="results" aria-live="polite">
        <div class="result">
          <div class="kicker">Circumference (C)</div>
          <div class="value" id="circ-val">—</div>
          <div class="formula">C = 2πr</div>
        </div>
        <div class="result">
          <div class="kicker">Area (A)</div>
          <div class="value" id="area-val">—</div>
          <div class="formula">A = πr²</div>
        </div>
      </section>
    </main>

    <footer>
      <span>Tip: You can type scientific notation like <code>1e-3</code> for 0.001.</span>
      <span class="sep">•</span>
      <span>Exact values use full <code>Math.PI</code>; rounded uses your decimals.</span>
    </footer>
  </div>

  <script>
    const byId = (id) => document.getElementById(id);
    const modeEl = byId('mode');
    const unitEl = byId('unit');
    const valueEl = byId('value');
    const precEl = byId('precision');
    const circEl = byId('circ-val');
    const areaEl = byId('area-val');
    const errEl = byId('err');
    const calcBtn = byId('calc');
    const clearBtn = byId('clear');

    function format(val, unit, dp) {
      if (!isFinite(val)) return '—';
      const rounded = (typeof dp === 'number') ? val.toFixed(dp) : String(val);
      return `${rounded} ${unit}`;
    }

    function update() {
      const mode = modeEl.value;         // 'radius' or 'diameter'
      const unit = unitEl.value;         // unit label
      const raw = Number(valueEl.value);
      const dp = Math.min(10, Math.max(0, Number(precEl.value || 0)));

      if (!raw || raw <= 0) {
        errEl.style.display = valueEl.value ? 'block' : 'none';
        circEl.textContent = '—';
        areaEl.textContent = '—';
        return;
      }
      errEl.style.display = 'none';

      const r = mode === 'radius' ? raw : raw / 2;
      const C = 2 * Math.PI * r;
      const A = Math.PI * r * r;

      // exact strings
      const unitLen = unit;
      const unitArea = `${unit}²`;

      circEl.textContent = `${format(C, unitLen, dp)}  (exact: ${C} ${unitLen})`;
      areaEl.textContent = `${format(A, unitArea, dp)}  (exact: ${A} ${unitArea})`;
    }

    calcBtn.addEventListener('click', update);
    [modeEl, unitEl, valueEl, precEl].forEach(el => el.addEventListener('input', update));

    clearBtn.addEventListener('click', () => {
      valueEl.value = '';
      precEl.value = 2;
      errEl.style.display = 'none';
      circEl.textContent = '—';
      areaEl.textContent = '—';
      valueEl.focus();
    });

    // Autofocus for quicker input
    window.addEventListener('DOMContentLoaded', () => valueEl.focus());
  </script>
</body>
</html>
