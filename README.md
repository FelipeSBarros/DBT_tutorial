# Estudando DBT

**Adapters:** DBT conecta e executa SQL em base de dados, *warehouse*, *lake*, or *query engine*. Todas essas *coisas*-SQL são agrupadas em um *bucket*, chamado plataforma de dados (*data platforms*). DBT pode ser extendido a qualquer plataforma de dados usando um *adapter*. Esses *adapters* são criados como módulos python que o `dbt` core identifica se estão instalados no sistema.

**CLI:** `dbt` provê uma interface de linha de comando (CLI) para execução do projeto. A CLI também está disponível para o [`dbt Cloud`](https://github.com/data-mie/dbt-cloud-cli). [Panorama geral da CLI](https://docs.getdbt.com/dbt-cli/cli-overview).


## Instalação

É possível instalar o DBT core e seus complementos por [docker](https://docs.getdbt.com/docs/get-started/docker-installv) ou por [`pip`](https://docs.getdbt.com/docs/get-started/pip-install). Sendo recomendado, para este último, o uso de ambientes virtuais.

### Preparando ambiente de desenvolvimento

**Ambiente virtual Python**:
```
mkdir DBT_tutorial
cd DBT_tutorial
python -m venv .venv
source .venv/bin/activa
pip install --upgrade pip wheel setuptools
```

> Tip: You can create an alias for the source command in your $HOME/.bashrc, $HOME/.zshrc, or whichever rc file your shell draws from. For example, you can add a command like alias env_dbt='source <PATH_TO_VIRTUAL_ENV_CONFIG>/bin/activate', replacing <PATH_TO_VIRTUAL_ENV_CONFIG> with the path to your virtual environment configuration.

Listando docker containers, para certificar existencia de container com postgre
```
docker container ls --all
# 8c928baa5d8d   postgres                     "docker-entrypoint.s…"   5 months ago    Exited (0) 2 days ago                  postgres
```

#### Instalando adapter

Neste estudo, vou usar o adaptador [`postgres`](https://docs.getdbt.com/reference/warehouse-setups/postgres-setup):
```
pip install dbt-postgres
```

Com esse comando serão instalados o `dbt-core` e `dbt-postgres`.

Confirmando a instalação:
```
dbt --version
#Core:
#  - installed: 1.3.0
#  - latest:    1.3.0 - Up to date!

#Plugins:
#  - postgres: 1.3.0 - Up to date!
```

## Criando um projeto

`dbt` core inclui o comando `init` que ajuda a estruturar um projeto `dbt`.
```
dbt init jaffle_shop

02:00:44  Running with dbt=1.3.0
02:00:44  Creating dbt configuration folder at /home/felipe/.dbt
Which database would you like to use?
[1] postgres

(Don't see the one you want? https://docs.getdbt.com/docs/available-adapters)

Enter a number: 1
02:00:55  Profile jaffle_shop written to /home/felipe/.dbt/profiles.yml using target's sample configuration. Once updated, you'll be able to start developing with dbt.
02:00:55  
Your new dbt project "jaffle_shop" was created!

For more information on how to configure the profiles.yml file,
please consult the dbt documentation here:

  https://docs.getdbt.com/docs/configure-your-profile

One more thing:

Need help? Don't hesitate to reach out to us via GitHub issues or on Slack:

  https://community.getdbt.com/

Happy modeling!

```

Resultado:
Conjunto de pastas já estruturadas com alguns arquivos `.sql` e .`yml` com alguns parâmetros pré-configurados:
```
.
├── jaffle_shop
│   ├── analyses
│   ├── dbt_project.yml
│   ├── macros
│   ├── models
│   │   └── example
│   │       ├── my_first_dbt_model.sql
│   │       ├── my_second_dbt_model.sql
│   │       └── schema.yml
│   ├── README.md
│   ├── seeds
│   ├── snapshots
│   └── tests
├── logs
│   └── dbt.log
└── README.md

```

### Confirgurando profile.yml

O arquivo `profile.yml` possui todas as informações e parâmetros necessários para o `dbt` poder acessar a base de dados. Para conhecer os parâmetros necessários a cada caso, veja a pagina de cada `adapter`. Para o caso o [`postgres`](https://docs.getdbt.com/reference/warehouse-setups/postgres-setup);

## dbt debug

```
dbt debug

02:44:16  Running with dbt=1.3.0
dbt version: 1.3.0
python version: 3.9.0
python path: /media/felipe/DATA/Repos/DBT_tutorial/.venv/bin/python
os info: Linux-5.4.0-131-generic-x86_64-with-glibc2.27
Using profiles.yml file at /media/felipe/DATA/Repos/DBT_tutorial/jaffle_shop/profiles.yml
Using dbt_project.yml file at /media/felipe/DATA/Repos/DBT_tutorial/jaffle_shop/dbt_project.yml

Configuration:
  profiles.yml file [OK found and valid]
  dbt_project.yml file [OK found and valid]

Required dependencies:
 - git [OK found]

Connection:
  host: localhost
  port: 5433
  user: postgres
  database: dbt_tutorial
  schema: public
  search_path: None
  keepalives_idle: 0
  sslmode: None
  Connection test: [OK connection ok]

All checks passed!
```

## dbt run

```
dbt run

02:45:43  Running with dbt=1.3.0
02:45:43  Partial parse save file not found. Starting full parse.
02:45:43  Found 2 models, 4 tests, 0 snapshots, 0 analyses, 289 macros, 0 operations, 0 seed files, 0 sources, 0 exposures, 0 metrics
02:45:43  
02:45:44  Concurrency: 1 threads (target='dev')
02:45:44  
02:45:44  1 of 2 START sql table model public.my_first_dbt_model ......................... [RUN]
02:45:44  1 of 2 OK created sql table model public.my_first_dbt_model .................... [SELECT 2 in 0.11s]
02:45:44  2 of 2 START sql view model public.my_second_dbt_model ......................... [RUN]
02:45:44  2 of 2 OK created sql view model public.my_second_dbt_model .................... [CREATE VIEW in 0.06s]
02:45:44  
02:45:44  Finished running 1 table model, 1 view model in 0 hours 0 minutes and 0.28 seconds (0.28s).
02:45:44  
02:45:44  Completed successfully
02:45:44  
02:45:44  Done. PASS=2 WARN=0 ERROR=0 SKIP=0 TOTAL=2
```

## Criando modelo

Para criar um novo modelo, basta criar um arquivo `.sql` na pasta `models`. Exemplo: `models/customer.sql`
