# Animabook

[![Animabook â€” estante de livros](doc/preview.jpg)](https://animabook.vercel.app)

**[animabook.vercel.app](https://animabook.vercel.app)**

Plataforma web mobile-first para criar, publicar e interagir com histÃ³rias em quadrinhos animadas. Qualquer pessoa lÃª os livros; usuÃ¡rios logados escrevem seu prÃ³prio rascunho e publicam versÃµes autorais que outros podem curtir e comentar.

---

## Funcionalidades

- **Reader animado** â€” balÃµes de fala com spring animation, narrador com efeito typewriter, transiÃ§Ãµes entre cenas, swipe para navegar
- **Sidebar de publicaÃ§Ãµes** â€” painel lateral na pÃ¡gina do livro lista todas as versÃµes publicadas; ao selecionar, a histÃ³ria e o painel social carregam inline
- **Editor por usuÃ¡rio** â€” cada usuÃ¡rio tem seu prÃ³prio rascunho por livro; salvo no Supabase com Ctrl+S
- **BalÃµes arrastÃ¡veis** â€” posicione cada balÃ£o livremente sobre a imagem
- **PublicaÃ§Ãµes** â€” botÃ£o "Publicar ðŸš€" cria versÃ£o pÃºblica; mÃºltiplas publicaÃ§Ãµes por livro, uma por autor
- **Likes e comentÃ¡rios** â€” interaÃ§Ãµes sociais nas publicaÃ§Ãµes (requer login)
- **Auth completo** â€” login/registro com email + senha via Supabase Auth
- **Contador de acessos** â€” home exibe total de pageviews, visitantes logados e anÃ´nimos (via tabela `visits`)
- **Aspect ratio dinÃ¢mico** â€” suporte a imagens landscape, portrait e square sem distorÃ§Ã£o
- **Mobile-first** â€” layout responsivo, swipe para navegar entre cenas

---

## Stack

| Tecnologia | Uso |
|---|---|
| Next.js 16 (App Router) | Framework, SSR, API routes, Proxy (middleware) |
| React 19 + TypeScript | UI |
| Framer Motion 12 | AnimaÃ§Ãµes, drag & drop |
| Tailwind CSS 4 | EstilizaÃ§Ã£o (sem config file) |
| Supabase | Auth, banco de dados (drafts, publications, likes, comments) |
| `@supabase/ssr` | SessÃ£o em cookies para SSR |
| Google Fonts | Fredoka, Bangers, Nunito |

---

## Estrutura

```
animabook/
â”œâ”€â”€ books/                         # Imagens PNG + story.md por livro
â”‚   â”œâ”€â”€ livro1/
â”‚   â”‚   â”œâ”€â”€ Cena_01_...png
â”‚   â”‚   â””â”€â”€ story.md
â”‚   â””â”€â”€ livro2/...
â”‚
â”œâ”€â”€ scripts/
â”‚   â””â”€â”€ copyBooks.mjs              # Copia books/ â†’ public/books/ antes do build
â”‚
â””â”€â”€ src/
    â”œâ”€â”€ proxy.ts                   # Protege /editor/* (redireciona para /login)
    â”œâ”€â”€ app/
    â”‚   â”œâ”€â”€ page.tsx               # Home: estante de livros
    â”‚   â”œâ”€â”€ sobre/                 # PÃ¡gina "Como funciona"
    â”‚   â”œâ”€â”€ login/                 # Login / registro
    â”‚   â”œâ”€â”€ book/[bookId]/         # Reader + lista de publicaÃ§Ãµes
    â”‚   â”œâ”€â”€ editor/[bookId]/       # Editor (requer login)
    â”‚   â”œâ”€â”€ publication/[id]/      # Visualiza publicaÃ§Ã£o + likes + comentÃ¡rios
    â”‚   â”œâ”€â”€ api/story/[bookId]/
    â”‚   â”‚   â”œâ”€â”€ route.ts           # GET/POST rascunho
    â”‚   â”‚   â””â”€â”€ publish/route.ts   # POST cria publicaÃ§Ã£o
    â”‚   â””â”€â”€ api/visit/
    â”‚       â””â”€â”€ route.ts           # POST registra visita (session_id + user_id opcional)
    â”œâ”€â”€ components/
    â”‚   â”œâ”€â”€ reader/                # BookReader, SceneView, SpeechBubble, NarratorBox
    â”‚   â”œâ”€â”€ editor/                # StoryEditor, SceneEditorPanel, EditorToolbar
    â”‚   â”œâ”€â”€ auth/                  # AuthForm
    â”‚   â””â”€â”€ ui/                   # BookShelf, ProgressDots, LikeButton, CommentSection, PublicationList
    â”œâ”€â”€ lib/
    â”‚   â”œâ”€â”€ types.ts               # Scene, StoryData, Publication, Comment, UserProfile
    â”‚   â”œâ”€â”€ parseStory.ts
    â”‚   â”œâ”€â”€ getBooks.ts
    â”‚   â”œâ”€â”€ supabase-browser.ts    # Cliente browser (anon key)
    â”‚   â”œâ”€â”€ supabase-server.ts     # Cliente server (cookies)
    â”‚   â””â”€â”€ supabase.ts            # Cliente admin (service role â€” server only)
    â””â”€â”€ hooks/
        â”œâ”€â”€ useTypewriter.ts
        â”œâ”€â”€ useSwipe.ts
        â””â”€â”€ useSceneState.ts
```

---

## Banco de dados (Supabase)

```sql
profiles      -- id, username
drafts        -- user_id, book_id, content   (unique user+book)
publications  -- user_id, book_id, content, published_at
likes         -- user_id, publication_id     (PK composta)
comments      -- user_id, publication_id, text, created_at
visits        -- session_id, user_id (nullable), created_at
```

Todas as tabelas tÃªm RLS habilitada. Leitura de publicaÃ§Ãµes/likes/comentÃ¡rios Ã© pÃºblica; escrita requer autenticaÃ§Ã£o. A tabela `visits` permite insert pÃºblico (anon); leitura via service role apenas.

```sql
-- criar tabela visits
create table visits (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users,
  session_id text not null,
  created_at timestamptz default now()
);
alter table visits enable row level security;
create policy "visits insert public" on visits for insert with check (true);
```

---

## VariÃ¡veis de ambiente

```env
NEXT_PUBLIC_SUPABASE_URL=https://...supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJ...          # chave pÃºblica (anon)
SUPABASE_SERVICE_ROLE_KEY=eyJ...              # chave secreta (server only)
```

---

## Formato do story.md

```markdown
# TÃ­tulo do Livro

<!-- scene: Cena_01_arquivo.png -->

> Texto do narrador aparece acima da imagem com efeito typewriter.

[Personagem@esq x=0.12 y=0.25]: Fala do personagem da esquerda.
[Personagem@dir x=0.65 y=0.40]: Fala do personagem da direita.

<!-- scene: Cena_02_arquivo.png -->

> PrÃ³xima cena...
```

**Regras:**
- `<!-- scene: arquivo.png -->` â†’ nova cena
- `> texto` â†’ narrador
- `[Nome@esq]` ou `[Nome@dir]` â†’ balÃ£o esquerdo ou direito
- `x=0.5 y=0.3` â†’ posiÃ§Ã£o do balÃ£o (fraÃ§Ã£o 0â€“1 do container); opcional

---

## Rodando localmente

```bash
npm install
npm run dev
# abre em http://localhost:3000
```

| URL | O que faz |
|---|---|
| `/` | Estante de livros |
| `/sobre` | Como funciona |
| `/login` | Login / registro |
| `/book/livro1` | LÃª o livro 1 |
| `/editor/livro1` | Editor (requer login) |
| `/publication/[id]` | Visualiza publicaÃ§Ã£o com likes/comentÃ¡rios |

---

## Fluxo de autoria

```
1. npm run dev
2. Acesse /login â†’ crie sua conta
3. Acesse /editor/livro1
4. Escreva narrador e diÃ¡logos por cena
5. Arraste os balÃµes para a posiÃ§Ã£o desejada
6. Ctrl+S para salvar rascunho no Supabase
7. Clique em "ðŸš€ Publicar" â†’ redirecionado para /publication/[id]
8. Outros usuÃ¡rios podem curtir e comentar
```

---

## Deploy

```bash
# Inclua o livro inteiro; use -f porque imagens em books/ ficam ignoradas no git
git add -f books/livro1/
git commit -m "conteÃºdo livro1"
git push   # Vercel faz build + deploy automaticamente (~90s)
```

**Vercel:** configurar as trÃªs variÃ¡veis de ambiente em Settings â†’ Environment Variables.

**Importante:** nunca commite `public/books/`; ele Ã© gerado no build a partir de `books/`.

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe