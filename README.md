<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Somu AI Food Preferencer — Daily Allergy Tracker</title>
<meta name="description" content="Daily tracker for allergic people — register, set preferences, track meals & calories, and compete on the leaderboard. Created by Soumyajyoti Konar." />
<style>
  /* ---------- Reset & base ---------- */
  :root{
    --bg-1:#fff7ff;
    --accent-1:#6b21a8;
    --accent-2:#ff6b6b;
    --card:#ffffff;
    --muted:#555;
    --glass: rgba(255,255,255,0.6);
    --success: #16a34a;
    --shadow: 0 6px 20px rgba(17,24,39,0.08);
    --radius: 14px;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0;
    font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    background:
      radial-gradient(800px 500px at 10% 10%, #fff0f7, transparent),
      radial-gradient(600px 400px at 95% 90%, #f0f8ff, transparent),
      linear-gradient(180deg,#fff 0%, #fffefc 100%);
    color:#111827;
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
    padding:24px;
  }

  /* ---------- Layout ---------- */
  .container{
    max-width:1100px;
    margin:0 auto;
    display:grid;
    grid-template-columns: 1fr 420px;
    gap:20px;
    align-items:start;
  }
  header{
    grid-column:1/-1;
    display:flex;
    gap:16px;
    align-items:center;
    margin-bottom:6px;
  }
  .logo{
    display:flex;
    align-items:center;
    gap:12px;
    padding:8px 12px;
    border-radius:12px;
    background: linear-gradient(135deg, rgba(107,33,168,0.12), rgba(255,107,107,0.06));
    box-shadow: var(--shadow);
  }
  .logo svg{width:60px;height:60px;flex-shrink:0}
  .brand{
    line-height:1;
  }
  .brand h1{margin:0;font-size:18px;color:var(--accent-1)}
  .brand p{margin:0;font-size:12px;color:var(--muted)}

  /* ---------- Card ---------- */
  .card{
    background: var(--card);
    border-radius:var(--radius);
    padding:18px;
    box-shadow: var(--shadow);
  }
  .card h2{margin:0 0 12px 0;font-size:16px;color:var(--accent-1)}
  .muted{color:var(--muted);font-size:13px}

  /* ---------- Form ---------- */
  form .grid{
    display:grid;
    grid-template-columns: 1fr 1fr;
    gap:12px;
  }
  label{display:block;font-weight:600;font-size:13px;margin-bottom:6px}
  input[type="text"], input[type="number"], input[type="email"], select, textarea{
    width:100%;
    padding:10px 12px;
    border-radius:10px;
    border:1px solid rgba(15,23,42,0.06);
    background:linear-gradient(180deg,rgba(255,255,255,0.9),rgba(250,250,255,0.9));
    font-size:14px;
  }
  textarea{min-height:80px;resize:vertical}
  .full{grid-column:1/-1}
  .actions{display:flex;gap:10px;align-items:center;margin-top:12px}
  button{
    border:0;padding:10px 14px;border-radius:10px;font-weight:700;cursor:pointer;
    box-shadow:0 6px 18px rgba(0,0,0,0.06)
  }
  .btn-primary{
    background:linear-gradient(90deg,var(--accent-1),#ff4d6d);
    color:white;
  }
  .btn-ghost{
    background:transparent;border:1px solid rgba(15,23,42,0.06);
    color:var(--accent-1);
  }

  /* ---------- Small UI ---------- */
  .pill{display:inline-block;padding:6px 10px;border-radius:999px;background:linear-gradient(90deg,#fff8ff,#fff4f4);font-weight:700;color:var(--accent-1);font-size:13px}
  .hr{height:1px;background:linear-gradient(90deg,rgba(0,0,0,0.04),transparent);margin:12px 0;border-radius:2px}

  /* ---------- Right column ---------- */
  .rightColumn{display:flex;flex-direction:column;gap:16px}
  .track-list{max-height:320px;overflow:auto;padding-right:6px}
  .meal-item{display:flex;justify-content:space-between;align-items:center;padding:10px;border-radius:10px;margin-bottom:8px;background:linear-gradient(180deg,#fff,#fffaf6);border:1px solid rgba(0,0,0,0.02)}
  .meal-item .meta{font-size:13px;color:var(--muted)}
  .leaderboard-row{display:flex;justify-content:space-between;gap:8px;padding:10px;border-radius:10px;margin-bottom:8px;background:linear-gradient(90deg,#fdf7ff,#fff7f7)}
  .leader-top{font-weight:800;color:var(--accent-1)}

  /* ---------- Responsive ---------- */
  @media (max-width:1000px){
    .container{grid-template-columns:1fr; padding-bottom:80px}
    .rightColumn{order:2}
  }

  /* ---------- fun colors ---------- */
  .pref-pill{display:inline-block;margin:6px 6px 6px 0;padding:8px 10px;border-radius:999px;background:linear-gradient(90deg,#f6f0ff,#fff0f0);font-weight:700;color:var(--accent-1);cursor:pointer;border:1px dashed rgba(107,33,168,0.06)}
  .pref-pill.selected{background: linear-gradient(90deg,#6b21a8,#ff6b6b); color:white; border:0; box-shadow: 0 10px 30px rgba(107,33,168,0.12)}
  .allergy-chip{display:inline-block;margin:6px 6px 6px 0;padding:6px 8px;border-radius:8px;background:#fff0f0;color:#b91c1c;font-weight:700;border:1px solid rgba(189,39,39,0.06)}

  /* ---------- tiny UI help ---------- */
  .small{font-size:12px;color:#6b7280}
  .center{display:flex;align-items:center;justify-content:center;gap:8px}
  .badge{display:inline-block;padding:6px 8px;border-radius:8px;background:linear-gradient(90deg,#f0f9ff,#eef2ff);font-weight:700;color:#0f172a}
  .logo-credit{font-size:12px;color:#6b7280;margin-top:8px}
  .notice{background:linear-gradient(90deg,#fff6e6,#fffaf0);padding:10px;border-radius:10px;border:1px solid rgba(0,0,0,0.03);font-size:13px}
  .kpi{display:flex;gap:10px}
  .kpi .k{font-weight:800;font-size:20px;color:var(--accent-1)}
  .focus{outline:3px solid rgba(107,33,168,0.08);outline-offset:2px}
</style>
</head>
<body>
  <div class="container">

    <header>
      <div class="logo">
        <!-- Inline SVG logo -->
        <svg viewBox="0 0 120 120" aria-hidden="true">
          <defs>
            <linearGradient id="g1" x1="0" x2="1">
              <stop offset="0" stop-color="#6b21a8"/>
              <stop offset="1" stop-color="#ff4d6d"/>
            </linearGradient>
          </defs>
          <rect x="8" y="8" width="104" height="104" rx="20" fill="url(#g1)" opacity="0.95"/>
          <g transform="translate(20,20)" fill="#fff">
            <circle cx="24" cy="24" r="20" opacity="0.95"/>
            <path d="M56 20c-6 0-10 5-10 11s4 11 10 11c6 0 10-5 10-11s-4-11-10-11zm-1 19c-2 0-3-1-3-3s1-3 3-3 3 1 3 3-1 3-3 3z" opacity="0.98"/>
          </g>
        </svg>

        <div class="brand">
          <h1>Somu AI Food Preferencer</h1>
          <p class="muted">Allergy-friendly daily tracker & leaderboard</p>
          <div class="logo-credit">The creator for this AI food preferencer is created by <strong>Soumyajyoti Konar</strong></div>
        </div>
      </div>
      <div style="margin-left:auto" class="pill">Colorful • Survey • Single-file</div>
    </header>

    <!-- Left: Main App -->
    <main class="card">
      <h2>Register / Survey</h2>
      <div class="muted">Complete the short survey so the app can remember allergies, preferences & show calorie totals on the leaderboard.</div>

      <form id="regForm" style="margin-top:12px">
        <div class="grid">
          <div>
            <label for="name">Full name</label>
            <input id="name" type="text" placeholder="e.g. Priya Sharma" required />
          </div>
          <div>
            <label for="email">Email (optional)</label>
            <input id="email" type="email" placeholder="you@example.com" />
          </div>

          <div>
            <label for="age">Age</label>
            <input id="age" type="number" placeholder="25" min="1" />
          </div>
          <div>
            <label for="gender">Gender</label>
            <select id="gender">
              <option value="">Prefer not to say</option>
              <option>Female</option><option>Male</option><option>Non-binary</option><option>Other</option>
            </select>
          </div>

          <div>
            <label for="height">Height (cm)</label>
            <input id="height" type="number" placeholder="170" min="30" />
          </div>
          <div>
            <label for="weight">Weight (kg)</label>
            <input id="weight" type="number" placeholder="65" min="2" />
          </div>

          <div class="full">
            <label>Allergies (select or add)</label>
            <div id="allergyList" class="muted" style="margin-bottom:8px">
              <span class="allergy-chip">Peanuts</span>
              <span class="allergy-chip">Gluten</span>
              <span class="allergy-chip">Dairy</span>
              <span class="allergy-chip">None</span>
            </div>
            <input id="allergyInput" type="text" placeholder="Type an allergy and press Enter (e.g. 'Soy')" />
          </div>

          <div class="full">
            <label>Food preferences (tap to toggle)</label>
            <div id="prefs" style="display:flex;flex-wrap:wrap">
              <span class="pref-pill" data-val="Vegetarian">Vegetarian</span>
              <span class="pref-pill" data-val="Vegan">Vegan</span>
              <span class="pref-pill" data-val="Low carb">Low carb</span>
              <span class="pref-pill" data-val="High protein">High protein</span>
              <span class="pref-pill" data-val="Spicy">Spicy</span>
              <span class="pref-pill" data-val="Non-veg">Non-veg</span>
              <span class="pref-pill" data-val="Keto">Keto</span>
              <span class="pref-pill" data-val="No preference">No preference</span>
            </div>
          </div>

          <div class="full">
            <label>Notes (dietary goals / likes / dislikes)</label>
            <textarea id="notes" placeholder="e.g. I prefer light dinners, avoid fried foods."></textarea>
          </div>
        </div>

        <div class="actions">
          <button type="submit" class="btn-primary">Save Profile & Register</button>
          <button type="button" id="loadSample" class="btn-ghost">Load Demo</button>
          <div style="margin-left:auto" class="small">Profiles are stored locally in your browser for GitHub Pages use.</div>
        </div>
      </form>

      <div class="hr"></div>

      <h2>Daily Tracker — Add meal</h2>
      <div class="muted">Select from common dishes or add your own. Calories sync to your registered name and leaderboard.</div>

      <div style="display:flex;gap:12px;margin-top:12px;align-items:flex-start;flex-wrap:wrap">
        <div style="flex:1;min-width:220px">
          <label>Registered user</label>
          <select id="userSelect"></select>
        </div>

        <div style="flex:1;min-width:200px">
          <label>Dish</label>
          <input id="dishName" type="text" placeholder="e.g. Egg omelette / Custom" />
        </div>

        <div style="width:140px">
          <label>Calories</label>
          <input id="dishCalories" type="number" placeholder="e.g. 320" min="0" />
        </div>

        <div style="width:150px">
          <label>Date</label>
          <input id="mealDate" type="date" />
        </div>

        <div style="width:120px;align-self:end">
          <button id="addMealBtn" class="btn-primary" style="width:100%">Add</button>
        </div>
      </div>

      <div style="margin-top:12px" class="center">
        <div class="small">Quick-add:</div>
        <div style="display:flex;gap:8px;margin-left:8px">
          <button class="btn-ghost quick" data-name="Apple" data-cal="95">Apple • 95</button>
          <button class="btn-ghost quick" data-name="Chapati" data-cal="120">Chapati • 120</button>
          <button class="btn-ghost quick" data-name="Rice(1cup)" data-cal="240">Rice • 240</button>
          <button class="btn-ghost quick" data-name="Egg(omelette)" data-cal="180">Egg • 180</button>
        </div>
      </div>

      <div class="hr"></div>

      <div style="display:flex;gap:12px;align-items:center">
        <div class="kpi">
          <div><div class="k" id="todayTotal">0</div><div class="small">cal today</div></div>
          <div style="margin-left:10px"><div class="k" id="userCount">0</div><div class="small">registered</div></div>
        </div>
        <div style="margin-left:auto" class="small">Tip: Register before adding meals.</div>
      </div>
    </main>

    <!-- Right: Side column -->
    <aside class="rightColumn">
      <div class="card">
        <h2>Today's Meals</h2>
        <div class="muted">Meals logged for the selected date and user.</div>
        <div class="track-list" id="mealsList" style="margin-top:10px"></div>
        <div class="hr"></div>
        <div style="display:flex;gap:8px;align-items:center">
          <button id="clearToday" class="btn-ghost">Clear Today</button>
          <button id="exportData" class="btn-ghost">Export JSON</button>
          <button id="importDataBtn" class="btn-ghost">Import JSON</button>
          <input id="importFile" type="file" accept="application/json" style="display:none" />
        </div>
      </div>

      <div class="card">
        <h2>Leaderboard (Today)</h2>
        <div class="muted">Who gained the most calories today? (For friendly contests)</div>
        <div id="leaderboard" style="margin-top:12px"></div>
        <div class="hr"></div>
        <div class="notice">
          <strong>Privacy:</strong> Data is stored in your browser only. If you want server syncing in future, we can add it.
        </div>
      </div>

      <div class="card">
        <h2>Your Profile Preview</h2>
        <div id="profilePreview" style="margin-top:8px">
          <div class="small muted">No profile selected</div>
        </div>
      </div>
    </aside>
  </div>

<script>
/* =========================
   Single-file app JS
   ========================= */

const NS = 'somu_food_pref_v1';

// ---------- Utilities ----------
const $ = (sel, root=document) => root.querySelector(sel);
const $$ = (sel, root=document) => Array.from(root.querySelectorAll(sel));
function uid() { return 'u_' + Math.random().toString(36).slice(2,9); }
function todayISO(){ const d = new Date(); return d.toISOString().slice(0,10); }

function saveStore(obj){
  localStorage.setItem(NS, JSON.stringify(obj));
}
function loadStore(){
  try { return JSON.parse(localStorage.getItem(NS) || '{}'); }
  catch(e){ return {}; }
}

// ---------- App state ----------
let store = loadStore();
if(!store.users) store.users = {};      // users by id
if(!store.meals) store.meals = [];      // meals: {id,userId,name,cal,date,ts}
saveStore(store);

// ---------- UI Elements ----------
const regForm = $('#regForm');
const nameInput = $('#name');
const emailInput = $('#email');
const ageInput = $('#age');
const heightInput = $('#height');
const weightInput = $('#weight');
const allergyList = $('#allergyList');
const allergyInput = $('#allergyInput');
const prefsArea = $('#prefs');
const notesInput = $('#notes');
const userSelect = $('#userSelect');
const dishName = $('#dishName');
const dishCalories = $('#dishCalories');
const mealDate = $('#mealDate');
const addMealBtn = $('#addMealBtn');
const mealsList = $('#mealsList');
const leaderboard = $('#leaderboard');
const todayTotalEl = $('#todayTotal');
const userCountEl = $('#userCount');
const profilePreview = $('#profilePreview');
const clearTodayBtn = $('#clearToday');
const exportBtn = $('#exportData');
const importBtn = $('#importDataBtn');
const importFile = $('#importFile');
const loadSample = $('#loadSample');

// ---------- Helpers ----------
function formatCal(n){ return Math.round(n) + ' kcal'; }
function persist(){ saveStore(store); }
function allPrefsOf(user){
  return user && user.prefs ? user.prefs.join(', ') : '—';
}
function allAllergiesOf(user){
  return user && user.allergies && user.allergies.length ? user.allergies.join(', ') : 'None';
}
function calcBMI(w,h){ if(!w||!h) return null; const m = w/100; return (w/(m*m)).toFixed(1); }

// ---------- UI Rendering ----------
function refreshUserSelect(){
  userSelect.innerHTML = '<option value="">Select a registered user</option>';
  const entries = Object.entries(store.users);
  entries.forEach(([id,u])=>{
    const opt = document.createElement('option');
    opt.value = id;
    opt.textContent = u.name + (u.age ? ' • ' + u.age : '');
    userSelect.appendChild(opt);
  });
  userCountEl.textContent = entries.length;
}

function renderMealsList(){
  const selUser = userSelect.value;
  const date = mealDate.value || todayISO();
  mealsList.innerHTML = '';
  const items = store.meals.filter(m => (!selUser || m.userId === selUser) && m.date === date)
    .sort((a,b)=> b.ts - a.ts);
  let total=0;
  for(const m of items){
    total += +m.cal;
    const el = document.createElement('div');
    el.className = 'meal-item';
    el.innerHTML = \`
      <div>
        <div style="font-weight:700">\${m.name}</div>
        <div class="meta">\${new Date(m.ts).toLocaleTimeString()} • \${m.userName}</div>
      </div>
      <div style="text-align:right">
        <div style="font-weight:800;color:var(--accent-1)">\${formatCal(m.cal)}</div>
        <div style="margin-top:6px"><button data-id="\${m.id}" class="btn-ghost small">Delete</button></div>
      </div>
    \`;
    mealsList.appendChild(el);
  }
  todayTotalEl.textContent = Math.round(total);
  if(items.length===0){
    mealsList.innerHTML = '<div class="muted small">No meals logged for selected date/user.</div>';
  }
}

function renderLeaderboard(date=todayISO()){
  // Sum calories per user for given date
  const sums = {};
  for(const m of store.meals.filter(x=> x.date===date)){
    sums[m.userId] = (sums[m.userId]||0) + (+m.cal||0);
  }
  const rows = Object.entries(sums).sort((a,b)=> b[1]-a[1]);
  leaderboard.innerHTML = '';
  if(rows.length===0){
    leaderboard.innerHTML = '<div class="muted small">No data for today.</div>';
    return;
  }
  rows.forEach(([uid,cal], i)=>{
    const user = store.users[uid] || {name:'Unknown'};
    const row = document.createElement('div');
    row.className = 'leaderboard-row';
    row.innerHTML = \`
      <div style="display:flex;flex-direction:column">
        <div class="\${i===0? 'leader-top' : ''}">\${i+1}. \${user.name}</div>
        <div class="small muted">\${allAllergiesOf(user)} • \${allPrefsOf(user)}</div>
      </div>
      <div style="text-align:right">
        <div style="font-weight:900">\${Math.round(cal)} kcal</div>
        <div class="small muted">Calories</div>
      </div>
    \`;
    leaderboard.appendChild(row);
  });
}

function renderProfilePreview(){
  const id = userSelect.value;
  if(!id){ profilePreview.innerHTML = '<div class="muted small">No profile selected</div>'; return; }
  const u = store.users[id];
  const bmi = calcBMI(u.weight, u.height);
  profilePreview.innerHTML = \`
    <div style="display:flex;gap:12px;align-items:center">
      <div style="width:64px;height:64px;border-radius:12px;background:linear-gradient(90deg,#fff8ff,#f0f7ff);display:flex;align-items:center;justify-content:center;font-weight:900;color:var(--accent-1)">\${u.name.split(' ').map(n=>n[0]||'').slice(0,2).join('').toUpperCase()}</div>
      <div>
        <div style="font-weight:800">\${u.name}</div>
        <div class="small muted">\${u.age ? u.age + ' yrs • ' : ''}\${u.gender || ''}</div>
      </div>
    </div>
    <div class="hr"></div>
    <div class="small"><strong>Height:</strong> \${u.height || '—'} cm • <strong>Weight:</strong> \${u.weight || '—'} kg • <strong>BMI:</strong> \${bmi || '—'}</div>
    <div style="margin-top:8px" class="small"><strong>Allergies:</strong> \${allAllergiesOf(u)}</div>
    <div style="margin-top:6px" class="small"><strong>Preferences:</strong> \${allPrefsOf(u)}</div>
    <div style="margin-top:8px" class="small"><strong>Notes:</strong> \${u.notes||'—'}</div>
  \`;
}

// ---------- Event wiring ----------

// Toggle preference pills
prefsArea.addEventListener('click', e=>{
  const p = e.target.closest('.pref-pill');
  if(!p) return;
  p.classList.toggle('selected');
});

// Allergy adding by Enter
allergyInput.addEventListener('keydown', e=>{
  if(e.key === 'Enter'){
    e.preventDefault();
    const v = allergyInput.value.trim();
    if(!v) return;
    const chip = document.createElement('span');
    chip.className = 'allergy-chip';
    chip.textContent = v;
    allergyList.appendChild(chip);
    allergyInput.value = '';
  }
});

// Register form submit
regForm.addEventListener('submit', e=>{
  e.preventDefault();
  const name = nameInput.value.trim();
  if(!name){ alert('Please enter a name'); return; }
  const id = uid();
  const prefs = $$('.pref-pill.selected').map(p=>p.dataset.val);
  const allergies = $$('.allergy-chip').map(c=>c.textContent).filter(x=>x && x.toLowerCase()!=='none');
  const user = {
    id, name, email: emailInput.value.trim(), age: ageInput.value, gender: genderSelectValue(), height: +heightInput.value || null, weight: +weightInput.value || null,
    prefs, allergies, notes: notesInput.value.trim()
  };
  store.users[id] = user;
  persist();
  refreshUserSelect();
  userSelect.value = id;
  renderProfilePreview();
  alert('Profile saved locally ✅');
});

// helper for gender value
function genderSelectValue(){ const sel = $('#gender'); return sel ? sel.value : ''; }

// Load sample data
loadSample.addEventListener('click', ()=>{
  // sample users
  const u1 = uid(), u2 = uid(), u3 = uid();
  store.users[u1] = {id:u1, name:'Anita Roy', age:28, gender:'Female', height:160, weight:55, prefs:['Vegetarian'], allergies:['Gluten'], notes:'Light dinners'};
  store.users[u2] = {id:u2, name:'Rohit Singh', age:34, gender:'Male', height:175, weight:72, prefs:['High protein'], allergies:[], notes:'Gym focus'};
  store.users[u3] = {id:u3, name:'Priya N', age:21, gender:'Female', height:165, weight:60, prefs:['Vegan'], allergies:['Peanuts'], notes:'Lactose intolerant'};

  // sample meals (today)
  const td = todayISO();
  store.meals = store.meals.concat([
    {id:uid(), userId:u1, userName:store.users[u1].name, name:'Chapati (2)', cal:240, date:td, ts:Date.now()-1000*60*60},
    {id:uid(), userId:u2, userName:store.users[u2].name, name:'Grilled Chicken', cal:450, date:td, ts:Date.now()-1000*60*50},
    {id:uid(), userId:u3, userName:store.users[u3].name, name:'Avocado Salad', cal:320, date:td, ts:Date.now()-1000*60*20},
  ]);
  persist();
  refreshUserSelect();
  userSelect.value = u1;
  mealDate.value = td;
  renderMealsList();
  renderLeaderboard(td);
  renderProfilePreview();
  alert('Sample users & meals loaded.');
});

// Add meal button
addMealBtn.addEventListener('click', ()=>{
  const uidSel = userSelect.value;
  if(!uidSel){ alert('Please select a registered user first.'); return; }
  const name = (dishName.value || '').trim();
  const cal = Number(dishCalories.value||0);
  if(!name){ alert('Please enter dish name.'); return; }
  if(!cal || cal < 0){ alert('Enter valid calories (0 or more).'); return; }
  const date = mealDate.value || todayISO();
  const item = { id: uid(), userId: uidSel, userName: store.users[uidSel].name, name, cal: Math.round(cal), date, ts: Date.now() };
  store.meals.push(item);
  persist();
  dishName.value = '';
  dishCalories.value = '';
  renderMealsList();
  renderLeaderboard(date);
  alert('Meal added ✅');
});

// Quick-add buttons
$$('.quick').forEach(b=>{
  b.addEventListener('click', ()=>{
    if(!userSelect.value){ alert('Select a registered user to quick-add'); return; }
    dishName.value = b.dataset.name;
    dishCalories.value = b.dataset.cal;
    mealDate.value = mealDate.value || todayISO();
    addMealBtn.click();
  });
});

// Delete meal (delegated)
mealsList.addEventListener('click', e=>{
  const btn = e.target.closest('button[data-id]');
  if(!btn) return;
  const id = btn.dataset.id;
  if(!confirm('Delete this meal?')) return;
  store.meals = store.meals.filter(m=> m.id !== id);
  persist();
  renderMealsList();
  renderLeaderboard(mealDate.value || todayISO());
});

// Clear today (for selected user or all?)
clearTodayBtn.addEventListener('click', ()=>{
  const date = mealDate.value || todayISO();
  if(!confirm('Clear all meals for ' + date + ' ?')) return;
  // remove only meals on that date
  store.meals = store.meals.filter(m => m.date !== date);
  persist();
  renderMealsList();
  renderLeaderboard(date);
  alert('Cleared meals for ' + date);
});

// Export JSON
exportBtn.addEventListener('click', ()=>{
  const data = JSON.stringify(store, null, 2);
  const blob = new Blob([data], {type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'somu_food_data.json';
  a.click();
  URL.revokeObjectURL(url);
});

// Import JSON
importBtn.addEventListener('click', ()=> importFile.click());
importFile.addEventListener('change', async (ev)=>{
  const f = ev.target.files[0]; if(!f) return;
  try{
    const txt = await f.text();
    const parsed = JSON.parse(txt);
    // merge safely: keep existing users/meals and append new ones
    store.users = Object.assign({}, store.users, parsed.users || {});
    store.meals = (store.meals || []).concat(parsed.meals || []);
    persist();
    refreshUserSelect();
    renderMealsList();
    renderLeaderboard(mealDate.value || todayISO());
    alert('Imported data. Merged with existing.');
  }catch(e){ alert('Import failed: ' + e.message); }
});

// User select change
userSelect.addEventListener('change', ()=>{
  renderProfilePreview();
  renderMealsList();
  renderLeaderboard(mealDate.value || todayISO());
});

// Date change
mealDate.addEventListener('change', ()=>{
  renderMealsList();
  renderLeaderboard(mealDate.value || todayISO());
});

// initial render
(function init(){
  mealDate.value = todayISO();
  refreshUserSelect();
  renderMealsList();
  renderLeaderboard(mealDate.value);
  renderProfilePreview();
})();

/* ========= Extra UI: allow clicking allergy chips to toggle/delete ========== */
allergyList.addEventListener('click', e=>{
  const chip = e.target.closest('.allergy-chip');
  if(!chip) return;
  if(confirm('Remove allergy "' + chip.textContent + '"?')){
    chip.remove();
  }
});

// allow deleting preference by shift-click
prefsArea.addEventListener('dblclick', e=>{
  // double click to deselect + remove
  const p = e.target.closest('.pref-pill');
  if(!p) return;
  if(confirm('Remove preference "'+p.textContent+'"?')){
    p.remove();
  }
});
</script>
</body>
</html>
