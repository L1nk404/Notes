# Neovim (LazyVim) — Cheat Sheet

> Guia pessoal, não um manual completo.

---

## Geral

### Navegação Geral

| Tecla | Ação |
|---|---|
| `h` `j` `k` `l` | Esquerda / baixo / cima / direita |
| `w` | Próxima palavra |
| `b` | Palavra anterior |
| `e` | Fim da palavra atual |
| `0` | Início **absoluto** da linha (coluna 0) |
| `^` | Início do **texto** da linha (ignora espaços) |
| `$` | Fim da linha |
| `gg` | Início do documento |
| `G` | Fim do documento |
| `{` / `}` | Parágrafo anterior / próximo |
| `%` | Pula pro parêntese/chave/colchete correspondente |
| `Ctrl+o` / `Ctrl+i` | Volta / avança na jump list (posições visitadas) |

**Busca de caractere na linha:**

| Tecla | Direção | Exemplo |
|---|---|---|
| `f{char}` | Busca pra frente na linha | `fa` → vai pro próximo `a` à direita |
| `F{char}` | Busca pra trás na linha | `Fa` → vai pro `a` anterior, à esquerda |
| `t{char}` | Vai até antes do próximo caractere | — |
| `T{char}` | Vai até depois do caractere anterior | — |
| `;` | Repete a última busca `f`/`F`/`t`/`T`, mesma direção | — |
| `,` | Repete a última busca, direção oposta | — |

Combo: `fa` seguido de `;` várias vezes pula de "a" em "a" na linha sem redigitar `fa`.

**Edição básica:**

| Tecla | Ação |
|---|---|
| `V` | Modo visual de linha (seleciona a linha inteira) |
| `y` | Yank (copiar) |
| `d` | Delete (recortar) |
| `w` (com operador) | Ex: `cw` muda a palavra atual |
| `ggVG` | Seleciona o documento inteiro |
| `u` | Undo |
| `Ctrl+r` | Redo |
| `/palavra` + `Enter` | Busca pra frente |
| `?palavra` + `Enter` | Busca pra trás |
| `n` / `N` | Repete a busca, mesma direção / direção oposta |

**Indentação:**

| Tecla | Ação |
|---|---|
| `=ap` | Reindenta o parágrafo atual (baseado no filetype) |
| `==` | Reindenta só a linha atual |
| `gg=G` | Reindenta o arquivo inteiro |
| `>>` / `<<` | Indenta / desindenta a linha atual em um nível |
| `>ip` / `<ip` | Indenta / desindenta o parágrafo atual |
| Visual + `>` / `<` | Indenta / desindenta a seleção |
| `gv` (após indentar em visual) | Reseleciona a mesma área, útil pra repetir `>`/`<` |
| `Ctrl+t` / `Ctrl+d` (modo inserção) | Indenta / desindenta a linha atual enquanto digita |

### Comentários

| Tecla | Ação |
|---|---|
| `gcc` | Comenta/descomenta a linha atual |
| `gc` + motion | Ex: `gcap` comenta o parágrafo inteiro |
| Modo visual + `gc` | Comenta as linhas selecionadas (equivalente ao `Ctrl+/` do VS Code) |

### Mover linhas (estilo VS Code Alt+Seta)

| Tecla | Ação |
|---|---|
| `Alt+j` | Move a linha atual (ou seleção) pra baixo |
| `Alt+k` | Move a linha atual (ou seleção) pra cima |

Funciona em modo normal, inserção e visual. Usamos `j`/`k` em vez das setas porque
`Alt+Seta` nem sempre é reconhecido corretamente por todos os terminais.

### Copiar para o clipboard do sistema via SSH (OSCYank)

| Tecla | Modo | Ação |
|---|---|---|
| `<leader>c` + motion | Normal | Copia o trecho (é um operador, ex: `<leader>ciw` copia a palavra) |
| `<leader>cc` | Normal | Copia a linha inteira pro clipboard do sistema |
| `<leader>c` | Visual | Copia a seleção pro clipboard do sistema |

Útil em sessões SSH remotas — usa o protocolo OSC 52, não depende de X11 forwarding.

### Descobrir atalhos (meta)

| Comando/Tecla | Ação |
|---|---|
| `<leader>` + esperar | Abre o which-key, mostra atalhos disponíveis a partir dali |
| `<leader>sk` | Busca fuzzy por todos os keymaps ativos, com descrição (Telescope) |
| `:map` | Lista todos os keymaps em texto puro (nativo, sem plugin) |
| `:nmap` / `:vmap` / `:imap` | Mesma coisa, filtrado por modo (normal/visual/inserção) |

---

## Tipo: Navegação por Modo

### Modo Normal

| Tecla | Ação |
|---|---|
| `i` | Entra em modo de inserção antes do cursor |
| `a` | Entra em modo de inserção depois do cursor |
| `o` / `O` | Abre nova linha abaixo / acima e entra em inserção |
| `x` | Deleta o caractere sob o cursor |
| `.` | Repete a última ação de edição |

### Modo Visual

| Tecla | Ação |
|---|---|
| `v` | Modo visual por caractere |
| `V` | Modo visual por linha |
| `Ctrl+v` | Modo visual em bloco (colunas) |
| `v$` (a partir de outro modo visual) | Estende a seleção até o fim da linha |
| `v0` / `v^` | Estende até o início da linha / início do texto |
| `gv` | Reseleciona a última seleção visual |

---

## Trabalhar com Múltiplos Arquivos (Splits e Janelas)

**Criar splits (nativo):**

| Tecla/Comando | Ação |
|---|---|
| `:split` ou `Ctrl+w s` | Divide a tela horizontalmente |
| `:vsplit` ou `Ctrl+w v` | Divide a tela verticalmente |
| `:close` ou `Ctrl+w q` | Fecha o split atual |
| `:only` ou `Ctrl+w o` | Fecha todos os splits, deixando só o atual |

**Navegar entre splits (padrão do LazyVim):**

| Tecla | Ação |
|---|---|
| `Ctrl+h` | Vai pro split à esquerda |
| `Ctrl+j` | Vai pro split abaixo |
| `Ctrl+k` | Vai pro split acima |
| `Ctrl+l` | Vai pro split à direita |
| `Ctrl+w` `w` | Cicla entre os splits abertos |

**Redimensionar splits (padrão do LazyVim):**

| Tecla | Ação |
|---|---|
| `Ctrl+Up` / `Ctrl+Down` | Aumenta / diminui a altura do split atual |
| `Ctrl+Left` / `Ctrl+Right` | Diminui / aumenta a largura do split atual |
| `Ctrl+w` `=` | Iguala o tamanho de todos os splits |
| `Ctrl+w` `_` | Maximiza a altura do split atual |
| `Ctrl+w` `\|` | Maximiza a largura do split atual |
| `Ctrl+w` `r` | Rotaciona a posição dos splits |
| `Ctrl+w` `Space` | Abre o "Window Hydra Mode" — modo interativo (which-key) pra várias operações de janela em sequência, sem precisar repetir `Ctrl+w` toda vez |

**Tabs (diferente de buffers — cada tab pode conter vários splits):**

| Comando | Ação |
|---|---|
| `:tabnew` | Abre uma nova tab |
| `gt` | Próxima tab |
| `gT` | Tab anterior |
| `:tabclose` | Fecha a tab atual |

Tabs no Vim são um agrupamento de janelas/splits, diferente de buffers (arquivos abertos)
e diferente do conceito de "aba" visual do bufferline. No dia a dia, a maioria dos fluxos
de trabalho usa mais buffers + splits do que tabs de verdade.

---

## Plugins Específicos

### Bufferline (abas de buffers)

| Tecla | Ação |
|---|---|
| `<S-h>` | Buffer anterior |
| `<S-l>` | Próximo buffer |
| `[b` / `]b` | Alternativas pra anterior/próximo |
| `<leader>bb` | Alterna pro último buffer usado |
| `<leader>bd` | Fecha o buffer atual |
| `<leader>bo` | Fecha todos os outros buffers |

### Telescope (nativo do LazyVim)

**Buscas rápidas:**

| Tecla | Ação |
|---|---|
| `<leader><space>` | Find Files (a partir da raiz do projeto) |
| `<leader>/` | Grep (busca por texto, a partir da raiz do projeto) |
| `<leader>,` | Troca de buffer (ordenado por uso mais recente) |
| `<leader>:` | Histórico de comandos |

**Prefixo `<leader>f` (find/arquivos):**

| Tecla | Ação |
|---|---|
| `<leader>ff` | Find Files (raiz do projeto) |
| `<leader>fF` | Find Files (diretório atual, não a raiz) |
| `<leader>fb` | Lista de buffers abertos |
| `<leader>fc` | Abre um arquivo de config do Neovim |
| `<leader>fg` | Find Files usando `git ls-files` (repositórios git) |
| `<leader>fr` | Arquivos recentes |
| `<leader>fR` | Arquivos recentes (diretório atual) |

Nota: `<leader>fg` aqui é "find files via git", não "live grep" — quem faz grep é
`<leader>/` ou `<leader>sg`.

**Prefixo `<leader>s` (search):**

| Tecla | Ação |
|---|---|
| `<leader>sg` | Grep (raiz do projeto) |
| `<leader>sG` | Grep (diretório atual) |
| `<leader>sw` | Busca a palavra sob o cursor (raiz do projeto) |
| `<leader>sW` | Busca a palavra sob o cursor (diretório atual) |
| `<leader>sb` | Busca dentro do buffer atual |
| `<leader>sh` | Help Pages |
| `<leader>sk` | Key Maps |
| `<leader>sm` | Jump to Mark |
| `<leader>so` | Opções do Vim/Neovim |
| `<leader>ss` | Símbolos do documento (LSP) |
| `<leader>sd` | Diagnósticos do documento atual |
| `<leader>sD` | Diagnósticos do workspace inteiro |
| `<leader>sR` | Resume a última busca do Telescope |
| `<leader>s"` | Registradores |

**Git:**

| Tecla | Ação |
|---|---|
| `<leader>gc` | Commits |
| `<leader>gs` | Status |

### Harpoon 2 + integração Telescope

| Tecla | Ação |
|---|---|
| `<leader>a` | Adiciona o arquivo atual à lista do Harpoon |
| `<leader>e` | Abre o menu rápido do Harpoon |
| `<C-p>` / `<C-n>` | Navega pro arquivo anterior/próximo da lista |
| `<leader>1` a `<leader>4` | Vai direto pro arquivo marcado 1–4 |
| `<leader>fl` | Abre a lista do Harpoon via Telescope (com preview) |

**Dentro da tela do Telescope do Harpoon:**

| Tecla | Ação |
|---|---|
| `<C-d>` | Deleta a mark selecionada |
| `<C-k>` / `<C-j>` | Move a mark selecionada pra cima/baixo na lista |

### Múltiplos Cursores (multiple-cursors.nvim)

| Tecla | Modo | Ação |
|---|---|---|
| `<C-j>` / `<C-k>` | Normal, Visual | Adiciona cursor e move pra baixo/cima |
| `<C-Down>` / `<C-Up>` | Normal, Inserção, Visual | Mesma coisa, com as setas |
| `<C-LeftMouse>` | Normal, Inserção | Adiciona/remove cursor no clique |
| `<C-Return>` | Normal | Adiciona cursor travado ou remove um existente |
| `<leader>m` | Visual | Adiciona cursores em cada linha da área selecionada |
| `<leader>a` | Normal, Visual | Adiciona cursores nas ocorrências da palavra sob o cursor |
| `<leader>A` | Normal, Visual | Mesma coisa, na área anterior |
| `<leader>d` | Normal, Visual | Adiciona cursor e pula pra próxima ocorrência |
| `<leader>D` | Normal, Visual | Só pula pra próxima ocorrência, sem adicionar cursor |
| `<leader>l` | Normal, Visual | Trava os cursores virtuais |

### Lazygit

| Tecla | Ação |
|---|---|
| `<leader>gg` | Abre o Lazygit numa janela flutuante |

Dentro do Lazygit (atalhos do próprio Lazygit, não do Neovim):

| Tecla | Ação |
|---|---|
| `Space` | Stage/unstage do arquivo selecionado |
| `c` | Commit |
| `P` | Push |
| `p` | Pull |
| `q` | Fecha o Lazygit e volta pro Neovim |

---

## LSP

### Navegação

| Tecla | Ação |
|---|---|
| `K` | Hover — mostra documentação do símbolo sob o cursor |
| `gd` | Vai pra definição do símbolo |
| `gr` | Lista todas as referências (onde é usado) |
| `gI` | Vai pra implementação |
| `<leader>ca` | Code actions (sugestões de correção/refatoração) |
| `<leader>cr` | Renomeia o símbolo em todos os lugares onde é usado |

### Diagnostics

| Tecla | Ação |
|---|---|
| `]d` / `[d` | Próximo / anterior diagnóstico (erro, warning) |
| `<leader>cd` | Mostra o diagnóstico da linha atual numa janela flutuante |

### Autocomplete (blink.cmp)

| Tecla | Ação |
|---|---|
| `Tab` / `Shift+Tab` | Navega entre sugestões |
| `Enter` | Confirma a sugestão selecionada |
| `Ctrl+Space` | Abre/fecha manualmente o menu de sugestões e a documentação |

---

## Dicas Rápidas / Combos

- Achar em qual arquivo você está no explorer: olhe a statusline (lualine) — o nome do
  buffer ativo aparece lá, inclusive mostrando `netrw`/`neo-tree` quando o explorer está focado.
- Trocar tema sem editar arquivo: troque a variável `theme_name` no seu `themes.lua` e
  reabra o Neovim.
- Ver de onde veio uma opção ativa: `:verbose set <opção>?` mostra qual arquivo definiu
  aquele valor por último (útil pra saber se é padrão do LazyVim ou customização sua).
- Confirmar que o LSP está ativo num arquivo: `:LspInfo` ou `:checkhealth lsp`.
