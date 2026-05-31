(function () {
  "use strict";

  const DICE_FACES = {
    D4: [1, 2, 3, 4],
    D8: [1, 1, 2, 2, 3, 3, 4, 4],
    D12: [1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4],
    D20: [1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 3, 3, 3, 3, 3, 4, 4, 4, 4, 4],
  };

  const DICE_TYPES = ["D4", "D8", "D12", "D20"];
  const FILTERS = ["全部", "D4", "D8", "D12", "D20"];
  const STORE_KEY = "offlineDiceToolState.v1";
  const LONG_PRESS_MS = 560;

  const MATERIALS = [
    { id: "silver", name: "统一银白金属", short: "银白" },
  ];

  const DEFAULT_SETTINGS = {
    shake: true,
    vibration: true,
    sound: true,
    animation3d: true,
    darkMode: true,
    material: "silver",
  };

  const state = {
    currentDice: "D20",
    currentPage: "roll",
    previousPage: "roll",
    historyFilter: "全部",
    statsFilter: "全部",
    lastPointer: {
      clientX: 0,
      clientY: 0,
      touchX: 0,
      touchY: 0,
    },
    nonce: 0,
    lastRoll: null,
    savedLastRollId: null,
    history: [],
    settings: { ...DEFAULT_SETTINGS },
    drawerOpen: false,
    shakeLock: false,
    audioContext: null,
    suppressNextClick: false,
  };

  const $ = (selector) => document.querySelector(selector);
  const $$ = (selector) => Array.from(document.querySelectorAll(selector));

  const el = {
    body: document.body,
    pages: $$(".page"),
    nav: $$(".bottom-nav button"),
    drawer: $("#sideDrawer"),
    drawerScrim: $("#drawerScrim"),
    drawerLinks: $$(".drawer-links button"),
    currentDiceLabel: $("#currentDiceLabel"),
    currentResult: $("#currentResult"),
    finalResult: $("#finalResult"),
    resultTitle: $("#resultTitle"),
    autoSaveHint: $("#autoSaveHint"),
    mainDiceVisual: $("#mainDiceVisual"),
    resultDiceVisual: $("#resultDiceVisual"),
    stylePreviewDice: $("#stylePreviewDice"),
    diceGrid: $("#diceGrid"),
    styleGrid: $("#styleGrid"),
    historyFilters: $("#historyFilters"),
    historyList: $("#historyList"),
    statsGrid: $("#statsGrid"),
    barChart: $("#barChart"),
    statsFilterLabel: $("#statsFilterLabel"),
    settingsList: $("#settingsList"),
    drawerDarkToggle: $("#drawerDarkToggle"),
    toast: $("#toast"),
  };

  function loadState() {
    try {
      const saved = JSON.parse(localStorage.getItem(STORE_KEY) || "{}");
      if (DICE_TYPES.includes(saved.currentDice)) state.currentDice = saved.currentDice;
      if (Number.isFinite(saved.nonce)) state.nonce = saved.nonce;
      if (Array.isArray(saved.history)) state.history = saved.history.filter(isValidHistoryItem);
      if (saved.settings && typeof saved.settings === "object") {
        state.settings = { ...DEFAULT_SETTINGS, ...saved.settings };
      }
      if (!MATERIALS.some((material) => material.id === state.settings.material)) {
        state.settings.material = DEFAULT_SETTINGS.material;
      }
      if (saved.lastRoll && isValidRoll(saved.lastRoll)) state.lastRoll = saved.lastRoll;
      if (typeof saved.savedLastRollId === "string") state.savedLastRollId = saved.savedLastRollId;
    } catch (error) {
      console.warn("Failed to load local state", error);
    }
  }

  function saveState() {
    const payload = {
      currentDice: state.currentDice,
      nonce: state.nonce,
      history: state.history,
      settings: state.settings,
      lastRoll: state.lastRoll,
      savedLastRollId: state.savedLastRollId,
    };
    localStorage.setItem(STORE_KEY, JSON.stringify(payload));
  }

  function isValidRoll(item) {
    return item && DICE_TYPES.includes(item.dice) && [1, 2, 3, 4].includes(item.result);
  }

  function isValidHistoryItem(item) {
    return isValidRoll(item) && typeof item.id === "string" && typeof item.time === "number";
  }

  function setPage(pageName, options = {}) {
    if (!pageName) return;
    const target = $(`.page[data-page="${pageName}"]`);
    if (!target) return;

    state.previousPage = state.currentPage;
    state.currentPage = pageName;
    el.pages.forEach((page) => page.classList.toggle("active", page.dataset.page === pageName));
    el.nav.forEach((button) => {
      const navPage = button.dataset.nav;
      const active = navPage === pageName || (pageName === "select" && navPage === "roll") || (pageName === "result" && navPage === "roll");
      button.classList.toggle("active", active);
    });
    el.drawerLinks.forEach((button) => button.classList.toggle("active", button.dataset.drawerNav === pageName));
    if (!options.keepDrawer) closeDrawer();
    render();
  }

  function render() {
    applyTheme();
    renderDiceVisuals();
    renderDiceGrid();
    renderStyleGrid();
    renderHistoryFilters();
    renderHistory();
    renderStats();
    renderSettings();
    updateRollLabels();
  }

  function updateRollLabels() {
    el.currentDiceLabel.textContent = state.currentDice;
    el.resultTitle.textContent = state.lastRoll ? state.lastRoll.dice : state.currentDice;
    el.currentResult.textContent = state.lastRoll ? state.lastRoll.result : "-";
    el.finalResult.textContent = state.lastRoll ? state.lastRoll.result : "-";
    el.statsFilterLabel.textContent = state.statsFilter;
    el.drawerDarkToggle.checked = state.settings.darkMode;
  }

  function renderDiceVisuals() {
    const visuals = [el.mainDiceVisual, el.resultDiceVisual, el.stylePreviewDice].filter(Boolean);
    visuals.forEach((visual) => {
      const dice = visual === el.resultDiceVisual && state.lastRoll ? state.lastRoll.dice : state.currentDice;
      visual.src = getDiceAsset(dice);
      visual.alt = `${dice} 骰子`;
      visual.dataset.dice = dice;
      visual.className = `dice-icon${visual.id === "mainDiceVisual" ? " dice-icon-main" : ""}${visual.id === "resultDiceVisual" ? " result-dice" : ""}`;
    });
  }

  function getDiceAsset(dice) {
    const normalized = DICE_TYPES.includes(dice) ? dice.toLowerCase() : "d20";
    return `./assets/dice-${normalized}.svg`;
  }

  function createDiceMarkup(dice) {
    return `<img class="dice-icon dice-icon-card" src="${getDiceAsset(dice)}" alt="${dice} 骰子" />`;
  }

  function renderDiceGrid() {
    el.diceGrid.innerHTML = DICE_TYPES.map((dice) => {
      const selected = dice === state.currentDice;
      return `
        <button class="select-card ${selected ? "selected" : ""}" type="button" data-dice-choice="${dice}" aria-label="选择 ${dice}">
          ${selected ? '<span class="checkmark">✓</span>' : ""}
          <strong>${dice}</strong>
          ${createDiceMarkup(dice)}
          <span>结果：1-4</span>
        </button>
      `;
    }).join("");
  }

  function renderStyleGrid() {
    el.styleGrid.innerHTML = MATERIALS.map((material) => {
      const selected = material.id === state.settings.material;
      return `
        <button class="style-card ${selected ? "selected" : ""}" type="button" data-style-choice="${material.id}" aria-label="${material.name}">
          ${selected ? '<span class="checkmark">✓</span>' : ""}
          ${createDiceMarkup(state.currentDice)}
          <strong>${material.short}</strong>
          <span>${material.name}</span>
        </button>
      `;
    }).join("");
  }

  function renderHistoryFilters() {
    el.historyFilters.innerHTML = FILTERS.map((filter) => {
      const active = filter === state.historyFilter;
      return `<button type="button" class="${active ? "active" : ""}" data-history-filter="${filter}">${filter}</button>`;
    }).join("");
  }

  function renderHistory() {
    const items = getFilteredHistory(state.historyFilter);
    if (!items.length) {
      el.historyList.innerHTML = '<div class="empty-state">暂无历史记录</div>';
      return;
    }

    el.historyList.innerHTML = items.map((item) => `
      <article class="history-item">
        <img class="history-dice-icon" src="${getDiceAsset(item.dice)}" alt="${item.dice} 骰子" />
        <div class="history-meta">
          <strong>${item.dice}</strong>
          <span>${item.result}</span>
        </div>
        <time>${formatDate(item.time)}</time>
      </article>
    `).join("");
  }

  function renderStats() {
    const items = getFilteredHistory(state.statsFilter);
    const counts = countResults(items);
    const values = items.map((item) => item.result);
    const total = items.length;
    const average = total ? (values.reduce((sum, value) => sum + value, 0) / total).toFixed(2) : "0.00";
    const min = total ? Math.min(...values) : "-";
    const max = total ? Math.max(...values) : "-";
    const cards = [
      ["总掷骰次数", total],
      ["平均值", average],
      ["最小值", min],
      ["最大值", max],
      ["1出现次数", counts[1]],
      ["2出现次数", counts[2]],
      ["3出现次数", counts[3]],
      ["4出现次数", counts[4]],
    ];
    el.statsGrid.innerHTML = cards.map(([label, value]) => `
      <div class="stat-card">
        <span>${label}</span>
        <strong>${value}</strong>
      </div>
    `).join("");

    const maxCount = Math.max(1, counts[1], counts[2], counts[3], counts[4]);
    el.barChart.innerHTML = [1, 2, 3, 4].map((result) => {
      const height = Math.max(4, Math.round((counts[result] / maxCount) * 100));
      return `
        <div class="bar-wrap">
          <span class="bar-count">${counts[result]}</span>
          <div class="bar" style="height:${height}%"></div>
          <span class="bar-label">${result}</span>
        </div>
      `;
    }).join("");
  }

  function renderSettings() {
    const rows = [
      { key: "shake", title: "摇一摇掷骰", desc: "摇动手机进行掷骰", icon: "shake", type: "toggle" },
      { key: "vibration", title: "震动反馈", desc: "掷骰结果震动提示", icon: "vibrate", type: "toggle" },
      { key: "sound", title: "音效", desc: "掷骰音效", icon: "sound", type: "toggle" },
      { key: "animation3d", title: "3D动画开关", desc: "显示骰子3D动画", icon: "cube", type: "toggle" },
      { key: "style", title: "骰子样式", desc: getMaterialName(state.settings.material), icon: "style", type: "link", page: "style" },
      { key: "darkMode", title: "深色模式", desc: "跟随系统之外的本地开关", icon: "moon", type: "toggle" },
      { key: "clear", title: "清除历史记录", desc: "删除本地保存的掷骰记录", icon: "trash", type: "action" },
      { key: "about", title: "关于应用", desc: "离线、本地、均分随机", icon: "info", type: "link", page: "about" },
    ];

    el.settingsList.innerHTML = rows.map((row) => `
      <button class="setting-row" type="button" data-setting-key="${row.key}" ${row.page ? `data-setting-page="${row.page}"` : ""}>
        ${icon(row.icon)}
        <span>
          <strong>${row.title}</strong>
          <span>${row.desc}</span>
        </span>
        ${settingControl(row)}
      </button>
    `).join("");
  }

  function settingControl(row) {
    if (row.type === "toggle") {
      return `
        <label class="switch" aria-label="${row.title}">
          <input type="checkbox" data-toggle="${row.key}" ${state.settings[row.key] ? "checked" : ""} />
          <i></i>
        </label>
      `;
    }
    if (row.type === "action") return '<span class="row-chevron"></span>';
    return '<span class="row-chevron"></span>';
  }

  function icon(name) {
    const paths = {
      shake: '<path d="M7 8a5 5 0 0 0 0 8M4 5a9 9 0 0 0 0 14M17 8a5 5 0 0 1 0 8M20 5a9 9 0 0 1 0 14" /><path d="M10 7h4v10h-4z" />',
      vibrate: '<path d="M8 7h8v10H8z" /><path d="M4 9v6M20 9v6M2 11v2M22 11v2" />',
      sound: '<path d="M11 5 6 9H3v6h3l5 4V5Z" /><path d="M15.5 8.5a5 5 0 0 1 0 7" />',
      cube: '<path d="M12 2 4 6.5v11L12 22l8-4.5v-11L12 2Z" /><path d="m4 6.5 8 4.5 8-4.5M12 11v11" />',
      style: '<path d="M12 3 4 7.5v9L12 21l8-4.5v-9L12 3Z" /><path d="M8 10h.01M16 10h.01M12 15h.01" />',
      moon: '<path d="M21 12.8A8.5 8.5 0 1 1 11.2 3 6.6 6.6 0 0 0 21 12.8Z" />',
      trash: '<path d="M3 6h18M8 6V4h8v2M6 6l1 15h10l1-15" /><path d="M10 11v6M14 11v6" />',
      info: '<path d="M12 22a10 10 0 1 0 0-20 10 10 0 0 0 0 20Z" /><path d="M12 16v-4M12 8h.01" />',
    };
    return `<svg viewBox="0 0 24 24" aria-hidden="true">${paths[name] || paths.info}</svg>`;
  }

  function getFilteredHistory(filter) {
    const items = filter === "全部" ? state.history : state.history.filter((item) => item.dice === filter);
    return [...items].sort((a, b) => b.time - a.time);
  }

  function countResults(items) {
    return items.reduce((counts, item) => {
      counts[item.result] += 1;
      return counts;
    }, { 1: 0, 2: 0, 3: 0, 4: 0 });
  }

  function getMaterialName(id) {
    return MATERIALS.find((material) => material.id === id)?.name || MATERIALS[0].name;
  }

  async function rollDice(eventLike = {}) {
    updatePointerFromEvent(eventLike);
    const rollButton = $("#rollButton");
    const diceVisuals = [el.mainDiceVisual, el.resultDiceVisual].filter(Boolean);
    diceVisuals.forEach((visual) => {
      visual.classList.toggle("rolling", state.settings.animation3d);
      visual.addEventListener("animationend", () => visual.classList.remove("rolling"), { once: true });
    });
    rollButton.disabled = true;

    const result = await randomResult(state.currentDice);
    const now = Date.now();
    state.lastRoll = {
      id: createId(),
      dice: state.currentDice,
      result,
      time: now,
    };
    autoSaveLastRoll();
    updateRollLabels();

    if (state.settings.vibration && navigator.vibrate) navigator.vibrate([18, 28, 22]);
    if (state.settings.sound) playRollSound();
    setTimeout(() => {
      rollButton.disabled = false;
      setPage("result");
      showAutoSaveHint();
    }, state.settings.animation3d ? 520 : 80);
  }

  async function randomResult(dice) {
    const faces = DICE_FACES[dice] || DICE_FACES.D20;
    state.nonce += 1;
    const entropy = collectEntropy();
    saveState();

    try {
      if (window.crypto?.subtle?.digest) {
        const text = JSON.stringify(entropy);
        const bytes = new TextEncoder().encode(text);
        const digest = await crypto.subtle.digest("SHA-256", bytes);
        const index = hashToModulo(new Uint8Array(digest), faces.length);
        return faces[index];
      }
    } catch (error) {
      console.warn("SHA-256 random path failed", error);
    }

    const digest = sha256Bytes(JSON.stringify(entropy));
    return faces[hashToModulo(digest, faces.length)];
  }

  function collectEntropy() {
    const randomWords = new Uint32Array(8);
    if (window.crypto?.getRandomValues) {
      crypto.getRandomValues(randomWords);
    } else {
      for (let index = 0; index < randomWords.length; index += 1) {
        randomWords[index] = fallbackEntropyWord(index);
      }
    }

    return {
      randomWords: Array.from(randomWords),
      dateNow: Date.now(),
      performanceNow: performance.now(),
      clientX: state.lastPointer.clientX,
      clientY: state.lastPointer.clientY,
      touchX: state.lastPointer.touchX,
      touchY: state.lastPointer.touchY,
      innerWidth: window.innerWidth,
      innerHeight: window.innerHeight,
      devicePixelRatio: window.devicePixelRatio || 1,
      nonce: state.nonce,
    };
  }

  function hashToModulo(bytes, modulo) {
    let value = 0n;
    for (let index = 0; index < 8; index += 1) {
      value = (value << 8n) + BigInt(bytes[index]);
    }
    return Number(value % BigInt(modulo));
  }

  function fallbackEntropyWord(index) {
    const value = `${Date.now()}|${performance.now()}|${state.nonce}|${index}|${window.innerWidth}|${window.innerHeight}|${state.lastPointer.clientX}|${state.lastPointer.clientY}`;
    let hash = 2166136261;
    for (let cursor = 0; cursor < value.length; cursor += 1) {
      hash ^= value.charCodeAt(cursor);
      hash = Math.imul(hash, 16777619);
    }
    return hash >>> 0;
  }

  function sha256Bytes(message) {
    const rightRotate = (value, amount) => (value >>> amount) | (value << (32 - amount));
    const mathPow = Math.pow;
    const maxWord = mathPow(2, 32);
    const words = [];
    const hash = [];
    const k = [];
    let primeCounter = 0;
    let candidate = 2;

    const isPrime = (number) => {
      const limit = Math.sqrt(number);
      for (let factor = 2; factor <= limit; factor += 1) {
        if (number % factor === 0) return false;
      }
      return true;
    };

    while (primeCounter < 64) {
      if (isPrime(candidate)) {
        if (primeCounter < 8) hash[primeCounter] = (mathPow(candidate, 0.5) * maxWord) | 0;
        k[primeCounter] = (mathPow(candidate, 1 / 3) * maxWord) | 0;
        primeCounter += 1;
      }
      candidate += 1;
    }

    const encoded = unescape(encodeURIComponent(message));
    const length = encoded.length;
    for (let index = 0; index < length; index += 1) {
      words[index >> 2] |= encoded.charCodeAt(index) << ((3 - index) % 4) * 8;
    }
    words[length >> 2] |= 0x80 << ((3 - length) % 4) * 8;
    words[((length + 8) >> 6 << 4) + 15] = length * 8;

    for (let block = 0; block < words.length; block += 16) {
      const w = words.slice(block, block + 16);
      const oldHash = hash.slice(0);
      for (let index = 0; index < 64; index += 1) {
        const w15 = w[index - 15];
        const w2 = w[index - 2];
        const a = hash[0];
        const e = hash[4];
        const temp1 = hash[7]
          + (rightRotate(e, 6) ^ rightRotate(e, 11) ^ rightRotate(e, 25))
          + ((e & hash[5]) ^ (~e & hash[6]))
          + k[index]
          + (w[index] = index < 16 ? w[index] : (
            w[index - 16]
            + (rightRotate(w15, 7) ^ rightRotate(w15, 18) ^ (w15 >>> 3))
            + w[index - 7]
            + (rightRotate(w2, 17) ^ rightRotate(w2, 19) ^ (w2 >>> 10))
          ) | 0);
        const temp2 = (rightRotate(a, 2) ^ rightRotate(a, 13) ^ rightRotate(a, 22))
          + ((a & hash[1]) ^ (a & hash[2]) ^ (hash[1] & hash[2]));

        hash[7] = hash[6];
        hash[6] = hash[5];
        hash[5] = hash[4];
        hash[4] = (hash[3] + temp1) | 0;
        hash[3] = hash[2];
        hash[2] = hash[1];
        hash[1] = hash[0];
        hash[0] = (temp1 + temp2) | 0;
      }

      for (let index = 0; index < 8; index += 1) {
        hash[index] = (hash[index] + oldHash[index]) | 0;
      }
    }

    const output = new Uint8Array(32);
    for (let index = 0; index < 8; index += 1) {
      output[index * 4] = (hash[index] >>> 24) & 255;
      output[index * 4 + 1] = (hash[index] >>> 16) & 255;
      output[index * 4 + 2] = (hash[index] >>> 8) & 255;
      output[index * 4 + 3] = hash[index] & 255;
    }
    return output;
  }

  function autoSaveLastRoll() {
    if (!state.lastRoll || state.savedLastRollId === state.lastRoll.id) return;
    state.history.unshift({ ...state.lastRoll });
    state.history = state.history.slice(0, 500);
    state.savedLastRollId = state.lastRoll.id;
    saveState();
    renderHistory();
    renderStats();
  }

  function showAutoSaveHint() {
    if (!el.autoSaveHint) return;
    el.autoSaveHint.classList.add("show");
    clearTimeout(showAutoSaveHint.timer);
    showAutoSaveHint.timer = setTimeout(() => {
      el.autoSaveHint.classList.remove("show");
    }, 2000);
  }

  function clearHistory() {
    state.history = [];
    state.savedLastRollId = null;
    saveState();
    renderHistory();
    renderStats();
    showToast("历史记录已清空");
  }

  function updatePointerFromEvent(eventLike) {
    const event = eventLike || {};
    const touch = event.touches?.[0] || event.changedTouches?.[0];
    if (touch) {
      state.lastPointer.touchX = Math.round(touch.clientX);
      state.lastPointer.touchY = Math.round(touch.clientY);
      state.lastPointer.clientX = Math.round(touch.clientX);
      state.lastPointer.clientY = Math.round(touch.clientY);
      return;
    }
    if (Number.isFinite(event.clientX)) state.lastPointer.clientX = Math.round(event.clientX);
    if (Number.isFinite(event.clientY)) state.lastPointer.clientY = Math.round(event.clientY);
  }

  function createId() {
    return `${Date.now().toString(36)}-${state.nonce.toString(36)}-${performance.now().toString(36).replace(".", "")}`;
  }

  function formatDate(time) {
    const date = new Date(time);
    const pad = (value) => String(value).padStart(2, "0");
    return `${date.getFullYear()}-${pad(date.getMonth() + 1)}-${pad(date.getDate())} ${pad(date.getHours())}:${pad(date.getMinutes())}:${pad(date.getSeconds())}`;
  }

  function showToast(message) {
    el.toast.textContent = message;
    el.toast.classList.add("show");
    clearTimeout(showToast.timer);
    showToast.timer = setTimeout(() => el.toast.classList.remove("show"), 1800);
  }

  function playRollSound() {
    try {
      const AudioContext = window.AudioContext || window.webkitAudioContext;
      if (!AudioContext) return;
      const context = state.audioContext || new AudioContext();
      state.audioContext = context;
      const oscillator = context.createOscillator();
      const gain = context.createGain();
      oscillator.type = "triangle";
      oscillator.frequency.setValueAtTime(220, context.currentTime);
      oscillator.frequency.exponentialRampToValueAtTime(420, context.currentTime + 0.08);
      gain.gain.setValueAtTime(0.0001, context.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.12, context.currentTime + 0.02);
      gain.gain.exponentialRampToValueAtTime(0.0001, context.currentTime + 0.16);
      oscillator.connect(gain);
      gain.connect(context.destination);
      oscillator.start();
      oscillator.stop(context.currentTime + 0.18);
    } catch (error) {
      console.warn("Audio unavailable", error);
    }
  }

  function applyTheme() {
    document.documentElement.classList.toggle("light-mode", !state.settings.darkMode);
    document.body.classList.toggle("light-mode", !state.settings.darkMode);
  }

  function openDrawer() {
    state.drawerOpen = true;
    el.drawer.classList.add("open");
    el.drawer.setAttribute("aria-hidden", "false");
  }

  function closeDrawer() {
    state.drawerOpen = false;
    el.drawer.classList.remove("open");
    el.drawer.setAttribute("aria-hidden", "true");
  }

  function toggleSetting(key, forcedValue) {
    if (!(key in state.settings)) return;
    state.settings[key] = typeof forcedValue === "boolean" ? forcedValue : !state.settings[key];
    saveState();
    render();
  }

  function bindEvents() {
    document.addEventListener("pointerdown", updatePointerFromEvent, { passive: true });
    document.addEventListener("touchstart", updatePointerFromEvent, { passive: true });
    document.addEventListener("touchmove", updatePointerFromEvent, { passive: true });

    $("#rollButton").addEventListener("click", rollDice);
    $("#rerollButton").addEventListener("click", rollDice);
    $("#soundPreviewButton").addEventListener("click", playRollSound);
    $("#dicePickerButton").addEventListener("click", () => setPage("select"));
    $("#stageDiceButton").addEventListener("click", () => setPage("select"));
    $("#quickSettingsButton").addEventListener("click", () => setPage("settings"));
    $("#menuButton").addEventListener("click", openDrawer);
    $("#drawerSettingsButton").addEventListener("click", () => setPage("settings"));
    el.drawerScrim.addEventListener("click", closeDrawer);
    $("#clearHistoryButton").addEventListener("click", clearHistory);
    $("#filterFocusButton").addEventListener("click", () => showToast(`当前筛选：${state.historyFilter}`));

    $("#statsDiceButton").addEventListener("click", () => {
      const index = FILTERS.indexOf(state.statsFilter);
      state.statsFilter = FILTERS[(index + 1) % FILTERS.length];
      renderStats();
      updateRollLabels();
    });

    el.drawerDarkToggle.addEventListener("change", (event) => {
      toggleSetting("darkMode", event.target.checked);
    });

    document.addEventListener("click", (event) => {
      const navButton = event.target.closest("[data-nav]");
      if (navButton) setPage(navButton.dataset.nav);

      const drawerNav = event.target.closest("[data-drawer-nav]");
      if (drawerNav) setPage(drawerNav.dataset.drawerNav);

      const backButton = event.target.closest("[data-back]");
      if (backButton) setPage(backButton.dataset.back || "roll");

      const diceChoice = event.target.closest("[data-dice-choice]");
      if (diceChoice) {
        state.currentDice = diceChoice.dataset.diceChoice;
        saveState();
        setPage("roll");
        showToast(`已选择 ${state.currentDice}`);
      }

      const styleChoice = event.target.closest("[data-style-choice]");
      if (styleChoice) {
        state.settings.material = styleChoice.dataset.styleChoice;
        saveState();
        render();
        showToast(getMaterialName(state.settings.material));
      }

      const historyFilter = event.target.closest("[data-history-filter]");
      if (historyFilter) {
        state.historyFilter = historyFilter.dataset.historyFilter;
        renderHistoryFilters();
        renderHistory();
      }

      const settingRow = event.target.closest("[data-setting-key]");
      if (settingRow) handleSettingClick(event, settingRow);
    });

    bindLongPress($("#rollButton"), rollDice);
    bindLongPress($("#stageDiceButton"), rollDice);
    bindShake();
  }

  function handleSettingClick(event, row) {
    const key = row.dataset.settingKey;
    const toggle = event.target.closest("[data-toggle]");
    if (toggle) {
      toggleSetting(toggle.dataset.toggle, toggle.checked);
      return;
    }
    if (key === "clear") {
      clearHistory();
      return;
    }
    const page = row.dataset.settingPage;
    if (page) {
      setPage(page);
      return;
    }
    if (key in state.settings) toggleSetting(key);
  }

  function bindLongPress(target, callback) {
    if (!target) return;
    let timer = null;
    target.addEventListener("pointerdown", (event) => {
      state.suppressNextClick = false;
      timer = setTimeout(() => {
        state.suppressNextClick = true;
        callback(event);
      }, LONG_PRESS_MS);
    });
    target.addEventListener("click", (event) => {
      if (!state.suppressNextClick) return;
      event.preventDefault();
      event.stopImmediatePropagation();
      state.suppressNextClick = false;
    }, true);
    target.addEventListener("pointerup", () => {
      clearTimeout(timer);
      timer = null;
      setTimeout(() => {
        state.suppressNextClick = false;
      }, 0);
    });
    ["pointercancel", "pointerleave"].forEach((type) => {
      target.addEventListener(type, () => {
        clearTimeout(timer);
        timer = null;
        state.suppressNextClick = false;
      });
    });
  }

  function bindShake() {
    let last = { x: 0, y: 0, z: 0, time: 0 };
    window.addEventListener("devicemotion", (event) => {
      if (!state.settings.shake || state.shakeLock) return;
      const acc = event.accelerationIncludingGravity;
      if (!acc) return;
      const now = Date.now();
      if (now - last.time < 240) return;
      const delta = Math.abs(acc.x - last.x) + Math.abs(acc.y - last.y) + Math.abs(acc.z - last.z);
      last = { x: acc.x || 0, y: acc.y || 0, z: acc.z || 0, time: now };
      if (delta > 28) {
        state.shakeLock = true;
        rollDice({ clientX: state.lastPointer.clientX, clientY: state.lastPointer.clientY });
        setTimeout(() => {
          state.shakeLock = false;
        }, 1200);
      }
    }, { passive: true });
  }

  loadState();
  bindEvents();
  render();
}());
