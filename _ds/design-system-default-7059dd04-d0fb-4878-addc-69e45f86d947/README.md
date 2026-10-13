# Design System Default (marca Duda Barzagli): como construir

Componentes shadcn/ui (Radix) com o tema da marca: claro, macio, uma cor de personalidade (bordô). Todos os exports estão em `window.DesignSystemDefault`.

## Setup

- Nenhum provider é necessário: os componentes funcionam soltos. O visual vem inteiro de `styles.css` (tema + utilitários Tailwind compilados + fontes Outfit e Figtree do Google Fonts). Sem ele, tudo sai sem estilo.
- Só existe tema claro. Não use a classe `dark`.
- Fundo da página: `bg-background` no container raiz, com texto `text-foreground`.
- Ícones: use os ícones Lucide de `window.DesignSystemDefault.icons` (ex.: `icons.Users`, `icons.LineChart`, `icons.Sparkles`, `icons.Mail`, `icons.Download`). São esses que `CaseCard` e `IconBubble` recebem em `icon`. Sem emoji.

## Idioma de estilo: Tailwind pré-compilado

Estilize o seu layout com classes Tailwind, mas **só existem as classes já compiladas em `_ds_bundle.css`**: o Tailwind não roda nos designs. Classes com valor arbitrário (`bg-[#...]`, `w-[37px]`) e cores da paleta padrão (`bg-red-500`) não funcionam. Para um valor fora da lista, use `style={{ ... }}` com as variáveis CSS do tema (`var(--primary)`, `var(--brand-deep)`).

| Família | Classes reais |
|---|---|
| Cor | `bg-/text-/border-` + `background`, `foreground`, `card`, `muted`, `muted-foreground`, `primary`, `primary-foreground`, `secondary`, `accent`, `border`, `brand`, `brand-deep`, `brand-soft`, `brand-accent`, `on-brand` |
| Tipografia | `text-display-xl` (só no Hero), `text-heading-1`, `text-heading-2` (abre seções), `text-heading-3`, `text-body-lg`, `text-base`, `text-caption`; `font-heading` (Outfit), `font-sans` (Figtree) |
| Raio | `rounded-xl` (40px, blocos de seção), `rounded-lg` (28px, cards), `rounded-md` (20px), `rounded-full` (tudo que se clica) |
| Sombra | `shadow-card`, `shadow-card-hover`, `shadow-float` (Hero e nav) |
| Layout | `flex`, `grid`, `grid-cols-{1,2,3,4}`, `gap-{2,3,4,6,8,12}`, `p-{4,6,8,10}`, `px-/py-`, `mb-/mt-{2..24}`, `max-w-{md,2xl,5xl}`, `mx-auto`, com `sm:`/`md:`/`lg:` |
| Movimento | `ease-brand`, `duration-150`, `duration-250` |

Regras da marca: o bordô aparece pouco, mas em lugares grandes (Hero, números de impacto, links). Cards se separam do fundo por sombra, nunca por borda. Sem eyebrow em caixa alta, sem "→" em botões, sem gradientes além do Hero. Copy em primeira pessoa e concreta.

## Onde está a verdade

Leia `guidelines/docs/marca.md` (princípios, cores, o que evitar) e `guidelines/docs/efeitos.md` (movimento). Cada componente tem o seu guia em `components/<grupo>/<Nome>/<Nome>.prompt.md`. As subpartes vêm junto com o componente principal: `CardHeader`, `CardTitle`, `CardDescription`, `CardContent`, `CardFooter` e `CardAction` (Card); `ToggleGroupItem` (ToggleGroup); `NavigationMenuList`, `NavigationMenuItem`, `NavigationMenuLink`, `NavigationMenuTrigger` e `NavigationMenuContent` (NavigationMenu); `GlassNavLink` (GlassNav).

## Exemplo

```tsx
const { Hero, Button, CaseCard, Badge, icons } = window.DesignSystemDefault

<main className="mx-auto flex max-w-5xl flex-col gap-16 bg-background px-4 py-6 md:gap-24">
  <Hero title={["Produtos que resolvem", "problemas de verdade."]}>
    <p className="text-body-lg mb-7 max-w-md">Sou Duda, Product Manager.</p>
    <Button size="xl" variant="highlight">Ver meus cases</Button>
  </Hero>
  <section>
    <h2 className="text-heading-2 mb-12">Cases</h2>
    <div className="grid gap-6 md:grid-cols-2">
      <CaseCard href="#onboarding" icon={icons.Users} title="Onboarding automatizado"
        context="ERP, jornada de implantação"
        impact={{ value: 40, unit: "%", label: "menos tempo até o primeiro valor" }} />
    </div>
    <div className="mt-6 flex flex-wrap gap-2"><Badge variant="secondary">Discovery</Badge></div>
  </section>
</main>
```

# DesignSystemDefault (design-system-default@0.1.0)

This design system is the published design-system-default React library, bundled as a single
browser global. All 13 components are the real upstream code.

## Where things are

- `_ds_bundle.js` — the whole-DS bundle at the project root; loads every component to `window.DesignSystemDefault`. First line is a `/* @ds-bundle: … */` metadata header.
- `styles.css` — the single stylesheet entry: it `@import`s the tokens, fonts, and component styles (`_ds_bundle.css`). Link this one file.
- `components/<group>/<Name>/<Name>.prompt.md` (example JSX + variants), `<Name>.d.ts` (types), `<Name>.html` (variant grid).
- `tokens/*.css` — CSS custom properties, names verbatim from upstream.
- `fonts/` — `@font-face` files + `fonts.css` (when the package ships fonts).
- `guidelines/` — the design system's own usage guidance (2 doc(s), see `guidelines/index.md`). Read these before composing larger layouts.

For a specific component, `read_file("components/<group>/<Name>/<Name>.prompt.md")`.

## Loading

Add these two lines to your page once (React must be on the page first):

```html
<link rel="stylesheet" href="styles.css">
<script src="_ds_bundle.js"></script>
```

Components are then available at `window.DesignSystemDefault.*`. Mount into a dedicated child node (e.g. `<div id="ds-root">`), not the host page's own React root, so the two trees don't collide:

```jsx
const { Badge } = window.DesignSystemDefault;
ReactDOM.createRoot(document.getElementById('ds-root')).render(<Badge />);
```

## Tokens

166 CSS custom properties from design-system-default. Names are
preserved verbatim from upstream. They are declared inside `_ds_bundle.css` (this DS ships one compiled stylesheet rather than separate token files).

- **color** (27): `--color-red-500`, `--color-white`, `--text-xs`, …
- **spacing** (6): `--tw-space-y-reverse`, `--tw-space-x-reverse`, `--tw-inset-shadow`, …
- **typography** (10): `--font-sans`, `--font-weight-normal`, `--font-weight-medium`, …
- **radius** (2): `--radius-md`, `--radius`
- **shadow** (7): `--tw-shadow`, `--tw-ring-shadow`, `--tw-shadow-alpha`, …
- **other** (114): `--spacing`, `--container-xs`, `--container-sm`, …

## Components

### conteudo
- `Badge`
- `Card`
- `CaseCard` — Card de case: bolha de cone, ttulo, contexto e nmero de impacto. Com href  clicvel e sobe 2px no hover.
- `IconBubble` — Bolha pill com cone Lucide (trao 1.5px). brand usa brand-soft muted usa surface-muted.
- `ImpactNumber` — Nmero de impacto em Outfit 600 cor brand, com unidade menor em ink-muted.
- `Separator`

### controles
- `Button`
- `Toggle`
- `ToggleGroup`

### navegacao
- `FilterChips` — Filtro em pills sobre o ToggleGroup do shadcn/ui, com um indicador
- `GlassNav` — Nav flutuante em pill com efeito de vidro (branco 72 + blur). nico uso de transparncia no sistema.
- `NavigationMenu`

### secoes
- `Hero` — Hero com gradiente brand-deep  brand, luz radial e gro.
