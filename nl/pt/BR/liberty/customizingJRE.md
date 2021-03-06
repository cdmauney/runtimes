---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-11"
subcollection: "liberty"

---

{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Customizar o JRE
{: #customizing_jre}

Os aplicativos executados em um Java Runtime Environment (JRE) que é fornecido e configurado pelo buildpack Liberty. O buildpack Liberty também
possibilita configurar a versão ou tipo de JRE,
customizar as opções da JVM ou sobrepor as funções do JRE.

## {{site.data.keyword.IBM_notm}}JRE

Por padrão, os aplicativos são configurados para executar com uma versão leve do {{site.data.keyword.IBM}} JRE. Esse
JRE leve é dividido para fornecer função principal essencial com um disco
e área de cobertura da memória muito reduzidos. Para obter mais informações sobre o
conteúdo do JRE leve, consulte
[Small Footprint JRE](http://www.ibm.com/support/knowledgecenter/SSYKE2_8.0.0/com.ibm.java.lnx.80.doc/user/small_jre.html).

 {{site.data.keyword.IBM_notm}}JRE versão 8 é usado por padrão. Use a variável de ambiente JBP_CONFIG_IBMJDK para especificar uma versão alternativa do {{site.data.keyword.IBM_notm}} JRE. Por exemplo, para usar o
{{site.data.keyword.IBM_notm}} JRE 7.1 mais recente, configure a seguinte variável de ambiente:
```
    ibmcloud cf set-env myapp JBP_CONFIG_IBMJDK "version: 1.7.+"
```
{: codeblock}

A
propriedade da versão pode ser configurada para um intervalo de versão. Existem dois intervalos
de versão suportados: 1.7.+ e 1.8.+. Para obter os melhores resultados, use Java 8.  O Java 7 foi removido do buildpack do Liberty na versão 3.19.  

## OpenJDK
{: #openjdk}

Opcionalmente, os aplicativos podem ser
configurados para execução com o OpenJDK como o JRE. Para ativar um aplicativo para execução com o OpenJDK, configure a variável de ambiente da JVM (Java virtual machine) como "openjdk". Por exemplo, usando a ferramenta de linha de comandos {{site.data.keyword.Bluemix_notm}}, execute o comando:
```
    ibmcloud cf set-env myapp JVM 'openjdk'
```
{: codeblock}

Se ativado, o OpenJDK versão 8 é usado, por padrão. Use a variável de ambiente JBP_CONFIG_OPENJDK para especificar uma versão alternativa do OpenJDK. Por exemplo, para usar o mais recente OpenJDK 7,
configure a variável de ambiente a seguir:
```
    ibmcloud cf set-env myapp JBP_CONFIG_OPENJDK "version: 1.7.+"
```
{: codeblock}

A propriedade da versão pode ser configurada para um intervalo de versão como 1.7.+, 1.8.+ ou qualquer versão específica listada na [lista de versões disponíveis do OpenJDK](https://download.run.pivotal.io/openjdk/lucid/x86_64/index.yml). Para obter os melhores resultados, use Java 8.

## OpenJ9
{: #openj9}

Opcionalmente, os aplicativos podem ser configurados para execução com o OpenJ9, como o JRE ou o JDK. Para permitir que um aplicativo seja executado com o OpenJ9, configure a variável de ambiente da JVM como "openj9". Por exemplo, usando a ferramenta de linha de comandos {{site.data.keyword.Bluemix_notm}}, execute o comando:
```
    ibmcloud cf set-env myapp JVM 'openj9'
```
{: codeblock}

Se ativado, o OpenJ9 versão 11 será usado por padrão. Use a variável de ambiente JBP_CONFIG_OPENJ9 para especificar uma versão alternativa do OpenJ9. Por exemplo, para usar o OpenJ9 8 mais recente, configure a variável de ambiente a seguir:
```
    ibmcloud cf set-env myapp JBP_CONFIG_OPENJ9 "version: 8.+"
```
{: codeblock}

Se ativado, o OpenJ9 JRE será usado por padrão. Use a variável de ambiente JBP_CONFIG_OPENJ9 para usar a versão JDK do OpenJ9. Por exemplo, para usar o JDK do OpenJ9, configure a variável de ambiente a seguir:
```
    ibmcloud cf set-env myapp JBP_CONFIG_OPENJ9 "type: jdk"
```
{: codeblock}

## Oracle JRE
{: #oracle_jre}

Consulte [Usando seu próprio JRE](/docs/runtimes/liberty/using_own_jre.html) para obter informações sobre o uso do Oracle JRE.

## Configurando as opções do JRE
{: #configuring_jre}

### Configuração padrão da JVM
{: #jvm_default_config}

O buildpack Liberty configura as opções da JVM
padrão considerando:

* Um limite de memória de um aplicativo.  As configurações de heap da JVM aplicadas
são calculadas com base em:
  * o limite de memória de um aplicativo, conforme explicado em [Limites de memória e o buildpack do Liberty](/docs/runtimes/liberty/memoryLimits.html#memory_limits)
  * o tipo de JRE, uma vez que as opções relacionadas ao heap para a JVM variam de acordo
com as opções suportadas do JRE.

* Os [recursos do Liberty suportados no {{site.data.keyword.Bluemix_notm}}](/docs/runtimes/liberty/libertyFeatures.html#libertyfeatures).
  * As transações do banco de dados global two-phase commit não são suportadas no {{site.data.keyword.Bluemix_notm}} e, portanto, são desativadas configurando -Dcom.ibm.tx.jta.disable2PC=true.

* O ambiente do {{site.data.keyword.Bluemix_notm}}.

    As opções da JVM são configuradas para fornecer otimização em um ambiente do {{site.data.keyword.Bluemix_notm}} e para ajudar os diagnósticos de condições de erro relacionado à memória.
  * a recuperação rápida de falha de um aplicativo é configurada desativando
as opções de dumps da JVM e eliminando os processos quando a memória de um
aplicativo está esgotada.
  * Ajuste de virtualização (somente {{site.data.keyword.IBM_notm}} JRE).
  * roteamento de informações nos recursos de memória disponível do aplicativo
no momento da falha para o Loggregator.
  * se um aplicativo estiver configurado para ativar os dumps de memória da JVM, o encerramento de processos Java será desativado e os dumps de memória da JVM serão roteados para um diretório "dumps" de aplicativo comum. Esses dumps podem então ser visualizados do painel do {{site.data.keyword.Bluemix_notm}} ou da CLI do {{site.data.keyword.Bluemix_notm}}.

A seguir está uma configuração da JVM padrão de exemplo que é gerada pelo buildpack para um aplicativo que é implementado com um Limite de memória de 512 M:

```
    -Xtune:virtualized
    -Xmx384M
    -Xdump:none
    -Xdump:heap:defaults:file=../../../../../dumps/heapdump.%Y%m%d.%H%M%S.%pid.%seq.phd
    -Xdump:java:defaults:file=../../../../../dumps/javacore.%Y%m%d.%H%M%S.%pid.%seq.txt
    -Xdump:snap:defaults:file=../../../../../dumps/Snap.%Y%m%d.%H%M%S.%pid.%seq.trc
    -Xdump:tool:events=systhrow,filter=java/lang/OutOfMemoryError,request=serial+exclusive,exec=../../../../.buildpack-diagnostics/killjava.sh
    -Dcom.ibm.tx.jta.disable2PC=true
```
{: codeblock}

### Customizando a configuração da JVM
{: #customizing_jvm}

Os aplicativos podem customizar as opções
da JVM com as especificações que são definidas pelo JRE configurado
para o aplicativo. Referencie as diretrizes sobre como especificar uma
opção e seu uso diretamente da documentação do JRE, uma vez
que as opções variam de acordo com o JRE.

<table>
<tr>
<th align="left">JRE</th>
<th align="left">Formato de opções da linha de comandos</th>
<th align="left">Referência</th>
</tr>

<tr>
<td> {{site.data.keyword.IBM_notm}}JRE</td>
<td>inclui opções de tempo de execução (prefixadas por -X), quaisquer propriedades de sistema do Java (prefixadas com -D) e não recomenda -XX para o uso casual (essas opções estão sujeitas à mudança)
</td>
<td>[Opções da linha de comandos da Versão 8](http://www-01.ibm.com/support/knowledgecenter/SSYKE2_8.0.0/com.ibm.java.lnx.80.doc/diag/appendixes/cmdline/cmdline.html), [Opções da linha de comandos da Versão 7](http://www-01.ibm.com/support/knowledgecenter/SSYKE2_7.0.0/com.ibm.java.lnx.70.doc/diag/appendixes/cmdline/cmdline.html)
</td>
</tr>

<tr>
<td> OpenJDK </td>
<td>é baseado no tempo de execução do HotSpot que possui a notação de
-X para não padrão, -XX para opções do desenvolvedor e sinalizações Booleanas
para ativar ou desativar a opção </td>
<td>[Visão geral do tempo de execução do HotSpot ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://openjdk.java.net/groups/hotspot//docs/RuntimeOverview.html) </td>
</tr>
</table>

Um aplicativo que requer opções customizadas da JVM pode configurar a opção como um valor para uma das variáveis de ambiente IBM_JAVA_OPTIONS, JAVA_OPTS ou JVM_ARGS no {{site.data.keyword.Bluemix_notm}}. Consulte a seção Variáveis de ambiente sobre como configurar a variável de ambiente de um aplicativo. Um servidor em pacote ou um diretório do servidor também pode incluir um arquivo jvm.options que contém as opções da linha de comandos em vez de configurar uma variável de ambiente.

Quando as opções da JVM são aplicadas ao JRE, as opções padrão
do buildpack Liberty são aplicadas primeiro, seguidas pelas opções
customizadas. As opções customizadas são anexadas em uma ordem específica,
que é listada na tabela. A sequência das opções Java aplicadas define quais opções têm precedência. As opções que são aplicadas por último têm precedência sobre as opções
que foram aplicadas anteriormente.

Nota: algumas opções podem não entrar em vigor a menos que a opção seja acionada por um agente.

<table>
<tr>
<th align="left">Ordem aplicada</th>
<th align="left">Configuração de opções da JVM do aplicativo</th>
<th align="left">Descrição</th>
<th align="left">Tipo de aplicativo suportado</th>
<th align="left">Para atualizar a opção JVM de um aplicativo implementado</th>
<th align="left">Aplicável ao JRE do OpenJDK</th>
</tr>

<tr>
<td>1</td>
<td>IBM_JAVA_OPTIONS</td>
<td>Uma variável de ambiente que é suportada pelo {{site.data.keyword.IBM_notm}} JRE</td>
<td>Todos</td>
<td>Reiniciar ou estagiar novamente o aplicativo</td>
<td>Não</td>
</tr>

<tr>
<td>2</td>
<td>JAVA_OPTS</td>
<td>uma variável de ambiente por meio da estrutura de Opções Java do buildpack do Liberty</td>
<td>Todos</td>
<td>Remontar o app</td>
<td>Sim</td>
</tr>

<tr>
<td>3</td>
<td>jvm.options</td>
<td>um arquivo de configuração da JVM que é suportado pelo servidor em pacote ou
diretório do servidor do tempo de execução do Liberty</td>
<td>Pacote do servidor</td>
<td>Remontar o app</td>
<td>Sim</td>
</tr>

<tr>
<td>4</td>
<td>JVM_ARGS</td>
<td>uma variável de ambiente que é suportada pelo tempo de execução do
Liberty</td>
<td>Todos</td>
<td>Reiniciar ou remontar o app</td>
<td>Sim</td>
</tr>
</table>

### Determinando as opções da JVM aplicadas de um aplicativo em execução
{: #determining_applied_jvm_options}

Exceto para opções definidas pelo aplicativo que são especificadas com a variável de ambiente JVM_ARGS, as opções resultantes são persistidas no ambiente de tempo de execução como opções da linha de comandos (aplicativos Java independentes) ou em um arquivo	`jvm.options` (aplicativos Java não independentes). As opções da JVM aplicadas para o aplicativo podem ser visualizadas por meio do console do
{{site.data.keyword.Bluemix_notm}} ou da CLI do {{site.data.keyword.Bluemix_notm}}.

As opções da JVM para aplicativo Java independente
são persistidas como opções da linha de comandos. Elas podem ser visualizadas por meio do arquivo `staging_info.yml`.

Para visualizar o arquivo `staging_info.yml` em um aplicativo em execução em um nó DEA, execute:

```
    ibmcloud cf files myapp staging_info.yml
```
{: codeblock}

Para visualizar o arquivo `staging_info.yml` em um aplicativo em execução em uma célula do Diego, execute:

```
    ibmcloud cf ssh myapp -c "cat staging_info.yml"
```
{: codeblock}

As opções da JVM para WAR, EAR, diretório do servidor e implementação do servidor em pacote são persistidas em um arquivo `jvm.options`. O arquivo `jvm.options` pode ser localizado no diretório `app/wlp/usr/servers/<serverName>/`. Na maioria dos casos, o  `<serverName>` é configurado como `defaultServer`, a menos que um servidor compactado tenha sido implementado com um nome de servidor diferente. Por exemplo:

Para visualizar o arquivo `jvm.options` em um aplicativo em execução em um nó DEA, execute:

```
    ibmcloud cf files myapp app/wlp/usr/servers/defaultServer/jvm.options
```
{: codeblock}

Para visualizar o arquivo `jvm.options` em um aplicativo em execução em uma célula do Diego, execute:

```
    ibmcloud cf ssh myapp -c "cat app/wlp/usr/servers/defaultServer/jvm.options"
```
{: codeblock}


#### Exemplo de uso
{: #example_usage}

Implementando um aplicativo com opções customizadas da JVM para ativar a criação de log de coleta de lixo detalhada do {{site.data.keyword.IBM_notm}} JRE:
* As opções da JVM incluídas no arquivo `manifest.yml` de um aplicativo:

```
    env:
      JAVA_OPTS: "-verbose:gc -Xverbosegclog:./verbosegc.log,10,1000"
```
{: codeblock}

* Para visualizar o arquivo de log de coleta de lixo detalhada gerado pela JVM em um aplicativo em execução em um nó DEA, execute:

```
    ibmcloud cf files myapp app/wlp/usr/servers/defaultServer/verbosegc.log.001
```
{: codeblock}

* Para visualizar o arquivo de log de coleta de lixo detalhada gerado pela JVM em um aplicativo em execução em uma célula do Diego, execute:

```
    ibmcloud cf ssh myapp -c "cat app/wlp/usr/servers/defaultServer/verbosegc.log.001"
```
{: codeblock}

* Para atualizar a opção {{site.data.keyword.IBM_notm}} JRE de um aplicativo implementado para acionar um heap, snap e javacore em uma condição OutOfMemory, configure a variável de ambiente do aplicativo com a opção JVM e reinicie o aplicativo:

```
    ibmcloud cf set-env myapp JVM_ARGS '-Xdump:heap+java+snap:events=systhrow,filter=java/lang/OutOfMemoryError'
    ibmcloud cf restart myapp
```
{: codeblock}

 Veja a documentação de [Criação de log e rastreio](/docs/runtimes/liberty/loggingAndTracing.html#download_dumps) para obter detalhes sobre como visualizar e fazer download de arquivos de dump gerados.

### Sobrepondo o JRE
{: #overlaying_jre}

Existem alguns casos em que os arquivos precisam ser empacotados com o JRE para expor suas funções. O desenvolvedor de aplicativos pode fornecer arquivos JRE para customização.

Os arquivos a serem sobrepostos podem ser colocados em pacote com o WAR, EAR ou JAR do aplicativo em uma pasta de recursos na raiz do archive. Para um servidor (arquivo compactado ou diretório do servidor), os arquivos podem ser colocados em pacote em uma pasta de recursos no diretório do servidor, com o arquivo server.xml.

* Arquivo WAR
  * WEB-INF
  * recursos
    * outros arquivos
    * .java-overlay


* Arquivo EAR
  * META-INF
  * recursos
    * outros arquivos
    * .java-overlay


* Arquivo JAR
  * META-INF
  * recursos
    * outros arquivos
    * .java-overlay


* server_name DIR
  * apps
  * dropins
  * server.xml
  * recursos
    * outros arquivos
    * .java-overlay

O diretório .java-overlay contém arquivos específicos na mesma hierarquia de arquivo que o Java JRE que está sendo sobreposto iniciando com .java/jre.

Por exemplo, se você desejar usar a criptografia AES de 256 bits, será necessário sobrepor estes arquivos de políticas Java:

```
    .java\jre\lib\security\US_export_policy.jar
    .java\jre\lib\security\local_policy.jar
```
{: codeblock}

Faça o download dos arquivos de políticas sem restrições apropriados e inclua-os em seu aplicativo como:

```
    resources\.java-overlay\.java\jre\lib\security\US_export_policy.jar
    resources\.java-overlay\.java\jre\lib\security\local_policy.jar
```
{: codeblock}

Ao enviar seu aplicativo por push, esses jars sobrepõem os jars de política padrão no tempo de execução do Java. Esse processo ativa a criptografia AES de 256 bits.
