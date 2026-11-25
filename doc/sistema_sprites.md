# Sistema de Sprites — Personagens Animados sobre Fundos

## Visêo Geral

O objetivo é evoluir o Animabook de **imagens estáticas** (tudo junto num PNG) para **personagens animados** posicionados sobre fundos de cena. Os personagens deslizam, entram e saem de quadro conforme a história avança — sem precisar renderizar um novo PNG para cada posiçêo.

```
HOJE:                          FUTURO:
┌─────────────────────┐        ┌─────────────────────┐
│                     │        │   [fundo.png]        │
│  tudo junto         │   →    │     🧒  🤖           │
│  num único PNG      │        │   lumi  pix-z        │
│                     │        │   (PNGs separados)   │
└─────────────────────┘        └─────────────────────┘
```

---

## Formato story.md (extensêo proposta)

O formato atual continua funcionando. Os sprites sêo declarados com novas tags dentro dos blocos de cena:

```markdown
# Ep01: Primeiro Dia

<!-- scene: sala_aula_fundo.png -->
<!-- sprite: lumi.png       x=20% y=72% scale=1.0 -->
<!-- sprite: caio.png       x=75% y=72% scale=1.0 -->

> A sala de aula estava silenciosa quando Lumi chegou.

[Lumi]: Oi, eu sou nova aqui!
[Caio]: Bem-vinda! Eu sou o Caio.

<!-- sprite: lumi.png x=45% y=72% -->   ← Lumi desliza para o centro

[PIX-Z]: CALCULANDO PROBABILIDADE DE AMIZADE... 99,7%!

<!-- sprite: pix-z.png x=60% y=60% scale=0.8 -->  ← PIX-Z aparece voando
```

### Regras do formato

| Tag | Comportamento |
|-----|--------------|
| `<!-- sprite: arquivo.png x=% y=% -->` | Posiciona o sprite. Se já estava na cena, **anima** para a nova posiçêo |
| `<!-- sprite: arquivo.png x=% y=% scale=N -->` | Posiciona com escala (1.0 = tamanho natural) |
| `<!-- sprite: arquivo.png saída -->` | Remove o sprite da cena (slide para fora) |
| Sem nenhuma tag sprite | Sprites da cena anterior permanecem no lugar |

- `x` e `y` sêo porcentagens em relaçêo ao container da cena (0%–100%)
- A âncora do sprite é a base central (`bottom-center`)
- Múltiplos sprites podem existir ao mesmo tempo
- A ordem das tags define quem fica "na frente" (z-index)

---

## Arquivos de sprite

Cada personagem é um PNG com **fundo transparente** (canal alpha):

```
public/sprites/
  lumi.png        # Lumi de frente, postura neutra
  lumi_feliz.png  # Lumi com expressêo feliz
  lumi_triste.png
  caio.png
  caio_surpreso.png
  pix-z.png
  pix-z_calculando.png
  natui.png       # ratinho Natui
  piva.png        # capivara Piva
  ...
```

Tamanho recomendado: **400×600px** (personagem ocupa ~80% da altura). O tamanho visual final é controlado pelo `scale` no story.md e pelo `y` de posicionamento.

---

## Como o leitor renderiza

### Camadas da cena

```
┌──────────────────────────────┐
│  z-index 3: Balões de fala   │
│  z-index 2: Sprites          │ ← posicionados com position: absolute
│  z-index 1: Fundo (PNG)      │ ← preenche o container
└──────────────────────────────┘
```

### Animaçêo com Framer Motion

Cada sprite usa `layoutId={sprite.filename}` — quando a posiçêo muda entre cenas, o Framer Motion interpola automaticamente com spring physics:

```tsx
// SceneView.tsx (pseudocódigo)
{scene.sprites.map(sprite => (
  <motion.div
    key={sprite.filename}
    layoutId={sprite.filename}        // ← mesma key = animaçêo automática
    initial={{ opacity: 0, y: 20 }}
    animate={{ opacity: 1, y: 0 }}
    exit={{ opacity: 0, x: -40 }}
    style={{
      position: 'absolute',
      left: sprite.x,
      bottom: `${100 - parseFloat(sprite.y)}%`,
      transform: `translateX(-50%) scale(${sprite.scale})`,
      zIndex: sprite.zIndex,
    }}
  >
    <Image src={`/sprites/${sprite.filename}`} alt={sprite.filename} />
  </motion.div>
))}
```

O segredo é o `<AnimatePresence>` envolvendo as cenas — quando a cena muda, os sprites com o mesmo `layoutId` nêo desmontam, apenas movem.

---

## Mudanças necessárias no código

### 1. `parseStory.ts`

Adicionar parsing das tags `<!-- sprite: ... -->`:

```typescript
interface SpriteState {
  filename: string;
  x: string;       // ex: "35%"
  y: string;       // ex: "72%"
  scale: number;   // default: 1.0
  zIndex: number;  // ordem de declaraçêo
}

interface Scene {
  imageUrl: string;
  sprites: SpriteState[];   // ← novo campo
  narrator: string;
  dialogues: Dialogue[];
}
```

### 2. `SceneView.tsx`

- Renderizar camada de sprites entre o fundo e os balões
- Usar `motion.div` com `layoutId` por sprite
- Envolver com `AnimatePresence mode="popLayout"`

### 3. `BookReader.tsx`

- Garantir que `AnimatePresence` envolva as cenas (provavelmente já envolve)
- Passar `sprites` para `SceneView`

### 4. Editor (opcional, fase 2)

- Drag-and-drop para posicionar sprites na cena
- Seletor de sprite (lista de PNGs disponíveis em `/sprites/`)
- Preview em tempo real da posiçêo (x%, y%)
- Gera automaticamente as tags `<!-- sprite: ... -->` no story.md

---

## Exemplo completo — Ep02: A Capivara Sumiu

```markdown
# Ep02: A Capivara Sumiu

<!-- scene: quintal_fundo.png -->
<!-- sprite: lumi.png       x=25% y=78% -->
<!-- sprite: caio.png       x=70% y=78% -->

> Era uma manhê ensolarada no quintal da escola.

[Lumi]: Caio! Cadê a Piva?
[Caio]: Ela estava aqui agora há pouco...

<!-- sprite: lumi.png x=50% y=78% -->
<!-- sprite: caio.png x=78% y=78% -->

[Lumi]: Vamos procurar!

<!-- scene: corredor_fundo.png -->
<!-- sprite: lumi.png x=30% y=78% -->

> Lumi correu pelo corredor.

[Lumi]: PIVA! Onde você está?

<!-- sprite: piva.png x=65% y=82% scale=0.7 -->

[PIX-Z]: DETECÇêO DE CAPIVARA: POSITIVO. POSIÇêO: 3 METROS À DIREITA.

[Lumi]: PIVA! Encontrei você!
```

---

## Fluxo de produçêo (com sprites)

```
1. Criar fundo da cena (PNG sem personagens)
   ↓
2. Criar sprite do personagem (PNG com fundo transparente)
   ↓
3. No story.md: declarar <!-- sprite --> com posiçêo inicial
   ↓
4. A cada mudança de posiçêo: nova tag <!-- sprite --> com x/y novo
   ↓
5. Framer Motion anima o deslizamento automaticamente
```

---

## Vantagens sobre o sistema atual

| | Hoje (PNG único) | Com sprites |
|-|-----------------|-------------|
| Posiçêo do personagem muda | Novo PNG necessário | Tag sprite, sem imagem nova |
| Expressêo muda | Novo PNG | Trocar filename do sprite |
| Personagem entra em cena | PNG diferente | `<!-- sprite: ... -->` aparece |
| Personagem sai | PNG diferente | `<!-- sprite: ... saída -->` |
| Produçêo de imagens | 1 PNG por estado | 1 fundo + N sprites reutilizáveis |
| Animaçêo de movimento | Nenhuma | Spring physics automático |

---

## Limitações e decisões de design

- **Sem animaçêo de frames**: os sprites nêo têm walk cycle, apenas mudam de posiçêo. Expressões diferentes = arquivos diferentes (`lumi.png`, `lumi_feliz.png`).
- **Fundos estáticos**: os fundos continuam sendo PNGs, só os personagens têm movimento.
- **Retrocompatibilidade**: episódios sem tags `<!-- sprite -->` continuam funcionando exatamente como hoje.
- **Mobile-first**: posições em `%` funcionam em qualquer tamanho de tela.

---

## Próximos passos

1. [ ] Criar 1 fundo de cena separado (sem personagens)
2. [ ] Exportar 1 sprite de personagem com fundo transparente (Lumi ou Caio)
3. [ ] Implementar parsing de `<!-- sprite -->` em `parseStory.ts`
4. [ ] Renderizar sprites em `SceneView.tsx` com Framer Motion
5. [ ] Testar em 1 cena de um livro existente
6. [ ] Se aprovado, converter episódios gradualmente

---

## Por que é têo leve e rápido — Análise Técnica

### O segredo: o pipeline de renderizaçêo do browser

O browser tem 3 fases de renderizaçêo:

```
Layout (Reflow)  →  Paint (Repaint)  →  Composite (GPU)
     LENTO               MÉDIO              RAPIDÍSSIMO
```

Mover um personagem via `transform: translateX()` **pula as duas primeiras fases** e vai direto para o compositor da GPU. O browser rasteriza o sprite **uma única vez**, manda para a GPU como textura, e depois só passa matrizes numéricas para mover — operaçêo de nanosegundos.

Comparado a trocar imagem ou animar frames (que aciona Paint toda vez), é ordens de magnitude mais rápido.

### Texture Atlas (Sprite Sheet)

Em vez de um PNG por peça de roupa/expressêo, tudo fica em **uma imagem grande**. A GPU faz bind de uma textura só e recorta as partes por coordenadas UV:

```
atlas.png  (2048x2048)
 ┌──────────┬──────────┬──────────┐
 │ cabelo1  │ cabelo2  │ chapéu   │
 ├──────────┼──────────┼──────────┤
 │ camisa1  │ camisa2  │ jaqueta  │
 └──────────┴──────────┴──────────┘

atlas.json
{
  "cabelo1": { "x": 0,   "y": 0,   "w": 256, "h": 256 },
  "camisa1": { "x": 0,   "y": 256, "w": 256, "h": 256 }
}
```

Resultado: sem múltiplos carregamentos, sem fragmentaçêo de pipeline. A GPU só precisa fazer **bind de uma textura** para desenhar todas as camadas do avatar.

### Draw Call Batching

Cada "draw call" é uma instruçêo CPU → GPU. O gargalo real nêo é a GPU em si, mas essa comunicaçêo:

```
10 sprites do mesmo atlas   = 1 draw call   ✓
10 sprites de PNGs separados = 10 draw calls  ✗
```

Engines como PixiJS fazem isso automaticamente. Para o Animabook com CSS + Framer Motion, o compositor do browser já faz o equivalente.

### Camadas independentes

```
Layer 0: fundo.png
Layer 1: corpo_base.png
Layer 2: roupa.png
Layer 3: cabelo.png
Layer 4: acessório.png
```

Trocar a roupa = trocar a textura de **uma layer** só. As outras nêo sêo tocadas. A GPU recomposita em microsegundos.

### Sistema de movimento, nêo de frames

Personagens do AvatarWorld nêo têm walk cycle — eles **deslizam** de posiçêo A para posiçêo B. Isso elimina frames de animaçêo: o movimento é uma translaçêo pura que a GPU executa de graça.

---

## Como o Framer Motion implementa isso: técnica FLIP

O `layoutId` do Framer Motion usa a técnica **FLIP** (First → Last → Invert → Play):

1. **First** — captura posiçêo atual do elemento
2. **Last** — React re-renderiza com nova posiçêo
3. **Invert** — Framer aplica `transform` inverso para "voltar" visualmente ao ponto inicial
4. **Play** — anima o `transform` até zero com spring physics

**Todo o movimento acontece via `transform` — 100% no compositor da GPU, sem tocar no main thread do JavaScript.** A animaçêo continua suave mesmo com JS pesado rodando em paralelo.

```tsx
// Implementaçêo no Animabook
<motion.img
  key="lumi"
  layoutId="sprite-lumi"         // FLIP automático entre cenas
  src="/sprites/lumi.png"
  style={{
    position: 'absolute',
    left: '20%',                  // nova posiçêo vinda do story.md
    bottom: '35%',
    width: '15%',
  }}
  transition={{ type: 'spring', stiffness: 120, damping: 20 }}
/>
```

Quando a cena muda e o sprite tem nova posiçêo no story.md, o Framer Motion detecta a mudança de `left`/`bottom`, converte para `transform: translate()` internamente e anima suavemente. **Zero reflow, zero repaint, tudo na GPU.**

---

## Comparaçêo de tecnologias web para sprites 2D

| Abordagem | GPU | Casos de uso |
|-----------|-----|-------------|
| CSS `transform` + Framer Motion | Sim, via compositor | Poucos sprites, React — **ideal para o Animabook** |
| Canvas 2D API | Parcial (CPU) | Jogos simples |
| WebGL via PixiJS | Completo | Centenas de sprites simultâneos |
| Three.js | Completo (3D) | Cenas 3D com sprites |

Para a escala do Animabook (5–10 sprites por cena), **CSS + Framer Motion é a escolha certa** — sem overhead de engine, sem WebGL, aproveitando o mesmo pipeline que faz sistemas como AvatarWorld serem rápidos.
