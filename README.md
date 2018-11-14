# Introdução

- [Introdução](#introdução)
- [Autenticacão](#autenticação)
- [Resposta em JSON](#resposta-em-json)
- [Considerações Iniciais](#considerações-iniciais)

# API

- [Criar usuário](#criar-usuário)
- [Alterar usuário](#alterar-usuário)
- [Obter dados do usuário](#obter-dados-do-usuário)
- [Criar dependentes](#criar-dependentes)
- [Criar contatos](#criar-contatos)
- [Criar lançamentos](#criar-lançamentos)
- [Processamento do Darf](#processamento-do-darf)
- [Obter dados do Darf](#obter-dados-do-darf)
- [Obter Livro Caixa](#obter-livro-caixa)

# Introdução

A API de integração Contify é um serviço que permite a invocação de procedimentos remotos que inserem dados no sistema Contify afim de gerar o Darf (Carne Leão).

O endereço base de comunicação é contify.com.br/api, acompanhado sempre do protocolo seguro https:// como prefixo. As versões da API são agrupadas por diretório, sufixadas ao endereço base (https://contify.com.br/api/v1).

Este documento trata da versão da API 1.0.

O endereço para se comunicar através desta versão é: https://contify.com.br/api/v1

# Autenticação

A API utiliza para validação dos parceiros o esquema de TOKEN que é fornecido ao parceiro após a firmação do contrato. 
Todas as chamadas devem ser executadas via método POST conforme exemplos em cada API.

# Resposta em JSON

Todas as nossas chamadas de API respondem no formato JSON. Não suportamos outro formato.

# Considerações Iniciais

Afim de garantir a integridade dos dados o processo de integração, deve seguir executar as operações da API na seguinte ordem no momento da emissão do carne leão.

1. Usuários
2. Dependentes
3. Contatos
4. Lançamentos
5. Processamento


## Criar usuário

### Requisição

`POST /user/insert`

### Parâmetros do corpo

| Campo              |   Tipo  | Obrigatório | Descrição                 |
|--------------------|:-------:|:-----------:|---------------------------|
| t_name             |  string |      S      | Nome abreviado            |
| t_clinicalName     |  string |      N      | Nome da clínica           |
| t_hasEmployee      | boolean |      N      | Tem funcionário(s)        |
| t_quantityEmployee | integer |      N      | Quantidade funcionário(s) |
| u_fullName         |  string |      S      | Nome completo             |
| u_phone            |  string |      S      | Telefone                  |
| u_mail             |  string |      S      | E-mail                    |
| u_cpf              |  string |      S      | CPF                       |
| u_cep              |  string |      S      | Cep                       |
| u_address          |  string |      S      | Endereço                  |
| u_number           |  string |      S      | Número endereço           |
| u_complement       |  string |      N      | Complemento               |
| u_state            |  string |      S      | Estado                    |
| u_city             |  string |      S      | Cidade                    |
| u_district         |  string |      N      | Bairro                    |
| expiration_date    |  string |      N      | Data de expiração         |

### Corpo

```
{  
   "data":{  
      "t_name": "Fulano",
      "u_fullName": "Fulano da Silva",
      "u_phone": "(00)90000-0000",
      "u_mail": "test@test.com",
      "u_cpf": "000.000.000-00",
      "u_cep": "99999-999",
      "u_address": "Rua das Aves",
      "u_number": "5130",
      "u_state": "SC",
      "u_city": "Joinville",
      "expiration_date": "2018-02-28"
   },
   "token":{  
      "value": ""
   }
}
```

### Resposta

```
{
   "status": "OK",
   "error_code" :0,
   "error_desc": null
}
```

### Status da Resposta

| status 	    | error_code | error_desc                                    |
|-------------------|:----------:|:----------------------------------------------|
| OK                | 0          | null                                          |
| ERROR             | 4          | Error Fields Not Found [field name not found] |

### Exemplo chamada em PHP

```
<?php
header('Content-Type: application/json');
$data_string = '{  
		   "data":{  
		      "t_name": "Fulano",
		      "u_fullName": "Fulano da Silva",
		      "u_phone": "(00)90000-0000",
		      "u_mail": "test@test.com",
		      "u_cpf": "000.000.000-00",
		      "u_cep": "99999-999",
		      "u_address": "Rua das Aves",
		      "u_number": "5130",
		      "u_state": "SC",
		      "u_city": "Joinville"
		   },
		   "token":{  
		      "value": ""
		   }
		}';

$url = 'https://contify.com.br/api/v1/user/insert';

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
   'Content-Type: application/json',
   'Content-Length: ' . strlen($data_string)
));

$content = curl_exec($ch);
echo($content);
```

## Alterar usuário

### Requisição

`PUT /user/change`

### Parâmetros do corpo

| Campo 	    | Tipo     | Descrição         |
|-------------------|:--------:|:------------------|
| expiration_date   | string   | Data de expiração |

### Corpo

```
{  
   "cpf_titular": {  
      "value": "999.999.999-99"
   },
   "token": {  
      "value": ""
   },
   "expiration_date": {
      "value": ""
   }   
}
```

### Resposta

```
{
   "status": "OK",
   "error_code": 0,
   "error_desc": null
}
```

### Status da Resposta

| status 	    | error_code | error_desc         |
|-------------------|:----------:|:-------------------|
| OK                | 0          | null               |
| ERROR             | 18         | Error Invalid date |


## Obter dados do usuário

### Requisição

`POST /user/read`

### Parâmetros do corpo

| Campo | Tipo   | Obrigatório | Descrição      |
|-------|:------:|:-----------:|----------------|
| cpf   | string |      S      | CPF do usuário |

### Corpo

```
{  
   "data": {  
      "cpf": "359.177.770-63"
   },
   "token": {  
      "value": ""
   }
}
```

### Resposta

```
{  
   "fullName": "Fulano da Silva",
   "phone": "(00)90000-0000",
   "mail": "test@test.com",
   "cpf": "000.000.000-00",
   "cep": "99999-999",
   "address": "Rua das Aves",
   "number": "4178",
   "complement": "",
   "state": "SC",
   "city": "Joinville",
   "district": "",
   "status": "OK",
   "error_code": 0,
   "error_desc": null
}
```

### Status da Resposta

| status 	    | error_code | error_desc     |
|-------------------|:----------:|:---------------|
| OK                | 0          | null           |
| ERROR             | 1          | User not found |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{
                    "data": {
                        "cpf": "000.000.000-00"
                    },
                    "token": {
                        "value": ""
                    }
                }';

$url = 'https://contify.com.br/api/user/read';

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string)
));

$content = curl_exec($ch);
echo ($content);
```

## Criar dependentes

### Requisição

`POST /dependent/insert`

### Parâmetros do corpo

| Campo |  Tipo  | Obrigatório | Descrição          |
|-------|:------:|:-----------:|--------------------|
| name  | string |      S      | Nome do dependente |
| cpf   | string |      S      | CPF do dependente  |
| birth | string |      S      | Data nascimento    |

### Corpo

```
{  
   "cpf_titular": {  
      "value": "999.999.999-99"
   },
   "token": {  
      "value": ""
   },
   "data": [  
      {  
         "name": "Dependente 1",
         "cpf": "111.111.111-11",
         "birth": "2010-01-15"
      },
      {  
         "name": "Dependente 2",
         "cpf": "222.222.222-22",
         "birth": "2011-07-26"
      },
      {  
         "name": "Dependente 3",
         "cpf": "333.333.333-33",
         "birth": "1981-04-20"
      }
   ]
}

```
### Resposta

```
{
   "status": "OK",
   "error_code": 0,
   "error_desc": null
}
```

### Status da Resposta

| status 	    | error_code | error_desc              |
|-------------------|:----------:|:------------------------|
| OK                | 0          | null                    |
| ERROR             | 405        | expiration date reached |
| ERROR             | 4          | Field Not Found         |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "cpf_titular": {  
		      "value": "999.999.999-99"
		   },
		   "token": {  
		      "value": ""
		   },
		   "data": [  
		      {  
			 "name": "Dependente 1",
			 "cpf": "111.111.111-11",
			 "birth": "2010-01-15"
		      },
		      {  
			 "name": "Dependente 2",
			 "cpf": "222.222.222-22",
			 "birth": "2011-07-26"
		      },
		      {  
			 "name": "Dependente 3",
			 "cpf": "333.333.333-33",
			 "birth": "1981-04-20"
		      }
		   ]
		}';

$url = 'https://contify.com.br/api/v1/dependent/insert';

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string)
));

$content = curl_exec($ch);
echo ($content);
```

## Criar contatos

### Requisição

`POST /contact/insert`

### Parâmetros do corpo

| Campo             |  Tipo  | Obrigatório | Descrição          |
|-------------------|:------:|:-----------:|--------------------|
| socialName        | string |      N      | Razão social       |
| fantasyName       | string |      S      | Nome fantasia      |
| cpfCnpj           | string |      S      | CPF ou CNPJ        |
| stateRegistration | string |      N      | Inscrição estadual |
| birth             | string |      N      | Data nascimento    |
| cep               | string |      N      | Cep                |
| address           | string |      N      | Endereço           |
| number            | string |      N      | Número             |
| complement        | string |      N      | Complemento        |
| state             | string |      N      | Estado             |
| city              | string |      N      | Cidade             |
| district          | string |      N      | Bairro             |
| phone             | string |      N      | Fone               |
| note              | string |      N      | Observação         |

### Corpo

```
{  
   "cpf_titular":{  
      "value": "000.000.000-00"
   },
   "token":{  
      "value": ""
   },
   "data":[  
      {  
         "fantasyName": "Fulano da Silva",
         "cpfCnpj": "999.999.999-99"
      },
      {  
         "fantasyName": "Distribuidora D",
         "cpfCnpj": "99.999.999/9999-99"
      }
   ]
}
```

### Resposta

```
{
   "status": "OK",
   "error_code": 0,
   "error_desc": null
}
```

### Status da Resposta

| status 	    | error_code | error_desc                                     |
|-------------------|:----------:|:-----------------------------------------------|
| OK                | 0          | null                                           |
| ERROR             | 405        | expiration date reached                        |
| ERROR             | 4          | Error Fields Not Found: [Field name not found] |

### Exemplo chamada PHP

```
<?php

header('Content-Type: application/json');

$data_string = '{  
		   "cpf_titular":{  
		      "value": "000.000.000-00"
		   },
		   "token":{  
		      "value": ""
		   },
		   "data":[  
		      {  
			 "fantasyName": "Fulano da Silva",
			 "cpfCnpj": "999.999.999-99"
		      },
		      {  
			 "fantasyName": "Distribuidora D",
			 "cpfCnpj": "99.999.999/9999-99"
		      }
		   ]
		}';

$url = 'https://contify.com.br/api/v1/contact/insert';

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
	'Content-Type: application/json',
	'Content-Length: ' . strlen($data_string)
));
$content = curl_exec($ch);
echo ($content);
```

## Criar lançamentos

### Requisição

`POST /launch/insert`

### Parâmetros do corpo

| Campo                 |  Tipo  | Obrigatório | Descrição                               |
|-----------------------|:------:|:-----------:|-----------------------------------------|
| cpfCnpj               | string |      S      | CPF ou CNPJ (Obrigatório para Receitas) |
| date                  | string |      S      | Data do lançamento                      |
| type                  | string |      S      | R = Receita D = Despesa                 |
| value                 | string |      S      | Valor                                   |
| description           | string |      N      | Se for despesa, informar o CPF ou CNPJ  |
| deductibleExpenseCode | string |      N      | Código da despesa se for dedutível      |

### Corpo

```
{  
   "cpf_titular":{  
      "value":"000.000.000-00"
   },
   "token": {  
      "value":""
   },
   "data":[  
      {  
         "cpfCnpj_contact":"000.000.000-00",
	 "date": "2018-09-05",
         "type": "R",
         "value": "90.00",
         "description": "999.999.999-99",
         "deductibleExpenseCode": null
      },
      {  
	 "date": "2018-09-10",
         "type": "D",
         "value": "790.00",
         "description": "99.999.999/9999-99",
         "deductibleExpenseCode": "1014"
      }
   ]
}
```

### Resposta

```
{
   "status": "OK",
   "error_code": 0,
   "error_desc": null
}
```

### Status da Resposta

| status 	    | error_code | error_desc                                     |
|-------------------|:----------:|:-----------------------------------------------|
| OK                | 0          | null                                           |
| ERROR             | 405        | expiration date reached                        |
| ERROR             | 16         | Error contact not found                        |
| ERROR             | 4          | Error Fields Not Found: [Field name not found] |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "cpf_titular":{  
		      "value": "000.000.000-00"
		   },
		   "token": {  
		      "value":""
		   },
		   "data":[  
		      {  
			 "cpfCnpj_contact":"032.639.739-67",
			 "date": "2018-09-05",
			 "type": "R",
			 "value": "90.00",
			 "description": "999.999.999-99",
			 "deductibleExpenseCode": null
		      },
		      {  
			 "date": "2018-09-10",
			 "type": "D",
			 "value": "790.00",
			 "description": "99.999.999/9999-99",
			 "deductibleExpenseCode": "1014"
		      }
		   ]
		}';

$url = 'https://contify.com.br/api/v1/launch/insert';

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string)
));

$content = curl_exec($ch);
echo ($content);
```

## Processamento do Darf

### Requisição

`POST /process/process`

### Parâmetros do Corpo

| Campo                 |  Tipo  | Obrigatório | Descrição                              |
|-----------------------|:------:|:-----------:|----------------------------------------|
| cpf                   | string |      S      | CPF do usuário                         |
| year                  | string |      S      | Ano de processamento                   |
| month                 | string |      S      | Mês de processamento                   |

### Corpo

```
{  
   "data": {  
      "cpf": "999.999.999-99"
   },
   "token": {  
      "value": ""
   },
   "date": [  
      {  
         "year": "2018"
      },
      {  
         "month": "09"
      }
   ]
}
```

### Resposta

```
{  
   "cpf": "359.177.770-63",
   "accountingPeriod": "2018-09-30",
   "maturity": "2018-10-31",
   "revenue": "12370.00",
   "deductibleExpenses": "100.00",
   "nonDeductibleRevenues": "229.54",
   "dependentDeduction": "568.77",
   "calculationBase": "11701.23",
   "mainValue": "2348.48",
   "year": "2018",
   "month": "09",
   "status": "OK",
   "error_code": 0,
   "error_desc": null
}
```

### Parâmetros da Resposta

| Campo                 |  Tipo  | Descrição                     |
|-----------------------|:------:|-------------------------------|
| cpf                   | string | CPF do usuário                |
| accountingPeriod      | string | Período de apuração           |
| maturity              | string | Vencimento do darf            |
| revenue               | string | Valor de receitas             |
| deductibleExpenses    | string | Valor despesas dedutíveis     |
| nonDeductibleRevenues | string | Valor despesas não dedutíveis |
| dependentDeduction    | string | Valor dedução dependentes     |
| calculationBase       | string | Valor base de cálculo         |
| mainValue             | string | Valor do principal            |
| year                  | string | Ano                           |
| month                 | string | Mês                           |
| status                | string | Status do processamento       |
| error_code            | string | Código do erro                |
| error_desc            | string | Descrição do erro             |

### Status da Resposta

| status 	    | error_code | error_desc           |
|-------------------|:----------:|:---------------------|
| OK                | 0          | null                 |
| ERROR             | 405        | Prazo expirado       |
| ERROR             | 6          | Error User Not Found |


### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "data": {  
		      "cpf": "999.999.999-99"
		   },
		   "token": {  
		      "value": ""
		   },
		   "date": [  
		      {  
			 "year": "2018"
		      },
		      {  
			 "month": "09"
		      }
		   ]
		}';

$url = 'https://contify.com.br/api/v1/process/process';

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string)
));

$content = curl_exec($ch);
echo ($content); 
```

## Obter dados do Darf

### Requisição

`POST /process/read`

### Corpo

```
{  
   "data": {  
      "cpf": "999.999.999-99"
   },
   "token": {  
      "value": ""
   },
   "date": [  
      {  
         "year": "2018"
      },
      {  
         "month": "09"
      }
   ]
}
```

### Resposta

```
{  
   "cpf": "359.177.770-63",
   "accountingPeriod": "2018-09-30",
   "maturity": "2018-10-31",
   "revenue": "12370.00",
   "deductibleExpenses": "100.00",
   "nonDeductibleRevenues": "229.54",
   "dependentDeduction": "568.77",
   "calculationBase": "11701.23",
   "mainValue": "2348.48",
   "year": "2018",
   "month": "09",
   "status": "OK",
   "error_code": 0,
   "error_desc": null,
   "cpf_titular": "359.177.770-63"
}
```

### Status da Resposta

| status 	    | error_code | error_desc              |
|-------------------|:----------:|:------------------------|
| OK                | 0          | null                    |
| ERROR             | 405        | expiration date reached |
| ERROR             | 15         | Error DARF not found    |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "data": {  
		      "cpf": "999.999.999-99"
		   },
		   "token": {  
		      "value": ""
		   },
		   "date": [  
		      {  
			 "year": "2018"
		      },
		      {  
			 "month": "09"
		      }
		   ]
		}';

$url = 'https://contify.com.br/api/v1/process/read';

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string)
));

$content = curl_exec($ch);
echo ($content); 
```

## Obter Livro Caixa

### Requisição

`POST /cash-book/read`

### Corpo

```
{  
   "data": {  
      "cpf": "999.999.999-99"
   },
   "token": {  
      "value": ""
   },
   "date": [  
      {  
         "year": "2018"
      },
      {  
         "initial_month": "09"
      },
      {  
         "final_month": "09"
      }
   ]
}
```

### Resposta

```
{  
   "cpf_titular": "999.999.999-99",
   "year": "2018",
   "initial_month": "09",
   "final_month": "09",
   "file": "JVBERi0xLjcKJeLjz9MKNyAwIG9iago8PCAvVHlwZSAvUGFnZSAvUGFyZ...",
   "status": "OK",
   "error_code": 0,
   "error_desc": null
}
```

### Parâmetros da Resposta

| Campo                 |  Tipo  | Descrição                     |
|-----------------------|:------:|-------------------------------|
| cpf_titular           | string | CPF do usuário                |
| year                  | string | Ano base                      |
| inicial_month         | string | Mês inicial do período        |
| final_month           | string | Mês final do período          |
| file                  | string | Arquivo codificado em Base64  |
| status                | string | Status do processamento       |
| error_code            | string | Código do erro                |
| error_desc            | string | Descrição do erro             |

### Status da Resposta

| status 	    | error_code | error_desc              |
|-------------------|:----------:|:------------------------|
| OK                | 0          | null                    |
| ERROR             | 405        | expiration date reached |
| ERROR             | 15         | Error User not found    |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "data": {  
		      "cpf": "999.999.999-99"
		   },
		   "token": {  
		      "value": ""
		   },
		   "date": [  
		      {  
			 "year": "2018"
		      },
		      {  
			 "initial_month": "09"
		      },
		      {  
			 "final_month": "09"
		      }
		   ]
		}';

$url = 'https://contify.com.br/api/v1/cash-book/read';

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string)
));

$content = curl_exec($ch);
echo ($content); 
```

### Exemplo resposta PHP

```
<?php
// Retorno do arquivo Base64
$doc = "JVBERi0xLjcKJeLjz9MKNyAwIG9iago8PCAvVHlwZSAvUGFnZSAvUGFyZ...";

// Salva o arquivo no local especificado
$fp = fopen('livro_caixa.pdf', 'w');
fwrite($fp, base64_decode($doc));
fclose($fp);
```
