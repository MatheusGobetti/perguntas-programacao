# Perguntas de Programação | Front-end

## 1. O que é SQL injection?

Resposta: A injeção de SQL é um tipo de ataque cibernético usado para explorar vulnerabilidades em sistemas de gerenciamento de banco de dados. Basicamente, os hackers inserem códigos maliciosos em campos de entrada, como formulários da web, para enganar o sistema e obter acesso não autorizado ao banco de dados.

Por exemplo, imagine um site de login onde você digita seu nome de usuário e senha. Se o site não tiver proteção adequada contra injeção de SQL, um invasor poderia digitar algo como " ' OR '1'='1" no campo de senha. Isso pode enganar o sistema, fazendo com que ele interprete a entrada como uma parte legítima da consulta SQL, permitindo ao invasor acessar o sistema sem a senha correta.

Em resumo, a injeção de SQL é como um "truque" usado por hackers para enganar sistemas de banco de dados e obter acesso não autorizado ou manipular informações.

Para prevenir o SQL Injection e o XSS (Cross-Site-Scripting), você pode incluir na sua aplicação React uma biblioteca como a DOM Purify, assim como no exemplo abaixo:

```jsx
import React, { useState } from 'react';
import axios from 'axios';
import DOMPurify from 'dompurify';

function LoginForm() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();

    // Validar os campos de entrada
    if (!username || !password) {
      setError('Por favor, preencha todos os campos.');
      return;
    }

    try {
      // Sanitize os dados de entrada antes de enviá-los para o backend
      const sanitizedUsername = DOMPurify.sanitize(username);
      const sanitizedPassword = DOMPurify.sanitize(password);

      const response = await axios.post('http://example.com/login', {
        username: sanitizedUsername,
        password: sanitizedPassword
      });

      console.log(response.data);
    } catch (error) {
      console.error(error);
      setError('Erro ao realizar login. Por favor, tente novamente.');
    }
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input type="text" placeholder="Username" value={username} onChange={(e) => setUsername(e.target.value)} />
        <input type="password" placeholder="Password" value={password} onChange={(e) => setPassword(e.target.value)} />
        <button type="submit">Login</button>
      </form>
      {error && <p>{error}</p>}
    </div>
  );
}

export default LoginForm;

```

## 2. O que é escopo em JavaScript?

Resposta: Escopo em JavaScript se refere à visibilidade e acessibilidade de variáveis, funções e objetos em determinadas partes do código. Existem dois tipos principais de escopo em JavaScript: escopo global e escopo local.

Exemplo de escopo global:

```js
var globalVar = 10;

function myFunction() {
    console.log(globalVar); // Acesso à variável globalVar
}

myFunction(); // Saída: 10

```

Exemplo de escopo local:

```js
function myFunction() {
    var localVar = 20;
    console.log(localVar); // Acesso à variável localVar dentro do escopo da função
}

myFunction(); // Saída: 20
console.log(localVar); // Erro: localVar não está definido fora do escopo da função
```

## 3. Explique o CSS "box model" e os componentes de layout que o compõem.

Resposta: O "box model" do CSS é uma representação visual de como os elementos HTML são renderizados. Ele consiste em margem, borda, preenchimento e conteúdo.

Exemplo:

```css
.box {
    width: 200px;
    height: 100px;
    padding: 20px;
    border: 2px solid black;
    margin: 10px;
}
```

## 4. Como JavaScript e jQuery são diferentes?

Resposta: JavaScript é uma linguagem de programação, enquanto jQuery é uma biblioteca JavaScript que simplifica a manipulação de elementos HTML e interações com o DOM.

Javascript:

```js
document.getElementById("myElement").innerHTML = "Hello, world!";

```

JQuery:

```js
$("#myElement").html("Hello, world!");
```

## 5. O que é um Callback Hell?

Resposta: Callback Hell é uma situação em que várias chamadas de função assíncronas são aninhadas, resultando em um código difícil de ler e manter.

Exemplo:

```js
asyncFunction1(function() {
    asyncFunction2(function() {
        asyncFunction3(function() {
            // Mais código aqui...
        });
    });
});

```

## 6. O que é Cross-Site Scripting (XSS)?

Resposta: XSS é um tipo de vulnerabilidade de segurança em que um invasor injeta código malicioso em um site, permitindo que ele execute scripts não autorizados no navegador do usuário.

Exemplo de XSS:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Exemplo de XSS</title>
</head>
<body>
  <h1>Bem-vindo!</h1>
  <p>Olá, <span id="username"></span>!</p>

  <form>
    <label for="name">Nome:</label>
    <input type="text" id="name" name="name">
    <button type="button" onclick="displayUsername()">Enviar</button>
  </form>

  <script>
    function displayUsername() {
      var name = document.getElementById("name").value;
      document.getElementById("username").innerText = name;
    }
  </script>
</body>
</html>

```

Se um invasor inserir o seguinte como seu nome:
```html
<script>alert("XSS executado!");</script>

```
Quando esse código for exibido na página, ele será executado como um script malicioso, exibindo um alerta com a mensagem "XSS executado!". Isso é apenas um exemplo básico para ilustrar o conceito de XSS. Em situações reais, os ataques XSS podem ser muito mais complexos e perigosos.

## 7. O que é Flux?

Resposta: Flux é uma arquitetura de gerenciamento de estado unidirecional utilizada em aplicações JavaScript, especialmente em conjunto com o framework React.

Exemplo de Flux com uma loja de roupas:

1. Actions: Definimos uma ação para adicionar um item ao carrinho.

```jsx
// actions.js
export const ADD_TO_CART = 'ADD_TO_CART';

export function addToCart(item) {
  return {
    type: ADD_TO_CART,
    item
  };
}

```
2. Store: Mantém o estado da aplicação (carrinho de compras) e responde à ação de adicionar um item ao carrinho.

```jsx
// cartStore.js
import { EventEmitter } from 'events';
import dispatcher from './dispatcher';

class CartStore extends EventEmitter {
  constructor() {
    super();
    this.cartItems = [];
  }

  getCartItems() {
    return this.cartItems;
  }

  handleActions(action) {
    switch(action.type) {
      case 'ADD_TO_CART':
        this.cartItems.push(action.item);
        this.emit('change');
        break;
      default:
        // Nenhuma ação correspondente
    }
  }
}

const cartStore = new CartStore();
dispatcher.register(cartStore.handleActions.bind(cartStore));
export default cartStore;


```

3. Componentes React: Um componente de lista de produtos e um componente de carrinho de compras que despacha a ação de adicionar ao carrinho.

```jsx
// ProductList.js
import React from 'react';
import * as CartActions from './actions';
import cartStore from './cartStore';

class ProductList extends React.Component {
  handleAddToCart(item) {
    CartActions.addToCart(item);
  }

  render() {
    return (
      <div>
        <h2>Produtos Disponíveis</h2>
        <ul>
          <li>Camiseta - R$ 29,99 <button onClick={() => this.handleAddToCart('Camiseta')}>Adicionar ao Carrinho</button></li>
          <li>Calça - R$ 49,99 <button onClick={() => this.handleAddToCart('Calça')}>Adicionar ao Carrinho</button></li>
          <li>Blusa - R$ 39,99 <button onClick={() => this.handleAddToCart('Blusa')}>Adicionar ao Carrinho</button></li>
        </ul>
      </div>
    );
  }
}

export default ProductList;


```

```jsx
// Cart.js
import React from 'react';
import cartStore from './cartStore';

class Cart extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      cartItems: []
    };
  }

  componentDidMount() {
    cartStore.on('change', () => {
      this.setState({
        cartItems: cartStore.getCartItems()
      });
    });
  }

  render() {
    return (
      <div>
        <h2>Carrinho de Compras</h2>
        <ul>
          {this.state.cartItems.map((item, index) => (
            <li key={index}>{item}</li>
          ))}
        </ul>
      </div>
    );
  }
}

export default Cart;


```

Este exemplo demonstra uma loja de roupas simples, onde os usuários podem ver os produtos disponíveis e adicionar itens ao carrinho de compras. O estado do carrinho de compras é gerenciado pelo Flux e atualizado automaticamente quando os itens são adicionados.

O Redux é um flux?

Sim, o Redux é uma implementação do padrão Flux. O Redux foi projetado para resolver algumas das limitações percebidas no padrão Flux original, simplificando a arquitetura e tornando-a mais previsível.

Assim como no Flux, o Redux mantém um único estado da aplicação em um armazenamento centralizado (store) e define um fluxo unidirecional de dados. No entanto, o Redux simplifica a arquitetura, removendo a necessidade de múltiplos stores e introduzindo conceitos como reducers para atualizar o estado de forma mais previsível e imutável.

O Redux é frequentemente usado em conjunto com bibliotecas como React, mas é independente de qualquer framework específico. Ele pode ser usado com qualquer biblioteca ou estrutura JavaScript que suporte a ligação de dados unidirecional.

Então, enquanto o Redux não é o Flux original, é uma implementação do padrão Flux que ganhou popularidade significativa na comunidade de desenvolvimento web devido à sua simplicidade e eficácia.

## 8. O que é Sass?

Resposta: Sass é uma linguagem de extensão CSS que adiciona recursos como variáveis, mixins e aninhamento para facilitar a escrita e manutenção de estilos.

Exemplos:

1. Variáveis:
O Sass permite definir variáveis para armazenar valores que podem ser reutilizados em todo o seu código.

```scss
$primary-color: #3498db;
$secondary-color: #2ecc71;

.button {
  background-color: $primary-color;
  color: white;
}

.alert {
  background-color: $secondary-color;
  color: white;
}
```

2. Aninhamento:
O Sass permite aninhar regras CSS dentro de outras, o que pode tornar o código mais legível.

```scss
.container {
  width: 100%;
  padding: 20px;

  .header {
    font-size: 24px;
    margin-bottom: 10px;
  }

  .content {
    font-size: 16px;

    p {
      line-height: 1.5;
    }
  }
}

```

3. Mixins:
Mixins permitem reutilizar blocos de código em várias partes do seu arquivo Sass.

```scss
@mixin flexbox {
  display: flex;
  justify-content: center;
  align-items: center;
}

.box {
  width: 200px;
  height: 200px;
  @include flexbox;
}

```

4. Importações:
Você pode dividir seu código em vários arquivos Sass e importá-los conforme necessário.

```scss
// base.scss
$primary-color: #3498db;

// components.scss
.button {
  background-color: $primary-color;
  color: white;
}

```

```scss
// main.scss
@import 'base';
@import 'components';

```

Estes são apenas alguns exemplos básicos de Sass. Ele oferece muitos recursos poderosos, como herança, funções, operadores e muito mais, para tornar a escrita de estilos CSS mais eficiente e organizada.

## 9. O que é encapsulamento?

Resposta: Encapsulamento é um princípio fundamental da programação orientada a objetos que envolve a combinação de dados e comportamentos relacionados em uma única unidade chamada de classe. Esse conceito permite ocultar os detalhes internos de uma classe e restringir o acesso aos seus membros apenas às partes relevantes do programa, promovendo assim a segurança e a modularidade do código.

Exemplo em .NET Core:

```csharp
using System;

public class ContaBancaria
{
    private string titular;
    private decimal saldo;

    public ContaBancaria(string titular, decimal saldoInicial)
    {
        this.titular = titular;
        this.saldo = saldoInicial;
    }

    public decimal Saldo
    {
        get { return saldo; }
        private set { saldo = value; }
    }

    // Método para depositar dinheiro na conta
    public void Depositar(decimal valor)
    {
        if (valor > 0)
        {
            saldo += valor;
            Console.WriteLine($"Depósito de {valor:C} realizado com sucesso. Novo saldo: {saldo:C}");
        }
        else
        {
            Console.WriteLine("O valor do depósito deve ser maior que zero.");
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        ContaBancaria conta = new ContaBancaria("João", 1000.00m);
        Console.WriteLine($"Saldo inicial da conta de {conta.Saldo:C}");

        conta.Depositar(500.00m);
        Console.WriteLine($"Novo saldo da conta de {conta.Saldo:C}");
    }
}

```

Neste exemplo em C#, a classe ContaBancaria possui um campo privado saldo, que só pode ser acessado e modificado através da propriedade pública Saldo. Isso encapsula o campo saldo, permitindo o controle sobre como ele é acessado e alterado.

Exemplo em Node.Js:

```js
class ContaBancaria {
    constructor(titular, saldoInicial) {
        this.titular = titular;
        this._saldo = saldoInicial; // Atributo protegido, acessível dentro da classe e por subclasses
    }

    get saldo() {
        return this._saldo;
    }

    // Método para depositar dinheiro na conta
    depositar(valor) {
        if (valor > 0) {
            this._saldo += valor;
            console.log(`Depósito de ${valor} realizado com sucesso. Novo saldo: ${this._saldo}`);
        } else {
            console.log("O valor do depósito deve ser maior que zero.");
        }
    }
}

const conta = new ContaBancaria("Maria", 1000.00);
console.log(`Saldo inicial da conta de ${conta.saldo}`);

conta.depositar(500.00);
console.log(`Novo saldo da conta de ${conta.saldo}`);

```

Neste exemplo em JavaScript utilizando Node.js, a classe ContaBancaria tem um atributo protegido _saldo, que é acessado através da propriedade saldo. Isso encapsula o acesso ao saldo, permitindo a validação e o controle de acesso aos dados internos da classe.

## 10. Qual o ponto de se usar Redux?

Resposta: Redux é uma biblioteca de gerenciamento de estado que facilita o compartilhamento e atualização de dados em aplicações JavaScript de grande escala.

Suponha que você tenha uma aplicação de lista de tarefas. O estado da aplicação inclui uma lista de tarefas e um campo para adicionar uma nova tarefa.

Actions:

```js
// actions.js
export const ADD_TASK = 'ADD_TASK';

export function addTask(task) {
  return {
    type: ADD_TASK,
    task
  };
}


```

Reducer:

```js

// reducer.js
import { ADD_TASK } from './actions';

const initialState = {
  tasks: [],
};

export function taskReducer(state = initialState, action) {
  switch (action.type) {
    case ADD_TASK:
      return {
        ...state,
        tasks: [...state.tasks, action.task]
      };
    default:
      return state;
  }
}


```

Store:

```js

// store.js
import { createStore } from 'redux';
import { taskReducer } from './reducer';

const store = createStore(taskReducer);

export default store;


```

Componente React:

```js

// TodoComponent.js
import React, { useState } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { addTask } from './actions';

function TodoComponent() {
  const [newTask, setNewTask] = useState('');
  const tasks = useSelector(state => state.tasks);
  const dispatch = useDispatch();

  const handleChange = (event) => {
    setNewTask(event.target.value);
  };

  const handleSubmit = () => {
    dispatch(addTask(newTask));
    setNewTask('');
  };

  return (
    <div>
      <h1>Lista de Tarefas</h1>
      <ul>
        {tasks.map((task, index) => (
          <li key={index}>{task}</li>
        ))}
      </ul>
      <input type="text" value={newTask} onChange={handleChange} />
      <button onClick={handleSubmit}>Adicionar Tarefa</button>
    </div>
  );
}

export default TodoComponent;


```

Este é um exemplo básico de como você pode usar Redux em uma aplicação JavaScript. O Redux gerencia o estado da lista de tarefas, e o componente React pode despachar ação para adicionar uma nova tarefa à lista.

### 10.1. Por que usar Redux Toolkit ao invés de Redux?

Resposta: Usar Redux Toolkit em vez de Redux puro oferece várias vantagens:

1. **Sintaxe simplificada**: Redux Toolkit fornece um conjunto de utilitários que simplificam muitas tarefas comuns do Redux, como a criação de reducers e actions. Isso reduz a quantidade de código necessário e torna o desenvolvimento mais rápido e menos propenso a erros.

2. **Configuração padrão de fluxo de dados**: Redux Toolkit configura automaticamente o fluxo de dados para você, seguindo as melhores práticas recomendadas pela equipe do Redux. Isso inclui o uso de Immer para permitir atualizações imutáveis no estado, bem como a configuração automática do Redux DevTools Extension para facilitar a depuração.

3. **Performance otimizada**: Redux Toolkit inclui funcionalidades como memoization de selectors e empacotamento de ações (action batching) para melhorar a performance da sua aplicação Redux.

4. **Módulo de slice**: O conceito de "slice" no Redux Toolkit permite definir partes independentes do estado e suas operações associadas em um único local. Isso promove a modularidade e a reutilização do código.

5. **Compatibilidade com ferramentas de desenvolvimento**: Redux Toolkit é compatível com ferramentas populares de desenvolvimento, como Redux DevTools Extension e Redux Toolkit RTK Query para gerenciamento de estado assíncrono e consulta de API.

Em resumo, usar Redux Toolkit simplifica o desenvolvimento de aplicações Redux, reduzindo a complexidade e o boilerplate, ao mesmo tempo em que oferece um desempenho otimizado e compatibilidade com ferramentas de desenvolvimento essenciais.

Exemplo do Redux Toolkit:

Actions:

```jsx
// actions.js
import { createSlice } from '@reduxjs/toolkit';

const tasksSlice = createSlice({
  name: 'tasks',
  initialState: [],
  reducers: {
    addTask(state, action) {
      state.push(action.payload);
    },
  },
});

export const { addTask } = tasksSlice.actions;
export default tasksSlice.reducer;

```

Store:

```jsx
// store.js
import { configureStore } from '@reduxjs/toolkit';
import taskReducer from './reducer';

const store = configureStore({
  reducer: {
    tasks: taskReducer,
  },
});

export default store;

```

Componente React:

```jsx
// TodoComponent.js
import React, { useState } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { addTask } from './actions';

function TodoComponent() {
  const [newTask, setNewTask] = useState('');
  const tasks = useSelector(state => state.tasks);
  const dispatch = useDispatch();

  const handleChange = (event) => {
    setNewTask(event.target.value);
  };

  const handleSubmit = () => {
    dispatch(addTask(newTask));
    setNewTask('');
  };

  return (
    <div>
      <h1>Lista de Tarefas</h1>
      <ul>
        {tasks.map((task, index) => (
          <li key={index}>{task}</li>
        ))}
      </ul>
      <input type="text" value={newTask} onChange={handleChange} />
      <button onClick={handleSubmit}>Adicionar Tarefa</button>
    </div>
  );
}

export default TodoComponent;

```

Neste exemplo, o uso do Redux Toolkit simplifica a definição de actions e reducers através do método createSlice, que encapsula tanto as actions quanto o estado inicial e os reducers relacionados em um único objeto. Isso elimina a necessidade de definir types de actions separadamente, como era necessário no Redux puro.

Além disso, a configuração da store no Redux Toolkit é feita através do método configureStore, que fornece algumas configurações padrão e otimizações automáticas, como a validação do estado durante o desenvolvimento.

Em resumo, o Redux Toolkit simplifica o desenvolvimento com Redux, reduzindo a quantidade de código necessário e fornecendo uma API mais intuitiva e concisa.

## 10.2 Por que usar o Zustand ao invés do Redux ou Redux Toolkit?

Resposta: Usar o Zustand em vez do Redux ou Redux Toolkit pode ser preferível por algumas razões:

1. **Simplicidade e leveza:** Zustand é uma biblioteca mínima de gerenciamento de estado que oferece uma API simples e intuitiva. Ela possui menos conceitos abstratos e menos boilerplate em comparação com o Redux, o que torna mais fácil de aprender e usar.

2. **Menos código e configuração:** Com Zustand, você pode definir o estado e as funções de atualização em um único lugar, sem a necessidade de criar actions, reducers, types e outras estruturas típicas do Redux. Isso resulta em menos código e configuração necessários para implementar o gerenciamento de estado em sua aplicação.

3. **Performance:** Zustand é otimizado para performance, oferecendo atualizações de estado eficientes e re-renderizações mínimas. Ele utiliza uma combinação de Immer para atualizações imutáveis e Proxy para rastrear mudanças no estado, o que pode resultar em um desempenho superior em comparação com algumas implementações do Redux.

4. **Flexibilidade:** Zustand oferece uma abordagem mais flexível para o gerenciamento de estado. Você pode definir o estado e as funções de atualização de forma modular e granular, permitindo uma maior flexibilidade na organização e na estruturação do seu código.

5. **Menos dependências externas:** Zustand tem menos dependências externas do que o Redux e o Redux Toolkit, o que pode resultar em um pacote menor e menos vulnerabilidades de segurança.

Exemplo de Uso:

Aqui está um exemplo de como você poderia usar Zustand para gerenciar o estado de uma lista de tarefas em uma aplicação React:

```jsx
import React, { useState } from 'react';
import create from 'zustand';

// Define o store Zustand
const useStore = create((set) => ({
  tasks: [],
  addTask: (task) => set((state) => ({ tasks: [...state.tasks, task] })),
}));

function TodoComponent() {
  const [newTask, setNewTask] = useState('');
  const { tasks, addTask } = useStore();

  const handleChange = (event) => {
    setNewTask(event.target.value);
  };

  const handleSubmit = () => {
    addTask(newTask);
    setNewTask('');
  };

  return (
    <div>
      <h1>Lista de Tarefas</h1>
      <ul>
        {tasks.map((task, index) => (
          <li key={index}>{task}</li>
        ))}
      </ul>
      <input type="text" value={newTask} onChange={handleChange} />
      <button onClick={handleSubmit}>Adicionar Tarefa</button>
    </div>
  );
}

export default TodoComponent;

```

Neste exemplo:

- Criamos um store Zustand usando a função create. No parâmetro desta função, definimos um objeto com o estado inicial e as funções de atualização. No caso, temos apenas o estado tasks (a lista de tarefas) e a função addTask para adicionar uma nova tarefa ao estado.

- No componente TodoComponent, usamos o hook useStore para acessar o estado e as funções de atualização do store Zustand.

- Quando o usuário insere uma nova tarefa e clica no botão "Adicionar Tarefa", chamamos a função addTask do store para adicionar a nova tarefa ao estado.

Este é apenas um exemplo simples de como você pode usar Zustand para gerenciamento de estado em uma aplicação React. Ele demonstra a simplicidade e concisão oferecidas por Zustand em comparação com outras bibliotecas de gerenciamento de estado mais robustas como Redux.

## 10.3 O que é o React Query e quando devo usá-lo?

Resposta: O React Query é uma biblioteca popular para gerenciamento de estado e caching de dados em aplicações React. Ele fornece uma maneira simples e poderosa de buscar, armazenar em cache, atualizar e sincronizar os dados do aplicativo com o servidor ou outras fontes de dados. O React Query simplifica o trabalho com APIs assíncronas, como solicitações HTTP, fornecendo uma interface simples e declarativa para buscar e manipular dados.

Uma das principais vantagens do React Query é a sua abordagem baseada em hooks, que permite aos desenvolvedores incorporar facilmente funcionalidades de gerenciamento de estado e caching em componentes React. Ele também oferece recursos avançados, como invalidação de cache, refetching automático, paginacão e manipulação de erros, tornando-o uma escolha popular para aplicações React que dependem fortemente de dados assíncronos.

Use React Query quando:

- **Gerenciamento de dados assíncronos:** Você precisa lidar com chamadas de API e operações assíncronas para buscar, criar, atualizar ou excluir dados em sua aplicação. React Query oferece uma API intuitiva e eficiente para lidar com esses cenários, incluindo cache integrado, refetching automático e manipulação de estados de carregamento, erro e sucesso.

- **Cache de dados integrado:** Você deseja aproveitar o sistema de cache integrado do React Query para reduzir o número de chamadas de rede redundantes e melhorar o desempenho e a responsividade da sua aplicação.

- **Paginação e refetching:** Sua aplicação requer suporte para paginação de dados e refetching automático para manter os dados atualizados em tempo real.

- **Integração com Suspense:** Você pretende utilizar o Suspense do React para suspender a renderização de componentes até que os dados estejam prontos, proporcionando uma experiência de usuário mais suave e responsiva.

- **Requerimentos específicos da API:** Sua API utiliza estratégias de cache complexas, necessita de invalidação manual da cache, ou requer configurações específicas de headers ou autenticação.

Exemplo:

```jsx

import React from 'react';
import { useQueryClient, useQuery, useMutation } from 'react-query';

// Função para buscar a tarefa do localStorage
const fetchTask = async () => {
  const storedTask = localStorage.getItem('task');
  if (!storedTask) return null;
  return JSON.parse(storedTask);
};

function TodoComponent() {
  const queryClient = useQueryClient();

  // Consulta React Query para buscar a tarefa
  const { data: task } = useQuery('task', fetchTask);

  // Mutation React Query para adicionar uma nova tarefa
  const addTaskMutation = useMutation(async () => {
    const response = await fetch('https://sua-api.com/tasks', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({}),
    });
    if (!response.ok) {
      throw new Error('Erro ao adicionar tarefa');
    }
    const newTask = await response.json();
    localStorage.setItem('task', JSON.stringify(newTask));
    return newTask;
  }, {
    onSuccess: (newTask) => {
      queryClient.setQueryData('task', newTask); // Atualiza a cache da consulta 'task' após adicionar uma nova tarefa
    },
  });

  const handleAddTask = async () => {
    try {
      await addTaskMutation.mutateAsync();
    } catch (error) {
      console.error(error);
    }
  };

  return (
    <div>
      <h1>Tarefa Atual</h1>
      {task ? (
        <div>
          <p>{task.name}</p>
          <p>{task.description}</p>
        </div>
      ) : (
        <p>Nenhuma tarefa disponível</p>
      )}
      <button onClick={handleAddTask}>Adicionar Tarefa</button>
    </div>
  );
}

export default TodoComponent;

```

Neste exemplo, utilizamos useMutation para realizar a operação de adicionar tarefa. Após o sucesso da operação, atualizamos a cache da consulta 'task' para refletir a nova tarefa. Em seguida, na interface, exibimos a tarefa atual armazenada no localStorage.

## 11. Explique a diferença entre null e undefined em JavaScript.

Resposta: null é um valor especial que representa a ausência intencional de um objeto, enquanto undefined é um valor que indica que uma variável não foi atribuída com um valor.

## 12. Liste as vantagens da arquitetura de microsserviços.

Resposta: A arquitetura de microsserviços oferece benefícios como escalabilidade, flexibilidade, independência de tecnologia e facilidade de manutenção.

## 13. Quais são as vantagens do NoSQL sobre o RDBMS tradicional?

Resposta: NoSQL oferece vantagens como escalabilidade horizontal, flexibilidade de esquema, desempenho e facilidade de integração com dados não estruturados.

## 14. O que é programação reativa?

Resposta: Programação reativa é um paradigma de programação que lida com fluxos de dados assíncronos e eventos, permitindo uma abordagem mais declarativa e responsiva.

## 15. O que são os reducers no Redux?

Resposta: Reducers são funções puras no Redux que especificam como o estado da aplicação deve ser atualizado em resposta a uma ação.

## 16. Qual o papel do HTML na indexação de páginas por buscadores?

Resposta: O HTML fornece a estrutura e o conteúdo de uma página web, que os buscadores analisam e indexam para exibir nos resultados de pesquisa.

## 17. Cite 3 conceitos da Programação Orientada a Objetos aplicada ao JavaScript.

Resposta: Encapsulamento, herança e polimorfismo são três conceitos da Programação Orientada a Objetos aplicados ao JavaScript.

## 18. Quais os benefícios do TypeScript?

Resposta: TypeScript adiciona recursos de tipagem estática ao JavaScript, o que ajuda a detectar erros de código mais cedo, melhorar a produtividade e facilitar a manutenção de projetos grandes.

## 19. O que é uma interface no TypeScript?

Resposta: Uma interface no TypeScript é uma estrutura que define a forma de um objeto, especificando os tipos de propriedades e métodos que ele deve ter.

## 20. Qual o significado de Mock?

Resposta: Mock é uma técnica de simulação usada em testes de software, onde objetos ou funções falsas são criados para substituir componentes reais e controlar o comportamento durante os testes.

## 21. O que é o esquema do GraphQL?

Resposta: O esquema do GraphQL define a estrutura e os tipos de dados disponíveis em uma API GraphQL, especificando as consultas e mutações que podem ser feitas.

## 22. O que é o Virtual DOM? Qual sua vantagem?

Resposta: O Virtual DOM é uma representação virtual da estrutura do DOM em memória. Sua vantagem é permitir atualizações eficientes e rápidas do DOM, minimizando a manipulação direta do DOM real.

## 23. O que é e como usar a convenção Block Element Modifier (BEM)?

Resposta: BEM é uma convenção de nomenclatura para classes CSS que ajuda a criar estilos reutilizáveis e modularizados, separando os blocos, elementos e modificadores.

## 24. JavaScript: Explique como você pode usar funções JavaScript, como forEach, Map ou Reduce.

Resposta: forEach, map e reduce são métodos de array em JavaScript. forEach executa uma função em cada elemento do array, map cria um novo array com os resultados da função aplicada a cada elemento, e reduce reduz o array a um único valor aplicando uma função acumuladora.

## 25. React: O que é e como você pode aproveitar as vantagens do PureComponent?

Resposta: PureComponent é uma classe do React que implementa a lógica de comparação de propriedades e estado para evitar renderizações desnecessárias. Isso melhora o desempenho da aplicação, pois evita atualizações quando os dados não foram alterados.

## 26. O que é serverless computing?

Resposta: Serverless computing é um modelo de computação em nuvem onde o provedor de serviços gerencia a infraestrutura e os recursos necessários para executar o código, permitindo que os desenvolvedores se concentrem apenas na lógica do aplicativo.

## 27. Quais são os tipos primitivos do JavaScript?

Resposta: Os tipos primitivos do JavaScript são: string, number, boolean, null, undefined e symbol.

## 28. Qual a diferença entre inline e inline-block?

Resposta: A diferença entre inline e inline-block é que elementos com display inline ocupam apenas o espaço necessário para o conteúdo, enquanto elementos com display inline-block ocupam o espaço necessário e permitem definir largura e altura.

## 29. Qual a diferença entre elementos posicionados como relative, fixed, absolute e static?

Resposta: relative posiciona um elemento em relação à sua posição original, fixed posiciona um elemento em relação à janela do navegador, absolute posiciona um elemento em relação ao seu ancestral posicionado mais próximo, e static é o posicionamento padrão, onde o elemento segue o fluxo normal do documento.

## 30. Você pode explicar a diferença entre codificar um site para ser responsivo e usar uma estratégia mobile-first?

Resposta: Codificar um site para ser responsivo significa criar um layout que se adapte a diferentes tamanhos de tela, enquanto usar uma estratégia mobile-first significa projetar o site primeiro para dispositivos móveis e depois expandir para telas maiores.