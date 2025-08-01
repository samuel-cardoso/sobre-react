# sobre-react

o React.js foi criado pelo Facebook (hoje chamado Meta).

Mais precisamente:

- Criado por: Jordan Walke, engenheiro de software do Facebook.
- Ano de criação: 2011 (usado internamente no Facebook).
- Lançamento público: 2013, como projeto open-source.

O React foi desenvolvido para resolver problemas de performance e manutenção em interfaces complexas e dinâmicas, como o feed de notícias do Facebook. Hoje ele é mantido pela Meta e uma grande comunidade de desenvolvedores.

O problema principal que motivou a criação do React não foi exatamente o chat, mas sim a dificuldade em manter interfaces de usuário complexas, interativas e em constante mudança, como:

- O feed de notícias do Facebook.
- A interface de comentários e curtidas, que exigia atualizações em tempo real de forma eficiente.

O problema na época, o Facebook usava outras soluções baseadas em jQuery e manipulação direta do DOM. Isso tornava o código:

- Difícil de manter.
- Muito sujeito a bugs em atualizações dinâmicas.
- Ineficiente em termos de performance.

A solução, Jordan Walke criou o React com foco em:

- Renderização declarativa.
- Componentização.

Um conceito revolucionário na época: 

- o Virtual DOM, que compara estados anteriores e novos para aplicar as mínimas mudanças no DOM real.

---

## 📌 1. DOM (Document Object Model)

**O que é:**

* O DOM é uma **representação em forma de árvore** da estrutura HTML de uma página web.
* Ele permite que **JavaScript interaja e modifique** elementos da página dinamicamente (adicionar, remover ou atualizar elementos).

**Exemplo visual simplificado de um DOM:**

```html
<body>
  <div>
    <h1>Olá, mundo!</h1>
    <p>Esse é um parágrafo.</p>
  </div>
</body>
```

Árvore DOM correspondente:

```
Document
└── html
    └── body
        └── div
            ├── h1
            └── p
```

**Problema do DOM tradicional:**

* Manipular o DOM diretamente é **lento**, especialmente quando há muitas alterações, pois o navegador precisa **recalcular layout, repintar a tela**, etc.

---

## ⚛️ 2. Virtual DOM (V-DOM)

**O que é:**

* É uma **cópia leve do DOM real**, mantida em memória pelas bibliotecas/frameworks como o **React**.
* Toda vez que você muda o estado de um componente, o React atualiza o Virtual DOM primeiro.

**Como funciona:**

1. O React **cria uma árvore virtual** com os elementos da interface.
2. Quando o estado muda, o React:

   * Gera uma nova árvore virtual (nova V-DOM).
   * Compara com a árvore anterior (usando um algoritmo de **"diff"**).
   * Descobre quais partes realmente mudaram.
   * Aplica **somente essas mudanças** no DOM real (usando **reconciliação**).

**Vantagem:**

* **Evita manipulações desnecessárias** no DOM real, tornando a atualização da UI muito mais eficiente.

---

## 🌲 3. Árvore de Renderização (Render Tree)

**O que é:**

* É uma estrutura interna criada pelo **navegador** ao processar o DOM + CSS.
* Ela **representa os elementos visíveis na tela** e **é usada para desenhar a interface**.

**Como é construída:**

1. O navegador interpreta o HTML e cria o DOM.
2. Interpreta o CSS e cria o **CSSOM** (CSS Object Model).
3. Junta os dois e forma a **árvore de renderização**.
4. Essa árvore é usada para:

   * **Calcular o layout** (posição e tamanho dos elementos).
   * **Desenhar os pixels** na tela.

---

## 🔄 Como DOM, Virtual DOM e Árvore de Renderização funcionam juntos

| Processo                            | O que acontece                                                                        |
| ----------------------------------- | ------------------------------------------------------------------------------------- |
| 1. Usuário interage com a página    | Ex: clica em um botão que muda o estado do app                                        |
| 2. Virtual DOM é atualizado         | O React gera uma nova árvore virtual com base no novo estado                          |
| 3. Diff e Reconciliação             | React compara a nova V-DOM com a anterior e identifica mudanças reais                 |
| 4. DOM real é atualizado            | Só as partes necessárias são atualizadas no DOM real                                  |
| 5. Navegador atualiza a render tree | O navegador refaz a renderização com base nas mudanças aplicadas ao DOM               |
| 6. Repaint/Reflow                   | O navegador redesenha a interface (repaint) e recalcula layout (reflow) se necessário |

---

## 🎯 Resumo Final

* **DOM:** Representa a estrutura real da página.
* **Virtual DOM:** Cópia leve em memória usada para otimizar atualizações.
* **Árvore de Renderização:** Usada pelo navegador para exibir o que aparece na tela.

Eles trabalham juntos para garantir uma **UI rápida, eficiente e reativa** ao usuário.

---

## 📍 **Quando o Diffing e Reconciliation acontecem?**

Esses dois processos ocorrem **automaticamente** **toda vez que o estado (`state`) ou as props de um componente mudam**.

### ✅ Exatamente **quando você chama `setState` (ou `useState`, `useReducer`, etc.)**:

1. O React **detecta que algo mudou** no componente.
2. Ele **gera uma nova Virtual DOM** com base no novo estado.
3. A nova Virtual DOM é **comparada com a anterior** → *isso é o processo de `diffing`*.
4. O React então **decide o que precisa ser alterado no DOM real** → *isso é o processo de `reconciliation`*.

---

## 🔍 O que é exatamente o **Diffing**?

> **Diffing** é o processo de **comparar duas árvores de Virtual DOM**: a antiga (antes da atualização) e a nova (após `setState` ou `props` mudarem).

### Como funciona:

* O React usa um **algoritmo eficiente (O(n))** que:

  * Compara **nós do mesmo nível** na árvore.
  * **Identifica elementos com a mesma `key`** (em listas).
  * Verifica **tipo de elemento (div, p, componente X, etc.)**.
  * Se o tipo for diferente, **recria o nó completamente**.
  * Se for o mesmo tipo, **compara props e filhos recursivamente**.

---

## 🔄 O que é exatamente a **Reconciliation**?

> **Reconciliation** é o processo onde o React **aplica as diferenças (diff)** encontradas ao **DOM real**, de maneira **eficiente e mínima**.

### O que ele faz:

* Atualiza apenas o que mudou.
* Remove, cria ou reordena elementos no DOM real **sem recarregar tudo**.
* **Evita o custo alto** de recriar toda a interface sempre que o estado muda.

---

## 📦 Exemplo prático com React

```tsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Você clicou {count} vezes</p>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
    </div>
  );
}
```

### O que acontece ao clicar no botão:

1. `setCount` é chamado → muda o estado.
2. React reexecuta o componente → gera nova V-DOM.
3. **Diffing**: compara o novo `<p>Você clicou 1 vezes</p>` com o antigo `<p>Você clicou 0 vezes</p>`.
4. **Reconciliation**: percebe que apenas o texto do `<p>` mudou → **altera apenas esse texto no DOM real**.
5. O botão e a `div` permanecem **inalterados no DOM real**.

---

## ✅ Resumo

| Conceito           | O que é                                                         | Quando ocorre                                         |
| ------------------ | --------------------------------------------------------------- | ----------------------------------------------------- |
| **Diffing**        | Comparação entre antiga e nova Virtual DOM                      | Após `setState`, mudança de props ou `forceUpdate()`  |
| **Reconciliation** | Aplicação das diferenças no DOM real com eficiência             | Imediatamente após o `diffing`                        |
| **Disparo**        | Quando há mudança de **estado ou props** em qualquer componente | Sempre que `useState`, `props`, `context`, etc. mudam |

---

Os dois pilares que tornaram o React tão popular e eficiente são exatamente:

---

### ✅ 1. **Componentização**

* **O que é:** A ideia de dividir a interface do usuário em pequenas partes independentes e reutilizáveis chamadas **componentes**.
* **Por que é importante:**

  * Permite organizar o código de forma mais clara e modular.
  * Facilita o desenvolvimento, manutenção e testes.
  * Cada componente tem sua **lógica, estilo e visual encapsulados**.

📌 *Exemplo:* Um botão, um card, um formulário — todos podem ser componentes separados e reutilizados em vários lugares da aplicação.

---

### ✅ 2. **Reutilização**

* **O que é:** Uma vez que você cria um componente, você pode usá-lo quantas vezes quiser, passando apenas props diferentes.
* **Por que é importante:**

  * Reduz duplicação de código.
  * Aumenta a produtividade.
  * Torna o sistema mais consistente visualmente.

📌 *Exemplo:* Um componente `<Button variant="primary" />` pode ser usado em várias telas com diferentes textos e comportamentos, sem reescrever a lógica.

---

### Outros conceitos fundamentais que se somam a esses:

| Conceito                     | Papel                                                                   |
| ---------------------------- | ----------------------------------------------------------------------- |
| **Virtual DOM**              | Melhora a performance ao atualizar apenas o necessário na tela          |
| **Unidirectional Data Flow** | Facilita o rastreamento de dados e debug                                |
| **Hooks**                    | Trazem mais poder e organização ao código funcional                     |
| **JSX**                      | Sintaxe que mistura HTML com JavaScript, tornando a UI mais declarativa |

Componentes atômicos no React fazem parte de uma **metodologia de design e desenvolvimento de interfaces chamada Atomic Design**, criada por Brad Frost. Essa abordagem organiza os componentes da interface em uma hierarquia de complexidade crescente, inspirada na estrutura da matéria (átomos → moléculas → organismos → templates → páginas).

No React (ou qualquer framework de componente), essa ideia é aplicada para criar **interfaces reutilizáveis, modulares e escaláveis**.

---

### 🧪 **Níveis de Componentes Atômicos**

#### 1. **Átomos**

* São os **componentes mais básicos e indivisíveis**.
* Exemplos: botão (`<Button />`), campo de input (`<Input />`), ícone (`<Icon />`), cor, tipografia.
* Eles não têm lógica complexa nem dependem de outros componentes.

```jsx
// Atom: Botão
export function Button({ children, onClick }) {
  return <button onClick={onClick}>{children}</button>;
}
```

---

#### 2. **Moléculas**

* **Composição de dois ou mais átomos**.
* Exemplo: um campo de busca que contém um `<Input />` + `<Button />`.
* Têm um pouco mais de lógica e estrutura, mas ainda são simples.

```jsx
// Molécula: Campo de busca
export function SearchField() {
  return (
    <div>
      <Input placeholder="Buscar..." />
      <Button>Pesquisar</Button>
    </div>
  );
}
```

---

#### 3. **Organismos**

* **Composição de moléculas e/ou átomos**, formando blocos funcionais mais complexos.
* Exemplo: um card de produto com imagem, título, botão e preço.

```jsx
// Organismo: Card de Produto
export function ProductCard({ product }) {
  return (
    <div>
      <img src={product.image} alt={product.title} />
      <h3>{product.title}</h3>
      <p>{product.price}</p>
      <Button>Comprar</Button>
    </div>
  );
}
```

---

#### 4. **Templates**

* Estrutura geral da página com **organismos posicionados**, mas ainda sem dados reais.
* Define a disposição dos componentes na tela.

---

#### 5. **Páginas**

* É um **template com dados reais**.
* Representa a renderização final do que o usuário verá.

---

### ✅ **Vantagens de usar Atomic Design no React**

* Maior **reutilização de código**.
* Interface mais **organizada e padronizada**.
* Facilita testes, manutenção e escalabilidade do projeto.

Ótima pergunta, Samuel!

A resposta é: **nem todo componente React é *atômico* no sentido do Atomic Design**, mas **todo componente React pode ser classificado dentro da hierarquia atômica** (átomo, molécula, organismo, etc.).

---

### 💡 Vamos esclarecer:

#### ✅ **Todo componente React é um "componente", mas não necessariamente um "componente atômico".**

O termo **“componente atômico”** se refere **especificamente àqueles que representam os elementos mais básicos** da interface, como um botão, input ou texto.

#### ❌ Por outro lado:

Um componente React pode ser complexo e composto por vários outros componentes dentro — nesse caso, ele se classificaria como **molécula**, **organismo**, **template** ou **página**, não como um átomo.

---

### 🧬 Exemplo prático:

| Componente React      | Classificação no Atomic Design |
| --------------------- | ------------------------------ |
| `<Button />`          | Átomo                          |
| `<InputWithLabel />`  | Molécula                       |
| `<UserProfileCard />` | Organismo                      |
| `<DashboardLayout />` | Template                       |
| `<DashboardPage />`   | Página                         |

---

### ✅ Conclusão:

* **Nem todo componente React é atômico**, mas **qualquer componente pode ser categorizado** de acordo com a ideia de Atomic Design.
* Essa categorização **não é obrigatória**, mas **ajuda a manter o projeto mais organizado e reutilizável**, principalmente em projetos grandes.

No React, a principal diferença entre **formulários *controlled* e *uncontrolled*** está na forma como os dados dos inputs são gerenciados:

---

### ✅ **Controlled Components (Controlados)**

* **O estado dos inputs é controlado pelo React.**
* Você **usa `useState` ou outro hook** para armazenar e atualizar o valor do input.
* O valor do campo é sempre determinado pela **props `value`** e atualizado pelo **evento `onChange`**.

#### Exemplo:

```tsx
import { useState } from 'react';

function FormControlled() {
  const [nome, setNome] = useState('');

  return (
    <input
      type="text"
      value={nome}
      onChange={(e) => setNome(e.target.value)}
    />
  );
}
```

* A cada tecla digitada, o valor é atualizado no `useState`.
* Isso permite **validação em tempo real**, máscaras, formatação etc.

---

### ✅ **Uncontrolled Components (Não Controlados)**

* O estado dos inputs é controlado **pelo próprio DOM** (como em HTML puro).
* Você acessa o valor do input usando uma **ref (`useRef`)** apenas quando necessário (ex: envio do formulário).

#### Exemplo:

```tsx
import { useRef } from 'react';

function FormUncontrolled() {
  const inputRef = useRef<HTMLInputElement>(null);

  const handleSubmit = () => {
    alert(inputRef.current?.value); // acessa o valor só no envio
  };

  return (
    <>
      <input type="text" ref={inputRef} />
      <button onClick={handleSubmit}>Enviar</button>
    </>
  );
}
```

* O React **não se importa com o valor do campo até você precisar dele**.
* Útil para formulários simples ou grandes, onde performance é uma preocupação.

---

### ⚖️ Comparativo

| Aspecto                 | Controlled                      | Uncontrolled                     |
| ----------------------- | ------------------------------- | -------------------------------- |
| Controle                | React                           | DOM                              |
| Leitura do valor        | `useState` / props              | `useRef().current.value`         |
| Validação em tempo real | Sim                             | Mais difícil                     |
| Reatividade             | Alta (cada digitação re-render) | Baixa                            |
| Complexidade            | Maior                           | Menor                            |
| Casos ideais            | Forms com lógica ou validação   | Forms simples ou com muitas refs |

---
Sim! Vamos analisar a diferença entre as duas formas que você mostrou nos botões `onClick`.

---

### 📌 Primeira forma:

```jsx
<button onClick={setLikeCount(likeCount + 1)} ...>
```

#### ❌ Problema:

Essa forma **chama imediatamente** a função `setLikeCount(likeCount + 1)` **durante a renderização do componente**, **não** quando o botão é clicado. Ou seja, o `likeCount` será incrementado assim que o componente for renderizado na tela, e **não ao clique**.

#### 🧠 Por quê?

Porque o React está executando o conteúdo dentro das chaves `{}`, e como você passou uma chamada de função (`setLikeCount(...)`) diretamente, ela será executada na hora, e o retorno dessa função será o que o `onClick` recebe (geralmente `undefined`).

---

### ✅ Segunda forma (correta):

```jsx
<button onClick={() => setLikeCount(likeCount + 1)} ...>
```

#### ✔️ Correto:

Aqui você está passando **uma função anônima** (`() => ...`) para o `onClick`. Ou seja, o React não executa nada imediatamente — ele apenas registra que, **quando o botão for clicado**, essa função deve ser executada.

---

### 🔄 Em resumo:

| Forma                               | Executa ao renderizar? | Executa ao clicar? | Correta para `onClick`? |
| ----------------------------------- | ---------------------- | ------------------ | ----------------------- |
| `onClick={setLikeCount(...)}`       | ✅ Sim                  | ❌ Não              | ❌ Não                   |
| `onClick={() => setLikeCount(...)}` | ❌ Não                  | ✅ Sim              | ✅ Sim                   |

---

Se quiser uma versão ainda mais segura ao trabalhar com valores antigos de `likeCount`, pode usar a versão com função de atualização:

```jsx
onClick={() => setLikeCount(prev => prev + 1)}
```

Isso evita problemas de sincronização se várias atualizações forem enfileiradas.

---

No React (e no JavaScript em geral, já que isso é uma funcionalidade do **ESModules**), existem dois tipos principais de exportações: **exportações nomeadas (named exports)** e **exportação padrão (default export)**. Vamos entender cada uma:

---

## ✅ 1. Exportação Nomeada (`named export`)

Com **exportação nomeada**, você exporta múltiplas coisas por nome e precisa importá-las **usando exatamente o mesmo nome**.

### Exemplo:

```tsx
// utils.js
export const somar = (a, b) => a + b;
export const subtrair = (a, b) => a - b;
```

### Para importar:

```tsx
import { somar, subtrair } from './utils';
```

> Você deve usar os mesmos nomes (`somar`, `subtrair`).
> Também é possível renomear na importação com `as`:

```tsx
import { somar as add } from './utils';
```

---

## ✅ 2. Exportação Padrão (`default export`)

A **exportação padrão** exporta **uma única entidade principal** do arquivo — pode ser uma função, classe, objeto, etc.
Você pode **dar qualquer nome** ao importar.

### Exemplo:

```tsx
// MeuComponente.jsx
export default function MeuComponente() {
  return <div>Hello!</div>;
}
```

### Para importar:

```tsx
import MeuComponente from './MeuComponente';
```

> Aqui, `MeuComponente` é o nome escolhido na importação — você poderia usar qualquer outro nome:

```tsx
import OutroNome from './MeuComponente';
```

---

## 🔁 Usar ambos no mesmo arquivo?

Sim, é possível combinar os dois:

```tsx
// math.js
export default function multiplicar(a, b) {
  return a * b;
}

export const somar = (a, b) => a + b;
```

Importação:

```tsx
import multiplicar, { somar } from './math';
```

---

## 🚨 Qual usar no React?

### 🔹 `export default`

Geralmente usado para **componentes React**:

```tsx
export default function Botao() {
  return <button>Ok</button>;
}
```

### 🔹 `export named`

Muito usado para **utilitários, hooks, contextos, constantes**, etc.:

```tsx
export const useAuth = () => { /* ... */ };
export const AuthContext = createContext();
```

---

## ✅ Resumo

| Tipo             | Exportação                          | Importação                | Pode renomear? |
| ---------------- | ----------------------------------- | ------------------------- | -------------- |
| Nomeada          | `export const x = ...`              | `import { x } from '...'` | ✅ com `as`     |
| Padrão (default) | `export default function X() {...}` | `import X from '...'`     | ✅              |

---

Ótima pergunta, Samuel! Usar o índice (`index`) como chave (`key`) ao fazer um `.map()` em React **funciona**, mas **não é o ideal** na maioria dos casos. Aqui vai uma explicação direta, com exemplos e as razões técnicas:

---

## 🚫 Por que não é ideal usar o `index` como `key` no `.map()`?

### 💥 1. **Problemas na Reconciliação do React**

O React usa a `key` para identificar quais itens mudaram, foram adicionados ou removidos. Se você usar o `index`, ele pode **associar um item errado a um componente** ao reordenar ou atualizar a lista.

#### Exemplo do problema:

```tsx
const [items, setItems] = useState(["A", "B", "C"]);

return (
  <ul>
    {items.map((item, index) => (
      <li key={index}>{item}</li> // 👈 usando o index
    ))}
  </ul>
);
```

Se você reordenar os itens:

```tsx
setItems(["C", "B", "A"]);
```

O React pode **preservar os componentes antigos** nos mesmos índices, o que pode gerar **comportamentos incorretos** se eles tiverem estado interno, animações, inputs, etc.

---

### 😵 2. **Comportamento inesperado em componentes com estado**

Imagine que cada item da lista seja um input controlado. Ao mudar a ordem da lista, os valores dos inputs podem se manter **associados ao índice antigo**, gerando confusão para o usuário.

---

## ✅ Quando é seguro usar o `index` como `key`?

* A lista **nunca será reordenada, filtrada ou alterada**.
* Os itens da lista são **estáticos** (ex: lista de dias da semana).
* O conteúdo é **imutável** durante o ciclo de vida do componente.

---

## ✅ Qual alternativa usar?

Se possível, use uma `id` única do próprio item:

```tsx
{items.map(item => (
  <li key={item.id}>{item.name}</li>
))}
```

Se não houver um `id`, crie um com `uuid`, `nanoid`, ou outro gerador de IDs confiável.

---

## Resumo

| Critério                 | Usar `index` como key |
| ------------------------ | --------------------- |
| Lista mutável / dinâmica | ❌ Não recomendado     |
| Lista estática / fixa    | ✅ Aceitável           |
| Componentes com estado   | ❌ Pode gerar bugs     |
| Reordenação ou remoção   | ❌ Pode quebrar render |

---

No **React**, **imutabilidade** refere-se ao princípio de **não modificar diretamente os dados originais (como objetos ou arrays), mas sim criar e usar cópias atualizadas desses dados**.

---

### 📌 Por que a imutabilidade é importante no React?

1. **Detecção de mudanças eficiente:**
   O React utiliza a comparação de estados (shallow comparison) para decidir quando precisa renderizar novamente. Se você **modifica um objeto diretamente**, o React **não detecta** que ele mudou, porque a referência continua a mesma.

2. **Previsibilidade do estado:**
   Ao evitar mutações, o fluxo de dados se torna mais claro e previsível, o que reduz bugs e efeitos colaterais.

3. **Facilidade de debug e teste:**
   Com dados imutáveis, é mais fácil rastrear e reverter estados anteriores.

---

### 📦 Exemplo de mutável (❌ errado):

```jsx
const [list, setList] = useState([1, 2, 3]);

// Mutando diretamente
list.push(4);
setList(list); // ❌ Isso não vai disparar uma nova renderização!
```

---

### ✅ Exemplo imutável (✔️ correto):

```jsx
const [list, setList] = useState([1, 2, 3]);

// Criando nova referência
const newList = [...list, 4];
setList(newList); // ✔️ React detecta a mudança e re-renderiza
```

---

### Imutabilidade com objetos:

```jsx
const [user, setUser] = useState({ name: 'Samuel', age: 25 });

// Atualizar o nome imutavelmente
setUser(prev => ({ ...prev, name: 'Carlos' }));
```

---

### 🧠 Dica:

Usar funções puras (que não modificam dados originais) ajuda a manter a imutabilidade, como `map`, `filter`, `concat`, `slice` (em vez de `push`, `splice`, etc).

---

Claro, Samuel! O hook `useRef` no React é uma ferramenta poderosa com **três** usos principais. Vou te explicar de forma clara, prática e com exemplos:

---

### ✅ O que o `useRef` faz?

O `useRef` cria um **objeto mutável** que persiste entre renderizações, sem causar uma nova renderização quando alterado.

Ele retorna um objeto assim:

```tsx
const minhaRef = useRef(valorInicial);
```

Esse objeto tem a propriedade `.current`, que é onde o valor fica armazenado:

```tsx
minhaRef.current // acessa o valor
```

---

### 🧠 Para que serve?

#### 1. **Referenciar elementos do DOM (como input, div etc)**

Quando você quer acessar diretamente um elemento HTML (como faria com `document.getElementById`), sem perder a abordagem declarativa do React.

```tsx
import { useRef } from 'react';

function MeuComponente() {
  const inputRef = useRef<HTMLInputElement>(null);

  const focarInput = () => {
    inputRef.current?.focus();
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={focarInput}>Focar no input</button>
    </>
  );
}
```

---

#### 2. **Armazenar valores que não precisam causar re-render**

Ideal para armazenar contadores, flags, valores anteriores etc.

```tsx
function Timer() {
  const segundos = useRef(0);

  useEffect(() => {
    const intervalo = setInterval(() => {
      segundos.current += 1;
      console.log('Segundos:', segundos.current);
    }, 1000);

    return () => clearInterval(intervalo);
  }, []);

  return <p>Olhe o console</p>;
}
```

> Como `useRef` **não causa re-render**, é ótimo para variáveis internas de controle.

---

#### 3. **Guardar valores anteriores**

Você pode usar `useRef` para guardar o valor de uma prop ou state anterior.

```tsx
function Exemplo({ contador }: { contador: number }) {
  const valorAnterior = useRef<number>();

  useEffect(() => {
    valorAnterior.current = contador;
  }, [contador]);

  return (
    <div>
      <p>Atual: {contador}</p>
      <p>Anterior: {valorAnterior.current}</p>
    </div>
  );
}
```

---

### 📌 Quando usar?

* Precisa **manipular elementos do DOM** diretamente (ex: foco, scroll, medida).
* Quer armazenar **valores entre renderizações** sem re-render.
* Precisa **acessar o valor anterior** de um `state` ou `prop`.
* Evitar criar **variáveis globais** ou fora do React (mantém no fluxo do React).

---
