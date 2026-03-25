# CLAUDE.md — LazyCode Development Guide

Bu dosya, LazyCode projesini geliştirirken AI asistanının (senin) davranışını belirler.

---

## Proje Kimliği

- **Ad:** LazyCode
- **Tür:** AI-Powered Code Editor (Cursor alternatifi)
- **Stack:** Wails (Go) + React + TypeScript + Monaco Editor + Claude Code CLI
- **UI Stil:** Terminal aesthetic, lazyIDE tarzı, dark theme

---

## Tasarım Kuralları

### 1. UI/UX

- **Font:** Sadece JetBrains Mono (veya Fira Code)
- **Theme:** Dark, neon vurgular (cyan, magenta, yellow)
- **Panel başlıkları:** `[N]-Name` formatında (örn: `[1]-Status`, `[0]-Stash`)
- **Layout:** 3 kolon — solda activity, ortada editor, sağda git + terminal
- **Minimalizm:** Gereksiz UI elementi yok, bilgi yoğun

### 2. Kod Kalitesi

- Go: `gofmt` + `golint` kullan
- TypeScript: Strict mode, type safety
- Component'ler: Küçük, single-responsibility
- Naming: Açıklayıcı, İngilizce

### 3. Dosya Yapısı

Frontend ve backend ayrı klasörlerde:
- `frontend/` — React + Vite + TypeScript
- `backend/` — Go + Wails

---

## Önemli Özellikler

### Klavye Öncelikli
Her özellik klavye kısayolu ile erişilebilir olmalı. Mouse opsiyonel.

### AI Entegrasyonu
- Claude Code CLI kullan
- Streaming: Anlık yanıt göster
- Context: Mevcut dosya + cursor pozisyonu + proje bilgisi

### Extension Sistemi
- Her extension izole çalışır
- Crash bir extension'ı diğerleri etkilemez
- Built-in: git, syntax highlighting, terminal, ai-chat

---

## Prohibited

1. **Commercial dependencies** — Açık kaynak ve ücretsiz tut
2. **Telemetry/Analytics** — Kullanıcı takip etme
3. **Over-engineering** — Basit tut, gereksiz soyutlama yapma

---

## IPC Protokolü (Frontend ↔ Backend)

### Frontend → Backend

```typescript
// Go function calls via @wails/runtime
window.go.main.App.OpenFile(path: string): Promise<string>
window.go.main.App.SaveFile(path: string, content: string): Promise<void>
window.go.main.App.RunTerminal(cmd: string): Promise<void>
window.go.main.App.GetGitStatus(): Promise<GitStatus>
window.go.main.App.AskClaude(prompt: string): Promise<string>
```

### Backend → Frontend

```go
// Events
Events.On("terminal:output", func(data string) {})
Events.On("ai:stream", func(chunk string) {})
Events.On("file:changed", func(path string) {})
```

---

## Örnek Component Yapısı

```tsx
// frontend/src/components/layout/StatusBar.tsx
export function StatusBar() {
  const { activeFile, language, cursorPosition } = useStore()
  
  return (
    <div className="h-6 bg-[#111] border-t border-[#2a2a2a] flex items-center px-2 text-xs font-mono text-[#888]">
      <span className="text-[#00ff9f]">🌿</span>
      <span className="ml-1">{activeFile}</span>
      <span className="ml-4">{language}</span>
      <span className="ml-auto">Ln {cursorPosition.line}, Col {cursorPosition.col}</span>
    </div>
  )
}
```

---

## Test Edilecek Senaryolar

1. Dosya açma / kaydetme
2. Multiple tabs
3. Terminal komut çalıştırma
4. Claude Code AI prompt
5. Git status güncellenmesi
6. Extension yükleme / kaldırma
7. Klavye kısayolları

---

## İleride

- [ ] LSP desteği
- [ ] Multiple AI providers
- [ ] Web version (PWA)
- [ ] Plugin marketplace

---

*Bu dosya AI asistanı için referans belgesidir.*
