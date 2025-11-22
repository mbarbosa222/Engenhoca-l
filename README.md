# Engenhoca-l
Calendário
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Engenhoca - Gestão de Folgas</title>
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🎢</text></svg>">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', system-ui, sans-serif; }
    body { min-height: 100vh; background: linear-gradient(180deg, #f97316 0%, #fb923c 100%); padding: 16px; }
    .container { max-width: 500px; margin: 0 auto; }
    .card { background: white; border-radius: 16px; overflow: hidden; box-shadow: 0 10px 40px rgba(0,0,0,0.2); border: 4px solid #22c55e; }
    .header { background: linear-gradient(90deg, #4ade80, #22c55e); padding: 16px; text-align: center; }
    .header h1 { color: white; font-size: 24px; text-shadow: 2px 2px 0 #166534; }
    .header p { color: #dcfce7; font-size: 12px; margin-top: 4px; }
    .lock-bar { background: #fed7aa; padding: 12px; display: flex; align-items: center; justify-content: space-between; border-bottom: 2px solid #fdba74; }
    .lock-bar.unlocked { background: #dcfce7; border-color: #86efac; }
    .lock-bar input { padding: 8px 12px; border: 2px solid #fdba74; border-radius: 8px; width: 80px; font-size: 14px; }
    .lock-bar button { padding: 8px 16px; background: #22c55e; color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }
    .tabs { display: flex; background: #fff7ed; border-bottom: 2px solid #fed7aa; }
    .tab { flex: 1; padding: 12px; text-align: center; font-weight: bold; cursor: pointer; color: #ea580c; border: none; background: none; font-size: 14px; }
    .tab.active { color: #16a34a; border-bottom: 3px solid #22c55e; background: white; }
    .content { padding: 16px; background: linear-gradient(180deg, white, #fff7ed); }
    .nav { display: flex; align-items: center; justify-content: space-between; margin-bottom: 16px; }
    .nav button { width: 40px; height: 40px; border-radius: 50%; border: none; background: #f0fdf4; color: #16a34a; font-size: 20px; cursor: pointer; }
    .nav h2 { font-size: 18px; color: #ea580c; }
    .filter { margin-bottom: 12px; }
    .filter select { width: 100%; padding: 10px; border: 2px solid #fdba74; border-radius: 8px; font-size: 14px; }
    .weekdays { display: grid; grid-template-columns: repeat(7, 1fr); gap: 4px; margin-bottom: 8px; }
    .weekday { text-align: center; font-size: 12px; font-weight: bold; color: #ea580c; padding: 8px 0; }
    .days { display: grid; grid-template-columns: repeat(7, 1fr); gap: 4px; }
    .day { min-height: 70px; padding: 4px; border: 2px solid #fed7aa; border-radius: 8px; background: white; cursor: pointer; }
    .day:hover { border-color: #86efac; }
    .day.today { background: #dcfce7; border-color: #22c55e; }
    .day.empty { background: transparent; border: none; cursor: default; }
    .day-num { font-size: 12px; font-weight: bold; color: #ea580c; }
    .day.today .day-num { color: #16a34a; }
    .event-dot { display: flex; align-items: center; gap: 4px; margin-top: 2px; }
    .event-dot .dot { width: 8px; height: 8px; border-radius: 50%; }
    .event-dot span { font-size: 10px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .legend { display: flex; flex-wrap: wrap; gap: 12px; justify-content: center; margin-top: 16px; padding: 12px; background: white; border-radius: 8px; border: 2px solid #fed7aa; }
    .legend-item { display: flex; align-items: center; gap: 4px; font-size: 12px; }
    .legend-item .dot { width: 12px; height: 12px; border-radius: 50%; }
    .employee-list { display: flex; flex-direction: column; gap: 12px; }
    .employee-card { background: white; border: 2px solid #fed7aa; border-radius: 12px; padding: 12px; }
    .employee-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px; }
    .employee-name { display: flex; align-items: center; gap: 8px; font-weight: bold; }
    .employee-color { width: 20px; height: 20px; border-radius: 50%; border: 2px solid white; box-shadow: 0 2px 4px rgba(0,0,0,0.2); }
    .employee-stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; }
    .stat { text-align: center; padding: 8px; border-radius: 8px; }
    .stat-num { font-size: 20px; font-weight: bold; }
    .stat-label { font-size: 10px; }
    .stat.folga { background: #dcfce7; color: #166534; }
    .stat.falta { background: #fee2e2; color: #991b1b; }
    .stat.ferias { background: #fef3c7; color: #92400e; }
    .stat.feriado { background: #ffedd5; color: #c2410c; }
    .btn-add { width: 100%; padding: 14px; background: #22c55e; color: white; border: none; border-radius: 12px; font-size: 16px; font-weight: bold; cursor: pointer; margin-bottom: 16px; }
    .btn-delete { background: none; border: none; color: #ef4444; cursor: pointer; font-size: 18px; }
    .report-cards { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-bottom: 16px; }
    .report-card { padding: 16px; border-radius: 12px; color: white; }
    .report-card.folga { background: linear-gradient(135deg, #4ade80, #16a34a); }
    .report-card.falta { background: linear-gradient(135deg, #f87171, #dc2626); }
    .report-card.ferias { background: linear-gradient(135deg, #fbbf24, #d97706); }
    .report-card.feriado { background: linear-gradient(135deg, #fb923c, #ea580c); }
    .report-card p { font-size: 12px; opacity: 0.9; }
    .report-card .num { font-size: 32px; font-weight: bold; }
    .btn-copy { width: 100%; padding: 12px; background: #22c55e; color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-bottom: 16px; }
    .modal { position: fixed; inset: 0; background: rgba(0,0,0,0.6); display: flex; align-items: center; justify-content: center; padding: 16px; z-index: 100; }
    .modal-content { background: white; border-radius: 16px; padding: 20px; width: 100%; max-width: 350px; border: 4px solid #22c55e; }
    .modal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; }
    .modal-header h3 { color: #ea580c; font-size: 18px; }
    .modal-close { background: none; border: none; font-size: 24px; color: #9ca3af; cursor: pointer; }
    .form-group { margin-bottom: 12px; }
    .form-group select, .form-group input { width: 100%; padding: 12px; border: 2px solid #fdba74; border-radius: 8px; font-size: 14px; }
    .form-group input[type="color"] { height: 50px; padding: 4px; cursor: pointer; }
    .btn-submit { width: 100%; padding: 14px; background: #22c55e; color: white; border: none; border-radius: 8px; font-size: 16px; font-weight: bold; cursor: pointer; }
    .event-list { margin-top: 16px; max-height: 200px; overflow-y: auto; }
    .event-item { display: flex; align-items: center; justify-content: space-between; padding: 10px; border-radius: 8px; margin-bottom: 8px; }
    .event-item.folga { background: #dcfce7; }
    .event-item.falta { background: #fee2e2; }
    .event-item.ferias { background: #fef3c7; }
    .event-item.feriado { background: #ffedd5; }
    .footer { text-align: center; color: white; font-size: 12px; margin-top: 12px; text-shadow: 0 1px 2px rgba(0,0,0,0.2); }
    .hidden { display: none !important; }
    .saving { background: #16a34a; color: white; padding: 6px 12px; border-radius: 20px; font-size: 12px; display: inline-flex; align-items: center; gap: 6px; margin-top: 8px; }
    .ranking { background: white; border: 2px solid #fed7aa; border-radius: 12px; padding: 16px; margin-bottom: 16px; }
    .ranking h4 { color: #ea580c; margin-bottom: 12px; }
    .ranking-item { display: flex; align-items: center; gap: 12px; margin-bottom: 10px; }
    .ranking-pos { font-size: 16px; font-weight: bold; color: #f97316; width: 30px; }
    .ranking-bar { flex: 1; height: 8px; background: #e5e7eb; border-radius: 4px; overflow: hidden; }
    .ranking-fill { height: 100%; background: linear-gradient(90deg, #fb923c, #ea580c); border-radius: 4px; }
    .ranking-info { display: flex; justify-content: space-between; align-items: center; margin-bottom: 4px; }
    .ranking-name { font-weight: 500; }
    .ranking-count { font-weight: bold; color: #ea580c; font-size: 14px; }
  </style>
</head>
<body>
  <div class="container">
    <div class="card">
      <div class="header">
        <h1>⚙️ ENGENHOCA ⚙️</h1>
        <p>O Parque da Sua Diversão 🎢</p>
        <p style="font-size:11px; margin-top:4px;">Gestão de Folgas e Escalas</p>
        <div id="savingIndicator" class="saving hidden">⏳ Salvando...</div>
      </div>

      <div id="lockBar" class="lock-bar">
        <span>🔒 Modo visualização</span>
        <div style="display:flex; gap:8px;">
          <input type="password" id="passwordInput" placeholder="Senha">
          <button onclick="unlock()">Entrar</button>
        </div>
      </div>

      <div class="tabs">
        <button class="tab active" onclick="showTab('calendar')">📅 Calendário</button>
        <button class="tab" onclick="showTab('employees')">👥 Equipe</button>
        <button class="tab" onclick="showTab('report')">📋 Relatório</button>
      </div>

      <div class="content">
        <div id="calendarView">
          <div class="nav">
            <button onclick="prevMonth()">◀</button>
            <h2 id="monthYear"></h2>
            <button onclick="nextMonth()">▶</button>
          </div>
          <div class="filter">
            <select id="filterEmployee" onchange="renderCalendar()">
              <option value="all">Todos da equipe</option>
            </select>
          </div>
          <div class="weekdays">
            <div class="weekday">Dom</div>
            <div class="weekday">Seg</div>
            <div class="weekday">Ter</div>
            <div class="weekday">Qua</div>
            <div class="weekday">Qui</div>
            <div class="weekday">Sex</div>
            <div class="weekday">Sáb</div>
          </div>
          <div class="days" id="calendarDays"></div>
          <div class="legend">
            <div class="legend-item"><div class="dot" style="background:#22c55e;"></div><span>Folga</span></div>
            <div class="legend-item"><div class="dot" style="background:#ef4444;"></div><span>Falta</span></div>
            <div class="legend-item"><div class="dot" style="background:#f59e0b;"></div><span>Férias</span></div>
            <div class="legend-item"><div class="dot" style="background:#f97316;"></div><span>Feriado</span></div>
          </div>
        </div>

        <div id="employeesView" class="hidden">
          <button class="btn-add" id="btnAddEmployee" onclick="showEmployeeModal()">+ Adicionar Funcionário</button>
          <p style="text-align:center; color:#ea580c; font-size:14px; margin-bottom:12px;">📊 Estatísticas de <span id="statsMonth"></span></p>
          <div class="employee-list" id="employeeList"></div>
        </div>

        <div id="reportView" class="hidden">
          <button class="btn-copy" onclick="copyReport()">📋 Copiar Relatório</button>
          <div class="report-cards">
            <div class="report-card folga"><p>Total Folgas</p><div class="num" id="totalFolgas">0</div></div>
            <div class="report-card falta"><p>Total Faltas</p><div class="num" id="totalFaltas">0</div></div>
            <div class="report-card ferias"><p>Total Férias</p><div class="num" id="totalFerias">0</div></div>
            <div class="report-card feriado"><p>Total Feriados</p><div class="num" id="totalFeriados">0</div></div>
          </div>
          <div class="ranking">
            <h4>👥 Ausências por Funcionário</h4>
            <div id="rankingList"></div>
          </div>
        </div>
      </div>
    </div>
    <p class="footer">🎢 Engenhoca - O Parque da Sua Diversão</p>
  </div>

  <div id="eventModal" class="modal hidden">
    <div class="modal-content">
      <div class="modal-header">
        <h3>📅 Dia <span id="modalDay"></span></h3>
        <button class="modal-close" onclick="closeEventModal()">×</button>
      </div>
      <div class="form-group">
        <select id="eventEmployee"></select>
      </div>
      <div class="form-group">
        <select id="eventType">
          <option value="folga">Folga</option>
          <option value="falta">Falta</option>
          <option value="ferias">Férias</option>
          <option value="feriado">Feriado</option>
        </select>
      </div>
      <div class="form-group">
        <input type="text" id="eventNote" placeholder="Nota (opcional)">
      </div>
      <button class="btn-submit" onclick="addEvent()">✓ Adicionar</button>
      <div class="event-list" id="dayEvents"></div>
    </div>
  </div>

  <div id="employeeModal" class="modal hidden">
    <div class="modal-content">
      <div class="modal-header">
        <h3>👤 Novo Funcionário</h3>
        <button class="modal-close" onclick="closeEmployeeModal()">×</button>
      </div>
      <div class="form-group">
        <input type="text" id="newEmployeeName" placeholder="Nome do funcionário">
      </div>
      <div class="form-group">
        <label style="font-size:14px; color:#666; margin-bottom:4px; display:block;">Cor:</label>
        <input type="color" id="newEmployeeColor" value="#4ade80">
      </div>
      <button class="btn-submit" onclick="addEmployee()">✓ Adicionar</button>
    </div>
  </div>

  <script>
    const PASS = '0811';
    const STORAGE_KEY = 'engenhoca-data-v3';
    const months = ['Janeiro','Fevereiro','Março','Abril','Maio','Junho','Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'];
    
    let currentDate = new Date();
    let isLocked = true;
    let employees = [];
    let events = [];
    let selectedDay = null;

    const defaultEmployees = [
      {id:1,name:'Wellington',color:'#4ade80'},
      {id:2,name:'Deylson',color:'#fb923c'},
      {id:3,name:'Matheus',color:'#facc15'},
      {id:4,name:'Lázaro',color:'#f87171'},
      {id:5,name:'Lucas',color:'#a3e635'},
      {id:6,name:'Vidal',color:'#fbbf24'},
      {id:7,name:'Késsio',color:'#34d399'},
      {id:8,name:'Aldair',color:'#fb7185'},
      {id:9,name:'Samuel',color:'#fcd34d'}
    ];

    function loadData() {
      try {
        const saved = localStorage.getItem(STORAGE_KEY);
        if (saved) {
          const data = JSON.parse(saved);
          employees = data.employees || defaultEmployees;
          events = data.events || [];
        } else {
          employees = [...defaultEmployees];
          events = [];
        }
      } catch(e) {
        employees = [...defaultEmployees];
        events = [];
      }
    }

    function saveData() {
      document.getElementById('savingIndicator').classList.remove('hidden');
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify({employees, events}));
      } catch(e) { console.error(e); }
      setTimeout(() => document.getElementById('savingIndicator').classList.add('hidden'), 800);
    }

    function unlock() {
      if (document.getElementById('passwordInput').value === PASS) {
        isLocked = false;
        document.getElementById('lockBar').innerHTML = '<span style="color:#166534;">✅ Modo edição ativo</span><button onclick="lock()" style="background:none;border:none;color:#16a34a;text-decoration:underline;cursor:pointer;">Bloquear</button>';
        document.getElementById('lockBar').className = 'lock-bar unlocked';
        document.getElementById('btnAddEmployee').classList.remove('hidden');
        renderAll();
      } else {
        alert('Senha incorreta!');
      }
    }

    function lock() {
      isLocked = true;
      document.getElementById('lockBar').innerHTML = '<span>🔒 Modo visualização</span><div style="display:flex;gap:8px;"><input type="password" id="passwordInput" placeholder="Senha"><button onclick="unlock()">Entrar</button></div>';
      document.getElementById('lockBar').className = 'lock-bar';
      document.getElementById('btnAddEmployee').classList.add('hidden');
      renderAll();
    }

    function showTab(tab) {
      document.querySelectorAll('.tab').forEach((t,i) => {
        t.classList.toggle('active', (tab==='calendar'&&i===0)||(tab==='employees'&&i===1)||(tab==='report'&&i===2));
      });
      document.getElementById('calendarView').classList.toggle('hidden', tab!=='calendar');
      document.getElementById('employeesView').classList.toggle('hidden', tab!=='employees');
      document.getElementById('reportView').classList.toggle('hidden', tab!=='report');
      if(tab==='report') renderReport();
    }

    function prevMonth() { currentDate.setMonth(currentDate.getMonth()-1); renderAll(); }
    function nextMonth() { currentDate.setMonth(currentDate.getMonth()+1); renderAll(); }

    function formatDate(day) {
      const y = currentDate.getFullYear();
      const m = String(currentDate.getMonth()+1).padStart(2,'0');
      const d = String(day).padStart(2,'0');
      return `${y}-${m}-${d}`;
    }

    function getMonthEvents() {
      const y = currentDate.getFullYear();
      const m = currentDate.getMonth();
      return events.filter(e => {
        const d = new Date(e.date);
        return d.getFullYear()===y && d.getMonth()===m;
      });
    }

    function renderCalendar() {
      document.getElementById('monthYear').textContent = `${months[currentDate.getMonth()]} ${currentDate.getFullYear()}`;
      
      const filter = document.getElementById('filterEmployee').value;
      const year = currentDate.getFullYear();
      const month = currentDate.getMonth();
      const firstDay = new Date(year, month, 1).getDay();
      const lastDate = new Date(year, month+1, 0).getDate();
      const today = new Date();
      
      let html = '';
      for(let i=0; i<firstDay; i++) html += '<div class="day empty"></div>';
      
      for(let d=1; d<=lastDate; d++) {
        const dateStr = formatDate(d);
        let dayEvents = events.filter(e => e.date === dateStr);
        if(filter !== 'all') dayEvents = dayEvents.filter(e => e.employeeId === parseInt(filter));
        
        const isToday = today.getDate()===d && today.getMonth()===month && today.getFullYear()===year;
        
        html += `<div class="day ${isToday?'today':''}" onclick="openDay(${d})">
          <div class="day-num">${d}</div>`;
        
        dayEvents.slice(0,3).forEach(ev => {
          const emp = employees.find(e => e.id===ev.employeeId);
          const colors = {folga:'#166534',falta:'#991b1b',ferias:'#92400e',feriado:'#c2410c'};
          html += `<div class="event-dot"><div class="dot" style="background:${emp?.color||'#999'}"></div><span style="color:${colors[ev.type]}">${emp?.name.split(' ')[0]||''}</span></div>`;
        });
        if(dayEvents.length>3) html += `<div style="font-size:10px;color:#f97316;font-weight:bold;">+${dayEvents.length-3}</div>`;
        html += '</div>';
      }
      
      document.getElementById('calendarDays').innerHTML = html;
    }

    function renderEmployeeFilter() {
      let html = '<option value="all">Todos da equipe</option>';
      employees.forEach(e => html += `<option value="${e.id}">${e.name}</option>`);
      document.getElementById('filterEmployee').innerHTML = html;
    }

    function renderEmployees() {
      document.getElementById('statsMonth').textContent = months[currentDate.getMonth()];
      const monthEvents = getMonthEvents();
      
      let html = '';
      employees.forEach(emp => {
        const empEvents = monthEvents.filter(e => e.employeeId === emp.id);
        const folgas = empEvents.filter(e => e.type==='folga').length;
        const faltas = empEvents.filter(e => e.type==='falta').length;
        const ferias = empEvents.filter(e => e.type==='ferias').length;
        const feriados = empEvents.filter(e => e.type==='feriado').length;
        
        html += `<div class="employee-card">
          <div class="employee-header">
            <div class="employee-name"><div class="employee-color" style="background:${emp.color}"></div>${emp.name}</div>
            ${!isLocked ? `<button class="btn-delete" onclick="deleteEmployee(${emp.id})">🗑️</button>` : ''}
          </div>
          <div class="employee-stats">
            <div class="stat folga"><div class="stat-num">${folgas}</div><div class="stat-label">Folgas</div></div>
            <div class="stat falta"><div class="stat-num">${faltas}</div><div class="stat-label">Faltas</div></div>
            <div class="stat ferias"><div class="stat-num">${ferias}</div><div class="stat-label">Férias</div></div>
            <div class="stat feriado"><div class="stat-num">${feriados}</div><div class="stat-label">Feriados</div></div>
          </div>
        </div>`;
      });
      
      document.getElementById('employeeList').innerHTML = html || '<p style="text-align:center;color:#999;">Nenhum funcionário</p>';
    }

    function renderReport() {
      const monthEvents = getMonthEvents();
      document.getElementById('totalFolgas').textContent = monthEvents.filter(e=>e.type==='folga').length;
      document.getElementById('totalFaltas').textContent = monthEvents.filter(e=>e.type==='falta').length;
      document.getElementById('totalFerias').textContent = monthEvents.filter(e=>e.type==='ferias').length;
      document.getElementById('totalFeriados').textContent = monthEvents.filter(e=>e.type==='feriado').length;

      const stats = employees.map(emp => {
        const empEvents = monthEvents.filter(e => e.employeeId === emp.id);
        return { ...emp, total: empEvents.length };
      }).sort((a,b) => b.total - a.total);

      const maxTotal = Math.max(...stats.map(s => s.total), 1);
      let html = '';
      stats.forEach((emp, i) => {
        html += `<div class="ranking-item">
          <span class="ranking-pos">${i+1}º</span>
          <div class="employee-color" style="background:${emp.color}"></div>
          <div style="flex:1">
            <div class="ranking-info"><span class="ranking-name">${emp.name}</span><span class="ranking-count">${emp.total} dias</span></div>
            <div class="ranking-bar"><div class="ranking-fill" style="width:${(emp.total/maxTotal)*100}%"></div></div>
          </div>
        </div>`;
      });
      document.getElementById('rankingList').innerHTML = html;
    }

    function renderAll() {
      renderEmployeeFilter();
      renderCalendar();
      renderEmployees();
    }

    function openDay(day) {
      if(isLocked) return;
      selectedDay = day;
      document.getElementById('modalDay').textContent = `${day}/${currentDate.getMonth()+1}`;
      
      let opts = '';
      employees.forEach(e => opts += `<option value="${e.id}">${e.name}</option>`);
      document.getElementById('eventEmployee').innerHTML = opts;
      document.getElementById('eventType').value = 'folga';
      document.getElementById('eventNote').value = '';
      
      renderDayEvents();
      document.getElementById('eventModal').classList.remove('hidden');
    }

    function renderDayEvents() {
      const dateStr = formatDate(selectedDay);
      const dayEvents = events.filter(e => e.date === dateStr);
      const labels = {folga:'Folga',falta:'Falta',ferias:'Férias',feriado:'Feriado'};
      
      let html = '';
      dayEvents.forEach(ev => {
        const emp = employees.find(e => e.id === ev.employeeId);
        html += `<div class="event-item ${ev.type}">
          <div><strong>${emp?.name||'?'}</strong> - ${labels[ev.type]}${ev.note?` (${ev.note})`:''}</div>
          <button class="btn-delete" onclick="deleteEvent(${ev.id})">🗑️</button>
        </div>`;
      });
      document.getElementById('dayEvents').innerHTML =
