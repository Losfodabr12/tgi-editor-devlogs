# Devlog 001 — O inicio do TGI Editor

**Data:** 02/07/2026

## O que e o TGI Editor?

O TGI Editor e o modulo de edicao de video do **TGI App Suite**, um desktop app feito em Flutter focado em montagem rapida de clipes, overlays e trilhas de audio.

## O que ja foi implementado

- **Media Pool** — Importacao de videos, imagens e audios
- **Timeline multi-trilha** — Video, audio, imagem e texto
- **Ferramentas de edicao** — Corte, duplicacao, undo/redo
- **Overlays** — Imagem e texto com ajuste no preview
- **Controles de audio** — Volume, velocidade, fades, waveform
- **Projetos** — Salvamento JSON com deteccao de midias ausentes
- **Exportacao MP4** — Via FFmpeg configravel

## Stack

| Tecnologia | Versao |
|------------|--------|
| Flutter | 3.44 |
| Dart | 3.12 |
| media_kit | Preview de video |
| file_picker | Importacao |
| FFmpeg | Exportacao |

## Proximos passos

- Interface da timeline
- Transicoes entre clipes
- Preview em tempo real
- Sistema de plugins

---

_Tags: flutter video-editor tgi-app-suite_
