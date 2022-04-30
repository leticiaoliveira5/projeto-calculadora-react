# Calculadora: passo a passo

## 1. Criar o componente principal

### Criar o diretório `src/components`

Dentro do diretório criado, criar os arquivos:

- `Calculator.jsx`, onde vai ficar o componente principal, e

- `Calculator.css`, com o respectivo código css.

### Criando o componente

Esse é o código básico para criação do componente, em `Calculator.jsx`:

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

Em `src/index.js`, substituir o `<App />` padrão de fábrica por `<Calculator />`:

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

Criar os arquivos `Button.jsx` e `Button.css` em `src/components`

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

- em _grid-template-columns_ configuramos a largura das colunas

- em _grid-template-rows_ configuramos a altura das linhas

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
export default props => {
  let classes = 'button '
  classes += props.operation ? 'operation' : ''
  classes += props.double ? 'double' : ''
  classes += props.triple ? 'triple' : ''

  return (
    <button 
      className={classes}>
      {props.label}
    </button>
  )
}
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

No componente Calculator, adicionar funções que serão chamadas ao clicar os botões

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

No componente Button, temos que preencher o onClick com a propriedade click que será enviada do componenete:

```javascript
    <button 
      onClick={e => props.click(props.label)}
      className={classes}>
      {props.label}
    </button>
```

Nos botões, vamos adicionar o código para quando o botão for clicado a função ser exectuada, por exemplo:

```javascript
  <Button label="AC" click={this.ClearMemory} triple/>
```

### Função AC

Esta função vai apagar a memória da calculadora e voltá-la para o estado inicial.

Para isso, podemos criar um estado inicial default para a calculadora.

Esse código fica depois dos _imports_ no arquivo do componente Calculator (`Calculator.jsx`)

```javascript
const initialState = {
  displayValue: '0',
  clearDisplay: false,
  operation: null,
  values: [0, 0],
  current: 0
}
```

E dentro do componente em si, podemos adicionar o `state`:

```javascript
export default class Calculator extends Component {

  state = { ...initialState }

...
```

Então preenchemos o método para aplicar na calculadora o seu estado inicial:

```javascript
  clearMemory() {
    this.setState({ ...initialState })
  }
```

E atualizar o componente para exibir o resultado da função na tela:

```javascript
  render() {
    return (
      <div className="calculator">
        <Display value={this.state.displayValue} />
        ...
```

### Função adicionar dígito

Essa função vai estabelecer a lógica da digitação dos algarismos.

Nos botões de números, adicionamos a função desta forma:

```javascript
  <Button label="7" click={this.addDigit} />
```

A função pode ser feita desta forma:

```javascript
  addDigit(n) {
    if (n === '.' && this.state.displayValue.includes('.')) { 
      return
    }

    const clearDisplay = (this.state.displayValue === '0') || this.state.clearDisplay
    const currentValue = clearDisplay ? '' : this.state.displayValue
    const displayValue = currentValue + n
    this.setState({ displayValue, clearDisplay: false })

    if (n !== '.') {
      const i = this.state.current
      const newValue = parseFloat(displayValue)
      const values = [...this.state.values]
      values[i] = newValue
      this.setState({ values })
    }
  }
```

### Função operação

Essa função deve estabelecer a operação que será feita com os números digitados.

Nos botões de símbolos de operações, adicionamos a função desta forma:

```javascript
  <Button label="-" click={this.setOperation} operation />
```

A função pode ser feita desta forma:

```javascript
  setOperation(operation) {
    if (this.state.current === 0) {
       this.setState({ operation, current: 1, clearDisplay: true })
    } else {
      const equals = (operation === '=')
      const currentOperation = this.state.operation
      const values = [...this.state.values]
      try {
        values[0] = eval(`${values[0]} ${currentOperation} ${values[1]}`)
      } catch(e) {
        values[0] = this.state.values[0]
      }
      values[1] = 0

      this.setState({
        displayValue: values[0],
        operation: equals ? null : operation,
        current: equals ? 0 : 1,
        clearDisplay: !equals,
        values
      })
    }
  }
```
