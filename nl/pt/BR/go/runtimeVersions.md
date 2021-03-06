---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-12"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Versões de runtime
{: #runtime_versions}

## Versões suportadas
{: #supported_versions}

Esse buildpack suporta as versões a seguir; aquelas marcadas como descontinuadas serão removidas em uma liberação de buildpack futura.  Veja [Instrução de suporte da Microsoft para liberações LTS e atuais](https://www.microsoft.com/net/core/support).


### Versão SDK .NET

| Versão SDK .NET        | Padrão          |
|-------------------------|------------------|
| 2.1.403                 |   Sim            |
| 2.1.302                 |   Não             |
| 2.0.3                   |   Não             |
| 1.1.11                  |   Não             |
| 1.1.10                  |   Não             |
| 1.0.4                   |   Não             |


### Versões de runtime do .NET Core

| Versão do runtime do .NET Core | Tipo de Release      |
|---------------------------|-------------------|
| 2.1.5                     | Atual           |  
| 2.1.4                     | LTS               |
| 2.1.2                     | LTS               |
| 2.0.9                     | LTS               |
| 2.0.7                     | LTS               |
| 1.1.10                    | LTS               |
| 1.1.9                     | LTS               |
| 1.0.12                    | LTS               |
| 1.0.11                    | LTS               |


### Versões .NET aspnetcore

| Versão .NET aspnetcore | Tipo de Release      |
|---------------------------|-------------------|
| 2.1.5                     | Atual           |  
| 2.1.4                     | LTS               |
| 2.1.2                     | LTS               |



## Especificando a versão SDK .NET

Controle a versão do .NET SDK com um arquivo `global.json` opcional no diretório-raiz do aplicativo. Por exemplo:
```
   {
      "sdk": {
        "version": "2.1.301"
      }
   }
```
{: codeblock}

Se não for especificado, o conjunto de ferramentas MSBuild mais recente será usado.  Para usar o conjunto de ferramentas
project.json, é possível especificar uma das versões do project.json listadas, mas lembre-se que essas versões serão
removidas no futuro.  Se a versão especificada não for incluída no buildpack, a versão padrão será usada e um aviso aparecerá nos
logs temporários.
