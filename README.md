# Desafio Dio - Criando a Sua Primeira Criptomoeda da Rede Ethereum 





## **Projeto Abrangente para a Criação de uma Criptomoeda na Rede Ethereum**



Este projeto abrangente irá guiá-lo através dos passos necessários para criar sua própria criptomoeda na rede Ethereum. Usaremos Python e seguindo códigos detalhados para cada etapa.



### **Pré-requisitos**

- Conhecimento básico de Python
- Ambiente de desenvolvimento Python configurado
- Carteira Ethereum
- Conexão com a internet



## **Etapas**



### **1. Criar um Contrato Inteligente ERC-20**



Um contrato inteligente ERC-20 define as regras e funcionalidades da sua criptomoeda. Use o seguinte código Python para criar um:



### python

```python
from web3 import Web3

# Conecte-se ao nó da rede Ethereum
w3 = Web3(Web3.HTTPProvider('URL_DO_NÓ'))

# Defina os parâmetros do token
nome_token = 'NomeDoToken'
simbolo_token = 'SimboloDoToken'
fornecimento_total = 1000000  # Número total de tokens

# Crie o contrato inteligente ERC-20
contrato_erc20 = w3.eth.contract(abi='ABI_DO_CONTRATO', bytecode='BYTECODE_DO_CONTRATO')

# Implante o contrato na rede
tx_hash = contrato_erc20.constructor(nome_token, simbolo_token, fornecimento_total).transact()

# Aguarde a confirmação da transação
w3.eth.wait_for_transaction_receipt(tx_hash)

# Obtenha o endereço do contrato
endereco_contrato = contrato_erc20.address
```



### **JavaScript**

javascript

```javascript
const Web3 = require('web3');

// Conecte-se ao nó da rede Ethereum
const web3 = new Web3(new Web3.providers.HttpProvider('URL_DO_NÓ'));

// Defina os parâmetros do token
const nomeToken = 'NomeDoToken';
const simboloToken = 'SimboloDoToken';
const fornecimentoTotal = 1000000;  // Número total de tokens

// Crie o contrato inteligente ERC-20
const contratoERC20 = new web3.eth.Contract([
  {
    "constant": false,
    "inputs": [
      {
        "name": "_name",
        "type": "string"
      },
      {
        "name": "_symbol",
        "type": "string"
      },
      {
        "name": "_totalSupply",
        "type": "uint256"
      }
    ],
    "name": "constructor",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  // Outras funções do contrato ERC-20
]);

// Implante o contrato na rede
contratoERC20.deploy({
  data: 'BYTECODE_DO_CONTRATO',
  arguments: [nomeToken, simboloToken, fornecimentoTotal]
}).send({
  from: 'ENDERECO_DA_SUA_CARTEIRA',
  gas: 1000000
}).then((novaInstanciaContrato) => {
  // Obtenha o endereço do contrato
  const enderecoContrato = novaInstanciaContrato.options.address;
});
```



### **Solidity**

plaintext

```plaintext
// Defina o contrato inteligente ERC-20
pragma solidity ^0.8.0;

contract TokenERC20 {
    string public nome;
    string public simbolo;
    uint256 public totalSupply;

    // Construtor para inicializar o token
    constructor(string memory _nome, string memory _simbolo, uint256 _totalSupply) {
        nome = _nome;
        simbolo = _simbolo;
        totalSupply = _totalSupply;
    }

    // Outras funções do contrato ERC-20
}
```

```plaintext
pragma solidity ^0.8.0;

// Definição do contrato inteligente ERC-20
contract TokenERC20 {
    // Atributos do token
    string public nome;
    string public simbolo;
    uint256 public totalSupply;

    // Construtor para inicializar o token
    constructor(string memory _nome, string memory _simbolo, uint256 _totalSupply) {
        nome = _nome;
        simbolo = _simbolo;
        totalSupply = _totalSupply;
    }

    // Função para verificar o fornecimento total de tokens
    function totalSupply() public view returns (uint256) {
        return totalSupply;
    }

    // Função para verificar o saldo de uma conta
    function balanceOf(address _account) public view returns (uint256) {
        return _account.balance;
    }

    // Função para transferir tokens entre contas
    function transfer(address _to, uint256 _amount) public returns (bool) {
        require(_to != address(0), "Destino inválido");
        require(_amount > 0, "Quantidade inválida");
        require(balanceOf(msg.sender) >= _amount, "Saldo insuficiente");

        // Executa a transferência de tokens
        _to.balance += _amount;
        msg.sender.balance -= _amount;

        // Emite um evento para notificar a transferência
        emit Transfer(msg.sender, _to, _amount);

        return true;
    }

    // Eventos para notificar sobre as transações
    event Transfer(address indexed from, address indexed to, uint256 amount);
}
```

Este é um contrato inteligente básico para um token ERC-20. Você pode usar este contrato como base para criar suas próprias criptomoedas na rede Ethereum.



### **2. Implantar o Contrato Inteligente**

Para implantar o contrato inteligente na rede Ethereum, execute o seguinte código:

### **Python**

python

```python
# Envie a transação para implantar o contrato
tx_hash = w3.eth.send_transaction({
    'from': 'ENDERECO_DA_SUA_CARTEIRA',
    'to': endereco_contrato,
    'value': 0,  # Nenhum valor de ETH é necessário para implantação
    'gas': 1000000  # Limite de gás para a transação
})

# Aguarde a confirmação da transação
w3.eth.wait_for_transaction_receipt(tx_hash)
```



### **JavaScript**

javascript

```javascript
contratoERC20.deploy({
  data: 'BYTECODE_DO_CONTRATO',
  arguments: [nomeToken, simboloToken, fornecimentoTotal]
}).send({
  from: 'ENDERECO_DA_SUA_CARTEIRA',
  gas: 1000000
}).then((novaInstanciaContrato) => {
  // A implantação foi bem-sucedida.
});
```



### **Solidity**

A implantação do contrato inteligente é feita durante a compilação e implantação do contrato usando um compilador Solidity.





### **3. Interagir com o Contrato Inteligente**

Depois de implantar o contrato, você pode interagir com ele usando as seguintes funções:



### **Python**

python

```python
# Verificar o fornecimento total
fornecimento = contrato_erc20.functions.totalSupply().call()

# Verificar o saldo de uma conta
saldo = contrato_erc20.functions.balanceOf('ENDERECO_DA_CONTA').call()

# Transferir tokens
tx_hash = contrato_erc20.functions.transfer('ENDERECO_DESTINO', 100).transact()
w3.eth.wait_for_transaction_receipt(tx_hash)
```



### **JavaScript**

javascript

```javascript
// Verificar o fornecimento total
contratoERC20.methods.totalSupply().call().then((resultado) => {
  const fornecimento = resultado;
});

// Verificar o saldo de uma conta
contratoERC20.methods.balanceOf('ENDERECO_DA_CONTA').call().then((resultado) => {
  const saldo = resultado;
});

// Transferir tokens
contratoERC20.methods.transfer('ENDERECO_DESTINO', 100).send({
  from: 'ENDERECO_DA_SUA_CARTEIRA'
}).then((resultado) => {
  // A transferência foi bem-sucedida.
});
```



### **Solidity**

Você pode interagir com o contrato inteligente chamando suas funções diretamente do código Solidity.



## **Conclusão**



Seguindo essas etapas e usando os códigos fornecidos, você pode criar com sucesso sua própria criptomoeda na rede Ethereum. Lembre-se de testar e auditar seu contrato inteligente cuidadosamente antes de lançá-lo para uso público.



