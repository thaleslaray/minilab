# Synth Space Academy — Design Document

## Visão

Um jogo auto-contido (Web Audio API + Web MIDI) que ensina sintetizador através do MiniLab 3.
O jogador não "aprende controles" — ele descobre um instrumento brincando e sai tendo CRIADO música.

## Filosofia

Baseado em 4 metodologias combinadas:

| Fonte | Camada | Contribuição |
|-------|--------|-------------|
| Annie Curwen (1886) | Macro-sequência | "A coisa antes do símbolo" — ouvir/fazer antes de ler. Uma coisa de cada vez, depois combinar |
| Marie Jaëll (1846-1925) | Sensorial | Toque consciente — cada gesto é sentido e conectado ao resultado sonoro |
| Dorothy Taubman (1917-2013) | Biomecânica | Movimento natural, zero tensão, postura desde o dia 1 |
| Berklee EPD | Síntese | Signal-flow first. Hear → Describe → Build. Aluno sai com um PATCH, não um badge |

Inspiração de produto: **Teenage Engineering** — constraint como jogo, descoberta sem manual, brinquedo sério com personalidade.

## Princípios de Design

1. **Mini-wow first** — Cada fase começa com uma descoberta surpreendente, não uma instrução
2. **Erro gera som interessante, não silêncio** — Nunca punir, sempre reagir
3. **O jogador CRIA, não completa** — O output é uma música, não uma pontuação
4. **Constraint abre, não fecha** — Limitação é trampolim criativo (estilo OP-1)
5. **Hear → Describe → Build** — O loop de aprendizado da Berklee aplicado a cada fase
6. **Standalone** — Tudo via Web Audio API, sem DAW externo

## Drives Psicológicos (pesquisa Reddit r/synthesizers + r/teenageengineering)

- **Autoria do som**: "Este som existe porque EU fiz" — não "eu toquei esta música"
- **Curiosidade infinita**: Rabbit hole onde cada tweak revela algo novo
- **Momentos mágicos comprimidos**: Um knob muda tudo — curiosidade + agência + surpresa
- **Flow sem pressão**: Girar knobs = meditação tátil com feedback imediato
- **Narrativa tecnológica**: Habitar um universo (espacial, no caso)
- **Identidade**: O MiniLab 3 se torna "o meu instrumento" ao longo do jogo

## Estrutura: 5 Fases de Aprendizado + 1 Modo Criativo

### Fase 0 — DESPERTAR (Teclas livres)
- **Controle:** Qualquer tecla
- **Mini-wow:** Cada tecla acorda um subsistema da nave com visual e som únicos
- **Constraint:** Nenhuma — só explorar
- **Output:** A nave liga (5 notas distintas, mas o jogador não sabe o número)
- **Metodologia:** Curwen L1 (explorar e descobrir), Taubman (contato relaxado)

### Fase 1 — HARMÔNICOS DO ESCUDO (Knobs/Filtro)
- **Controle:** Qualquer knob, fader ou mod strip
- **Mini-wow:** Drone soando + knob transforma o som completamente. Cor do visual muda junto
- **Constraint:** Achar a zona de ressonância do escudo
- **Output:** Filtro alinhado — o jogador entendeu que controle contínuo = timbre
- **Metodologia:** Jaëll (toque consciente → som muda), Berklee (filtro = primeiro processador)

### Fase 2 — CALIBRAÇÃO DE PROPULSÃO (Velocity)
- **Controle:** Teclas com força diferente
- **Mini-wow:** Suave = nave flutua. Forte = nave dispara com fogo. Intensidade visível
- **Constraint:** 3 rounds (SUAVE / MEDIO / FORTE)
- **Output:** Jogador controla expressão via toque
- **Metodologia:** Curwen L2 (acento/dinâmica), Jaëll (pentacordes pp→ff), Taubman (peso do braço)

### Fase 3 — PORTAL HARMÔNICO (Pitch/Notas)
- **Controle:** Teclas específicas (C4, E4, G4, C5)
- **Mini-wow:** Portal abre com explosão visual quando nota certa é tocada. Hot/cold
- **Constraint:** 4 portais em sequência
- **Output:** Jogador encontra notas no teclado por ouvido
- **Metodologia:** Curwen Step 1 (intervalos), Berklee (pitch = posição)

### Fase 4 — PRIMEIRO CONTATO (Sequência/Call-Response)
- **Controle:** Teclas (repetir padrão)
- **Mini-wow:** Planeta "fala" melodia, jogador responde. Diálogo musical
- **Constraint:** 3 rounds (2→3→4 notas)
- **Output:** Jogador reproduz sequências — memória musical
- **Metodologia:** Curwen (duetos), Jaëll (expressão artística)

### Fase 5 — TRANSMISSÃO (Modo Criativo / Loop Builder)
**O CORAÇÃO DO JOGO. Tudo antes é preparação pra este momento.**

- **Formato:** Loop de 4-8 compassos, 4 tracks (estilo OP-1 tape)
- **Controle:** TUDO — teclas, velocity, knobs, faders, pads
- **Constraint:** 4 tracks, cada uma com 1 timbre pré-configurado (bass, lead, pad, perc)
- **Fluxo:**
  1. Escolhe track (pad ou botão do MiniLab)
  2. Ouve o metrônomo/groove base
  3. Toca ao vivo — o jogo grava como um looper
  4. Passa pra próxima track
  5. No final: as 4 camadas tocam juntas = SUA música
- **Visual:** A nave viaja pelo espaço enquanto a música toca. O visual reage às 4 camadas
- **Mini-wow:** O momento em que a 2ª camada entra sobre a 1ª. "Eu fiz isso."
- **Output:** Uma composição de 4 camadas que o jogador criou do zero

#### Mecânica do Loop Builder

```
Track 1 (BASS):   [████████████████] ← grava tocando teclas (graves)
Track 2 (LEAD):   [████████████████] ← grava tocando teclas (agudos) + velocity
Track 3 (PAD):    [████████████████] ← grava acorde sustentado + knob (filtro)
Track 4 (PERC):   [████████████████] ← grava nos pads de percussão
```

- Cada track toca em loop enquanto o jogador grava a próxima
- Knobs controlam filtro/efeito em tempo real durante gravação
- Velocity da performance é preservada
- Pode regravar uma track (mas não editar nota a nota — constraint OP-1)
- Metrônomo visual (não sonoro) pra manter o tempo

#### Referências de implementação (Web Audio API)

- **Gravação:** Capturar eventos MIDI com timestamps relativos ao início do loop
- **Playback:** Replay dos eventos MIDI no tempo, usando o mesmo AudioEngine
- **Quantização:** Opcional, leve (snap to 16th) pra soar musical sem ser rígido
- **Overdub:** Cada track é um array de {note, velocity, time, duration}
- **Metrônomo:** ScheduleAheadTime pattern com AudioContext.currentTime

## Progressão de Mini-wows

```
Fase 0: "Uau, cada tecla faz algo diferente!"        → DESCOBERTA
Fase 1: "Uau, este knob TRANSFORMA o som!"           → CONTROLE CONTÍNUO
Fase 2: "Uau, a FORÇA do meu toque importa!"         → EXPRESSÃO
Fase 3: "Uau, achei a nota que abre o portal!"        → PRECISÃO
Fase 4: "Uau, estou tendo uma CONVERSA musical!"      → COMUNICAÇÃO
Fase 5: "Uau, EU FIZ ESSA MÚSICA!"                   → AUTORIA ← o ponto de tudo
```

## Mudanças técnicas necessárias (vs código atual)

### Ordem das fases
- Atual: 0-Wake, 1-Velocity, 2-Filter, 3-Pitch, 4-Sequence
- Nova:  0-Wake, 1-Filter, 2-Velocity, 3-Pitch, 4-Sequence, **5-Create**
- Impacto: trocar fases 1↔2 no código + adicionar fase 5

### Fase 5 (nova) — Loop Builder
- Novo estado de jogo: 'CREATE' (pós-COMPLETE)
- LoopRecorder class: captura MIDI events com timing relativo
- LoopPlayer class: reproduz eventos MIDI em loop
- TrackManager: 4 tracks independentes com mute/solo
- Quantizer: snap opcional a 1/16
- Visual: nave viajando + waveform/activity das 4 tracks
- UI: indicador de track ativa, botão rec (pad do MiniLab?), metrônomo visual

### Áudio
- 4 timbres pré-configurados no AudioEngine (bass, lead, pad, perc)
- Cada track usa seu próprio set de oscillators/filters
- Pads do MiniLab mapeados a percussão (drum hits via samples ou síntese)

### Remover index.html
- Manter apenas game.html (renomear para index.html)
- Deploy como single-page app

## Hardware: Arturia MiniLab 3

### Controles usados por fase
| Fase | Teclas | Velocity | Knobs | Faders | Mod Strip | Pads |
|------|--------|----------|-------|--------|-----------|------|
| 0    | ✓      |          |       |        |           |      |
| 1    |        |          | ✓     | ✓      | ✓         |      |
| 2    | ✓      | ✓        |       |        |           |      |
| 3    | ✓      |          |       |        |           |      |
| 4    | ✓      |          |       |        |           |      |
| 5    | ✓      | ✓        | ✓     | ✓      | ✓         | ✓    |

### Compatibilidade (fixes já aplicados)
- Endless encoders: auto-detect relative vs absolute (2's complement)
- Velocity SUAVE: min 1 (mini-keys não produzem sub-20 controladamente)
- Mod strip (CC 1): desbloqueado para uso em todas as fases
- Notas alvo (C4-C5): acessíveis no range padrão C3-C5

## Fontes da Pesquisa

### Pedagogia musical
- Mrs. Curwen's Pianoforte Method — Teacher's Guide (Archive.org)
- Marie Jaëll — Le Toucher: Nouveaux principes (1894)
- Dorothy Taubman / Golandsky Institute — Taubman Approach
- curwenmusic.com, mariejaell.org, golandskyinstitute.org

### Berklee
- MTEC-222: Intro to Synthesizer Programming and Sound Design
- EP-383: Advanced Modular Synthesis for Composition and Performance
- CMSP-568: Synthesis Techniques
- OMPRD-202: Sound Design for the Electronic Musician (Berklee Online)

### Psicologia e design
- r/synthesizers — motivações, GAS, flow states
- r/teenageengineering — cult following, design philosophy
- Teenage Engineering: Constraints as Aesthetic (blakecrosley.com)
- Teenage Engineering Marketing Strategy (marketergems.com)
