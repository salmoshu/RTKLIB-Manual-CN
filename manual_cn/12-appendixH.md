---
sidebarDepth: 2
---

# 附录H. 常见问题

## H.1 PRN 与 SAT 编号对应关系

RTKLIB为了方便矩阵的构建与运算，内部对卫星进行了顺序编号，与之相关的函数是 `satid2no` 和 `satno2id` 。下面是卫星 prn 与 sat 编号的转换工具（输入卫星 ID 或编号，点击按钮进行转换）：

<style>
  /* 输入框和按钮样式 */
  input.H-1-1-input, button.H-1-1-button {
    margin: 5px;
    padding: 8px 12px;
    border: 1px solid #ced4da;
    border-radius: 4px;
    font-size: 14px;
  }
  input#H-1-1-MAXCMP {
    width: 30px;
    margin: 5px;
    padding: 4px 6px;
    border: 1px solid #ced4da;
    border-radius: 4px;
    font-size: 14px;
    transition: border-color 0.3s ease;
  }
  input.H-1-1-input {
    width: 200px;
    transition: border-color 0.3s ease;
  }
  input.H-1-1-input:focus {
    border-color: #007bff;
    outline: none;
    box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
  }

  /* 按钮样式优化 */
  button.H-1-1-button {
    background-color: #6c757d; /* 灰色背景 */
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s ease;
    padding: 8px 15px;
    font-weight: 500;
  }
  button.H-1-1-button:hover {
    background-color: #5a6268; /* 悬停时变深 */
    transform: translateY(-1px); /* 轻微上移 */
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  button.H-1-1-button:active {
    transform: translateY(0);
    box-shadow: none;
  }
</style>

<script>
const DEFAULT_SYSTEMS = {
  GPS: { min: 1, max: 32, offset: 0, code: 'G' },
  GLO: { min: 1, max: 27, offset: 32, code: 'R' },
  GAL: { min: 1, max: 36, offset: 59, code: 'E' },
  QZS: { min: 193, max: 202, offset: 95, code: 'J' },
  CMP: { min: 1, max: 46, offset: 105, code: 'C' }, // 默认最大值 46
  IRN: { min: 1, max: 14, offset: 151, code: 'I' },
  SBS: { min: 120, max: 158, offset: 165, code: 'S' }
};

let enabledSystems = { ...DEFAULT_SYSTEMS };

function updateOffsets() {
  let currentOffset = 0;
  for (const sys in enabledSystems) {
    enabledSystems[sys].offset = currentOffset;
    currentOffset += enabledSystems[sys].max - enabledSystems[sys].min + 1;
  }
}

function updateSystemMax(system, maxValue) {
  if (DEFAULT_SYSTEMS[system] && !isNaN(maxValue) && maxValue >= DEFAULT_SYSTEMS[system].min) {
    enabledSystems[system] = { ...DEFAULT_SYSTEMS[system], max: parseInt(maxValue) };
    updateOffsets();
  }
}

function toggleSystem(system, checked) {
  if (checked) {
    enabledSystems[system] = { ...DEFAULT_SYSTEMS[system] };
    // 更新 CMP 的 max 值
    if (system === 'CMP') {
      const maxCmp = parseInt(document.getElementById('H-1-1-MAXCMP').value) || DEFAULT_SYSTEMS.CMP.max;
      updateSystemMax('CMP', maxCmp);
    }
  } else {
    delete enabledSystems[system];
  }
  updateOffsets();
}

function updateEnabledSystems() {
  const checkboxes = {
    'GPS': document.getElementById('checkbox-GPS'),
    'GLO': document.getElementById('checkbox-GLO'),
    'GAL': document.getElementById('checkbox-GAL'),
    'QZS': document.getElementById('checkbox-QZS'),
    'CMP': document.getElementById('checkbox-CMP'),
    'IRN': document.getElementById('checkbox-IRN'),
    'SBS': document.getElementById('checkbox-SBS')
  };
  enabledSystems = {};
  for (const [sys, checkbox] of Object.entries(checkboxes)) {
    if (checkbox.checked) {
      enabledSystems[sys] = { ...DEFAULT_SYSTEMS[sys] };
      if (sys === 'CMP') {
        const maxCmp = parseInt(document.getElementById('H-1-1-MAXCMP').value) || DEFAULT_SYSTEMS.CMP.max;
        updateSystemMax(sys, maxCmp);
      }
    }
  }
  updateOffsets();
}

function satid2no(id) {
  id = id.trim();
  let sys, prn;
  if (/^\d+$/.test(id)) {
    prn = parseInt(id);
    if (enabledSystems.GPS && enabledSystems.GPS.min <= prn && prn <= enabledSystems.GPS.max) sys = 'GPS';
    else if (enabledSystems.SBS && enabledSystems.SBS.min <= prn && prn <= enabledSystems.SBS.max) sys = 'SBS';
    else if (enabledSystems.QZS && enabledSystems.QZS.min <= prn && prn <= enabledSystems.QZS.max) sys = 'QZS';
    else if (enabledSystems.CMP && enabledSystems.CMP.min <= prn && prn <= enabledSystems.CMP.max) sys = 'CMP';
    else return 0;
    return satno(sys, prn);
  }
  const match = id.match(/^([GREJCIS])(\d+)$/);
  if (!match) return 0;
  const code = match[1], prnStr = match[2];
  prn = parseInt(prnStr);
  switch (code) {
    case 'G': sys = 'GPS'; prn += DEFAULT_SYSTEMS.GPS.min - 1; break;
    case 'R': sys = 'GLO'; prn += DEFAULT_SYSTEMS.GLO.min - 1; break;
    case 'E': sys = 'GAL'; prn += DEFAULT_SYSTEMS.GAL.min - 1; break;
    case 'J': sys = 'QZS'; prn += DEFAULT_SYSTEMS.QZS.min - 1; break;
    case 'C': sys = 'CMP'; prn += DEFAULT_SYSTEMS.CMP.min - 1; break;
    case 'I': sys = 'IRN'; prn += DEFAULT_SYSTEMS.IRN.min - 1; break;
    case 'S': sys = 'SBS'; prn += 100; break;
    default: return 0;
  }
  if (!enabledSystems[sys] || prn < enabledSystems[sys].min || prn > enabledSystems[sys].max) return 0;
  return satno(sys, prn);
}

function satno(sys, prn) {
  const system = enabledSystems[sys];
  if (!system || prn < system.min || prn > system.max) return 0;
  return system.offset + (prn - system.min + 1);
}

function satno2id(sat) {
  for (const [sys, { min, max, offset, code }] of Object.entries(enabledSystems)) {
    const satMin = offset + 1;
    const satMax = offset + (max - min + 1);
    if (sat >= satMin && sat <= satMax) {
      const prn = 1 + (sat - offset - 1);
      if (sys === 'SBS') return prn.toString().padStart(3, '0');
      return `${code}${prn.toString().padStart(2, '0')}`;
    }
  }
  return '';
}

function convertIdToNo() {
  updateEnabledSystems(); // 更新所有复选框状态，包括 CMP 的 max
  const id = document.getElementById('satIdInput').value;
  const sat = satid2no(id);
  document.getElementById('H-1-1-sat').value = 
    sat ? `${sat}` : 'Invalid input';
}

function convertNoToId() {
  updateEnabledSystems(); // 更新所有复选框状态，包括 CMP 的 max
  const sat = parseInt(document.getElementById('satNoInput').value);
  const id = satno2id(sat);
  document.getElementById('H-1-1-prn').value = 
    id ? `${id}` : 'Invalid input';
}

// 仅在浏览器环境中绑定全局函数
if (typeof window !== 'undefined') {
  window.convertIdToNo = convertIdToNo;
  window.convertNoToId = convertNoToId;
  window.toggleSystem = toggleSystem;
}
</script>

<div class="constellation">
  <label><input type="checkbox" id="checkbox-GPS" checked onchange="toggleSystem('GPS', this.checked)"> GPS</label>
  <label><input type="checkbox" id="checkbox-GLO" checked onchange="toggleSystem('GLO', this.checked)"> GLONASS</label>
  <label><input type="checkbox" id="checkbox-GAL" checked onchange="toggleSystem('GAL', this.checked)"> Galileo</label>
  <label><input type="checkbox" id="checkbox-QZS" checked onchange="toggleSystem('QZS', this.checked)"> QZSS</label>
  <label><input type="checkbox" id="checkbox-CMP" checked onchange="toggleSystem('CMP', this.checked)"> BeiDou (MAX=<input id="H-1-1-MAXCMP" value="46">) </label>
  <label><input type="checkbox" id="checkbox-IRN" checked onchange="toggleSystem('IRN', this.checked)"> IRNSS</label>
  <label><input type="checkbox" id="checkbox-SBS" checked onchange="toggleSystem('SBS', this.checked)"> SBAS</label>
</div>

<div>
  <input type="text" id="satIdInput" class="H-1-1-input" placeholder="例如 G05">
  <button id="H-1-1" class="H-1-1-button" onclick="convertIdToNo()">转换为SAT</button>
  <input type="text" id="H-1-1-sat" class="H-1-1-input" disabled="disabled">
</div>
<div>
  <input type="text" id="satNoInput" class="H-1-1-input" placeholder="例如 5">
  <button id="H-1-1" class="H-1-1-button" onclick="convertNoToId()">转换为PRN</button>
  <input type="text" id="H-1-1-prn" class="H-1-1-input" disabled="disabled">
</div>
