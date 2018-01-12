# Criando componentes realmente reutilizaveis com React

Quando estamos trabalhando com React o principio da REUSABILIDADE se torna praticamente um mantra para ter em mente durante todo o desenvolvimento, isso porque ganhamos muito em produtividade e escalabilidade da aplicação além de outros beneficios. Componentes reutilizáveis são a chave para um projeto bem construido em React, devemos pensar no menor e mais generico componente possível para que a partir dele, os componentes maiores e mais complexos sejam construídos.

### SmallComponent + AnotherSmallComponent = BigComponent

Vamos pensar numa tela de login, que deve possuir os campos e-mail, senha e um botão "Fazer Login". Neste caso quando 'quebramos' nosso componente maior (que é esse formulário de login) em pequenas partes, chegaremos nos dois menores componentes: um input e um button.

#### Input

```js
import React from 'react'

import PropTypes from 'prop-types'

import TextField from 'material-ui/TextField'

/* aqui importamos os modulos necessarios, neste caso estou usando o Material UI que são componentes React com a estilização do Google Material Design.*/

const Input = ({ hintText, type }) => <TextField
  hintText={hintText}
  type={type} />
  
/* Percebam que não estamos utilizando esses componentes como classes, uma vez que são dumb components (componentes que não precisarão trabalhar com o state do react), logo eles irão se comportar exatamente da maneira esperada. No nosso caso temos duas propriedades que serão recebidas na chamada do componente, o hintText que é uma espécie de placeholder e o type que é o tipo do input(texto ou senha).
*/

const { string } = PropTypes

Input.propTypes = {
  hintText: string.isRequired,
  type: string.isRequired,
}

/* Aqui está uma coisa importante e as vezes negligenciada por muitos, a checagem de tipos com prop-types, sem ela nós não sabemos quais props esperar, se são necessarias na construção do componente e de que tipos elas serão, imaginem se nesse componente input dessemos uma função ao invés de texto no nosso hintText, quais erros teriamos e como saberiamos disso numa aplicação complexa? Bom, além desses detalhes ela serve também como documentação para sua aplicação, ao lermos essa parte do codigo sabemos automaticamente o que esperar do componente.
*/

export default Input
```

#### Button

```js
import React from 'react'

import PropTypes from 'prop-types'

import RaisedButton from 'material-ui/RaisedButton'

const Button = ({ label, primary, secondary, onClick }) => <RaisedButton
  label={label}
  primary={primary}
  secondary={secondary}
  onClick={onClick} />

const { string, bool, func } = PropTypes

Button.propTypes = {
  label: string.isRequired,
  primary: bool.isRequired,
  secondary: bool,
  onClick: func.isRequired,
}

export default Button
```
E a construção de um componente maior ficaria assim:

```js
import React, { Component } from 'react'

import Input from '../components/input'
import Button from '../components/button'

class Home extends Component {
  constructor(props) {
    super(props)
    this.handleLogin = this.handleLogin.bind(this)
  }

  handleLogin() {
    console.log('clicou')
  }

  render() {
    return (
      <div>
        <Input
          hintText="E-mail"
          type="text"
        />
        <Input
          hintText="Senha"
          type="password"
        />
        <Button
          label="Fazer Login"
          primary={true}
          onClick={this.handleLogin}
        />
      </div>
    )
  }
}

export default Home
```

Temos nosso exemplo de código reutilizável seguindo o 'mantra' de criação de componentes em React, então cada vez que precisarmos utilizar um desses componentes em nosso projeto não teremos que reescrever código,sendo assim nossa aplicação fica extremamente escalável e como dito antes ganhamos muito na produtividade além de um código confiável.
