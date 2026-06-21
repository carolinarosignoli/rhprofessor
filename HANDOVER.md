# HANDOVER — Site rhprofessor (carolrosignoli.com.br)

## 1. Objetivo e estado atual

Site de marca pessoal da **Carol Rosignoli** ("RH Professor"). Site estático (HTML/CSS/JS puro), publicado no Vercel a partir da branch `main` do repo `carolinarosignoli/rhprofessor`.

**Posicionamento central:** vender **autoridade e marca pessoal**, não solução de T&D para empresas. O público (profissionais de RH/T&D, escolas, etc.) deve **querer ser como a Carol**. A "dor" é de identidade/aspiração (ter a sala na mão, ser lembrado, ter autoridade), não operacional.

**Estado atual:**
- Home (`index.html`) reformulada para alta conversão. **Commitada e no ar.**
- Página de **escolas** (`escolas.html`) redesenhada como landing page completa de conversão. **NÃO commitada** (está na working tree, aguardando aprovação da Carol). É o **template de referência** para as outras.
- Páginas `empresas.html` e `mentoria.html` ainda estão na estrutura **antiga e simples** (só nav + intro + form). Precisam receber a mesma estrutura da `escolas.html`.
- Não existe página dedicada de **T&D**; o card 01 da home aponta para `workshop.html`.

Fluxo de trabalho combinado: construir → Carol revisa a prévia → ela diz "commit" → depois "push" (o Vercel republica sozinho). **Não commitar sem aprovação dela.**

## 2. Decisões tomadas e por quê

- **Site estático no Vercel, sem build.** O `vite build` só emitia `index.html`, então todas as outras páginas davam 404. Solução em `vercel.json`: `installCommand`/`buildCommand` como `echo skip`, `outputDirectory: "."`, `cleanUrls: true`. Há `.vercelignore`.
- **Multipágina:** `index.html` = home/hub; `workshop.html` = LP do evento; `empresas/mentoria/escolas.html` = páginas de captura de lead. URLs sem `.html` via cleanUrls.
- **Leads no Supabase, tabela única `leads`** (escolha da Carol, em vez de tabela por produto). Distinção por coluna **`produto`** (`Empresas`/`Mentoria`/`Escolas`). Há também **`conta_mais`** (texto livre: "o que a pessoa precisa"). As duas colunas foram adicionadas via SQL pela Carol. Os forms fazem POST na REST do Supabase com a publishable key e têm **fallback**: se as colunas novas não existirem, reenviam sem elas (lead nunca se perde).
- **Voz da Carol (skill `carol-voice-writing`):** sem travessão (usar vírgula ou `·`), sem emoji, sem construção "não é X, é Y" (exceto citação literal), sem "real" como adjetivo vago, frases assertivas e densas. Tom **"booksmart but cool kid"**.
- **Hero da home:** citação da **bell hooks** "A sala de aula é o espaço mais radical de possibilidade." (atribuição "bell hooks" em minúsculas, como a autora prefere) + gancho da Carol "Mas só se você tiver as manhas de engajamento e retenção de verdade...".
- **Funil da home (ordem):** hero → faixa de credibilidade (logos USP/Stanford/FGV + números) → "Você reconhece isso?" (4 dores em primeira pessoa) → "No que eu acredito" (citação Paulo Freire em campo escuro) → ofertas (4 caixas 2×2) → "Por que confiar em mim" (bio como prova + carrossel) → destaque do workshop → newsletter (Substack "Cá entre nós").
- **Conversão: tudo estático, nada de carrossel** para fotos/depoimentos (carrossel sofre de "banner blindness"). Padrão das LPs: 1 foto forte no hero + galeria estática + depoimentos em caixas fixas.
- **escolas.html** segue: hero (foto real) → faixa "Já dei formação em" (10 instituições) → temas (lista completa) → 2 depoimentos reais (Tarsila do Amaral, Colégio PH) → galeria "Carol em ação" (2×2) → mini-bio (prova) → form "Vamos conversar?" com campo "Conta mais".

## 3. Arquivos relevantes (por caminho) e status

- `index.html` — Home/hub, funil de conversão. **Commitada e no ar** (último commit `01663e5`).
- `escolas.html` — LP de escolas redesenhada (template das demais). **Modificada, NÃO commitada.**
- `empresas.html` — Página de form, **estrutura antiga**. No ar. **Falta** aplicar a estrutura da escolas + campo "Conta mais".
- `mentoria.html` — Página de form, **estrutura antiga**. No ar. **Falta** o mesmo.
- `workshop.html` — LP do evento (era o index original). No ar. Tem botão "← Início". Obs: contém logos em base64 (linhas enormes; `Read` falha, usar `Grep` ou leitura por offset).
- `vercel.json` — Config estática sem build (cleanUrls). Commitada.
- `.vercelignore` — Ignora `node_modules`, `.claude`, `.git`. Commitada.
- `package.json` — Vite (não usado; build sobrescrito no vercel.json). Presente.
- `.claude/launch.json` e `.claude/serve.ps1` — Servidor estático em PowerShell para preview (a máquina não tem Node nem Python). Commitados.
- `imagem/` — Assets. Fotos: `carol.png`, `foto_aula01.jpg`, `foto_aula02.jpg`, `foto_work01.jpg`, `foto_work02.jpg`, `escola01.jpeg`..`escola06.jpeg/jpg`. Logos: `usp.png`, `stanford.png`, `fgv.png`. (Galeria de escolas usa escola02, 03, 05, 06.)
- Memória do projeto em `.claude/projects/C--Users-carolina-rosignoli-Desktop-Deploy-rhprofessor/memory/`: `home-conversion-direction.md`, `site-structure.md`, `dev-preview-server.md`.

**Supabase:** projeto `ourncyxuudixmzamheaj`, tabela `leads`, publishable key no `<script>` de cada página. Colunas: `nome, email, empresa, cargo, telefone, aceite_whatsapp, produto, conta_mais`.

## 4. Descartado / não funcionou (e por quê)

- **Vite build no Vercel** → só gerava `index.html`, 404 nas demais e imagem quebrada. Trocado por site estático.
- **Tabelas separadas por produto no Supabase** → Carol preferiu tabela `leads` única com coluna `produto`.
- **Carrossel de fotos / de depoimentos** → descartado por converter menos. Tudo estático.
- **Hero "Você domina o conteúdo / sala na mão"** e depois **"Ensinar é um ato de coragem"** → substituídos pela citação da bell hooks.
- **"Eu te ensino o que licenciatura nenhuma ensina:"** → removido (a Carol achou pedante).
- **Manifesto "Educação não transforma o mundo / Pessoas transformam o mundo"** → tirado do hero; a âncora de crença virou a citação "Ensinar não é transferir conhecimento, mas criar as possibilidades para a sua produção" (Paulo Freire).
- **Caixas de serviço empilhadas** → Carol preferiu 2×2; listas longas de temas foram para as LPs.
- **Galeria de escolas em 3 colunas (5 fotos)** → voltou para 2×2 (4 fotos), removida a `escola04` (auditório duplicado, parecido com `escola03`).
- **Preview com `python3 -m http.server`** → não há Python; usar o `serve.ps1` (PowerShell HttpListener) via `preview_start "Landing Page"`.

## 5. Próximo passo concreto

1. **Aprovar e commitar `escolas.html`** (a Carol revisa a prévia; ao ok, commit + push).
2. **Replicar a estrutura da `escolas.html`** em `empresas.html` e `mentoria.html` (e avaliar criar uma página de T&D), adaptando: copy por produto, `produto` correto no script, campo "Conta mais", e coletando fotos/depoimentos/logos reais de cada um.
3. Quando a Carol mandar os **PNGs dos logos** das instituições, trocar os chips de texto por logos-imagem na faixa "Já dei formação em".

### Notas operacionais para a nova sessão
- **Preview:** `preview_start` com a config "Landing Page" (roda `serve.ps1`). O capturador de screenshot trava com frequência; **reiniciar o servidor** (stop/start) destrava. Antes de rolar+screenshot, desligar o smooth scroll via eval: `document.documentElement.style.scrollBehavior='auto'`.
- **Arquivos com base64** (`workshop.html`): `Read` estoura tokens; usar `Grep` ou `Read` com offset/limit pequenos.
- **Testar Supabase:** o `curl` no Git Bash corrompe acentos (gera erro PGRST102 "invalid json"). Para testar de verdade, usar o form no navegador (envia UTF-8 correto) ou payload sem acentos. Há linhas "TESTE ... (pode apagar)" na tabela `leads` que a Carol pode deletar.
- **Workflow git:** branch `main`; commitar só quando a Carol pedir; push publica no Vercel.
