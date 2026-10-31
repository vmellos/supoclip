# SupoClip

Sistema autônomo de inteligência artificial para detecção de momentos virais, transcrição precisa e reframing vertical (9:16) de vídeos longos em clipes curtos com legendas animadas.

## Language

### Video Processing

**Fonte (Source Video)**:
O vídeo original de longa duração fornecido por upload direto do usuário ou via link de download (YouTube, Twitch ou Kick).
_Avoid_: Gravação bruta, VOD solto

**Provedor de Transcrição (Transcription Provider)**:
O componente responsável por converter o áudio da Fonte em texto estruturado contendo marcas temporais (timestamps) palavra por palavra, viabilizando o corte preciso e a sincronização das legendas animadas. Pode operar em nuvem (AssemblyAI) ou localmente no dispositivo (Whisper).
_Avoid_: STT solto, Extrator de áudio, Reconhecedor de voz

**Agente de Análise (Analysis Agent)**:
O agente de IA (baseado em LLMs como Google Gemini) encarregado de examinar a transcrição completa, identificar ganchos de engajamento (hooks) e selecionar as melhores janelas temporais com pontuação de viralidade.
_Avoid_: Prompt de corte, Bot de resumo, IA de vídeo

**Corte (Clip)**:
O trecho final renderizado em formato vertical (proporção 9:16), extraído da Fonte com base nas recomendações do Agente de Análise, enriquecido com legendas dinâmicas e reenquadramento de câmera (reframing).
_Avoid_: Snippet, Pedaço, Fatiamento, Short solto

**Modo de Processamento (Processing Mode)**:
A estratégia de balanceamento entre velocidade e profundidade na geração de cortes (como o modo `fast`, que prioriza rapidez e limita a quantidade inicial de clipes analisados).
_Avoid_: Nível de renderização, Velocidade do worker

**Worker**:
O processo assíncrono em segundo plano (gerenciado via Arq e Redis) que executa o download da Fonte, chamada ao Provedor de Transcrição, corte com FFmpeg e composição das legendas.
_Avoid_: Thread de fundo, Script de render

### Repository & Upstream Sync

**Repositório Upstream (Upstream Repository)**:
O repositório original do projeto (`FujiwaraChoki/supoclip`), de onde se originam as novas features, correções de bugs (hotfixes) e dependências atualizadas.
_Avoid_: Repositório pai, Repo central, Repo fonte

**Repositório Fork (Fork Repository / Origin)**:
O repositório pessoal derivado (`vmellos/supoclip`), destino exclusivo dos commits locais e onde as customizações do usuário são hospedadas.
_Avoid_: Clone remoto, Repo secundário

**Ramo Base (Main Branch)**:
A branch `main` local que rastreia `origin/main` e atua como canal direto de sincronização com `upstream/main`.
_Avoid_: Master, Branch default

**Ramo de Customizações (Custom Branch)**:
A branch de trabalho dedicada a ajustes locais e melhorias personalizadas, mantida isolada para evitar conflitos diretos de merge com o upstream.
_Avoid_: Branch dev solta, Branch de teste

**Sincronização com Upstream (Upstream Sync)**:
O procedimento de buscar alterações do Repositório Upstream (`git fetch upstream`) e integrá-las no Repositório Fork (`git merge` ou `rebase`), preservando customizações sem sobrescrevê-las.
_Avoid_: Pull geral, Atualização forçada
