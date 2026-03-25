# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## Email (himalaya)

- **emretarhan:** post@emretarhan.net
- **simurg:** emre.tarhan@simurg.de
- **ugur:** ugur@emretarhan.net (benim kişisel email)

## MCP Server (MiniMax)

- **Kurulum:** mcporter ile minimax MCP server eklendi
- **web_search:** `mcporter call minimax.web_search query="aranacak şey"`
- **understand_image:** `mcporter call minimax.understand_image prompt="soru" image_source="URL veya dosya yolu"`
- **API Key:** sk-cp-LEYLv0NODyBEGlgl8Ba2CtnTPU9Z2fpaiSkzcNgG0ACwRVTl_YAZDcm9fJqfm7tx9IO6UTksr7lQsKYYuaaTZWffhELWZCP6Lejz1s0FGmUe3jpAXQw8HfA
- **Not:** Bu key Coding Plan key - sadece MCP için geçerli

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## n8n

- **URL:** http://72.61.176.195:1865
- **API Key:** eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIwNDg5ZjI5Mi1kZDJhLTRiZTItOWY2Yy03MjIxYzIxYzRjNjgiLCJpc3MiOiJuOG4iLCJhdWQiOiJwdWJsaWMtYXBpIiwianRpIjoiN2RhODMzMmEtMzU1Yi00MDE5LWJhY2MtNGRmNjg2MGI5NTNlIiwiaWF0IjoxNzcxMzA3NzcyfQ.GxxMAgm_p3k0u_edzssQ5R-eOLS9QIDbSOprQCkWzak

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.
