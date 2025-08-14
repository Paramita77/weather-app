# weather-app
using HTML,CSS JAVASCRIPT.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Weatherly – Minimal Weather App</title>
  <meta name="color-scheme" content="light dark">
  <style>
    :root{
      --bg: #0f172a;           /* slate-900 */
      --panel: #111827;        /* gray-900 */
      --muted: #94a3b8;        /* slate-400 */
      --text: #e5e7eb;         /* gray-200 */
      --accent: #38bdf8;       /* sky-400 */
      --accent-2: #22d3ee;     /* cyan-400 */
      --ok: #22c55e;           /* green-500 */
      --warn: #f59e0b;         /* amber-500 */
      --bad: #ef4444;          /* red-500 */
      --chip: #1f2937;         /* gray-800 */
      --ring: rgba(56,189,248,.35);
      --shadow: 0 10px 30px rgba(2,6,23,.45);
    }

    @media (prefers-color-scheme: light){
      :root{
        --bg: #f6f7fb;
        --panel: #ffffff;
        --muted: #64748b;
        --text: #0f172a;
        --chip: #eff2f7;
        --shadow: 0 10px 25px rgba(2,6,23,.08);
      }
    }

    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, "Apple Color Emoji", "Segoe UI Emoji";
      background: radial-gradient(1200px 800px at 90% -10%, rgba(56,189,248,.15), transparent 60%),
                  radial-gradient(1000px 600px at -10% 110%, rgba(34,211,238,.12), transparent 60%),
                  var(--bg);
      color:var(--text); display:flex; align-items:center; justify-content:center; padding:24px;
    }

    .app{
      width:min(1100px, 100%); max-width:1100px;
      display:grid; gap:18px;
      grid-template-columns: 1fr;
    }

    header{
      display:flex; gap:12px; align-items:center; justify-content:space-between;
    }
    .brand{ display:flex; align-items:center; gap:12px; }
    .logo{
      width:44px; height:44px; display:grid; place-items:center; border-radius:14px;
      background: linear-gradient(135deg, var(--accent), var(--accent-2));
      color:#001b2a; font-weight:800; box-shadow: var(--shadow);
    }
    .title{ font-weight:800; letter-spacing:.2px; font-size: clamp(18px, 3vw, 26px); }

    .controls{
      display:flex; gap:10px; align-items:center; flex-wrap:wrap;
    }
    .chip{ background:var(--chip); color:var(--text); border-radius:999px; padding:8px 12px; display:inline-flex; gap:8px; align-items:center; border:1px solid rgba(148,163,184,.15); }
    .chip button, .chip input, .chip select{ background:transparent; border:none; color:inherit; outline:none; font:inherit; }
    .chip input[type="text"]{ min-width:200px; }
    .chip:focus-within{ box-shadow:0 0 0 3px var(--ring); }
    .btn{
      border:none; border-radius:12px; padding:10px 14px; background:linear-gradient(135deg, var(--accent), var(--accent-2)); color:#01131c; font-weight:700; cursor:pointer; box-shadow: var(--shadow);
    }
    .btn.secondary{ background: var(--chip); color:var(--text); box-shadow:none; border:1px solid rgba(148,163,184,.15); }

    main{ display:grid; grid-template-columns: 1.2fr .8fr; gap:18px; }
    @media (max-width: 900px){ main{ grid-template-columns: 1fr; } }

    .card{
      background:var(--panel); border:1px solid rgba(148,163,184,.12); border-radius:22px;
      box-shadow: var(--shadow); padding:18px; overflow:hidden; position:relative;
    }

    .current{
      display:grid; grid-template-columns: 1.6fr 1fr; gap:12px; align-items:stretch;
    }
    @media (max-width: 700px){ .current{ grid-template-columns: 1fr; } }

    .city-line{ display:flex; justify-content:space-between; align-items:center; gap:8px; margin-bottom:2px; }
    .city{ font-size: clamp(18px, 4vw, 28px); font-weight:800; letter-spacing:.3px; }
    .date{ color:var(--muted); font-size:14px; }

    .temp-wrap{ display:flex; align-items:center; justify-content:center; }
    .temp{
      font-size:clamp(48px, 9vw, 86px); font-weight:900; line-height:1; letter-spacing:-1px;
      display:flex; align-items:flex-start; gap:8px;
    }
    .temp small{ font-size:.32em; font-weight:700; color:var(--muted); margin-top:12px; }

    .desc{ margin-top:6px; color:var(--muted); font-weight:600; letter-spacing:.2px; text-transform:capitalize; }
    .big-icon{ width:110px; height:110px; filter: drop-shadow(0 10px 20px rgba(2,6,23,.25)); }

    .details{ display:grid; grid-template-columns: repeat(2, minmax(0,1fr)); gap:10px; margin-top:10px; }
    .detail{ background: linear-gradient(180deg, rgba(148,163,184,.08), transparent); border:1px solid rgba(148,163,184,.15); padding:12px; border-radius:16px; display:flex; justify-content:space-between; align-items:flex-end; }
    .detail .lbl{ color:var(--muted); font-size:12px; }
    .detail .val{ font-weight:800; font-size:16px; }

    .forecast{
      display:grid; grid-template-columns: repeat(5, 1fr); gap:12px;
    }
    @media (max-width: 900px){ .forecast{ grid-template-columns: repeat(5, minmax(120px,1fr)); overflow:auto; padding-bottom:6px; }
      .forecast .day{ min-width:140px; }
    }

    .day{ text-align:center; background: linear-gradient(180deg, rgba(148,163,184,.08), transparent); border:1px solid rgba(148,163,184,.15); border-radius:16px; padding:12px; }
    .day .name{ font-weight:800; margin-bottom:6px; }
    .day .range{ color:var(--muted); font-weight:700; font-size:13px; }

    .subtle{ color:var(--muted); font-size:12px; }

    .sr-only{ position:absolute; width:1px; height:1px; padding:0; margin:-1px; overflow:hidden; clip:rect(0,0,0,0); border:0; }

    .spinner{ width:18px; height:18px; border:3px solid rgba(148,163,184,.35); border-top-color:var(--accent); border-radius:50%; animation:spin 1s linear infinite; }
    @keyframes spin{ to{ transform:rotate(360deg) } }

    .toast{ position:fixed; right:18px; bottom:18px; background:var(--panel); color:var(--text); border:1px solid rgba(148,163,184,.2); padding:12px 14px; border-radius:14px; box-shadow: var(--shadow); display:none; }
    .toast.show{ display:block; }
    .link{ color:var(--accent); text-decoration:none; border-bottom:1px dashed rgba(56,189,248,.45); }

    footer{ text-align:center; color:var(--muted); font-size:12px; margin-top:6px; }
  </style>
</head>
<body>
  <div class="app" role="application" aria-labelledby="app-title">
    <header>
      <div class="brand">
        <div class="logo" aria-hidden="true">⛅</div>
        <div>
          <div class="title" id="app-title">Weatherly</div>
          <div class="subtle">Sleek, single‑file weather app</div>
        </div>
      </div>
      <div class="controls" role="search">
        <label class="chip" aria-label="Search city">
          <span>🔎</span>
          <input id="cityInput" type="text" placeholder="Search city (e.g., Mumbai)" autocomplete="off" />
          <button id="searchBtn" class="sr-only">Search</button>
        </label>
        <button id="geoBtn" class="btn secondary" title="Use my location">📍 Use Location</button>
        <label class="chip" aria-label="Unit toggle">
          <span>🌡️</span>
          <select id="unitSelect" aria-label="Temperature unit">
            <option value="metric">°C</option>
            <option value="imperial">°F</option>
          </select>
        </label>
        <label class="chip" aria-label="API key">
          <span>🔑</span>
          <input id="apiKeyInput" type="password" placeholder="OpenWeather API key" />
        </label>
        <button id="saveKeyBtn" class="btn" title="Save API key">Save</button>
      </div>
    </header>

    <main>
      <section class="card current" aria-live="polite" aria-busy="false">
        <div>
          <div class="city-line">
            <div>
              <div class="city" id="cityName">—</div>
              <div class="date" id="dateStr">—</div>
            </div>
            <img id="bigIcon" class="big-icon" alt="Weather icon" src="" />
          </div>
          <div class="temp-wrap">
            <div class="temp"><span id="temp">--</span><small id="unitLabel">°C</small></div>
          </div>
          <div class="desc" id="desc">—</div>

          <div class="details">
            <div class="detail"><div class="lbl">Feels like</div><div class="val" id="feels">--</div></div>
            <div class="detail"><div class="lbl">Humidity</div><div class="val" id="humidity">--</div></div>
            <div class="detail"><div class="lbl">Wind</div><div class="val" id="wind">--</div></div>
            <div class="detail"><div class="lbl">Pressure</div><div class="val" id="pressure">--</div></div>
            <div class="detail"><div class="lbl">Sunrise</div><div class="val" id="sunrise">--</div></div>
            <div class="detail"><div class="lbl">Sunset</div><div class="val" id="sunset">--</div></div>
          </div>
        </div>

        <aside class="card" style="background:transparent; box-shadow:none; border:none; padding:0;">
          <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px;">
            <div style="font-weight:800;">5‑Day Forecast</div>
            <div class="subtle" id="timezone">—</div>
          </div>
          <div class="forecast" id="forecast"></div>
        </aside>
      </section>

      <section class="card" aria-labelledby="recent-title">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
          <div id="recent-title" style="font-weight:800;">Recent Searches</div>
          <button id="clearRecent" class="btn secondary">Clear</button>
        </div>
        <div id="recentChips" style="display:flex; gap:8px; flex-wrap:wrap;"></div>
        <div class="subtle" style="margin-top:10px;">Tip: Save your API key above to avoid typing it each time. Data by <a class="link" href="https://openweathermap.org/" target="_blank" rel="noreferrer noopener">OpenWeather</a>.</div>
      </section>
    </main>

    <footer>
      Built with ♥ using HTML, CSS & JavaScript. No frameworks, no build step.
    </footer>
  </div>

  <div class="toast" id="toast" role="status" aria-live="polite"></div>

  <script>
    const els = {
      cityInput: document.getElementById('cityInput'),
      searchBtn: document.getElementById('searchBtn'),
      geoBtn: document.getElementById('geoBtn'),
      unitSelect: document.getElementById('unitSelect'),
      apiKeyInput: document.getElementById('apiKeyInput'),
      saveKeyBtn: document.getElementById('saveKeyBtn'),
      cityName: document.getElementById('cityName'),
      dateStr: document.getElementById('dateStr'),
      bigIcon: document.getElementById('bigIcon'),
      temp: document.getElementById('temp'),
      unitLabel: document.getElementById('unitLabel'),
      desc: document.getElementById('desc'),
      feels: document.getElementById('feels'),
      humidity: document.getElementById('humidity'),
      wind: document.getElementById('wind'),
      pressure: document.getElementById('pressure'),
      sunrise: document.getElementById('sunrise'),
      sunset: document.getElementById('sunset'),
      forecast: document.getElementById('forecast'),
      timezone: document.getElementById('timezone'),
      recentChips: document.getElementById('recentChips'),
      clearRecent: document.getElementById('clearRecent'),
      toast: document.getElementById('toast'),
      currentCard: document.querySelector('section.current')
    };

    const STORAGE = {
      key: 'weatherly:key',
      unit: 'weatherly:unit',
      recent: 'weatherly:recent'
    };

    // Load stored settings
    els.apiKeyInput.value = localStorage.getItem(STORAGE.key) || '';
    els.unitSelect.value = localStorage.getItem(STORAGE.unit) || 'metric';
    updateUnitLabel();
    renderRecent();

    function showToast(msg){
      els.toast.textContent = msg;
      els.toast.classList.add('show');
      setTimeout(()=>els.toast.classList.remove('show'), 2400);
    }

    function updateUnitLabel(){
      els.unitLabel.textContent = els.unitSelect.value === 'metric' ? '°C' : '°F';
    }

    function saveKey(){
      const k = els.apiKeyInput.value.trim();
      if(!k){ showToast('Enter an API key first'); return; }
      localStorage.setItem(STORAGE.key, k);
      showToast('API key saved to this browser');
    }

    async function searchCity(){
      const q = els.cityInput.value.trim();
      if(!q){ showToast('Type a city name'); return; }
      await fetchByCity(q);
      pushRecent(q);
    }

    function pushRecent(q){
      const arr = JSON.parse(localStorage.getItem(STORAGE.recent) || '[]');
      const next = [q, ...arr.filter(x=>x.toLowerCase()!==q.toLowerCase())].slice(0,8);
      localStorage.setItem(STORAGE.recent, JSON.stringify(next));
      renderRecent();
    }

    function renderRecent(){
      const arr = JSON.parse(localStorage.getItem(STORAGE.recent) || '[]');
      els.recentChips.innerHTML = '';
      arr.forEach(city=>{
        const b = document.createElement('button');
        b.className = 'chip'; b.textContent = city; b.title = 'Search ' + city;
        b.addEventListener('click', ()=>{ els.cityInput.value = city; searchCity(); });
        els.recentChips.appendChild(b);
      })
    }

    function clearRecent(){
      localStorage.removeItem(STORAGE.recent);
      renderRecent();
    }

    function setBusy(state){
      els.currentCard.setAttribute('aria-busy', state? 'true':'false');
      if(state){
        els.desc.innerHTML = '<span class="spinner" aria-hidden="true"></span> Loading…';
      }
    }

    function fmtTime(ts, tzOffset){
      // ts in seconds, tzOffset in seconds
      const d = new Date((ts + tzOffset) * 1000);
      return d.toUTCString().match(/\d{2}:(\d{2}:\d{2})/)[0];
    }

    function kph(mps){ return (mps * 3.6).toFixed(0) + ' km/h'; }
    function mph(mps){ return (mps * 2.23694).toFixed(0) + ' mph'; }

    async function fetchByCity(q){
      const key = localStorage.getItem(STORAGE.key) || els.apiKeyInput.value.trim();
      if(!key){ showToast('Add your OpenWeather API key first'); return; }
      const unit = els.unitSelect.value;
      try{
        setBusy(true);
        const geo = await (await fetch(`https://api.openweathermap.org/geo/1.0/direct?q=${encodeURIComponent(q)}&limit=1&appid=${key}`)).json();
        if(!geo.length){ throw new Error('City not found'); }
        const { lat, lon, name, country, state } = geo[0];
        await fetchWeather(lat, lon, `${name}${state? ', ' + state: ''}, ${country}`, unit, key);
      }catch(err){
        console.error(err);
        showToast(err.message || 'Failed to fetch');
      }finally{
        setBusy(false);
      }
    }

    async function fetchByGeo(){
      const key = localStorage.getItem(STORAGE.key) || els.apiKeyInput.value.trim();
      if(!key){ showToast('Add your OpenWeather API key first'); return; }
      const unit = els.unitSelect.value;
      if(!('geolocation' in navigator)){
        showToast('Geolocation not supported'); return;
      }
      navigator.geolocation.getCurrentPosition(async pos=>{
        const { latitude:lat, longitude:lon } = pos.coords;
        await fetchWeather(lat, lon, 'Your location', unit, key);
      }, err=>{
        showToast('Location access denied');
      }, { enableHighAccuracy:true, timeout:10000 });
    }

    async function fetchWeather(lat, lon, label, unit, key){
      try{
        setBusy(true);
        const [cur, fc] = await Promise.all([
          fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${key}&units=${unit}`).then(r=>r.json()),
          fetch(`https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${key}&units=${unit}`).then(r=>r.json())
        ]);
        if(cur.cod !== 200){ throw new Error(cur.message || 'Failed'); }
        renderCurrent(cur, label, unit);
        renderForecast(fc, unit);
        els.timezone.textContent = cur.timezone ? 'UTC' + (cur.timezone>=0? '+':'') + (cur.timezone/3600) : '—';
      }catch(err){
        console.error(err);
        showToast(err.message || 'Failed to fetch');
      }finally{
        setBusy(false);
      }
    }

    function renderCurrent(data, label, unit){
      const t = Math.round(data.main.temp);
      const feels = Math.round(data.main.feels_like);
      els.cityName.textContent = label || data.name;
      els.dateStr.textContent = new Date(data.dt * 1000).toLocaleString(undefined, { weekday:'short', month:'short', day:'numeric', hour:'2-digit', minute:'2-digit' });
      els.temp.textContent = t;
      els.desc.textContent = data.weather?.[0]?.description || '—';
      els.bigIcon.src = `https://openweathermap.org/img/wn/${data.weather?.[0]?.icon || '01d'}@2x.png`;
      els.bigIcon.alt = data.weather?.[0]?.main || 'icon';
      els.feels.textContent = feels + (unit==='metric' ? '°C' : '°F');
      els.humidity.textContent = data.main.humidity + '%';
      els.wind.textContent = unit==='metric' ? kph(data.wind.speed) : mph(data.wind.speed);
      els.pressure.textContent = data.main.pressure + ' hPa';
      if(data.sys){
        els.sunrise.textContent = new Date(data.sys.sunrise * 1000).toLocaleTimeString([], { hour:'2-digit', minute:'2-digit' });
        els.sunset.textContent = new Date(data.sys.sunset * 1000).toLocaleTimeString([], { hour:'2-digit', minute:'2-digit' });
      }
    }

    function groupByDay(list){
      const by = {};
      for(const item of list){
        const d = new Date(item.dt * 1000);
        const key = d.toISOString().slice(0,10);
        by[key] = by[key] || [];
        by[key].push(item);
      }
      return by;
    }

    function pickNoon(items){
      // choose the forecast closest to 12:00 local
      let best = items[0];
      let bestDelta = 1e9;
      for(const it of items){
        const h = new Date(it.dt * 1000).getHours();
        const delta = Math.abs(12 - h);
        if(delta < bestDelta){ best = it; bestDelta = delta; }
      }
      return best;
    }

    function renderForecast(fc, unit){
      els.forecast.innerHTML = '';
      if(!fc.list){ return; }
      const grouped = groupByDay(fc.list);
      const days = Object.keys(grouped).slice(0,5);
      days.forEach(dateKey =>{
        const sample = pickNoon(grouped[dateKey]);
        const max = Math.round(Math.max(...grouped[dateKey].map(x=>x.main.temp_max)));
        const min = Math.round(Math.min(...grouped[dateKey].map(x=>x.main.temp_min)));
        const div = document.createElement('div');
        div.className = 'day';
        const dayName = new Date(dateKey).toLocaleDateString(undefined, { weekday:'short' });
        div.innerHTML = `
          <div class="name">${dayName}</div>
          <img alt="${sample.weather?.[0]?.main || ''}" src="https://openweathermap.org/img/wn/${sample.weather?.[0]?.icon || '01d'}@2x.png" width="64" height="64" />
          <div class="range">${min}° / ${max}° ${unit==='metric'?'C':'F'}</div>
          <div class="subtle" style="text-transform:capitalize; margin-top:4px;">${sample.weather?.[0]?.description || ''}</div>
        `;
        els.forecast.appendChild(div);
      });
    }

    // Events
    els.searchBtn.addEventListener('click', searchCity);
    els.cityInput.addEventListener('keydown', e=>{ if(e.key==='Enter'){ searchCity(); } });
    els.geoBtn.addEventListener('click', fetchByGeo);
    els.unitSelect.addEventListener('change', ()=>{ localStorage.setItem(STORAGE.unit, els.unitSelect.value); updateUnitLabel(); showToast('Unit updated'); });
    els.saveKeyBtn.addEventListener('click', saveKey);
    els.clearRecent.addEventListener('click', clearRecent);

    // First paint: show something nice
    (function demo(){
      const examples = ['Mumbai','Delhi','Bengaluru','New York','London','Tokyo','Sydney'];
      els.cityInput.placeholder = 'Search city (e.g., ' + examples[Math.floor(Math.random()*examples.length)] + ')';
    })();

  </script>
</body>
</html>

