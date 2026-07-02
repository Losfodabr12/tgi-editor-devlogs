# Devlog 002 - IA Assistente de Edicao

**Data:** 02/07/2026 | **Status:** Planejamento | **Feature:** IA Editor

---

## Visao

Ter uma IA integrada ao TGI Editor que entende comandos em portugues e executa edicoes complexas na timeline. O usuario fala o que quer e a IA faz.

Exemplo: *corta um highlight de PvP, bota um som quando o jogo finaliza e recorta 4 segundos antes do som tocar*

---

## Como funciona

### 1. O usuario fala o que quer

"Faz um clipe dos melhores momentos de PvP, coloca a trilha `Chosen_One` de fundo e quando tocar o som de `Sucesso.mp3` recorta 4 segundos antes"

### 2. O TGI Editor extrai o contexto

```json
{ "comando": "faz um clipe dos melhores momentos..." }
```

O editor monta um pacote com:
- Timeline atual (clips, marcadores, duracoes)
- Media pool (arquivos disponiveis)
- Waveforms com timestamps de picos de audio
- Acoes disponiveis (cortar, split, volume, overlay, export)

### 3. A IA processa e responde com acoes

```json
{ "actions": [
  { "type": "add_track", "kind": "audio" },
  { "type": "import_media", "file": "Sucesso.mp3" },
  { "type": "find_audio_peak", "file": "Sucesso.mp3", "label": "som_vitoria" },
  { "type": "split_clip", "track": 0, "at": "som_vitoria - 4s" },
  { "type": "import_media", "file": "Chosen_One.mp3" },
  { "type": "add_to_track", "track": 3, "clip": "Chosen_One", "volume": 0.3 },
  { "type": "add_fade", "track": 3, "in": 1.0, "out": 2.0 }
], "explicacao": "Detectei o som de vitoria aos 45.2s, recortei 4s antes, adicionei trilha de fundo com fade" }
```

### 4. O controller executa

O `EditorController` (que voce ja tem) recebe as acoes e executa uma por uma usando os metodos existentes: `splitAt()`, `addTrack()`, `importMedia()`, `setVolume()`, etc.

---

## Inteligencia que a IA precisa ter

| Capacidade | Como fazer |
|------------|-----------|
| Entender audio | Extrair waveform + timestamps do FFmpeg e mandar como contexto
| Reconhecer sons especificos | Comparar picos de amplitude ou usar modelo leve de audio classification
| Cortar no momento certo | A IA recebe os timestamps exatos dos picos e calcula offsets
| Manter coesao | A IA ve a timeline inteira como JSON, sabe onde cada coisa esta
| Desfazer cagada | Undo/redo ja existe, a IA so manda mais acoes

---

## API gratuita pra comecar

| API | Cota gratis | Modelo |
|-----|-------------|--------|
| Gemini 2.5 Flash | 1500 req/dia | google_generative_ai |
| OpenRouter | Creditos gratuitos | Varios modelos |
| Groq | 30 req/min | llama-4 |

---

## Arquivos novos necessarios
```
lib/features/editor/application/ai_assistant_service.dart  # Servico de IA
lib/features/editor/application/ai_context_builder.dart     # Monta contexto da timeline
lib/features/editor/application/ai_action_executor.dart     # Executa acoes da IA
lib/features/editor/presentation/widgets/ai_panel.dart      # UI do chat com IA
lib/features/editor/domain/models/ai_action.dart            # Modelo de acao
```

---

## Riscos e limites

- **Custo:** APIs gratuitas tem limite, mas 1500 req/dia da pra muito teste
- **Token limit:** Contexto da timeline pode ser grande - comprimir JSON ou usar resumo
- **IA alucina:** Pode inventar arquivo que nao existe. Validar todas as acoes antes de executar
- **Latencia:** Uns 2-5 segundos por comando complexo. Mostrar loading/spinner
- **Offline:** Sem internet = sem IA. Pensar em fallback com regras basicas locais

---

*Tags: ai, flutter, video-editor, tgi-app-suite, devlog, gemini*
