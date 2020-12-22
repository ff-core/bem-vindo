# Bem vindo ao ff-Core

A **ff-core** é um biblioteca de **código aberto** que facilita a vida e aumenta a performance do desenvolvedor. Com poucas linhas e simples de codifica-las você cria um CRUD completo e estável.

## Objetivo da ff-Core

Nosso objetivo é criar componentes para os desenvolvedores criarem softwares de alta performance com agilidade para seus clientes.

## Exemplo ao vivo

Veja o funcionamento do componente ao vivo. [Click aqui!](https://matheusmello.info/live/public/Exemples/Escritorios)

## Requisitos do ff-Core

A **ff-core** é uma biblioteca em PHP 7.4 utilizando do Framework Codeigniter 4.0.4, Bootstrap 5.0.0 e Javascript.
      
  - Esta biblioteca não é aplicavel a outros Frameworks e outras versões anteriores do Codeigniter.

Suporte ao Banco de Dados Mysql.

## Projeto completo ff-Core

Você pode baixar o projeto completo para ser testador. [Baixe aqui](https://github.com/ff-core/codeigniter4)

A Database se encontra na pasta raiz do projeto para ser importada. `db-exemples.sql`.

É importante configurar o arquivo .env com a conexão da base de dados.

## Instalação e Configuração da biblioteca ff-Core

### CodeIgniter

#### Via composer

```composer
composer create-project codeigniter4/appstarter ffCoreCrud
```
OU

#### Download

```Download
http://codeigniter.com/download
```

### Biblioteca

Clone o projeto [ff-core](https://github.com/ff-core/ff-core.git)

Copie a estrutura de pasta para dentro do projeto CodeIgniter ffCoreCrud (Nome do Projeto criado via composer)

As estruruta de pasta sao idênticas para para este proposita em facilitar a copia dos arquivo ff-core-biblioteca para dentro do projeto CodeIgniter.

```Download
--app
  --Controller
  --ibraries
  --Models
  --Views
--public
  --assets
--db-exemples.sql
```

## Configurações

### CodeIgniter

No CodeIgniter é importante realizar a configuração do .env

1. URL

```env
app.baseURL = 'http://localhost/ffCoreCrud/public'
```

2. DATABASE

```db
 database.default.hostname = localhost
 database.default.database = exemples
 database.default.username = root
 database.default.password = 
 database.default.DBDriver = MySQLi
```

### Database

Na raíz do projeto encontra-se o script com alguns teste realizado. Nome do arquivo com script é db-exemples.sql.

1. Crie a Database com o nome de Exemples

2. Excute os scripts

No desenvolvimento das tabelas é importante se atentar aos seguintes campos:
```
-created_at
-updated_at
-deleted_at
```
Este campos sao obrigatório sua criação para o comportamento da biblioteca funcionar.

## Exemplos

### 1-N

Tabela

```1n
CREATE TABLE `escritorios` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `cidade` varchar(150) NOT NULL,
  `telefone` varchar(50) NOT NULL,
  `endereco` varchar(200) NOT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  `deleted_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4;

CREATE TABLE `empregados` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `id_escritorio` int(11) NOT NULL,
  `nome` varchar(150) NOT NULL,
  `email` varchar(150) NOT NULL,
  `salario` decimal(7,2) NOT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  `deleted_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `FK_EMPREGADOS_ESCRITORIOS` (`id_escritorio`),
  CONSTRAINT `FK_EMPREGADOS_ESCRITORIOS` FOREIGN KEY (`id_escritorio`) REFERENCES `escritorios` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4;

```

Código Fonte

```codigofonte
public function Escritorios()
{	
  $Ct = new Controlador($this);
  $Ct->tabela = 'escritorios';
  $Ct->nome = 'Escritórios';
  $Ct->titulo = 'Cadastrar de Escritórios';

  $Ct->displayAsFiltro([
    'cidade' => 'Cidade'
  ]);
  $Ct->displayAsGride([
    'id' => 'Id', 
    'cidade' => 'Cidade', 
    'telefone' => 'Telefone', 
    'endereco' => 'Endereço'
  ]);
  $Ct->displayAsAdd([
    'id' => 'Id', 
    'cidade' => 'Cidade', 
    'telefone' => 'Telefone', 
    'endereco' => 'Endereço'
  ]);
  $Ct->displayAsEdit([
    'id' => 'Id', 
    'cidade' => 'Cidade', 
    'telefone' => 'Telefone', 
    'endereco' => 'Endereço'
  ]);
  $Ct->displayAsView([
    'id' => 'Id', 
    'cidade' => 'Cidade', 
    'telefone' => 'Telefone', 
    'endereco' => 'Endereço'
  ]);
  $Ct->displayAsDelete(true);

  $Ct->displayAsLink([
    '/Exemples/Funcionarios' => 'Funcionários'
  ]);

  return $Ct->show();
}

public function Funcionarios($id){
  $Ct = new Controlador($this);
  $Ct->tabela = 'empregados';
  $Ct->nome = 'Funcionário';
  $Ct->titulo = 'Cadastrar de Funcionários';

  $Ct->displayAsFiltro([
    'email' => 'E-mail'
  ]);
  $Ct->displayAsGride([
    'id' => 'Id', 
    'nome' => 'Nome', 
    'email' => 'E-mail', 
    'salario' => 'Salário'
  ]);
  $Ct->displayAsAdd([
    'id' => 'Id', 
    'nome' => 'Nome', 
    'email' => 'E-mail', 
    'salario' => 'Salário'
  ]);
  $Ct->displayAsEdit([
    'id' => 'Id', 
    'nome' => 'Nome', 
    'email' => 'E-mail', 
    'salario' => 'Salário'
  ]);
  $Ct->displayAsView([
    'id' => 'Id', 
    'nome' => 'Nome', 
    'email' => 'E-mail', 
    'salario' => 'Salário'
  ]);
  $Ct->displayAsDelete(true);

  $Ct->fieldNameDefaultValue(['id_escritorio' => $id]);
  $Ct->where(['id_escritorio' => $id]);

  return $Ct->show();
}
```
### N-N

Tabela

```nn
CREATE TABLE `atores` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `nome` varchar(255) NOT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  `deleted_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4;

CREATE TABLE `filme` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `titulo` varchar(255) NOT NULL,
  `ano` varchar(4) DEFAULT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  `deleted_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4;

CREATE TABLE `filme_atores` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `id_filme` int(11) NOT NULL,
  `id_atores` int(11) NOT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  `deleted_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `FK_FILME` (`id_filme`),
  KEY `FK_ATORES` (`id_atores`),
  CONSTRAINT `FK_ATORES` FOREIGN KEY (`id_atores`) REFERENCES `atores` (`id`),
  CONSTRAINT `FK_FILME` FOREIGN KEY (`id_filme`) REFERENCES `filme` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

Código Fonte

```codigofonte
public function Filmes(){
  $Ct = new Controlador($this);
  $Ct->tabela = 'filme';
  $Ct->nome = 'Filmes';
  $Ct->titulo = 'Cadastrar Filmes';

  $Ct->displayAsFiltro([
    'ano' => 'Ano'
  ]);
  $Ct->displayAsGride([
    'id' => 'Id', 
    'titulo' => 'Título', 
    'ano' => 'Ano'
  ]);
  $Ct->displayAsAdd([
    'id' => 'Id', 
    'titulo' => 'Título', 
    'ano' => 'Ano'
  ]);
  $Ct->displayAsEdit([
    'id' => 'Id', 
    'titulo' => 'Título', 
    'ano' => 'Ano'
  ]);
  $Ct->displayAsView([
    'id' => 'Id', 
    'titulo' => 'Título', 
    'ano' => 'Ano'
  ]);
  $Ct->displayAsDelete(true);

  $Ct->displayAsLink([
    '/Exemples/FilmAtoresView' => 'Atores do Filme'
  ]);

  return $Ct->show();
}

public function Atores(){
  $Ct = new Controlador($this);
  $Ct->tabela = 'atores';
  $Ct->nome = 'Atores';
  $Ct->titulo = 'Cadastrar de Atores';

  $Ct->displayAsFiltro([
    'nome' => 'Nome'
  ]);
  $Ct->displayAsGride([
    'id' => 'Id', 
    'nome' => 'Nome'
  ]);
  $Ct->displayAsAdd([
    'id' => 'Id', 
    'nome' => 'Nome'
  ]);
  $Ct->displayAsEdit([
    'id' => 'Id', 
    'nome' => 'Nome'
  ]);
  $Ct->displayAsView([
    'id' => 'Id', 
    'nome' => 'Nome'
  ]);
  $Ct->displayAsDelete(true);

  $Ct->displayAsLink([
    '/Exemples/AtoresFilmeView' => 'Filmes do Atores'
  ]);

  return $Ct->show();
}

public function FilmeAtores(){
  $Ct = new Controlador($this);
  $Ct->tabela = 'filme_atores';
  $Ct->nome = 'Filme por Atores';
  $Ct->titulo = 'Cadastrar Atores por Filme';

  $Ct->displayAsGride([
    'id' => 'Id', 
    'id_filme' => 'Filme', 
    'id_atores' => 'Ator'
  ]);
  $Ct->displayAsAdd([
    'id' => 'Id', 
    'id_filme' => 'Filme', 
    'id_atores' => 'Ator'
  ]);
  $Ct->displayAsEdit([
    'id' => 'Id', 
    'id_filme' => 'Filme', 
    'id_atores' => 'Ator'
  ]);
  $Ct->displayAsView([
    'id' => 'Id', 
    'id_filme' => 'Filme', 
    'id_atores' => 'Ator'
  ]);
  $Ct->displayAsDelete(true);

  $Ct->addReferencesKey('id_filme', 'filme', 'id', 'titulo', []);
  $Ct->addReferencesKey('id_atores', 'atores', 'id', 'nome', []);

  return $Ct->show();
}
```

- Observação: As funções devem serem colocar dentro da classe controller conforme a documentação do Codeigniter.
      
## Uso da Classe Controlador

### Uso do Objeto Controlador

```
$Ct = new Controlador($this);
```

#### Propriedades

As propritedas são obrigatórios para realizarem as configuração de tela e banco de dados.

```
$Ct->tabela;
$Ct->nome;
$Ct->titulo;
```
#### Funções

1. displayAsGride
      - Tem como objetivo configurar as colunas da Gride. Através do array associativo é informado o nome do campo da tabela e o nome de exibição na Gride.
```
$Ct->displayAsGride(array);
```

2. displayAsAdd
      - Tem como objetico configurar as colunas no formulário de adição. Através do array associativo é informado o nome do campo da tabela e o nome de exibição na label.
      - Se não informado esta função não é apresentado também seu respctivo botão de adicionar.
```
$Ct->displayAsAdd(array);
```

3. displayAsEdit
      - Tem como objetico configurar as colunas no formulário de edição. Através do array associativo é informado o nome do campo da tabela e o nome de exibição na label.
      - Se não informado esta função não é apresentado também seu respctivo botão de editar na gride na coluna de Ação.
```
$Ct->displayAsEdit(array);
```

4. displayAsView
      - Tem como objetico configurar as colunas no formulário de visualização. Através do array associativo é informado o nome do campo da tabela e o nome de exibição na label.
      - Se não informado esta função não é apresentado também seu respctivo botão de visualizar na gride na coluna de Ação.
```
$Ct->displayAsView(array);
```

5. displayAsDelete
      - Tem como objetico configurar as colunas no formulário de deleção. 
      - Se não informado esta função ou passado o parametro False não é apresentado também seu respctivo botão de deletar na gride na coluna de Ação.
      - O delete é feito atrávez da chave primária.
```
$Ct->displayAsView(bool True|False);
```

6. addReferencesKey
      - O objetivo é onde existir a referencia será apresentado a ligação com a Coluna de Exibição da Tabela Referenciada. E onde é formulário trazer linhas desta tabela Referenciada.
```
$Ct->addReferencesKey(Coluna Referencia, Tabela Referenciada, Coluna PK Referenciada, Coluna de Exibição Referenciada, Where = []);
```

7. fieldNameDefaultValue
      - É utilizada para deixar em seus formulários o campo com um valor default. Através do array associativo é informado o nome do campo e o valor default.
```
$Ct->fieldNameDefaultValue(array);
```

8. where
      - É utilizada para criar alguma condição no filtro da Gride. Através do array associativo é informado o nome do campo e o valor do filtro.
```
$Ct->where(array);
```

9. show
      - É através desta função que renderizado toda a configuração e coloca em HTML para ser manda na view Cadastro que esta recepcionando.
```
return $Ct->show();
```

### Duvidas ou Contato

Para duvidas ou contato segue alguns canais de comunicação.

E-mail: ffcore.contato@gmail.com

E-mail: matheusnarciso@hotmail.com

Skype: matheusmell0
