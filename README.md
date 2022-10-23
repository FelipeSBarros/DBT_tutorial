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

