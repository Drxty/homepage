# Guia Completo de Customização da Página Inicial

Este guia é um tutorial passo a passo para ajudá-lo a entender e personalizar **index.html**, incluindo estilos, cores, funcionalidades, adição de músicas e muito mais.

---

## 1. Ferramenta Essencial: Como Usar o "Localizar" (CTRL+F)

A ferramenta mais importante para navegar e editar o código será a função "Localizar".

1. \*\*Abra o arquivo \*\*\`\` em um editor de texto (como VS Code, Sublime, Notepad++ etc.).
2. Pressione **CTRL + F** (Windows/Linux) ou **CMD + F** (Mac).
3. Digite o termo que deseja encontrar (ex: `.btn`, `--bg-color`, `id="search-form"`).
4. Use as setas para navegar entre ocorrências.

---

## 2. Exemplo de Personalização de Widget (Pomodoro)

Para ativar/desativar o widget Pomodoro:

```html
<!-- ANTES -->
<section id="pomodoro-section" class="widget-section">
  <!-- conteúdo ... -->
</section>

<!-- DEPOIS -->
<section id="pomodoro-section" class="widget-section hidden">
  <!-- conteúdo ... -->
</section>
```

Basta adicionar/remover a classe `hidden` no `<section>` correspondente.

---

## 3. Cores Personalizadas no Painel de Configurações

No painel de configurações (`#settings-panel`), localize:

```html
<section id="custom-colors-section" class="settings-section border-t">
  <h3 data-lang-key="customColors">Cores Customizadas</h3>
  <div id="color-inputs-container"></div>
  <button id="reset-colors-btn" class="btn btn-secondary">Resetar Cores</button>
</section>
```

Cada `<input type="color" data-css-var="--nome-da-variavel">` altera a variável CSS no `:root` ou `.dark:root`.

**Adicionar**: copie um `<div>` de controle, ajuste `id`, `for` e `data-css-var`, e garanta que a variável exista no começo do CSS.

---

## 3.1 Fundo de Seleção de Texto (::selection)

O estilo de seleção de texto (quando você arrasta para selecionar) é definido nas regras globais:

```css
::selection {
  background-color: var(--selection-bg-color);
  color: var(--bg-color);
}
```

- **Variável CSS**: `--selection-bg-color` está configurada em `:root` e em `.dark:root`, permitindo cores diferentes para temas claro e escuro.
- **Modificação**:
  1. Altere o valor de `--selection-bg-color` no `:root` para mudar a cor de fundo da seleção.
  2. Para mudar a cor do texto selecionado, altere `color` dentro de `::selection` ou adapte `var(--bg-color)`.
  3. Se desejar um estilo mais customizado (ex: sombra ou contorno), adicione propriedades CSS extras dentro de `::selection`.

Exemplo de customização:

```css
:root {
  --selection-bg-color: rgba(200, 200, 0, 0.5);
}
.dark:root {
  --selection-bg-color: rgba(100, 100, 100, 0.5);
}
::selection {
  background-color: var(--selection-bg-color);
  color: var(--text-color);
}
```

---

## 4. Traduções e Textos (Internacionalização)

No final do `<script>` você encontra:

```js
const translations = {
  en: { /* ... */ },
  pt: { /* ... */ }
};
```

- **Novo idioma**: adicione `es: { chaves }` em `translations`.
- **Seleção**: inclua `<option value="es">🇪🇸 ES</option>` em `<select id="language-selector">`.

Para alterar textos, basta editar o valor da chave desejada (ex: `start`, `searchPlaceholder`).

---

## 5. Widget de Visibilidade

Dentro de:

```html
<div data-lang-key="visibility" class="settings-section border-t">
  <div class="flex-between">
    <label for="toggle-music-player">Habilitar Player</label>
    <input id="toggle-music-player" type="checkbox" class="form-checkbox">
  </div>
  <!-- outros toggles similares -->
</div>
```

IDs disponíveis: `toggle-music-player`, `toggle-widgets`, `toggle-pomodoro`. A troca adiciona/remove `hidden` nos widgets.

---

## 6. Plano de Fundo (Background)

Localize:

```html
<input id="background-image-url" type="text">
<button id="clear-background-btn" class="btn btn-secondary">Limpar Plano de Fundo</button>
```

A URL salva-se em `localStorage.backgroundImageUrl` e aplica-se via:

```css
:root { --bg-image: url('...'); }
```

Use `localStorage.clear()` no console para resetar.

---

## 7. Adicionando/Removendo Músicas Padrão

No objeto `musicPlayer.defaultSongs` dentro do `<script>`:

```js
defaultSongs: [
  { name: "Chrono Cross", url: "...mp3" },
  { name: "Minha Música", url: "https://example.com/minha.mp3" }
],
```

URLs devem ter CORS habilitado. Para remover, delete o item da lista.

---

## 8. Personalizando Botões e Ícones em Detalhe

### 8.1 Estilo Base de Todos os Botões

Todas as regras estão na classe `.btn`, no `<style>` inicial do `index.html`:

```css
.btn {
  border: 2px solid var(--hr-color);
  background: transparent;
  color: inherit;
  border-radius: 9999px;
  padding: 10px 16px;
  margin: 2px;
  font: inherit;
  font-size: 1.15em;
  font-weight: 600;
  white-space: nowrap;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: .5rem;
  transition: background-color .2s ease, transform .2s ease, color .2s;
  cursor: pointer;
  text-decoration: none;
}
```

As propriedades controlam:

- **Borda** (`border`, `--hr-color` define cor)
- **Arredondamento** (`border-radius`)
- **Espaçamento interno** (`padding`)
- **Espaçamento externo** (`margin`)
- **Tamanho e peso da fonte** (`font-size`, `font-weight`)
  - Para modificar o tamanho do texto dos botões, ajuste `font-size` diretamente na classe `.btn` (ex: `font-size: 1.25em;`) ou crie uma classe auxiliar (ex: `.btn-lg { font-size: 1.5em; }`).
- **Alinhamento** (`display`, `align-items`, `justify-content`)
- **Alinhamento** (`display`, `align-items`, `justify-content`)
- **Distância entre ícone e texto** (`gap`)

### 8.2 Ícones e Botões Circulares

Classe `.btn-icon` especializa botões ícones:

```css
.btn-icon {
  padding: .75rem;
  font-size: 1em;
  border-radius: 50%;
}
.btn svg {
  width: 1.25em;
  height: 1.25em;
  color: var(--svg-icon-color);
}
```

Use `class="btn btn-primary btn-icon"` para botões de ícone com estilo primário.

### 8.3 Estados Interativos (Hover, Focus, Active)

```css
.btn:hover,
.btn:focus-visible {
  outline: none;
  background-color: var(--btn-secondary-hover-bg-color);
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  text-decoration: none;
}
.btn:active {
  transform: scale(.92);
}
```

- **Hover/Focus**: muda o fundo e adiciona sombra.
- **Active**: aplica `transform: scale(.92)` (efeito de clique). Você pode substituir por `translateY(2px)` para um “afundamento”.

### 8.4 Variações de Botões (Primário, Secundário, Danger)

- `.btn-primary`: usa `--primary-color` na borda e texto, ao hover preenche com essa cor e altera o texto para `--btn-primary-text-color`.
- `.btn-secondary`: borda e texto com cores secundárias, hover muda apenas o texto para `--btn-secondary-hover-text-color`.
- `.btn-danger`: borda em `--danger-color`, hover preenche e muda texto para `--danger-hover-text-color`.

Cada variação herda a estrutura `.btn` e redefine apenas cores e variáveis de foco (semi-automático via CSS custom properties).

### 8.5 Espaçamento Entre Botões

- O `margin: 2px` em `.btn` garante espaçamento mínimo.
- Para grupos de botões (`<div class="flex gap-2">`), utilize utilitários `gap-2` ou `gap-4` nas containers para espaçamentos consistentes.

### 8.6 Detalhes do Botão de Pesquisa

O botão de pesquisa, dentro do formulário `#search-form`, possui personalizações próprias:

```html
<form id="search-form" class="flex items-center gap-2">
  <input id="search-input" type="text" placeholder="Buscar..." class="flex-grow form-input">
  <button id="search-button" type="submit" class="btn btn-primary">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 24 24"><path d="M..."/></svg>
  </button>
</form>
```

- **Tamanho:** o `.btn` herda `padding: 10px 16px`; para reduzir, crie uma classe extra, ex: `.btn-search { padding: 8px 12px; }` e aplique-a junto de `.btn`.
- **Ícone interno:** o `<svg>` usa dimensões de `h-5 w-5` (1.25em). Ajuste as classes do SVG ou defina diretamente `width`/`height` na tag.
- **Espaçamento entre campo e botão:** o container `.flex` e `gap-2` definem 0.5rem; modifique para `gap-1` ou `gap-3` conforme necessidade.
- **Cor de fundo no hover:** para diferenciar, adicione:

```css
#search-button:hover {
  background-color: var(--primary-color-light);
}
```

- **Sombras específicas:** use um seletor dedicado:

```css
#search-button:focus-visible {
  box-shadow: 0 0 0 3px var(--primary-color-transparent);
}
```

- **Transições personalizadas:** adicione ou ajuste no CSS:

```css
#search-button {
  transition: background-color .15s ease-in, transform .1s ease;
}
#search-button:active {
  transform: scale(.95);
}
```

---

## 9. Layout Responsivo (Media Queries)

O arquivo inclui:

```css
@media (max-width: 640px) { /* mobile */ }
@media (min-width: 640px) { /* tablets */ }
@media (min-width: 1024px) { /* desktop */ }
```

Adicione novas regras `@media` conforme necessário.

---

## 10. Como Testar Suas Alterações

1. Abra `index.html` no navegador.
2. Inspecione com DevTools (F12).
3. Veja console para erros JS.
4. Use `localStorage.clear()` para resetar configurações.

---

# Fim do Guia

Salve este documento como `CUSTOMIZATION_GUIDE.md` e mantenha-o junto ao seu `index.html`.\
Este guia é iterativo: atualize-o sempre que adicionar novas personalizações.

