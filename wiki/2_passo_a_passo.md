# Calculadors: passo a passo

## 1. Criar o componente principal

### Criar o diretório .src/components/calculator

Dentro do diretório criado, criar os arquivos:
- 'Calculator.jsx', onde vai ficar o componente principal, e 
- 'Calculator.css', com o respectivo código css.

### Criando o componente

Esse é o código básico para criação do componente, em 'Calculator.jsx':

```javascript
import React, { Component } from 'react'
import './Calculator.css'

export default class Calculator extends Component {
  render() {
    return (
      <div className="calculator">

      </div>
    )
  }
}
```

## 2. Colocar o componente na página inicial do App

Em `src/index.js` importar o componente:

```javascript
import Calculator from './components/Calculator';
```

Em `src/index.js`, substituir o <App /> padrão de fábrica por <Calculator />:

```diff
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
-    <App />
+    <Calculator />
  </React.StrictMode>
);
```

## 3. Instalando uma fonte para o app

Criar uma pasta `fonts` em `src/`
Nela, colocar o arquivo .ttf da fonte escolhida.

Em `src/index.css`, limpar as configurações iniciais e adicionar o código para utilizar a fonte no app:

```javascript
@font-face {
  font-family: 'RobotoMono';
  src: url('./fonts/RobotoMono-Thin.ttf')
}

* {
  font-family: 'RootoMono', monospace;
} 
```

Asterístico (*) é o seletor universal, pra utilizar a fonte em todos os componentes

## 4. Criando o componente botão

Criar o arquivo `Button.jsx` e `Button.css` em `src/components`

Em `Button.jsx`, colocar o código inicial para criação do componente e exibição do mesmo na tela:

```javascript
import React from 'react'
import './Button.css'

export default props =>
  <button className='button'>{props.label}</button>
```
O componente botão vai receber informações futuramente (props).

Agora no arquivo `Calculator.jsx` é preciso importar o componente:

```javascript
import Button from './Button'
```

E podemos inserir o botão dentro do componente Calculator.

```javascript
export default class Calculator extends Component {
  render() {
    return (
      <div className="calculator">
        <Button label="AC" />
      </div>
    )
  }
}
```

## 5. Organizando os botões em um grid

No arquivo `Calculator.css`, na classe `.calculator`, adicionar as propriedades:

```css
  display: grid;
  grid-template-columns: repeat(4, 25%);
  grid-template-rows: 25% 15% 15% 15% 15% 15%;
```

em grid-template-columns configuramos a largura das colunas
em grid-template-rows configuramos a altura das linhas

A minha calculadora terá 6 linhas. A primeira linha (25%) ficará com o display, e as outras linhas ficarão com os botões (15%).

## 6. Criando o componente Display

Criar o arquivo `Display.jsx` e `Display.css` em `src/components`

Em `Display.jsx`, colocar o código inicial para criação do componente e exibição do mesmo na tela:

```javascript
import React from 'react'
import './Display.css'

export default props =>
  <button className='display'>{props.value}</button>
```
O componente display vai receber informações futuramente (props).

Agora no arquivo `Calculator.jsx` é preciso importar o componente:

```javascript
import Display from './Display'
```

E podemos inserir o componente 'Display' dentro do componente Calculator.

```javascript
export default class Calculator extends Component {
  render() {
    return (
      <div className="calculator">
        <Display value={100} />
        <Button label="AC" />
        <Button label="/" />
        <Button label="7" />
        <Button label="8" />
        <Button label="9" />
        <Button label="*" />
        <Button label="4" />
        <Button label="5" />
        <Button label="6" />
        <Button label="-" />
        <Button label="1" />
        <Button label="2" />
        <Button label="3" />
        <Button label="+" />
        <Button label="0" />
        <Button label="." />
        <Button label="=" />
      </div>
    )
  }
}
```

## 7. Exibindo o display

Para exibir o Display corretamente, precisamos alterar o arquivo Display.css para ele ocupar as 4 colunas na primeira linha.

```css
.display {
  grid-column: span 4;
}
```

## 8. Modificando as propriedades dos botões

Como na calculadora temos diferentes tipos de botão e podemos querer personalizar o espaço que cada botão vai ocupar na linha, precisamos adicionar classes css para essas exibições diferentes.

Em `Button.css`, adicionar, por exemplo:

```css
.button.double {
  grid-column: span 2;
}

.button.triple {
  grid-column: span 3;
}

.button.operation {
  background-color: pink;
  color: white;
}

.button.operation:active {
  background-color: purple;
  color: white;
}
```

Em `Button.jsx` podemos escrever um código javascript para manipular a escrita da classe dentre outras formas:

```javascript
export default props =>
  <button className={`
    button
    ${props.operation ? 'operation' : ''}
    ${props.double ? 'double' : ''}
    ${props.double ? 'triple' : ''}
  `}>
    {props.label}
  </button>
```

E atualizar em `Calculator.jsx` as propriedades dos componentes:

```javascript
export default class Calculator extends Component {
  render() {
    return (
      <div className="calculator">
        <Display value={100} />
        <Button label="AC" triple/>
        <Button label="/" operation/>
        <Button label="7" />
        <Button label="8" />
        <Button label="9" />
        <Button label="*" operation/>
        <Button label="4" />
        <Button label="5" />
        <Button label="6" />
        <Button label="-" operation/>
        <Button label="1" />
        <Button label="2" />
        <Button label="3" />
        <Button label="+" operation/>
        <Button label="0" double/>
        <Button label="." />
        <Button label="=" operation/>
      </div>
    )
  }
}
```

## 9. Adicionando funções aos botões

```javascript
export default class Calculator extends Component {

  constructor(props) {
    super(props)
    this.clearMemory = this.clearMemory.bind(this)
    this.setOperation = this.setOperation.bind(this)
    this.addDigit = this.addDigit.bind(this)
  }

  clearMemory() {}

  setOperation() {}

  addDigit() {}
...
```
Nos botões, vamos adicionar o código para quando o botão for clicado, a função ser exectuada, por exemplo:

```javascript
  <Button label="AC" triple click={this.ClearMemory}/>
```

### Função apagar ou AC

### Função adicionar dígito

### Função operação