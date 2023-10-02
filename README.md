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
