<template>
  <!-- QR слева: оценка МК -->
  <div class="qr-left">
    <img src="/qr-eval.png" class="qr-img" alt="Оценить МК" />
    <div class="qr-label">оценить МК</div>
  </div>

  <!-- QR справа: репозиторий -->
  <div class="qr-right">
    <img src="/qr-repo.png" class="qr-img" alt="Репозиторий" />
    <div class="qr-label">репозиторий</div>
  </div>

  <!-- Wi-Fi футер по центру -->
  <div class="wifi-footer">
    Wi-Fi&nbsp;&nbsp;·&nbsp;&nbsp;Сеть: <strong>GenAI</strong>&nbsp;&nbsp;·&nbsp;&nbsp;Пароль: <strong>SberAI2026</strong>
  </div>
</template>

<style>
/* ── QR левый (оценка МК) ── */
.qr-left {
  position: fixed;
  left: 20px;
  bottom: 18px;
  z-index: 50;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
  pointer-events: none;
}

/* ── QR правый (репозиторий) ── */
.qr-right {
  position: fixed;
  right: 20px;
  bottom: 18px;
  z-index: 50;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
  pointer-events: none;
}

.qr-img {
  width: 90px;
  height: 90px;
  border-radius: 8px;
  background: white;
  padding: 4px;
  box-shadow: 0 4px 16px rgba(15, 23, 42, 0.12);
}

.qr-label {
  font-size: 10px;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  color: #475569;
  padding: 2px 8px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.82);
  box-shadow: 0 2px 8px rgba(15, 23, 42, 0.08);
}

/* ── Wi-Fi футер ── */
.wifi-footer {
  position: fixed;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 50;
  font-size: 11px;
  color: #64748b;
  background: rgba(255, 255, 255, 0.80);
  padding: 3px 16px;
  border-radius: 999px;
  box-shadow: 0 2px 8px rgba(15, 23, 42, 0.08);
  white-space: nowrap;
  pointer-events: none;
}

/* ── Отступы контента, чтобы не перекрывался QR ── */
.qr-safe {
  padding-left: 120px;
  padding-right: 120px;
  padding-bottom: 60px;
}

.slidev-layout.cover h1,
.slidev-layout.end h1 {
  max-width: 900px;
}
</style>
