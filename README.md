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

| Campo             |   Tipo  | Obrigatório | Descrição                 |
|-------------------|:-------:|:-----------:|---------------------------|
| name              | string  |      S      | Nome abreviado            |
| clinical_name     | string  |      N      | Nome da clínica           |
| has_employee      | boolean |      N      | Tem funcionário(s)        |
| quantity_employee | integer |      N      | Quantidade funcionário(s) |
| full_name         | string  |      S      | Nome completo             |
| phone             | string  |      S      | Telefone                  |
| mail              | string  |      S      | E-mail                    |
| cpf               | string  |      S      | CPF                       |
| cep               | string  |      S      | Cep                       |
| address           | string  |      S      | Endereço                  |
| number            | string  |      S      | Número endereço           |
| complement        | string  |      N      | Complemento               |
| state             | string  |      S      | Estado                    |
| city              | string  |      S      | Cidade                    |
| district          | string  |      N      | Bairro                    |
| expiration_date   | string  |      N      | Data de expiração         |

### Corpo

```
{  
   "data":{  
      "name": "Fulano",
      "full_name": "Fulano da Silva",
      "phone": "(00)90000-0000",
      "mail": "test@test.com",
      "cpf": "000.000.000-00",
      "cep": "99999-999",
      "address": "Rua das Aves",
      "number": "5130",
      "state": "SC",
      "city": "Joinville",
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
   "status_code": 201,
   "error" : null,
   "message": "created user id"
}
```

### Status da Resposta

| status_code | error      | message                                       |
|:-----------:|:----------:|:----------------------------------------------|
| 201         | null       | Created user id                               |
| 422         | Un.Entidy  | User already registered                       |
| 422         | Un.Entidy  | Error invalid date [expiration_date]          |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |

### Exemplo chamada em PHP

```
<?php
header('Content-Type: application/json');
$data_string = '{  
		   "data":{  
		      "name": "Fulano",
		      "full_name": "Fulano da Silva",
		      "phone": "(00)90000-0000",
		      "mail": "test@test.com",
		      "cpf": "000.000.000-00",
		      "cep": "99999-999",
		      "address": "Rua das Aves",
		      "number": "5130",
		      "state": "SC",
		      "city": "Joinville",
		      "expiration_date": "2019-01-01"
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

`POST /user/change`

### Parâmetros do corpo

| Campo 	    | Tipo     | Descrição         |
|:-----------------:|:--------:|:------------------|
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
   "status_code": 200,
   "error": null,
   "message": "Updated expiration date user"
}
```

### Status da Resposta

| status_code | error      | message                                       | 
|:-----------:|:----------:|:---------------------------------------------:|
| 200         | null       | Expiration date of the updated user           |         
| 422         | Un.Entidy  | Error invalid date [expiration_date]          |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |


## Obter dados do usuário

### Requisição

`POST /user/read`

### Parâmetros do corpo

| Campo       | Tipo   | Obrigatório | Descrição      |
|:-----------:|:------:|:-----------:|----------------|
| cpf_titular | string |      S      | CPF do usuário |

### Corpo

```
{  
   "cpf_titular": {
	"value": "999.999.999-99"
   },
   "token": {  
      "value": ""
   }
}
```

### Resposta

```
{  
   "full_name": "Fulano da Silva",
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
   "status_code": 200,
   "error": null,
   "message": null
}
```

### Status da Resposta

| status_code | error      | message                                       |
|:-----------:|:----------:|:----------------------------------------------|
| 200         | null       | null                                          |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{
                    "cpf_titular": {
                        "value": "000.000.000-00"
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
   "status_code": 200,
   "error": null,
   "message": "Dependents created / updated successfully"
}
```

### Status da Resposta

| status_code | error      | message                                       |
|:-----------:|:----------:|:----------------------------------------------|
| 200         | null       | Dependents created / updated successfully     |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |

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

| Campo              |  Tipo  | Obrigatório | Descrição          |
|--------------------|:------:|:-----------:|--------------------|
| social_name        | string |      N      | Razão social       |
| fantasy_name       | string |      S      | Nome fantasia      |
| cpf_cnpj           | string |      S      | CPF ou CNPJ        |
| state_registration | string |      N      | Inscrição estadual |
| birth              | string |      N      | Data nascimento    |
| cep                | string |      N      | Cep                |
| address            | string |      N      | Endereço           |
| number             | string |      N      | Número             |
| complement         | string |      N      | Complemento        |
| state              | string |      N      | Estado             |
| city               | string |      N      | Cidade             |
| district           | string |      N      | Bairro             |
| phone              | string |      N      | Fone               |
| note               | string |      N      | Observação         |

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
         "fantasy_name": "Fulano da Silva",
         "cpf_cnpj": "999.999.999-99"
      },
      {  
         "fantasy_name": "Distribuidora D",
         "cpf_cnpj": "99.999.999/9999-99"
      }
   ]
}
```

### Resposta

```
{
   "status_code": 200,
   "error": null,
   "message": "Contacts created / updated successfully"
}
```

### Status da Resposta

| status_code | error      | message                                       |
|:-----------:|:----------:|:----------------------------------------------|
| 200         | null       | Contacts created / updated successfully       |
| 422         | Un.Entidy  | Expiration date reached                       |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |

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
			 "fantasy_name": "Fulano da Silva",
			 "cpf_cnpj": "999.999.999-99"
		      },
		      {  
			 "fantasy_name": "Distribuidora D",
			 "cpf_cnpj": "99.999.999/9999-99"
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

| Campo                   |  Tipo  | Obrigatório | Descrição                               |
|-------------------------|:------:|:-----------:|-----------------------------------------|
| cpf_cnpj_contact        | string |      S      | CPF ou CNPJ (Obrigatório para Receitas) |
| date                    | string |      S      | Data do lançamento                      |
| type                    | string |      S      | R = Receita D = Despesa                 |
| value                   | string |      S      | Valor                                   |
| description             | string |      N      | Se for despesa, informar o CPF ou CNPJ  |
| deductible_expense_code | string |      N      | Código da despesa se for dedutível      |

### Corpo

```
{  
   "cpf_titular": {  
      "value": "000.000.000-00"
   },
   "base_year": {  
      "value": "2018"
   },
    "base_month": {  
      "value": "11"
   },
   "token": {  
      "value": ""
   },
   "data": [  
      {  
         "cpf_cnpj_contact": "000.000.000-00",
	 "date": "2018-09-05",
         "type": "R",
         "value": "90.00",
         "description": "999.999.999-99",
         "deductible_expense_code": null
      },
      {  
	 "date": "2018-09-10",
         "type": "D",
         "value": "790.00",
         "description": "99.999.999/9999-99",
         "deductible_expense_code": 1014
      }
   ]
}
```

### Resposta

```
{
   "status_code": 200,
   "error": null,
   "message": "Releases created / updated successfully"
}
```

### Status da Resposta

| status_code | error      | message                                       |
|:-----------:|:----------:|:----------------------------------------------|
| 200         | null       | Releases created / updated successfully       |
| 422         | Un.Entidy  | Expiration date reached                       |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "cpf_titular":{  
		      "value": "000.000.000-00"
		   },
		   "base_year": {  
		      "value": "2018"
		   },
		   "base_month": {  
		      "value": "11"
		   },
		   "token": {  
		      "value": ""
		   },
		   "data":[  
		      {  
			 "cpf_cnpj_contact": "032.639.739-67",
			 "date": "2018-09-05",
			 "type": "R",
			 "value": "90.00",
			 "description": "999.999.999-99",
			 "deductible_expense_code": null
		      },
		      {  
			 "date": "2018-09-10",
			 "type": "D",
			 "value": "790.00",
			 "description": "99.999.999/9999-99",
			 "deductible_expense_code": "1014"
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

| Campo       |  Tipo  | Obrigatório | Descrição            |
|-------------|:------:|:-----------:|----------------------|
| cpf_titular | string |      S      | CPF do usuário       |
| year        | string |      S      | Ano de processamento |
| month       | string |      S      | Mês de processamento |

### Corpo

```
{  
   "cpf_titular": {  
      "value": "999.999.999-99"
   },
   "year": {  
      "value": "2018"
   },
   "month": {  
      "value": "11"
   },
   "token": {  
      "value": ""
   }
}
```

### Resposta

```
{  
   "cpf": "359.177.770-63",
   "accounting_period": "2018-09-30",
   "maturity": "2018-10-31",
   "revenue": "12370.00",
   "deductible_expenses": "100.00",
   "non_deductible_revenues": "229.54",
   "dependent_deduction": "568.77",
   "calculation_base": "11701.23",
   "main_value": "2348.48",
   "year": "2018",
   "month": "09",
   "status_code": 200,
   "error": null,
   "message": null
}
```

### Parâmetros da Resposta

| Campo                   |  Tipo  | Descrição                     |
|-------------------------|:------:|-------------------------------|
| cpf                     | string | CPF do usuário                |
| accounting_period       | string | Período de apuração           |
| maturity                | string | Vencimento do darf            |
| revenue                 | string | Valor de receitas             |
| deductible_expenses     | string | Valor despesas dedutíveis     |
| non_deductible_revenues | string | Valor despesas não dedutíveis |
| dependent_deduction     | string | Valor dedução dependentes     |
| calculation_base        | string | Valor base de cálculo         |
| main_value              | string | Valor do principal            |
| year                    | string | Ano                           |
| month                   | string | Mês                           |
| status_code             | string | Status do processamento       |
| error                   | string | Código do erro                |
| message                 | string | Descrição do erro             |

### Status da Resposta

| status_code | error      | message                                       |
|:-----------:|:----------:|:----------------------------------------------|
| 200         | null       | null                                          |
| 422         | Un.Entidy  | Expiration date reached                       |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "cpf_titular": {  
		      "cpf": "999.999.999-99"
		   },
		   "year": {
		      "value": "2018"
		   },
		   "month": {
		      "value": "11"
		   },
		   "token": {  
		      "value": ""
		   }
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
   "cpf_titular": {  
      "value": "999.999.999-99"
   },
   "year": {  
      "value": "2018"
   },
   "month": {  
      "value": "11"
   },
   "token": {  
      "value": ""
   }
}
```

### Resposta

```
{  
   "cpf": "359.177.770-63",
   "accounting_period": "2018-09-30",
   "maturity": "2018-10-31",
   "revenue": "12370.00",
   "deductible_expenses": "100.00",
   "non_deductible_revenues": "229.54",
   "dependent_deduction": "568.77",
   "calculation_base": "11701.23",
   "main_value": "2348.48",
   "year": "2018",
   "month": "09",
   "status_code": 200,
   "error": null,
   "message": null
}
```

### Status da Resposta

| status_code | error      | message                                       |
|:-----------:|:----------:|:----------------------------------------------|
| 200         | null       | null                                          |
| 422         | Un.Entidy  | Expiration date reached                       |
| 422         | Un.Entidy  | Error DARF not found                          |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "cpf_titular": {  
		      "value": "999.999.999-99"
		   },
		   "year": {
		      "value": "2018"
		   },
		   "month": {
		      "value": "11"
		   },
		   "token": {  
		      "value": ""
		   }
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
   "cpf_titular": {  
      "value": "999.999.999-99"
   },
   "year": {  
      "value": "2018"
   },
   "initial_month": {
      "value": "11"
   },
   "final_month": {
      "value": "11"
   },
   "token": {
	"value": ""
   }
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
   "status_code": 200,
   "error": null,
   "message": null
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

| status_code | error      | message                                       |
|:-----------:|:----------:|:----------------------------------------------|
| 200         | null       | null                                          |
| 422         | Un.Entidy  | Expiration date reached                       |
| 422         | Un.Entidy  | Error fields not found [field_name not found] |

### Exemplo chamada PHP

```
<?php
header('Content-Type: application/json');

$data_string = '{  
		   "cpf_titular": {  
		      "value": "999.999.999-99"
		   },
		   "year": {  
		      "value": "2018"
		   },
		   "initial_month": {
		      "value": "11"
		   },
		   "final_month": {
		      "value": "11"
		   },
		   "token": {
		      "value": ""
		   }
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
