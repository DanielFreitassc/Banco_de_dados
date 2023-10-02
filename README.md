# Banco de Dados
> Jorge

# Azure para Criação do Banco de Dados na Nuvem. ☁️

# Primeira Apresentação foi sobre NoSQL, no dia 14/08
## NoSQL
- Estruturas Flexíveis
- Escalabilidade Horizontal e Vertical
- Baixa Latência | Alta Velocidade [Fonte](https://tudosobrehospedagemdesites.com.br/latencia-servidor-o-que-e-como-medir/)
- [Big Data](https://www.oracle.com/br/big-data/what-is-big-data/)
- Sem Garantia Transacional
- Not Only SQL (Não Somente SQL)

# Na Aula do dia 21.
 Vimos Modelo Entidade Relacionado - DER  Em seguida construimos um modelo nosso no site [BR Modelo Web](https://www.brmodeloweb.com/lang/pt-br/index.html)

![Exemplo](https://github.com/DanielFreitassc/Banco_de_dados/assets/129224303/64556b0c-078e-44ed-a8d9-442f1989ce84)

Em seguida vimos Modelo Lógico.
![modelo](https://github.com/DanielFreitassc/Banco_de_dados/assets/129224303/a99cf7af-fc3e-46d8-82f3-1e7613925a54)

# Criação de um banco de dados azure.
```
https://learn.microsoft.com/pt-br/training/modules/build-serverless-api-with-functions-api-management/5-exercise-import-additional-functions-existing-api-gateway
```
- Acesse o link acima.
- Clique em Ativar área restrita.
- Examinar permissões
- Aceitar
- Acesse o link a baixo.
```
https://portal.azure.com/#home 
````
- Clique no seu nome.
- Mude o diretório(Microsoft Learn SandBox)
- Pesquise [por SQL DO AZURE](https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.Sql%2Fazuresql)
- Clique em criar
- Banco de dados individual e criar.
- Grupo de Recursos learn-e660a5a0-a67c-40b8-9a9f-25f61e6fa188
- Nome do banco de dados: Nome de sua preferência
- Ambiente de carga de trabalho: Produção
- Servior.
- Clique em criar embaixo de servidor
- Nome do Servidor: Nome unico que não pode se repetir.
- Localização: Brazil
- Método de autenticação: Usar autenticação SQL
- senha: senha1234
- Revisar e criar
- Computação armazenamento: Configurar banco de dados
- Sem servidor
- Desativar Atraso de pausa automática
- Redundância do armaenamento de backup: Armazenamento de backup com redundância local
- Revisar + Criar
- Vá para [SQL DO AZURE](https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.Sql%2Fazuresql)
- Nome do servidor deve estar em SERVER-SQL
- Agora libera [FireWall](https://portal.azure.com/#@learn.docs.microsoft.com/resource/subscriptions/155cbeab-3d1b-425f-bc74-00626dabdb12/resourceGroups/learn-d7bdbf91-2d2f-4bd4-88c2-709b8fcec0d8/providers/Microsoft.Sql/servers/dan-server-sql/networking)




```
/*==============================================================*/
/* DBMS name:      Microsoft SQL Server 2008                    */
/* Created on:     02/10/2023 19:10:13                          */
/*==============================================================*/


if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('apolice') and o.name = 'fk_cliente__apolice')
alter table apolice
   drop constraint fk_cliente__apolice
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('apolice') and o.name = 'fk_carro__apolice')
alter table apolice
   drop constraint fk_carro__apolice
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('sinistro') and o.name = 'fk_carro__sinistro')
alter table sinistro
   drop constraint fk_carro__sinistro
go

if exists (select 1
            from  sysobjects
           where  id = object_id('apolice')
            and   type = 'U')
   drop table apolice
go

if exists (select 1
            from  sysobjects
           where  id = object_id('carro')
            and   type = 'U')
   drop table carro
go

if exists (select 1
            from  sysobjects
           where  id = object_id('cliente')
            and   type = 'U')
   drop table cliente
go

if exists (select 1
            from  sysobjects
           where  id = object_id('sinistro')
            and   type = 'U')
   drop table sinistro
go

/*==============================================================*/
/* Table: apolice                                               */
/*==============================================================*/
create table apolice (
   cod_apolice          int                  not null,
   cod_cliente          int                  not null,
   data_inicio_vigencia datetime             null,
   data_fim_vigencia    datetime             null,
   valor_cobertura      numeric(10,2)        null,
   valor_franquia       numeric(10,2)        null,
   placa                char(10)             null,
   constraint pk_apolice primary key nonclustered (cod_apolice)
)
go

/*==============================================================*/
/* Table: carro                                                 */
/*==============================================================*/
create table carro (
   placa                char(10)             not null,
   modelo               varchar(50)          null,
   chassi               varchar(30)          not null,
   marca                varchar(30)          null,
   ano                  int                  null,
   cor                  varchar(10)          null,
   constraint pk_carro primary key nonclustered (placa)
)
go

/*==============================================================*/
/* Table: cliente                                               */
/*==============================================================*/
create table cliente (
   cod_cliente          int                  not null,
   nome                 varchar(50)          null,
   cpf                  char(11)             not null,
   sexo                 char(1)              null,
   endereco             varchar(200)         null,
   telefone_fixo        varchar(10)          null,
   telefone_celular     varchar(11)          null,
   constraint pk_cliente primary key nonclustered (cod_cliente)
)
go

/*==============================================================*/
/* Table: sinistro                                              */
/*==============================================================*/
create table sinistro (
   cod_sinistro         int                  not null,
   placa                char(10)             not null,
   data_sinistro        datetime             null,
   hora_sinistro        datetime             null,
   local_sinistro       varchar(100)         null,
   condutor             varchar(50)          null,
   constraint pk_sinistro primary key nonclustered (cod_sinistro)
)
go

alter table apolice
   add constraint fk_cliente__apolice foreign key (cod_cliente)
      references cliente (cod_cliente)
go

alter table apolice
   add constraint fk_carro__apolice foreign key (placa)
      references carro (placa)
go

alter table sinistro
   add constraint fk_carro__sinistro foreign key (placa)
      references carro (placa)
go

```
```
/*==============================================================*/
/* DBMS name:      Microsoft SQL Server 2008                    */
/* Created on:     02/10/2023 19:05:48                          */
/*==============================================================*/


if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('avaliacao_aluno') and o.name = 'fk_aluno__avaliacao_aluno')
alter table avaliacao_aluno
   drop constraint fk_aluno__avaliacao_aluno
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('avaliacao_aluno') and o.name = 'fk_avaliacao__avaliacao_aluno')
alter table avaliacao_aluno
   drop constraint fk_avaliacao__avaliacao_aluno
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('questao') and o.name = 'fk_avaliacao__questao')
alter table questao
   drop constraint fk_avaliacao__questao
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('questao_item') and o.name = 'fk_questao__questao_item')
alter table questao_item
   drop constraint fk_questao__questao_item
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('resposta_aberta') and o.name = 'fk_avaliacao_aluno__resposta_aberta')
alter table resposta_aberta
   drop constraint fk_avaliacao_aluno__resposta_aberta
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('resposta_aberta') and o.name = 'fk_questao__resposta_aberta')
alter table resposta_aberta
   drop constraint fk_questao__resposta_aberta
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('resposta_fechada') and o.name = 'fk_avaliacao_aluno__resposta_fechada')
alter table resposta_fechada
   drop constraint fk_avaliacao_aluno__resposta_fechada
go

if exists (select 1
   from sys.sysreferences r join sys.sysobjects o on (o.id = r.constid and o.type = 'F')
   where r.fkeyid = object_id('resposta_fechada') and o.name = 'fk_questao_item__resposta_fechada')
alter table resposta_fechada
   drop constraint fk_questao_item__resposta_fechada
go

if exists (select 1
            from  sysobjects
           where  id = object_id('aluno')
            and   type = 'U')
   drop table aluno
go

if exists (select 1
            from  sysobjects
           where  id = object_id('avaliacao')
            and   type = 'U')
   drop table avaliacao
go

if exists (select 1
            from  sysobjects
           where  id = object_id('avaliacao_aluno')
            and   type = 'U')
   drop table avaliacao_aluno
go

if exists (select 1
            from  sysobjects
           where  id = object_id('questao')
            and   type = 'U')
   drop table questao
go

if exists (select 1
            from  sysobjects
           where  id = object_id('questao_item')
            and   type = 'U')
   drop table questao_item
go

if exists (select 1
            from  sysobjects
           where  id = object_id('resposta_aberta')
            and   type = 'U')
   drop table resposta_aberta
go

if exists (select 1
            from  sysobjects
           where  id = object_id('resposta_fechada')
            and   type = 'U')
   drop table resposta_fechada
go

/*==============================================================*/
/* Table: aluno                                                 */
/*==============================================================*/
create table aluno (
   cd_aluno             int                  identity,
   nm_aluno             varchar(100)         not null,
   email                varchar(100)         not null,
   constraint pk_aluno primary key (cd_aluno)
)
go

/*==============================================================*/
/* Table: avaliacao                                             */
/*==============================================================*/
create table avaliacao (
   cd_avaliacao         int                  identity,
   ds_avaliacao         varchar(100)         not null,
   dt_abertura          datetime             not null,
   dt_fechamento        datetime             not null,
   constraint pk_avaliacao primary key (cd_avaliacao)
)
go

/*==============================================================*/
/* Table: avaliacao_aluno                                       */
/*==============================================================*/
create table avaliacao_aluno (
   cd_avaliacao         int                  not null,
   cd_aluno             int                  not null,
   ds_avaliacao_aluno   varchar(100)         not null,
   dt_inicio            datetime             not null default getdate(),
   dt_fim               datetime             null,
   constraint pk_avaliacao_aluno primary key (cd_aluno, cd_avaliacao)
)
go

/*==============================================================*/
/* Table: questao                                               */
/*==============================================================*/
create table questao (
   cd_questao           int                  identity,
   cd_avaliacao         int                  not null,
   ds_questao           varchar(max)         not null,
   tp_questao           tinyint              not null
      constraint ckc_tp_questao_questao check (tp_questao in (1,2,3)),
   constraint pk_questao primary key (cd_questao)
)
go

/*==============================================================*/
/* Table: questao_item                                          */
/*==============================================================*/
create table questao_item (
   cd_questao_item      int                  identity,
   cd_questao           int                  not null,
   ds_questao_item      varchar(max)         not null,
   is_correta           bit                  not null,
   constraint pk_questao_item primary key (cd_questao_item)
)
go

/*==============================================================*/
/* Table: resposta_aberta                                       */
/*==============================================================*/
create table resposta_aberta (
   cd_avaliacao         int                  not null,
   cd_aluno             int                  not null,
   cd_questao           int                  not null,
   ds_resposta_aberta   varchar(max)         null,
   dt_resposta          datetime             not null,
   constraint pk_resposta_aberta primary key (cd_questao, cd_aluno, cd_avaliacao)
)
go

/*==============================================================*/
/* Table: resposta_fechada                                      */
/*==============================================================*/
create table resposta_fechada (
   cd_avaliacao         int                  not null,
   cd_aluno             int                  not null,
   cd_questao_item      int                  not null,
   dt_resposta          datetime             not null,
   constraint pk_resposta_fechada primary key (cd_questao_item, cd_aluno, cd_avaliacao)
)
go

alter table avaliacao_aluno
   add constraint fk_aluno__avaliacao_aluno foreign key (cd_aluno)
      references aluno (cd_aluno)
go

alter table avaliacao_aluno
   add constraint fk_avaliacao__avaliacao_aluno foreign key (cd_avaliacao)
      references avaliacao (cd_avaliacao)
go

alter table questao
   add constraint fk_avaliacao__questao foreign key (cd_avaliacao)
      references avaliacao (cd_avaliacao)
go

alter table questao_item
   add constraint fk_questao__questao_item foreign key (cd_questao)
      references questao (cd_questao)
go

alter table resposta_aberta
   add constraint fk_avaliacao_aluno__resposta_aberta foreign key (cd_aluno, cd_avaliacao)
      references avaliacao_aluno (cd_aluno, cd_avaliacao)
go

alter table resposta_aberta
   add constraint fk_questao__resposta_aberta foreign key (cd_questao)
      references questao (cd_questao)
go

alter table resposta_fechada
   add constraint fk_avaliacao_aluno__resposta_fechada foreign key (cd_aluno, cd_avaliacao)
      references avaliacao_aluno (cd_aluno, cd_avaliacao)
go

alter table resposta_fechada
   add constraint fk_questao_item__resposta_fechada foreign key (cd_questao_item)
      references questao_item (cd_questao_item)
go

```
