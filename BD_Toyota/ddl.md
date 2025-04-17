#Data Defnition Language
## ddl

-- id INTEGER PRIMARY KEY -> int quando deve-se incrementar o id
-- id SERIAL PRIMARY KEY -> int quando incrementa o id automaticamente


```sql
    CREATE TABLE cliente(
        id SERIAL PRIMARY KEY,
        ativo BOOLEAN NOT NULL DEFAULT TRUE  -- DEFAULT TRUE -> 
    );

    CREATE TABLE pessoa_fisica (
        id INTEGER PRIMARY KEY REFERENCES cliente(id) ON DELETE CASCADE,
        nome VARCHAR(100) NOT NULL,
        CPF VARCHAR(14) UNIQUE NOT NULL,
        data_nascimento DATE NOT NULL
    )

    CREATE TABLE pessoa_juridica (
        id INTEGER PRIMARY KEY REFERENCES cliente(id) ON DELETE CASCADE,
        nome_fantasia VARCHAR(100) NOT NULL,
        razao_social VARCHAR(50) NOT NULL,
        CNPJ VARCHAR(18) UNIQUE NOT NULL,
    )

    CREATE TABLE endereço(
        id SERIAL PRIMARY KEY,
        cliente_id  INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
        logradouro VARCHAR(100) NOT NULL,
        numero VARCHAR(10),
        complemento VARCHAR(100),
        bairro VARCHAR(100),
        cidade VARCHAR(100) NOT NULL,
        estado VARCHAR(2) NOT NULL,
        cep VARCHAR(9) NOT NULL,
        tipo VARCHAR(20) NOT NULL CHECK (tipo IN ('Comercial', 'Residencial')) DEFAULT 'Residencial' --  CHECK -> gera opções  
    )

    CREATE TABLE telefone(
        id SERIAL PRIMARY KEY,
        cliente_id  INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
        ddd VARCHAR(2) NOT NULL,
        numero VARCHAR(10) NOT NULL,
        tipo VARCHAR(12) NOT NULL CHECK (tipo IN ('Movel', 'Fixo', 'Recado')) DEFAULT 'Fixo'
    )

    CREATE TABLE email(
        id SERIAL PRIMARY KEY, 
        cliente_id  INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
        email VARCHAR(200) NOT NULL
    )

    CREATE TABLE veiculo(
        id SERIAL PRIMARY KEY,
        ano VARCHAR(4) NOT NULL,
        cor VARCHAR(50) NOT NULL,
        chassi VARCHAR(20) NOT NULL UNIQUE,
        placa VARCHAR(20) NOT NULL UNIQUE,
    )

    CREATE TABLE acessorios(
        id SERIAL PRIMARY KEY,
        acessorio VARCHAR(50) NOT NULL,
        preco NUMERIC(10,2) NOT NULL,
        descricao VARCHAR(100) NOT NULL
    )

    CREATE TABLE modelo(
        id_modelo SERIAL PRIMARY KEY,
        modelo VARCHAR(50) NOT NULL,
        potencia VARCHAR(10) NOT NULL,
        versao VARCHAR(50) NOT NULL,
        tipo varchar(50) NOT NULL,
    )

    CREATE TABLE veiculo_acessorio(
        veiculo_id INTEGER REFERENCES veiculo(id) ON DELETE CASCADE, -- ON DELETE CASCADE -> qundo o id da tabela pai for excluido o id da tabela filha é excluido tbm
        acessorio_id INTEGER REFERENCES acessorio(id) ON DELETE CASCADE,
        PRIMARY KEY(veiculo_id, acessorio_id)
    )

        CREATE TABLE acessorio_modelo(
        acessorio_id INTEGER REFERENCES acessorios(id) ON DELETE CASCADE,
        modelo_id INTEGER REFERENCES modelo(id) ON DELETE CASCADE,
        PRIMARY KEY(acessorio_id, modelo_id)
    )
```