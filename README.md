# Introdução

A API de integração Contify é um serviço que permite a invocação de procedimentos remotos que inserem dados no sistema Contify afim de gerar o Darf (Carne Leão).

# Autenticação

A API utiliza para validação dos parceiros o esquema de TOKEN que é fornecido ao parceiro após a firmação do contrato. 
Todas as chamadas devem ser executadas via método POST conforme exemplos em cada API.

# Considerações Iniciais

Afim de garantir a integridade dos dados o processo de integração, deve seguir executar as operações da API na seguinte ordem no momento da emissão do carne leão.

1 - Usuários
2 - Dependentes
3 - Contatos
4 - Lançamentos
5 - Processamento

# Integração

## Usuários

### Requisição

`POST /user/insert`

### Exemplo chamada em PHP

```
<?php
header('Content-Type: application/json');

$postdata = http_build_query(
    array(
	    array(
			't_name'            => 'Mário',
			't_clinicalName'    => null,
			't_hasEmployee'     => true,
			't_quantityEmployee'=> 5,
			'u_fullName' 	      => 'DINOSOURA DA SILVA SAURO',
			'u_phone'  	        => '11 98585-7744',
			'u_mail'      	    => 'roger.sauro@gmail.com',
			'u_cpf'             => '081.055.870-09',
			'u_cep'       	    => '03040-555',
			'u_address'         => 'Rua Frederico Machado',
			'u_number'    	    => '158',
			'u_complement'	    => 'Fundos',
			'u_state'     	    => 'SP',
			'u_city'      	    => 'São Paulo',
			'u_district'  	    => 'Centro',
			'u_mailCode'  	    => '',),
		array('token'         => ''),				
    )
);
$options = array(
			'http' => array(
			'method'=>"POST",
			'content' => $postdata,
			'header'  => 'Content-type: application/x-www-form-urlencoded',
	),
);

$url = 'https://contify.com.br/api/user/insert.php';
$content = file_get_contents($url, NULL, stream_context_create($options));
print_r(json_decode($content));
```





