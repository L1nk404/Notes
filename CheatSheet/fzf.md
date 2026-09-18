## fzf (Fuzzy Finder) — Cheatsheet

### Instalação

| Distro        | Comando                                              |
| ------------- | ----------------------------------------------------- |
| Ubuntu/Debian | `sudo apt update && sudo apt install fzf`            |
| Fedora/RHEL   | `sudo dnf install fzf`                               |
| Arch Linux    | `sudo pacman -S fzf`                                 |
| A partir do source | `git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf && ~/.fzf/install` |

**Integração pós-instalação (shell):**

```bash
# Bash
echo '[ -f ~/.fzf.bash ] && source ~/.fzf.bash' >> ~/.bashrc

# Zsh
echo '[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh' >> ~/.zshrc
```

> Nota: no openSUSE, `fzf` costuma estar disponível via `zypper install fzf` também.

### Comandos básicos

| Comando                     | Descrição                                          |
| ---------------------------- | ---------------------------------------------------- |
| `fzf`                       | Abre o fuzzy finder interativo na stdin ou arquivos |
| `vim $(fzf)`                | Encontra e abre arquivo no vim                      |
| `ls \| fzf`                 | Busca fuzzy na saída do `ls`                        |
| `fzf --multi`               | Habilita multi-seleção (use Tab para selecionar)    |
| `fzf --preview 'cat {}'`    | Mostra preview do arquivo durante a navegação       |
| `fzf --reverse`              | Exibe resultados de cima para baixo                 |
| `fzf --height 40%`           | Define altura do finder como 40% da tela            |
| `fzf --border`               | Adiciona borda na interface do finder               |
| `fzf --prompt "Select: "`    | Customiza o prompt de input                         |
| `fzf --exact`                | Usa correspondência exata em vez de fuzzy           |
| `fzf +i`                     | Habilita busca case-sensitive                       |
| `fzf --inline-info`          | Exibe informações inline junto ao prompt            |
| `find . -name "*.py" \| fzf` | Encontra e filtra tipos específicos de arquivo      |
| `ps aux \| fzf`               | Busca interativa por processos rodando              |
| `history \| fzf`              | Busca no histórico de comandos                      |

### Atalhos de teclado padrão

| Tecla                 | Ação                                                    |
| ----------------------- | ---------------------------------------------------------- |
| `Ctrl+T`               | Cola arquivos/diretórios selecionados na linha de comando |
| `Ctrl+R`               | Busca no histórico e cola o comando selecionado         |
| `Alt+C`                | Encontra diretório via fuzzy e faz `cd` nele             |
| `**<Tab>`              | Aciona a fuzzy completion (ex: `vim **<Tab>`)            |
| `Ctrl+J` / `Ctrl+N`    | Move cursor para baixo                                  |
| `Ctrl+K` / `Ctrl+P`    | Move cursor para cima                                   |
| `Enter`                | Seleciona item e sai                                     |
| `Tab`                  | Seleciona/desmarca item no modo multi-select             |
| `Shift+Tab`            | Desmarca item no modo multi-select                       |
| `Ctrl+C` / `Esc`       | Cancela e sai                                            |

### Uso avançado

| Comando                                                       | Descrição                                              |
| ---------------------------------------------------------------- | ---------------------------------------------------------- |
| `fzf --preview 'bat --color=always {}'`                      | Preview com syntax highlighting usando bat             |
| `fzf --preview-window=right:50%`                              | Posiciona janela de preview à direita, 50% de largura  |
| `fzf --preview-window=hidden`                                  | Inicia com preview escondido (toggle com `Ctrl+/`)     |
| `fzf --bind 'ctrl-e:execute(vim {})'`                         | Executa vim no item selecionado com `Ctrl+E`           |
| `fzf --bind 'ctrl-y:execute-silent(echo {} \| pbcopy)'`       | Copia seleção para clipboard silenciosamente           |
| `fzf --delimiter=' ' --nth=2..`                                | Busca somente a partir do 2º campo                     |
| `fzf --header="Select file to edit"`                          | Adiciona texto de header no finder                     |
| `fzf --color=dark`                                             | Usa esquema de cores escuro                            |
| `fzf --query="initial"`                                        | Pré-popula a query de busca                            |
| `fzf --select-1 --exit-0`                                      | Auto-seleciona se houver 1 match, sai se não houver nenhum |
| `fzf --expect=ctrl-d,ctrl-e`                                   | Retorna exit codes diferentes para teclas diferentes   |
| `fzf --print-query`                                            | Imprime a string de busca mesmo sem match              |
| `fzf --cycle`                                                  | Habilita ciclagem de resultados (volta ao início)      |
| `fzf --no-mouse`                                                | Desabilita suporte a mouse                             |
| `fzf --algo=v2`                                                 | Usa algoritmo de matching mais rápido para datasets grandes |
| `fzf --tac`                                                    | Inverte ordem do input                                 |
| `fzf --no-sort`                                                 | Desabilita ordenação, mantém ordem do input            |
| `fzf --border=rounded`                                          | Usa estilo de borda arredondada                        |
| `fzf --sync`                                                    | Usa filtragem síncrona (sem async)                     |
| `fzf --scroll-off=5`                                            | Mantém item selecionado a 5 linhas da borda            |

### Configuração

**Arquivo de configuração principal:** `~/.bashrc` ou `~/.zshrc`

```bash
# Comando padrão para buscar arquivos (usando fd)
export FZF_DEFAULT_COMMAND='fd --type f --hidden --follow --exclude .git'

# Opções padrão aplicadas a todas as invocações do fzf
export FZF_DEFAULT_OPTS='
--height 40%
--layout=reverse
--border
--inline-info
--preview "bat --style=numbers --color=always --line-range :500 {}"
--preview-window=right:50%:hidden
--bind "ctrl-/:toggle-preview"
--bind "ctrl-y:execute-silent(echo {} | pbcopy)"
'

# Configuração do Ctrl+T (finder de arquivos)
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_CTRL_T_OPTS="
--preview 'bat --color=always --line-range :50 {}'
--bind 'ctrl-/:change-preview-window(down|hidden|)'
"

# Configuração do Alt+C (finder de diretórios)
export FZF_ALT_C_COMMAND='fd --type d --hidden --follow --exclude .git'
export FZF_ALT_C_OPTS="
--preview 'tree -C {} | head -50'
"

# Configuração do Ctrl+R (busca de histórico)
export FZF_CTRL_R_OPTS="
--preview 'echo {}'
--preview-window up:3:hidden:wrap
--bind 'ctrl-/:toggle-preview'
--bind 'ctrl-y:execute-silent(echo -n {2..} | pbcopy)+abort'
--color header:italic
--header 'Press CTRL-Y to copy command into clipboard'
"
```

### Esquemas de cores

```bash
# Monokai
export FZF_DEFAULT_OPTS='
--color=bg+:#293739,bg:#1B1D1E,border:#808080
--color=spinner:#E6DB74,hl:#7E8E91,fg:#F8F8F2
--color=header:#7E8E91,info:#A6E22E,pointer:#A6E22E
--color=marker:#F92672,fg+:#F8F8F2,prompt:#F92672,hl+:#F92672
'

# Dracula
export FZF_DEFAULT_OPTS='
--color=fg:#f8f8f2,bg:#282a36,hl:#bd93f9
--color=fg+:#f8f8f2,bg+:#44475a,hl+:#bd93f9
--color=info:#ffb86c,prompt:#50fa7b,pointer:#ff79c6
--color=marker:#ff79c6,spinner:#ffb86c,header:#6272a4
'

# Nord
export FZF_DEFAULT_OPTS='
--color=fg:#e5e9f0,bg:#3b4252,hl:#81a1c1
--color=fg+:#e5e9f0,bg+:#434c5e,hl+:#81a1c1
--color=info:#eacb8a,prompt:#bf6069,pointer:#b48dac
--color=marker:#a3be8b,spinner:#b48dac,header:#a3be8b
'
```

### Casos de uso comuns

**Checkout de branch no git**

```bash
# Seleção interativa de branch
git checkout $(git branch -a | fzf | sed 's/^[ *]*//' | sed 's/remotes\/origin\///')

# Ou como alias
alias gcb='git checkout $(git branch | fzf | sed "s/^[ *]*//")'
```

**Matar processo**

```bash
# Encontra e mata processo interativamente
kill -9 $(ps aux | fzf | awk '{print $2}')

# Ou como função
fkill() {
  local pid
  pid=$(ps aux | sed 1d | fzf -m | awk '{print $2}')
  if [ "x$pid" != "x" ]; then
    echo $pid | xargs kill -${1:-9}
  fi
}
```

**Seleção de host SSH**

```bash
# Seleciona a partir dos hosts do config SSH
ssh $(grep "^Host" ~/.ssh/config | grep -v "[?*]" | cut -d " " -f2- | fzf)

# Ou como alias
alias fssh='ssh $(grep "^Host" ~/.ssh/config | grep -v "[?*]" | cut -d " " -f2- | fzf)'
```

**Navegação de diretórios com histórico**

```bash
# Função para navegar e ir a um diretório
fcd() {
  local dir
  dir=$(find ${1:-.} -path '*/\.*' -prune -o -type d -print 2> /dev/null | fzf +m) && cd "$dir"
}

# Pular para diretórios usados com frequência (requer z ou autojump)
fz() {
  local dir
  dir=$(z -l 2>&1 | fzf --height 40% --nth 2.. --reverse --inline-info +s --tac --query "${*}" | sed 's/^[0-9,.]* *//')
  cd "$dir"
}
```

**Gerenciamento de containers Docker**

```bash
# Seleciona e entra em container rodando
docker exec -it $(docker ps | fzf | awk '{print $1}') /bin/bash

# Seleciona e para containers
docker stop $(docker ps -a | fzf -m | awk '{print $1}')

# Seleciona e remove imagens
docker rmi $(docker images | fzf -m | awk '{print $3}')
```

**Visualizador de variáveis de ambiente**

```bash
# Navega e copia variáveis de ambiente
env | fzf --preview 'echo {}' --preview-window=up:3:wrap \
  --bind 'enter:execute(echo {} | cut -d= -f2 | pbcopy)+abort'
```

**Análise de arquivos de log**

```bash
# Busca em arquivos de log com preview
find /var/log -type f 2>/dev/null | fzf \
  --preview 'tail -100 {}' \
  --bind 'enter:execute(less {})'

# Busca conteúdo de log
grep -r "ERROR" /var/log 2>/dev/null | fzf \
  --delimiter=: \
  --preview 'bat --color=always {1} --highlight-line {2}' \
  --preview-window +{2}-/2
```

### Boas práticas

- **Use com fd ou ripgrep:** substitua `find` por `fd` para busca de arquivos mais rápida: `export FZF_DEFAULT_COMMAND='fd --type f'`
- **Habilite preview por padrão:** adicione janelas de preview para ver conteúdo antes de selecionar, melhorando precisão e reduzindo erros
- **Crie aliases de shell:** monte aliases customizados para workflows frequentes (operações git, conexões SSH, gerenciamento de processos)
- **Combine com outras ferramentas:** integre fzf com `bat` para syntax highlighting, `tree` para preview de diretórios, e `delta` para diffs do git
- **Use multi-select com sabedoria:** habilite `--multi` quando precisar operar em múltiplos itens (deletar arquivos, checkout de múltiplos arquivos no git)
- **Aproveite os key bindings:** customize opções `--bind` para criar atalhos para ações comuns (abrir no editor, copiar para clipboard, executar comandos)
- **Otimize para datasets grandes:** use `--algo=v2` e considere modo `--sync` para listas de arquivos muito grandes, melhorando performance
- **Defina alturas apropriadas:** use `--height 40%` ou similar para evitar tomar a tela inteira, mantendo contexto do que você estava fazendo
- **Configure esquemas de cores:** combine as cores do fzf com o tema do seu terminal para melhor integração visual
- **Use delimitadores de campo:** ao processar saída estruturada (como `ps` ou `docker`), use `--delimiter` e `--nth` para buscar em colunas específicas

### Troubleshooting

| Problema                              | Solução                                                                                  |
| ---------------------------------------- | -------------------------------------------------------------------------------------------- |
| Key bindings não funcionam             | Rode o script de instalação `~/.fzf/install` ou faça source do arquivo de integração no seu RC |
| Performance lenta com listas grandes    | Use `fd` ou `ripgrep` em vez de `find`: `export FZF_DEFAULT_COMMAND='fd --type f'` e considere `--algo=v2` |
| Preview não mostra conteúdo do arquivo | Confira se o comando de preview está correto e o arquivo é legível. Tente `--preview 'cat {}'` ou instale o `bat` |
| Cores não aparecem corretamente        | Confira se o terminal suporta 256 cores. Use `--color=16` para suporte básico ou ajuste o esquema |
| Comando não encontrado após instalação  | Adicione o fzf ao PATH ou reinicie o shell                                              |
| Caracteres Unicode não renderizam      | Garanta que terminal e locale suportam UTF-8: `export LC_ALL=en_US.UTF-8` e use fonte com suporte Unicode |
| fzf consumindo muita memória            | Reduza o tamanho do input ou use modo `--sync`. Considere filtrar o input antes de passar pro fzf |
| Janela de preview não atualiza         | Use `--preview-window=follow` para auto-scroll ou dê toggle no preview com `Ctrl+/`     |
| Não consigo selecionar itens com mouse | Suporte a mouse pode estar desabilitado. Remova a opção `--no-mouse` ou clique fora do fzf e tente de novo |
| Completion não funciona com `**<Tab>`   | Garanta que a integração do shell está carregada. Confira se `~/.fzf.bash` ou `~/.fzf.zsh` está sendo carregado no seu RC |

### Sintaxe de busca

| Sintaxe          | Descrição                                | Exemplo                                          |
| ------------------ | -------------------------------------------- | --------------------------------------------------- |
| `term`           | Match fuzzy                                 | `fzf` com input "abc" dá match em "a_b_c.txt"      |
| `'term`          | Match exato (prefixo aspas simples)         | `'abc` dá match apenas em "abc" exato              |
| `^term`          | Match exato de prefixo                      | `^abc` dá match em "abc…" mas não em "zabc"        |
| `term$`          | Match exato de sufixo                       | `abc$` dá match em "…abc" mas não em "abcz"        |
| `!term`          | Match inverso (exclusão)                     | `!test` exclui linhas com "test"                   |
| `term1 term2`    | Match AND (ambos exigidos)                   | `foo bar` dá match em linhas com ambos os termos   |
| `term1 \| term2` | Match OR (um ou outro exigido)               | `foo \| bar` dá match em linhas com um ou outro    |

### Funções úteis de shell

```bash
# Abre arquivo no editor com preview
fe() {
  local files
  files=$(fzf --query="$1" --multi --select-1 --exit-0 \
    --preview 'bat --color=always --line-range :500 {}')
  [[ -n "$files" ]] && ${EDITOR:-vim} "${files[@]}"
}

# Muda para diretório com preview
fcd() {
  local dir
  dir=$(fd --type d --hidden --follow --exclude .git | fzf \
    --preview 'tree -C {} | head -100') && cd "$dir"
}

# Navegador de commits do git
fgc() {
  git log --oneline --color=always | fzf --ansi \
    --preview 'git show --color=always {1}' \
    --bind 'enter:execute(git show {1} | less -R)'
}

# Navega e instala pacotes (Debian/Ubuntu)
fap() {
  apt-cache search . | fzf --multi --preview 'apt-cache show {1}' | \
    awk '{print $1}' | xargs -ro sudo apt install
}
```

---

*Fonte: [1337skills.com/cheatsheets/fzf](https://1337skills.com/cheatsheets/fzf/)*
