# Desafio de programação

Obrigado por participar do nosso desafio de Python 🐍 Estamos empolgados em poder conhecer melhor sua maneira de pensar, estilo de código e habilidade de programação!

Para este desafio, estimamos uma duração de até 3h, mas sinta-se à vontade para utilizar o tempo que precisar. Algumas recomendações:

- leia com atenção este documento completo antes de começar;
- faça um fork do repositório;
- resolva o desafio, fazendo commits constantes para entendermos sua maneira de pensar;
- **seu código deve estar em python versão 3.6 ou mais recente**
- e abra um PR contendo os arquivos indicados abaixo.

Nosso time corrigirá seu PR e você estará pronto!

## Contexto

Como bem sabemos, na vida real os dados raramente chegam limpos e organizados como gostaríamos. Para este desafio, você vai fazer uma conversão de formato, como se estivesse trazendo dados de uma base NoSQL para um banco relacional.

Na Liv Up, os clientes em geral pedem mais de um produto por compra. Esses produtos podem ser um item avulso ou uma refeição, que é um grupo de três ou mais itens. O arquivo [JSON](https://pt.wikipedia.org/wiki/JSON) que você vai receber vai conter uma lista de vendas, cada venda contendo uma lista de produtos.

Sua tarefa é desagregar os dados e gerar um arquivo TSV - similar ao [CSV](https://pt.wikipedia.org/wiki/Comma-separated_values), mas utilizando "tabs" (ou `\t`) para separar as colunas, de forma que cada linha contenha um único item.

## Esquemas e amostras de dados de entrada e saída

Os dados de entrada representam um array JSON, em que cada objeto segue o seguinte esquema:

```
venda_id: string (GUID),
venda_dt: horário unix com milisegundos,
cliente_id: string (GUID),
cliente_email: "cwillgoss1@unc.edu",
distribuidora_id: string (GUID),
entrega_tipo: string ("entrega" ou "retirada"),
produtos: array de objetos JSON com o seguinte schema:
  id: string (GUID),
  tipo: string ("avulso" ou "prato"),
  valor: integer (preço do item em centavos),
  quantidade: integer,
  itens: array de string (GUID), com um único elemento (igual ao id do produto) se o tipo for "avulso", ou uma lista de ids se o tipo for "prato".
```

Os dados de que você deve usar para o desafio de fato estão na pasta `inputs`, mas aqui temos uma amostra:

```json
[{
  "venda_id": "d6767d5e-54b7-4519-9f62-13ddafaac3f2",
  "venda_data": 1621302409.168,
  "cliente_id": "55c5cd11-3615-43c6-a6b5-e880a202a260",
  "cliente_email": "cwillgoss1@unc.edu",
  "distribuidora_id": "29234170-850e-4435-bffb-d7817b8d5da2",
  "entrega_tipo": "entrega",
  "produtos": [
    {
      "id": "983d6c11-c6d1-4742-b0f7-9321070ef80a",
      "tipo": "avulso",
      "valor": 4010,
      "qtd": 3,
      "itens": [
        "983d6c11-c6d1-4742-b0f7-9321070ef80a"
      ]
    },
    {
      "id": "8a2179eb-2d40-4e45-9e5b-816555030590",
      "tipo": "prato",
      "valor": 3980,
      "quantidade": 4,
      "itens": [
        "4a299468-a2f8-43c9-bff8-cac27ca4e59e",
        "9dffe294-a675-407c-9189-abe193761ad7",
        "31ccfb80-3a81-48b2-bbf5-af69564937e8"
      ]
    }
  ]
}]
```

O output deve ser um TSV com as seguintes colunas:

```
venda_id: string
venda_dt: string no formato ISO
cliente_email: string
distribuidora_id: string
entrega_tipo: string
produto_id: string
produto_valor: string (preço do produto em reais, apenas números)
item_id: string
item_qtd: inteiro (corresponde ao campo quantidade)
```

Atenção para o campo `produto_valor`, que muda de tipo e de unidade! Para a amostra JSON acima, a saída esperada é:

```TSV
venda_id  venda_dt  cliente_email  distribuidora_id entrega_tipo  produto_id  produto_valor  item_id item_qtd
"d6767d5e-54b7-4519-9f62-13ddafaac3f2"  "2021-05-17T22:46:49.168000"  "cwillgoss1@unc.edu"  "29234170-850e-4435-bffb-d7817b8d5da2"  "entrega" "983d6c11-c6d1-4742-b0f7-9321070ef80a"  "40.10"  "983d6c11-c6d1-4742-b0f7-9321070ef80a"  3
"d6767d5e-54b7-4519-9f62-13ddafaac3f2"  "2021-05-17T22:46:49.168000"  "cwillgoss1@unc.edu"  "29234170-850e-4435-bffb-d7817b8d5da2"  "entrega" "8a2179eb-2d40-4e45-9e5b-816555030590"  "39.80"  "4a299468-a2f8-43c9-bff8-cac27ca4e59e"  4
"d6767d5e-54b7-4519-9f62-13ddafaac3f2"  "2021-05-17T22:46:49.168000"  "cwillgoss1@unc.edu"  "29234170-850e-4435-bffb-d7817b8d5da2"  "entrega" "8a2179eb-2d40-4e45-9e5b-816555030590"  "39.80"  "9dffe294-a675-407c-9189-abe193761ad7"  4
"d6767d5e-54b7-4519-9f62-13ddafaac3f2"  "2021-05-17T22:46:49.168000"  "cwillgoss1@unc.edu"  "29234170-850e-4435-bffb-d7817b8d5da2"  "entrega" "8a2179eb-2d40-4e45-9e5b-816555030590"  "39.80"  "31ccfb80-3a81-48b2-bbf5-af69564937e8"  4
```

## BÔNUS

Esta seção contém desafios adicionais, inteiramente opcionais, mas que vão nos impressionar 🙂

1. O time vai crescer! Documente seu código com docstrings (e quaisquer arquivos de apoio que precisar)
2. Precisamos compartilhar sua tabela com um terceiro! Como podemos anonimizar os dados dos clientes? Responda por escrito ou adicione uma flag de execução que retorne os dados anonimizados/pseudonimizados
3. Consegue resolver tudo (inclusive os desafios extra) sem precisar instalar bibliotecas adicionais? Adeus pandas 😉

# O que seu PR deve conter
Após clonado, esperamos que seu PR contenha a seguinte estrutura de arquivos:

```
├─ inputs/
│    └─ sales.json      - o arquivo de entrada do seu programa
├─ outputs/
│    └─ sales.tsv       - o arquivo de saída do seu programa
├─ README.md            - este arquivo
├─ requirements.txt     - dependências do seu código (se não houver, inclua o arquivo em branco)
└─ src/
     ├─ README.md       - arquivo descrito abaixo
     └─ ...             - demais arquivos contendo o código python da sua solução
```

No README dentro da pasta `src`, inclua:
- instruções de como executar seu código, incluindo a versão de python que usou para resolver;
- comentários sobre sua solução que queira compartilhar com o time;
- respostas para os desafios adicionais.
