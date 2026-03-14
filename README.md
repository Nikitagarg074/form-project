<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Registration Form</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=DM+Mono:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0e0e0f;
    --surface: #161618;
    --border: #2a2a2e;
    --border-focus: #c8a96e;
    --text: #f0ede8;
    --muted: #6b6b75;
    --gold: #c8a96e;
    --gold-dim: #8a7349;
    --error: #e05252;
    --success: #52b788;
    --warn: #e09052;
    --pw-1: #e05252;
    --pw-2: #e09052;
    --pw-3: #d4c24a;
    --pw-4: #52b788;
  }

  html { font-size: 16px; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Mono', monospace;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    position: relative;
    overflow-x: hidden;
  }

  /* Background texture */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background:
      radial-gradient(ellipse 80% 50% at 20% 10%, rgba(200,169,110,0.06) 0%, transparent 60%),
      radial-gradient(ellipse 60% 40% at 80% 80%, rgba(82,183,136,0.04) 0%, transparent 60%);
    pointer-events: none;
  }

  .grid-bg {
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(200,169,110,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(200,169,110,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
  }

  /* Layout */
  .page {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    max-width: 1000px;
    width: 100%;
    align-items: start;
  }

  @media (max-width: 720px) {
    .page { grid-template-columns: 1fr; gap: 2rem; }
  }

  /* Left panel */
  .intro {
    padding-top: 1rem;
  }

  .eyebrow {
    font-size: 0.65rem;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 1.5rem;
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
  .eyebrow::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--gold-dim);
    opacity: 0.4;
  }

  .intro h1 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(2rem, 4vw, 3rem);
    line-height: 1.15;
    font-weight: 700;
    margin-bottom: 1.25rem;
    letter-spacing: -0.01em;
  }

  .intro h1 em {
    font-style: italic;
    color: var(--gold);
  }

  .intro p {
    font-size: 0.78rem;
    line-height: 1.8;
    color: var(--muted);
    margin-bottom: 2.5rem;
    font-weight: 300;
  }

  /* Submissions list */
  .submissions-panel h3 {
    font-size: 0.65rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 1rem;
  }

  .submissions-list {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    max-height: 240px;
    overflow-y: auto;
  }
  .submissions-list::-webkit-scrollbar { width: 2px; }
  .submissions-list::-webkit-scrollbar-track { background: transparent; }
  .submissions-list::-webkit-scrollbar-thumb { background: var(--border); }

  .submission-item {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 0.65rem 0.85rem;
    font-size: 0.7rem;
    line-height: 1.6;
    animation: slideIn 0.3s ease;
  }
  .submission-item .s-name { color: var(--text); font-weight: 500; }
  .submission-item .s-email { color: var(--muted); }
  .submission-item .s-time { color: var(--gold-dim); font-size: 0.6rem; }

  .empty-state {
    font-size: 0.7rem;
    color: var(--muted);
    font-style: italic;
    padding: 1rem 0;
  }

  @keyframes slideIn {
    from { opacity: 0; transform: translateY(-6px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* Form */
  .form-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 2px;
    padding: 2.5rem;
    position: relative;
  }
  .form-card::before {
    content: '';
    position: absolute;
    top: 0; left: 2rem; right: 2rem;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--gold), transparent);
  }

  .form-group {
    margin-bottom: 1.5rem;
    position: relative;
  }

  label {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.65rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 0.5rem;
    transition: color 0.2s;
  }
  .form-group:focus-within label { color: var(--gold); }

  .input-wrap {
    position: relative;
  }

  input[type="text"],
  input[type="email"],
  input[type="tel"],
  input[type="password"] {
    width: 100%;
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 2px;
    color: var(--text);
    font-family: 'DM Mono', monospace;
    font-size: 0.85rem;
    font-weight: 400;
    padding: 0.75rem 1rem;
    outline: none;
    transition: border-color 0.2s, box-shadow 0.2s;
    -webkit-appearance: none;
  }

  input:focus {
    border-color: var(--gold-dim);
    box-shadow: 0 0 0 3px rgba(200,169,110,0.08);
  }

  input.valid {
    border-color: var(--success);
  }
  input.invalid {
    border-color: var(--error);
  }

  /* Toggle password */
  .pw-toggle {
    position: absolute;
    right: 0.75rem;
    top: 50%;
    transform: translateY(-50%);
    background: none;
    border: none;
    cursor: pointer;
    color: var(--muted);
    padding: 0.25rem;
    display: flex;
    align-items: center;
    transition: color 0.2s;
  }
  .pw-toggle:hover { color: var(--gold); }
  .pw-toggle svg { width: 16px; height: 16px; }

  input[type="password"],
  input.pw-input {
    padding-right: 2.5rem;
  }

  /* Field status icon */
  .field-icon {
    position: absolute;
    right: 0.75rem;
    top: 50%;
    transform: translateY(-50%);
    font-size: 0.75rem;
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.2s;
  }
  .field-icon.show { opacity: 1; }
  .field-icon.ok { color: var(--success); }
  .field-icon.err { color: var(--error); }

  /* Feedback */
  .feedback {
    font-size: 0.65rem;
    margin-top: 0.4rem;
    min-height: 1em;
    transition: color 0.2s;
    letter-spacing: 0.03em;
  }
  .feedback.error { color: var(--error); }
  .feedback.success { color: var(--success); }
  .feedback.info { color: var(--muted); }

  /* Password strength meter */
  .pw-strength {
    margin-top: 0.6rem;
  }
  .strength-bar {
    display: flex;
    gap: 3px;
    margin-bottom: 0.4rem;
  }
  .strength-seg {
    flex: 1;
    height: 3px;
    background: var(--border);
    border-radius: 2px;
    transition: background 0.3s, transform 0.2s;
    transform-origin: left;
  }
  .strength-seg.active { transform: scaleY(1.4); }

  .strength-label {
    font-size: 0.6rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    transition: color 0.3s;
  }

  /* Checklist */
  .pw-checklist {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.3rem;
    margin-top: 0.6rem;
  }
  .pw-check {
    font-size: 0.62rem;
    color: var(--muted);
    display: flex;
    align-items: center;
    gap: 0.35rem;
    transition: color 0.2s;
  }
  .pw-check.met { color: var(--success); }
  .pw-check .dot {
    width: 5px; height: 5px;
    border-radius: 50%;
    background: currentColor;
    flex-shrink: 0;
  }

  /* Submit */
  .submit-btn {
    width: 100%;
    background: var(--gold);
    color: #0e0e0f;
    border: none;
    border-radius: 2px;
    font-family: 'DM Mono', monospace;
    font-size: 0.7rem;
    font-weight: 500;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    padding: 1rem;
    cursor: pointer;
    transition: background 0.2s, transform 0.1s, opacity 0.2s;
    margin-top: 0.5rem;
  }
  .submit-btn:hover:not(:disabled) { background: #d4b87a; }
  .submit-btn:active:not(:disabled) { transform: scale(0.99); }
  .submit-btn:disabled { opacity: 0.35; cursor: not-allowed; }

  /* Toast */
  .toast {
    position: fixed;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%) translateY(120%);
    background: var(--success);
    color: #0e0e0f;
    font-family: 'DM Mono', monospace;
    font-size: 0.72rem;
    letter-spacing: 0.08em;
    padding: 0.75rem 1.5rem;
    border-radius: 2px;
    transition: transform 0.35s cubic-bezier(0.34,1.56,0.64,1);
    z-index: 100;
    white-space: nowrap;
  }
  .toast.show { transform: translateX(-50%) translateY(0); }

  /* Divider */
  .divider {
    border: none;
    border-top: 1px solid var(--border);
    margin: 1.5rem 0;
  }
</style>
</head>
<body>
<div class="grid-bg"></div>

<div class="page">
  <!-- Left: Intro + Submissions -->
  <div class="intro">
    <div class="eyebrow">Registration</div>
    <h1>Create your<br><em>account.</em></h1>
    <p>Real-time validation on every field. Password strength analysis. Each submission is persisted to localStorage for review.</p>

    <hr class="divider">

    <div class="submissions-panel">
      <h3>Submissions (<span id="sub-count">0</span>)</h3>
      <div class="submissions-list" id="submissions-list">
        <div class="empty-state">No submissions yet.</div>
      </div>
    </div>
  </div>

  <!-- Right: Form -->
  <div class="form-card">
    <!-- Name -->
    <div class="form-group">
      <label for="name">Full Name</label>
      <div class="input-wrap">
        <input type="text" id="name" autocomplete="name" placeholder="Jane Smith">
        <span class="field-icon" id="name-icon"></span>
      </div>
      <div class="feedback info" id="name-fb"></div>
    </div>

    <!-- Email -->
    <div class="form-group">
      <label for="email">Email Address</label>
      <div class="input-wrap">
        <input type="email" id="email" autocomplete="email" placeholder="jane@example.com">
        <span class="field-icon" id="email-icon"></span>
      </div>
      <div class="feedback info" id="email-fb"></div>
    </div>

    <!-- Phone -->
    <div class="form-group">
      <label for="phone">Phone Number</label>
      <div class="input-wrap">
        <input type="tel" id="phone" autocomplete="tel" placeholder="+1 (555) 000-0000">
        <span class="field-icon" id="phone-icon"></span>
      </div>
      <div class="feedback info" id="phone-fb"></div>
    </div>

    <!-- Password -->
    <div class="form-group">
      <label for="password">Password</label>
      <div class="input-wrap">
        <input type="password" id="password" class="pw-input" autocomplete="new-password" placeholder="Create a strong password">
        <button class="pw-toggle" id="pw-toggle" type="button" aria-label="Toggle password visibility">
          <svg id="eye-icon" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
            <path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z"/>
            <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"/>
          </svg>
        </button>
      </div>

      <!-- Strength meter -->
      <div class="pw-strength" id="pw-strength" style="display:none">
        <div class="strength-bar">
          <div class="strength-seg" id="seg0"></div>
          <div class="strength-seg" id="seg1"></div>
          <div class="strength-seg" id="seg2"></div>
          <div class="strength-seg" id="seg3"></div>
        </div>
        <div class="strength-label" id="strength-label">—</div>
        <div class="pw-checklist">
          <div class="pw-check" id="chk-length"><span class="dot"></span>8+ characters</div>
          <div class="pw-check" id="chk-upper"><span class="dot"></span>Uppercase</div>
          <div class="pw-check" id="chk-lower"><span class="dot"></span>Lowercase</div>
          <div class="pw-check" id="chk-number"><span class="dot"></span>Number</div>
          <div class="pw-check" id="chk-special"><span class="dot"></span>Special char</div>
          <div class="pw-check" id="chk-long"><span class="dot"></span>12+ characters</div>
        </div>
      </div>
      <div class="feedback info" id="password-fb"></div>
    </div>

    <button class="submit-btn" id="submit-btn" disabled>Submit Registration</button>
  </div>
</div>

<div class="toast" id="toast">✓ Submission saved</div>

<script>
  // ── State ──────────────────────────────────────────────────────────────
  const validity = { name: false, email: false, phone: false, password: false };

  // ── Elements ───────────────────────────────────────────────────────────
  const $ = id => document.getElementById(id);
  const nameEl = $('name'), emailEl = $('email'), phoneEl = $('phone'), pwEl = $('password');
  const submitBtn = $('submit-btn');

  // ── Helpers ────────────────────────────────────────────────────────────
  function setField(id, ok, msg, type = ok ? 'success' : 'error') {
    const el = $(id);
    el.classList.remove('valid','invalid');
    el.classList.add(ok ? 'valid' : 'invalid');

    const fb = $(`${id}-fb`);
    if (fb) { fb.textContent = msg; fb.className = `feedback ${type}`; }

    const icon = $(`${id}-icon`);
    if (icon) {
      icon.textContent = ok ? '✓' : '✗';
      icon.className = `field-icon show ${ok ? 'ok' : 'err'}`;
    }
  }

  function clearField(id) {
    $(id).classList.remove('valid','invalid');
    const fb = $(`${id}-fb`);
    if (fb) { fb.textContent = ''; fb.className = 'feedback info'; }
    const icon = $(`${id}-icon`);
    if (icon) icon.className = 'field-icon';
  }

  function updateSubmit() {
    submitBtn.disabled = !Object.values(validity).every(Boolean);
  }

  // ── Name ───────────────────────────────────────────────────────────────
  nameEl.addEventListener('input', () => {
    const v = nameEl.value.trim();
    if (!v) { clearField('name'); validity.name = false; updateSubmit(); return; }

    if (v.length < 2) {
      setField('name', false, 'Name must be at least 2 characters');
      validity.name = false;
    } else if (!/^[a-zA-Z\s'\-\.]+$/.test(v)) {
      setField('name', false, 'Only letters, spaces, hyphens and apostrophes allowed');
      validity.name = false;
    } else if (v.split(/\s+/).filter(Boolean).length < 2) {
      setField('name', false, 'Please enter your full name');
      validity.name = false;
    } else {
      setField('name', true, 'Looks good!');
      validity.name = true;
    }
    updateSubmit();
  });

  // ── Email ──────────────────────────────────────────────────────────────
  emailEl.addEventListener('input', () => {
    const v = emailEl.value.trim();
    if (!v) { clearField('email'); validity.email = false; updateSubmit(); return; }

    const re = /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/;
    if (!re.test(v)) {
      setField('email', false, 'Enter a valid email address');
      validity.email = false;
    } else {
      setField('email', true, 'Valid email address');
      validity.email = true;
    }
    updateSubmit();
  });

  // ── Phone ──────────────────────────────────────────────────────────────
  phoneEl.addEventListener('input', () => {
    const raw = phoneEl.value;
    const digits = raw.replace(/\D/g, '');

    if (!raw.trim()) { clearField('phone'); validity.phone = false; updateSubmit(); return; }

    if (digits.length < 7) {
      setField('phone', false, 'Too short — enter a valid phone number');
      validity.phone = false;
    } else if (digits.length > 15) {
      setField('phone', false, 'Too many digits');
      validity.phone = false;
    } else if (digits.length >= 10) {
      setField('phone', true, 'Valid phone number');
      validity.phone = true;
    } else {
      setField('phone', false, `${digits.length} of 10+ digits entered`, 'info');
      validity.phone = false;
    }
    updateSubmit();
  });

  // Auto-format phone as user types
  phoneEl.addEventListener('keyup', () => {
    let v = phoneEl.value.replace(/\D/g, '');
    if (v.length > 10) v = v.slice(0,10);
    if (v.length >= 6) v = `(${v.slice(0,3)}) ${v.slice(3,6)}-${v.slice(6)}`;
    else if (v.length >= 3) v = `(${v.slice(0,3)}) ${v.slice(3)}`;
    // Only reformat if input doesn't already have non-digit chars (avoid cursor issues)
    if (phoneEl.value !== v && v.length > 0) phoneEl.value = v;
  });

  // ── Password ───────────────────────────────────────────────────────────
  const checks = {
    length:  v => v.length >= 8,
    upper:   v => /[A-Z]/.test(v),
    lower:   v => /[a-z]/.test(v),
    number:  v => /[0-9]/.test(v),
    special: v => /[^A-Za-z0-9]/.test(v),
    long:    v => v.length >= 12,
  };
  const checkIds = { length:'chk-length', upper:'chk-upper', lower:'chk-lower', number:'chk-number', special:'chk-special', long:'chk-long' };
  const strengthColors = ['#e05252','#e09052','#d4c24a','#52b788'];
  const strengthLabels = ['Weak','Fair','Good','Strong'];

  pwEl.addEventListener('input', () => {
    const v = pwEl.value;
    const strengthPanel = $('pw-strength');

    if (!v) {
      strengthPanel.style.display = 'none';
      clearField('password');
      validity.password = false;
      updateSubmit();
      return;
    }

    strengthPanel.style.display = 'block';

    // Evaluate checks
    const met = {};
    Object.entries(checks).forEach(([k,fn]) => {
      met[k] = fn(v);
      $(`chk-${k === 'length' ? 'length' : k === 'long' ? 'long' : k}`);
      const el = $(checkIds[k]);
      el.classList.toggle('met', met[k]);
    });

    // Score 0-4
    const coreScore = ['length','upper','lower','number','special'].filter(k=>met[k]).length;
    const score = Math.min(4, Math.floor(coreScore * 0.85) + (met.long ? 1 : 0));
    const clampedScore = Math.min(3, Math.max(0, score - 1));

    // Update bars
    for (let i = 0; i < 4; i++) {
      const seg = $(`seg${i}`);
      const active = i < score;
      seg.style.background = active ? strengthColors[clampedScore] : 'var(--border)';
      seg.classList.toggle('active', active);
    }

    $('strength-label').textContent = score > 0 ? strengthLabels[clampedScore] : '—';
    $('strength-label').style.color = score > 0 ? strengthColors[clampedScore] : 'var(--muted)';

    // Field validity
    const allCore = ['length','upper','lower','number','special'].every(k=>met[k]);
    if (!met.length) {
      setField('password', false, 'Password is too short');
      validity.password = false;
    } else if (!allCore) {
      setField('password', false, 'Missing requirements — see checklist above', 'error');
      validity.password = false;
    } else {
      setField('password', true, score >= 3 ? 'Strong password!' : 'Password meets requirements');
      validity.password = true;
    }
    updateSubmit();
  });

  // ── Show/Hide Password ─────────────────────────────────────────────────
  const pwToggle = $('pw-toggle');
  const eyeIcon = $('eye-icon');
  let pwVisible = false;

  pwToggle.addEventListener('click', () => {
    pwVisible = !pwVisible;
    pwEl.type = pwVisible ? 'text' : 'password';
    eyeIcon.innerHTML = pwVisible
      ? `<path stroke-linecap="round" stroke-linejoin="round" d="M3.98 8.223A10.477 10.477 0 0 0 1.934 12C3.226 16.338 7.244 19.5 12 19.5c.993 0 1.953-.138 2.863-.395M6.228 6.228A10.451 10.451 0 0 1 12 4.5c4.756 0 8.773 3.162 10.065 7.498a10.522 10.522 0 0 1-4.293 5.774M6.228 6.228 3 3m3.228 3.228 3.65 3.65m7.894 7.894L21 21m-3.228-3.228-3.65-3.65m0 0a3 3 0 1 0-4.243-4.243m4.242 4.242L9.88 9.88"/>`
      : `<path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z"/><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"/>`;
    pwToggle.style.color = pwVisible ? 'var(--gold)' : '';
  });

  // ── Submission ─────────────────────────────────────────────────────────
  submitBtn.addEventListener('click', () => {
    const entry = {
      name: nameEl.value.trim(),
      email: emailEl.value.trim(),
      phone: phoneEl.value.trim(),
      timestamp: new Date().toISOString()
    };

    // localStorage persistence
    let subs = [];
    try {
      subs = JSON.parse(localStorage.getItem('submissions') || '[]');
    } catch(e) {}
    subs.unshift(entry);
    try {
      localStorage.setItem('submissions', JSON.stringify(subs));
    } catch(e) {}

    renderSubmissions(subs);
    showToast();
    resetForm();
  });

  function renderSubmissions(subs) {
    $('sub-count').textContent = subs.length;
    const list = $('submissions-list');
    if (!subs.length) {
      list.innerHTML = '<div class="empty-state">No submissions yet.</div>';
      return;
    }
    list.innerHTML = subs.map(s => `
      <div class="submission-item">
        <div class="s-name">${escHtml(s.name)}</div>
        <div class="s-email">${escHtml(s.email)}</div>
        <div class="s-time">${new Date(s.timestamp).toLocaleString()}</div>
      </div>
    `).join('');
  }

  function escHtml(s) {
    return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
  }

  function showToast() {
    const t = $('toast');
    t.classList.add('show');
    setTimeout(() => t.classList.remove('show'), 2400);
  }

  function resetForm() {
    [nameEl, emailEl, phoneEl, pwEl].forEach(el => { el.value = ''; });
    Object.keys(validity).forEach(k => validity[k] = false);
    ['name','email','phone','password'].forEach(clearField);
    $('pw-strength').style.display = 'none';
    pwEl.type = 'password';
    pwVisible = false;
    eyeIcon.innerHTML = `<path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z"/><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"/>`;
    pwToggle.style.color = '';
    updateSubmit();
  }

  // ── Init: load existing submissions ───────────────────────────────────
  (function init() {
    let subs = [];
    try { subs = JSON.parse(localStorage.getItem('submissions') || '[]'); } catch(e) {}
    renderSubmissions(subs);
  })();
</script>
</body>
</html># form-project
