# usestyles
Userstyles amadores e extremamente vibecodeds (lide com seu preconceito sobre o vibecoding dando prum jumento)

## Styles

- `one-monokai-proxmox-pve.user.css` — One Monokai para Proxmox VE, limitado a `https://192.168.0.100:8006/`.
- `readwise-reader-prh.user.css` — tipografia da página de imprensa da Penguin Random House no Readwise Reader. Guia abaixo.

---

## Guia: Readwise Reader com tipografia PRH

Aplica ao painel de leitura do [Readwise Reader](https://read.readwise.io) (versão web) a composição tipográfica da [página de imprensa de *The Technological Republic*](https://sites.prh.com/technologicalrepublicpressrelease): ITC Clearface, texto preto puro (`#000000`) sobre fundo branco (`#ffffff`) e a mesma escala de títulos.

O estilo só acrescenta regras ao que o Reader já faz. Barra lateral, menus, destaques (*highlights*), tema escuro e o menu **Aa** continuam funcionando.

### 1. Requisitos

- Extensão [Stylus](https://github.com/openstyles/stylus) (Chrome, Firefox, Edge). O estilo usa `@preprocessor less` e `@var`, recursos de UserCSS que o Stylus suporta nativamente.
- Só funciona na web (`read.readwise.io`). Os apps de desktop, iOS e Android não aceitam estilos de usuário.

### 2. Instalação

1. Com o Stylus instalado, abra o [arquivo bruto (*raw*)](https://raw.githubusercontent.com/angbandz/userstyles/main/readwise-reader-prh.user.css).
2. O Stylus abre a tela de instalação. Clique em **Instalar estilo**.
3. Recarregue o `read.readwise.io`.

O cabeçalho do estilo declara `@updateURL`, então o Stylus acha novas versões sozinho em **Verificar atualizações**.

### 3. Configuração

No ícone do Stylus, clique na engrenagem ao lado do estilo e ajuste **Preto no branco (PRH)**:

| Opção | Quando usar |
| --- | --- |
| **Seguir o sistema** *(padrão)* | O Reader está com o tema em *Auto*. O preto no branco entra quando o sistema operacional está no modo claro (`prefers-color-scheme: light`). |
| **Sempre** | O Reader está fixo no tema claro, mas o sistema está no modo escuro. Sem essa opção o preto no branco nunca entraria. |
| **Nunca** | Você quer só a tipografia, com as cores originais do Reader. |

Tamanho da fonte, largura da coluna e entrelinha continuam no menu **Aa** do Reader. Os títulos usam `em`, então acompanham o tamanho que você escolher lá.

### 4. O que o estilo faz

| Área | Regra |
| --- | --- |
| **Família tipográfica** | ITC Clearface embutida em base64 (`@font-face`, pesos 400, 700, 800 e 900, com itálicos). Não precisa instalar nada. Ordem de *fallback*: Clearface do sistema → Fraunces (Google Fonts) → Georgia. |
| **Escala tipográfica** | Tamanhos da PRH (corpo de 23px) convertidos para `em`: h1 68/23, h2 52/23, h3 32/23, h4 20/23, h5 15/23, legenda 15/23, `small` 12/23. |
| **Cor do texto** | Sobrescreve os *design tokens* `--reading-text-primary` e `--reading-text-title` com `#000000`. |
| **Fundo do painel de leitura** | `--content-background-color: #ffffff`. As outras superfícies do Reader não mudam. |
| **Renderização** | Troca `-webkit-font-smoothing: antialiased` por `auto` no tema claro, para o traço sair com o peso real da fonte. |
| **Links** | Mesma cor do texto, com sublinhado de 1px, como na PRH. |
| **Código** | `pre`, `code`, `kbd` e `samp` mantêm a fonte monoespaçada do Reader. |

### 5. Por que o texto ficava cinza (até a 1.0.0)

1. **Cor aplicada no nó errado.** A 1.0.0 aplicava `color` em `#document-text-content`. Só que o Reader não pinta os parágrafos por herança: cada elemento lê a cor dos *tokens* `--reading-text-*`, definidos em `.document-content` e, separadamente, em `#document-header`. O `color` do contêiner era ignorado. A 1.1.0 sobrescreve os próprios tokens.
2. **Tema do sistema ≠ tema do Reader.** A *media query* `prefers-color-scheme` segue o sistema operacional, não o tema escolhido no Reader. Com o sistema escuro e o Reader claro, a regra nunca entrava. Por isso agora existe a opção **Sempre**.
3. **Antialiasing em escala de cinza.** Com `antialiased`, o navegador desliga o reforço de traço (*stem darkening*) no macOS. Letras finas como as da Clearface ficam com aparência acinzentada mesmo em `#000`.
4. **Fundo quase branco.** O preto só parece preto de verdade sobre `#ffffff`, que é o fundo da PRH.

### 6. Solução de problemas

| Sintoma | Verificação |
| --- | --- |
| Texto ainda cinza | Confira se o modo é **Sempre** quando o sistema está no escuro e o Reader no claro. Recarregue a página com o cache limpo (Ctrl/Cmd + Shift + R). |
| Fonte não mudou | Veja se o estilo está ativo para `read.readwise.io` no ícone do Stylus. O Reader precisa estar na web, não no app. |
| Tema escuro com fundo branco | O modo **Sempre** está ativo com o Reader no escuro. Volte para **Seguir o sistema**. |
| Parou de funcionar depois de uma atualização do Reader | O Readwise pode ter renomeado os tokens `--reading-text-*`. Abra o DevTools (F12), inspecione um parágrafo e procure as variáveis `--reading-*` no painel *Computed*. |

### 7. Glossário

Os pedidos originais, em linguagem natural, e o termo técnico correspondente:

| Linguagem natural | Termo técnico |
| --- | --- |
| "reproduzir fielmente a página da PRH" | Replicar a composição tipográfica: família, escala tipográfica, pesos, cor e fundo. |
| "está meio cinza em vez de preto no branco" | Contraste abaixo do alvo: cor de texto diferente de `#000000`, fundo diferente de `#ffffff` e traço afinado pelo antialiasing em escala de cinza. |
| "adicionar ao site sem perder as características" | Estilo aditivo e não destrutivo: sobrescreve *design tokens* e seletores estáveis (`#document-text-content`, `.document-content`, `#document-header`), não classes com *hash* do *build*, e não altera a interface fora do painel de leitura. |
| "tema claro / escuro" | *Color scheme*; a detecção via CSS é a *media query* `prefers-color-scheme`. |
| "fonte" | Família tipográfica (*font family*) e as faces carregadas por `@font-face`. |
| "tamanho dos títulos" | Escala tipográfica (*type scale*), expressa em `em` relativa ao corpo. |
| "letra fininha" | Peso de traço reduzido por `-webkit-font-smoothing: antialiased`. |
| "configuração do estilo" | Variáveis UserCSS (`@var`) processadas pelo pré-processador Less no Stylus. |

### 8. Licença

MIT. A ITC Clearface é uma fonte comercial da ITC/Monotype. Ela está embutida para uso pessoal, então confira a sua licença antes de redistribuir.
