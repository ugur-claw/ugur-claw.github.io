# LazyCode — AI-Powered Code Editor

## Proje Vizyonu

LazyIDE tarzında, terminal aesthetic'li, Claude Code'a bağlı, %100 Go + Wails AI code editor. 

**Referans UI:** https://pbs.twimg.com/media/HBbRsNsbsAAad7p?format=jpg

Bu UI'dan ilham al:
- Terminal/TUI tarzı, bilgi yoğun
- Solda AI activity log, ortada files + editor, sağda git status + terminal
- Monospace font (JetBrains Mono veya Fira Code)
- Dark theme, neon vurgular (cyan, magenta, yellow)

---

## Tech Stack

| Katman | Teknoloji |
|--------|-----------|
| Framework | Wails 3.x (Go + WebView) |
| UI | React + TypeScript + Vite |
| Styling | TailwindCSS |
| Editor | Monaco Editor |
| Terminal | xterm.js |
| AI | Claude Code CLI |
| State | Zustand |
| Icons | Lucide React (minimal, çizgi sitil) |

---

## Klasör Yapısı

```
lazycode/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── layout/
│   │   │   │   ├── StatusBar.tsx      # Bottom bar
│   │   │   │   ├── ActivityLog.tsx    # Left panel - AI log
│   │   │   │   ├── GitPanel.tsx       # Right panel - git status
│   │   │   │   └── TerminalPanel.tsx  # Right panel - terminal
│   │   │   ├── editor/
│   │   │   │   ├── Editor.tsx         # Monaco wrapper
│   │   │   │   ├── TabBar.tsx         # Tab bar
│   │   │   │   └── FileTree.tsx       # Left panel - file explorer
│   │   │   └── ai/
│   │   │       └── AIChat.tsx         # AI chat widget
│   │   ├── hooks/
│   │   │   ├── useAI.ts               # Claude Code wrapper
│   │   │   ├── useFiles.ts            # File operations
│   │   │   ├── useGit.ts              # Git operations
│   │   │   └── useExtensions.ts       # Extension loader
│   │   ├── lib/
│   │   │   ├── wails.ts               # Go backend bridge
│   │   │   └── store.ts               # Zustand store
│   │   ├── extensions/                # Built-in extensions
│   │   └── styles/
│   │       └── theme.css              # Terminal-style CSS
│   └── package.json
├── backend/
│   ├── main.go
│   ├── commands/
│   │   ├── file.go                    # File operations
│   │   ├── terminal.go                # PTY management
│   │   ├── ai.go                      # Claude Code bridge
│   │   └── git.go                     # Git operations
│   ├── extensions/
│   │   └── registry.go                # Extension registry
│   └── go.mod
└── docs/
    └── extension-dev.md                # Extension API docs
```

---

## UI/UX — Terminal Aesthetic

### Layout (3 Kolon)

```
┌──────────────────────────────────────────────────────────────────────┐
│  LazyCode v0.1                    main.go*  │  Go  │  UTF-8  │ Ln 12 │
├─────────┬─────────────────────────────────┬──────────────────────────┤
│ [1]-Brain        │ [1]-Files                │ [1]-Status              │
│                 │                         │                         │
│ 🤖 Thinking...  │ > src/                  │ 📦 3 files              │
│ 📝 Planning...  │   > main.go             │ 📥 1 unstaged           │
│ ✅ Done         │   > util.go             │ 🌿 main                 │
│                 │   > config/             │                         │
│                 │                         │ [0]-Stash               │
│ ─────────────── │ ───────────────         │ No stash entries       │
│                 │                         │                         │
│ Project:       │ ┌─ Product-plan.md ─┐   │ ───────────────         │
│ creative-video │ │ # Product Plan    │   │ [2]-Terminal            │
│                 │ │ ## Goals...      │   │                         │
│ ⏱ 14:32        │ │                  │   │ $ git status            │
│                 │ └──────────────────┘   │ >                        │
├─────────┴─────────────────────────────────┴──────────────────────────┤
│ F4 Help  │  Ctrl+S Save  │  Ctrl+Q Quit  │  Ctrl+K AI  │  Ctrl+P Open  │
└──────────────────────────────────────────────────────────────────────┘
```

### Renk Paleti — "Terminal Dark"

```css
:root {
  /* Background - deep black */
  --bg-primary: #0a0a0a;
  --bg-secondary: #111111;
  --bg-tertiary: #1a1a1a;
  --bg-panel: #0d0d0d;
  
  /* Borders - subtle grey */
  --border-default: #2a2a2a;
  --border-muted: #1a1a1a;
  --border-active: #3a3a3a;
  
  /* Text - high contrast */
  --text-primary: #e0e0e0;
  --text-secondary: #888888;
  --text-muted: #555555;
  --text-dim: #333333;
  
  /* Accents - neon */
  --accent-cyan: #00ff9f;     /* Success, AI */
  --accent-magenta: #ff00ff;  /* Headers, active */
  --accent-yellow: #ffcc00;   /* Warnings, important */
  --accent-blue: #00ccff;     /* Links, info */
  --accent-red: #ff3366;      /* Errors */
  
  /* Syntax */
  --syntax-keyword: #ff79c6;
  --syntax-string: #f1fa8c;
  --syntax-comment: #6272a4;
  --syntax-function: #50fa7b;
  --syntax-number: #bd93f9;
}
```

### Panel Başlıkları

Her panel için `[N]-Name` formatında başlık:
- `[1]` = aktif / odaklanmış panel
- `[0]` = boş panel
- `[2]` = dolu ama pasif

### Font

- **Tüm UI:** JetBrains Mono (ligatures için)
- **Boyut:** 13px base, 12px panel headers
- **Satır yüksekliği:** 1.4

### Panel Boyutları

- **Left (Activity):** 180px default, resize edilebilir
- **Center (Editor):** Flex, kalan alan
- **Right (Git + Terminal):** 280px default, resize edilebilir

---

## Temel Özellikler

### 1. Editor (Monaco)

- Multiple tabs (üstte)
- Syntax highlighting (built-in: Go, JS, TS, Python, Rust, Markdown)
- Line numbers
- Minimap (opsiyonel)
- Find/Replace (`Cmd+F` / `Cmd+Opt+F`)
- Word wrap toggle

### 2. AI Entegrasyonu — Claude Code

**Bağlantı:**
```go
func (a *AI) StreamPrompt(ctx context.Context, prompt string) (<-chan string, error) {
    cmd := exec.CommandContext(ctx, "claude", "-p", "--print", "--dangerously-skip-permissions")
    cmd.Stdin = strings.NewReader(prompt)
    
    stdout, _ := cmd.StdoutPipe()
    // Stream stdout to channel
}
```

**Kullanım:**
- Activity log'a AI düşüncesi yazılır
- Editor içinde inline completion için `Cmd+K`
- Sağ panelde chat widget için `Cmd+Shift+K`

### 3. Terminal

- xterm.js + Go PTY (proot veya go-pty)
- Sağ alt panelde
- Multiple terminal tabs (`, `, `` ` ``)
- Komut history (yukarı/aşağı ok)

### 4. File Explorer

- Sol orta panelde (editor'in üstünde)
- Klasör açma / dosya oluşturma / silme
- Quick open: `Cmd+P`

### 5. Git Entegrasyonu (Built-in)

**Sağ üst panel:**
- Değişen dosyalar listesi
- Branch bilgisi
- Stash durumu
- Commit history (son 5)

**Kısayollar:**
- `Cmd+Shift+G` — Git panel toggle

### 6. Activity Log (Sol Panel)

- AI'ın düşünce sürecini göster
- Yapılan işlemleri logla
- Proje istatistikleri (dosya sayısı, satır sayısı)

---

## Klavye Kısayolları

| Action | Shortcut |
|--------|----------|
| Quick Open (Cmd+P) | `Cmd+P` |
| Command Palette | `Cmd+Shift+P` |
| Save | `Cmd+S` |
| Close Tab | `Cmd+W` |
| New Tab | `Cmd+N` |
| AI Inline | `Cmd+K` |
| AI Chat | `Cmd+Shift+K` |
| Git Panel | `Cmd+Shift+G` |
| Toggle Terminal | `` Cmd+` `` |
| Toggle Sidebar | `Cmd+B` |
| Find | `Cmd+F` |
| Replace | `Cmd+Opt+F` |
| Help | `F4` |
| Quit | `Cmd+Q` |

---

## Extension Sistemi

### Extension Interface

```go
type Extension interface {
    Name() string
    Version() string
    Activate(ctx *ExtensionContext) error
    Deactivate() error
}

type ExtensionContext struct {
    // Events
    OnDidChangeActiveTextEditor(func(*TextEditor))
    OnDidSaveTextDocument(func(*TextDocument))
    OnDidChangeFileSystem(func(*FileChange))
    
    // UI
    RegisterCommand(cmd Command)
    RegisterTreeProvider(provider TreeProvider)
    
    // AI
    AskAI(prompt string) (*AIResponse, error)
    
    // Storage
    GetState(key string) (any, error)
    SetState(key string, value any) error
}
```

### Built-in Extensions

1. **git-extension** — Git status, branch, diff, stash
2. **syntax-{lang}** — Language support (Go, JS, TS, Python, Rust, Markdown, JSON, HTML, CSS)
3. **ai-chat** — AI chat paneli
4. **terminal** — Terminal paneli

### Extension Manifest

```json
{
  "name": "my-extension",
  "version": "1.0.0",
  "main": "dist/index.js",
  "contributes": {
    "commands": [{
      "id": "myext.hello",
      "title": "Say Hello",
      "keybinding": "Cmd+Shift+H"
    }],
    "languages": [{
      "id": "mylang",
      "extensions": [".mylang"]
    }],
    "themes": [{
      "id": "my-theme",
      "label": "My Theme"
    }]
  }
}
```

---

## MVP Sıralama

### Sprint 1 — Core
1. Wails projesi kurulumu
2. Layout (3 kolon, paneller)
3. Monaco Editor entegrasyonu
4. Tab sistemi

### Sprint 2 — File + Git
5. File explorer
6. Dosya açma / kaydetme
7. Git status paneli
8. Quick open (`Cmd+P`)

### Sprint 3 — AI + Terminal
9. Terminal (PTY) entegrasyonu
10. Claude Code CLI bağlantısı
11. Activity log
12. `Cmd+K` inline completion

### Sprint 4 — Extensions
13. Extension API
14. Built-in extensions
15. Settings panel

---

## Önemli Detaylar

1. **Font:** Tüm UI JetBrains Mono — monospace everywhere
2. **Panel başlıkları:** `[N]-Name` formatında
3. **Animasyon:** Minimal — sadece panel resize'da
4. **Colors:** Neon vurgular (cyan, magenta, yellow) koyu background üzerinde
5. **Klavye öncelikli:** Mouse kullanmadan her şey yapılabilmeli

---

## Örnek AI Context Prompt

```
You are LazyCode, an AI coding assistant.

Project: creative-video-engine
Current file: /src/main.go
Language: Go

Content:
```go
package main

func main() {
    fmt.Println("Hello")
}
```

Cursor at line 4, column 1.

User: "Add fibonacci function"

Provide code below in markdown blocks.
```

---

*Bu proje Emre Tarhan için 💙*
