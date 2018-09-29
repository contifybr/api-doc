# Introdução

- [Introdução](#introdução)
- [Autenticacão](#autenticação)
- [Usando a API com JSON](#usando-a-api-com-json)
- [Considerações Iniciais](#considerações-iniciais)

# API

- [Criar usuário](#criar-usuário)
- [Obter dados do usuário](#obter-dados-do-usuário)
- [Criar dependentes](#criar-dependentes)
- [Criar lançamentos](#criar-lançamentos)
- [Processamento do Darf](#processamento-do-darf)
- [Obter dados do Darf](#obter-dados-do-darf)

# Introdução

A API de integração Contify é um serviço que permite a invocação de procedimentos remotos que inserem dados no sistema Contify afim de gerar o Darf (Carne Leão).

# Autenticação

A API utiliza para validação dos parceiros o esquema de TOKEN que é fornecido ao parceiro após a firmação do contrato. 
Todas as chamadas devem ser executadas via método POST conforme exemplos em cada API.

A URL base da API é https://contify.com/api/

# Usando a API com JSON

A API suporta apenas JSON, não suportamos outro formato.

# Considerações Iniciais

Afim de garantir a integridade dos dados o processo de integração, deve seguir executar as operações da API na seguinte ordem no momento da emissão do carne leão.

1. Usuário
2. Dependentes
3. Lançamentos
4. Processamento


## Criar usuário

### Requisição

`POST /user/insert`

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
      "u_city": "Joinville"
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

$url = 'https://contify.com.br/api/user/insert';

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

## Obter dados do usuário

### Requisição

`POST /user/read`

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

$url = 'https://contify.com.br/api/dependent/insert';

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

$url = 'https://contify.com.br/api/launch/insert';

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

$url = 'https://contify.com.br/api/process/process';

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

$url = 'https://contify.com.br/api/process/read';

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
