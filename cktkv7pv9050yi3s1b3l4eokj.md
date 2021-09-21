## Como consumir uma API em Python

​	Hoje em dia, temos varios serviços conectados, enviando e recebendo dados, através de diversas formas, uma delas sendo atraves de APIs, que são intermediários que permitem que dois softwares se comuniquem, e hoje iremos aprender a consumir uma API em Python.


[English Version/Versão em Inglês](https://mph7.hashnode.dev/how-to-consume-an-api-in-python)


## Com qual API iremos nos conectar?

​	Primeiramente, utilizaremos uma API chamada [ViaCEP](http://viacep.com.br), oque ela faz, em poucas palavras, é receber o numero de um CEP e devolver as informações sobre aquele CEP informado.

## Primeiros passos

​	Para podermos nos conectar à API, precisaremos da biblioteca request, entao iremos importa-la e montar a estrutura básica do nosso código:

```python
import requests

def main():
    
    
if __name__ == '__main__':
    main()
```

​	Todo nosso código será escrito dentro da função main.

​	No contexto da nossa API, precisaremos de um CEP para realizar nossa pesquisa, e o mesmo será digitado pelo usuário que utilizar o programa:

```python
cep = input('Digite o CEP para a consulta: ')
```

​	Um CEP é composto de 8 dígitos, algo fora disso é considerado um CEP inválido, entao podemos conferir se o CEP é válido para prosseguir, ou pedir um novo CEP até receber um com 8 dígitos:

```python
while 1:
	cep = input('Digite o CEP para a consulta: ')
    
    if len(cep) != 8:
        break
    print('Quantidade de dígitos inválida.')
```

​	Agora que temos um CEP com uma quantidade de dígitos válida, podemos começar a interagir com a API em si, no [site oficial](http://viacep.com.br) temos o formato de URL que será utilizada:

> https://viacep.com.br/ws/01001000/json/

​	'01001000' será o local onde irá o CEP, e json será o formato que receberemos as informações. Podemos então substituir e fazer o request:

```python
request = requests.get('https://viacep.com.br/ws/{}/json/'.format(cep))
```

​	A função .get nos permite extrair o conteúdo da URL com as informaçoes, as duas chaves {} marcam um local que podemos substituir por alguma coisa usando a função .format(), e substituímos pelo CEP.

​	Já que nosso arquivo é um json, podemos apresentar como um json usando a função .json():

```python
address_data = request.json()
```

​	Um arquivo json pode se transformar em um dicionário em Python, oque facilita para utilizar os valores, sendo necessário apenas informar a chave como index, vamos observar o arquivo json da API no cep 01001000:

```json
{
  "cep": "01001-000",
  "logradouro": "Praça da Sé",
  "complemento": "lado ímpar",
  "bairro": "Sé",
  "localidade": "São Paulo",
  "uf": "SP",
  "ibge": "3550308",
  "gia": "1004",
  "ddd": "11",
  "siafi": "7107"
}
```

​	Nesse caso, queremos mostrar ao usuários somente alguns dos dados, sendo o CEP, logradouro, complemento, bairro, localidade e o estado, estas são as chaves que utilizarem no nosso dicionário, mas só podemos mostrar se o CEP for válido, no caso de CEP inválido recebemos um json com um erro:

```json
{
  "erro": true
}
```

​	Então iremos excluir resultados com erros, e formatar o texto para mostrar de uma forma mais organizada:

```python
if "erro" not in address_data:
    print("### CEP ENCONTRADO ###")
    print("CEP: {}".format(address_data["cep"]))
    print("Logradouro: {}".format(address_data["logradouro"]))
    print("Complemento: {}".format(address_data["complemento"]))
    print("Bairro: {}".format(address_data["bairro"]))
    print("Cidade: {}".format(address_data["localidade"]))
    print("Estado: {}".format(address_data["uf"]))

else:
    print("{}: CEP Inválido".format(cep))

```

​	Como dito, basicamente formate o texto usando o dicíonario com os índices desejados. Com isso novo programa está praticamente pronto, podemos incluir no final também uma opção para o usuário poder reutilizar e pesquisar um CEP novamente:

```python
option = int(input("Deseja realizar uma nova consulta? \n1. Sim\n2. Sair\n"))
if option == 1:
    main()
else:
    print("Saindo...")
```

​	Chamando a função main(), todo o processo irá ocorrer novamente, e isso você pode modificar da maneira que desejar para mostrar os dados, inclusive, podemos colocar um cabeçalho no começo antes de inserir o CEP:

```python
print("#" * 30)
print("#" * 7, " Consultar CEP ", "#" * 7)
print("#" * 30, "\n")
```

​	O programa funcionando:

![CEP com menos de 8 dígitos](https://cdn.hashnode.com/res/hashnode/image/upload/v1631671714453/p4z_zca5e.png)

​	No caso de um CEP com menos de 8 Dígitos:

![image](https://cdn.hashnode.com/res/hashnode/image/upload/v1631671716312/WPEHrF0Sg.png)
  

​	E no caso de um CEP inválido:

![CEP inválido](https://cdn.hashnode.com/res/hashnode/image/upload/v1631671718210/voZykAYa-.png)
------

​	E assim temos um programa funcional para consulta de CEP utilizando uma API, se você leu até aqui, espero que você tenha aprendido, você pode conferir o código [aqui](https://github.com/mph7/CEPChecker), obrigado por ler e até uma próxima!