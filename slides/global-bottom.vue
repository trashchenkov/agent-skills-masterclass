<template>
  <!-- QR слева: Telegram-канал -->
  <div class="qr-left">
    <img src="/qr-telegram.svg" class="qr-img" alt="Telegram-канал" />
    <div class="qr-label">Telegram</div>
  </div>

  <!-- QR справа: репозиторий -->
  <div class="qr-right">
    <img src="/qr-repo.svg" class="qr-img" alt="Репозиторий" />
    <div class="qr-label">репозиторий</div>
  </div>
</template>

<style>
/* ── QR левый: Telegram ── */
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

/* ── QR правый: репозиторий ── */
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
