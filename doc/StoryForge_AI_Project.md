# ê°Å¸Å½Â¬ StoryForge AI

Sistema de CriaêÂ§êÂ£o Automatizada de SêÂ©ries com IA

------------------------------------------------------------------------

# ê°Å¸âÅ VisêÂ£o Geral

StoryForge AI êÂ© uma plataforma que permite:

1.  Criar um projeto de sêÂ©rie
2.  Gerar mundo, personagens e episêÂ³dios automaticamente
3.  Editar e aprovar cada etapa
4.  Gerar roteiro
5.  Gerar imagens
6.  Gerar êÂ¡udio
7.  Gerar mêÂºsica
8.  Gerar vêÂ­deo
9.  Fazer montagem final automêÂ¡tica

Fluxo estruturado em 10 etapas.

------------------------------------------------------------------------

# ê°Å¸Ââê¯Â¸Â Estrutura do Projeto

Cada projeto contêÂ©m:

Projeto_X/ ê¢âÅê¢ââ¬ê¢ââ¬ 1_Descricao_Geral ê¢âÅê¢ââ¬ê¢ââ¬ 2_Mundo ê¢âÅê¢ââ¬ê¢ââ¬ 3_Personagens ê¢âÅê¢ââ¬ê¢ââ¬
4_Arco_Temporada ê¢âÅê¢ââ¬ê¢ââ¬ 5_Lista_Episodios ê¢âÅê¢ââ¬ê¢ââ¬ 6_Episodio_Selecionado ê¢âÅê¢ââ¬ê¢ââ¬
7_Roteiro_Aprovado ê¢âÅê¢ââ¬ê¢ââ¬ 8_Audio_Imagens ê¢âÅê¢ââ¬ê¢ââ¬ 9_Video_Cenas ê¢ââê¢ââ¬ê¢ââ¬
10_Montagem_Final

------------------------------------------------------------------------

# ê°Å¸âÂ Fluxo de ExecuêÂ§êÂ£o (1 ê¢â â 10)

## 1ê¯Â¸Âê¢ÆÂ£ DescriêÂ§êÂ£o Geral

UsuêÂ¡rio escreve ideia base.

Sistema gera: - Tema - GêÂªnero - PêÂºblico-alvo - Tom narrativo -
ReferêÂªncias

Status possêÂ­veis: - draft - generated - edited - approved

------------------------------------------------------------------------

## 2ê¯Â¸Âê¢ÆÂ£ Mundo

Sistema gera: - Ano - AmbientaêÂ§êÂ£o - Tecnologia - Regras do universo -
Conflitos globais

------------------------------------------------------------------------

## 3ê¯Â¸Âê¢ÆÂ£ Personagens

Sistema gera: - Protagonista - Antagonista - SecundêÂ¡rios - Conflitos
internos - MotivaêÂ§êÂ£o

UsuêÂ¡rio pode editar e aprovar.

------------------------------------------------------------------------

## 4ê¯Â¸Âê¢ÆÂ£ Arco da Temporada

Sistema gera: - Conflito central - MistêÂ©rio principal - ClêÂ­max - Final
da temporada

------------------------------------------------------------------------

## 5ê¯Â¸Âê¢ÆÂ£ Lista de EpisêÂ³dios

Sistema gera 5--10 episêÂ³dios:

-   Ep 1 -- TêÂ­tulo
-   Ep 2 -- TêÂ­tulo
-   Ep 3 -- TêÂ­tulo
-   ...

UsuêÂ¡rio pode: - Criar novo - Reordenar - Apagar - Selecionar episêÂ³dio

------------------------------------------------------------------------

## 6ê¯Â¸Âê¢ÆÂ£ EpisêÂ³dio Selecionado

Sistema gera: - Sinopse detalhada - Estrutura em 3 atos - Cliffhanger

ApêÂ³s aprovaêÂ§êÂ£o ê¢â â libera roteiro completo.

------------------------------------------------------------------------

## 7ê¯Â¸Âê¢ÆÂ£ Roteiro Completo

Formato profissional:

INT. LABORATêâRIO -- NOITE

CAEL: (voz baixa) --- Isso nêÂ£o êÂ© um eco.

Inclui: - DivisêÂ£o por cenas - DescriêÂ§êÂ£o visual - EmoêÂ§êÂ£o - Movimentos de
cêÂ¢mera - DuraêÂ§êÂ£o estimada

------------------------------------------------------------------------

## 8ê¯Â¸Âê¢ÆÂ£ GeraêÂ§êÂ£o de MêÂ­dia

Para cada cena:

### ê°Å¸Å½Â¨ Imagem

-   Prompt estruturado
-   Estilo consistente
-   Seed fixa para personagens

### ê°Å¸Å½â¢ê¯Â¸Â êÂudio

-   Voz por personagem
-   EmoêÂ§êÂ£o ajustada
-   Sons ambiente

### ê°Å¸Å½Âµ MêÂºsica

-   Trilha cARCKHêtogrêÂ¡fica
-   Loop base ou faixa completa

Arquivos organizados:

/assets/images/ /assets/audio/ /assets/music/ /assets/video/

------------------------------------------------------------------------

## 9ê¯Â¸Âê¢ÆÂ£ GeraêÂ§êÂ£o de VêÂ­deo

Processo: - Animar imagens - Sincronizar êÂ¡udio - Inserir mêÂºsica -
Adicionar transiêÂ§êÂµes

Ferramentas possêÂ­veis: - FFmpeg - MoviePy - Runway API - Pika API

SaêÂ­das: - MP4 horizontal - MP4 vertical - Trailer automêÂ¡tico

------------------------------------------------------------------------

## ê°Å¸âÅ¸ Montagem Final

Sistema: - Junta cenas - Equaliza êÂ¡udio - Aplica LUT cARCKHêtogrêÂ¡fico -
Adiciona crêÂ©ditos - Exporta versêÂ£o final

Status final: - rendered - final

------------------------------------------------------------------------

# ê°Å¸Â§Â  Arquitetura TêÂ©cnica

## Backend

-   Python + FastAPI ou
-   Node.js + Express

## Banco de Dados

Inicial: - SQLite local

ProduêÂ§êÂ£o: - Supabase (PostgreSQL)

------------------------------------------------------------------------

# ê°Å¸ââê¯Â¸Â Estrutura de Banco (Supabase)

## Tabela: projects

-   id (uuid)
-   user_id (uuid)
-   title (text)
-   description (text)
-   status (text)
-   created_at (timestamp)

------------------------------------------------------------------------

## Tabela: episodes

-   id (uuid)
-   project_id (uuid)
-   number (int)
-   title (text)
-   synopsis (text)
-   approved (boolean)

------------------------------------------------------------------------

## Tabela: scenes

-   id (uuid)
-   episode_id (uuid)
-   scene_number (int)
-   script (text)
-   image_prompt (text)
-   audio_status (text)
-   video_status (text)

------------------------------------------------------------------------

## Tabela: assets

-   id (uuid)
-   scene_id (uuid)
-   type (image/audio/video/music)
-   url (text)
-   status (text)

------------------------------------------------------------------------

# ê°Å¸Â¤â APIs Integradas

## LLM (roteiro)

-   OpenAI
-   Anthropic
-   OpenRouter
-   Ollama (local)

## Imagem

-   Stable Diffusion API
-   Leonardo
-   OpenAI Images

## êÂudio

-   ElevenLabs
-   OpenAI TTS
-   PlayHT

## MêÂºsica

-   Suno
-   Udio
-   Stable Audio

## VêÂ­deo

-   Runway
-   Pika
-   FFmpeg (local)

------------------------------------------------------------------------

# ê°Å¸ÅÂ Interface

## Web (principal)

-   Next.js
-   React
-   Tailwind

Menu lateral:

\[1\] DescriêÂ§êÂ£o \[2\] Mundo \[3\] Personagens \[4\] Arco \[5\] EpisêÂ³dios
\[6\] EpisêÂ³dio Atual \[7\] Roteiro \[8\] MêÂ­dia \[9\] VêÂ­deo \[10\] Final

------------------------------------------------------------------------

## Telegram (extensêÂ£o)

Permite: - Criar projeto - Aprovar etapas - Gerar novo episêÂ³dio -
Receber vêÂ­deo final

------------------------------------------------------------------------

# ê°Å¸ââ Sistema de Status

draft generated edited approved rendered final

Cada etapa sêÂ³ desbloqueia a prêÂ³xima quando estiver `approved`.

------------------------------------------------------------------------

# ê°Å¸Å¡â¬ Roadmap de Desenvolvimento

## Fase 1

-   Backend
-   Banco
-   GeraêÂ§êÂ£o de roteiro

## Fase 2

-   Imagem
-   êÂudio

## Fase 3

-   VêÂ­deo automêÂ¡tico

## Fase 4

-   Plataforma SaaS completa

------------------------------------------------------------------------

# ê°Å¸Å½Â¯ Objetivo Final

Criar um:

ê°Å¸Å½Â¬ EstêÂºdio Automatizado de SêÂ©ries com IA

Possibilidades: - SaaS - Ferramenta para criadores - Plataforma de anime
IA - Gerador de novelas IA - Sistema de produêÂ§êÂ£o automatizada

------------------------------------------------------------------------

# FIM DO DOCUMENTO
