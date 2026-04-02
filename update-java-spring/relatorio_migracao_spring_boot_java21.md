# Relatório de Modernização Tecnológica

## Atualização para Spring Boot 4.0.4 e Java 21

------------------------------------------------------------------------

# 1. Visão Geral da Atualização

Este documento descreve o processo de modernização tecnológica realizado
na aplicação, envolvendo a atualização do ambiente de execução e
framework principal.

A atualização representa um **salto geracional significativo**, migrando
de tecnologias consideradas legadas, como **Java 8**, para tecnologias
modernas baseadas em **Java 21**, proporcionando melhorias relevantes
em:

-   🚀 **Performance**
-   🔒 **Segurança**
-   🧩 **Compatibilidade com novas bibliotecas**
-   🛠️ **Manutenibilidade do código**

O processo foi conduzido de forma incremental, seguindo as recomendações
oficiais do guia de atualização do Spring Boot, respeitando a ordem
obrigatória de migração entre versões principais:

-   Spring Boot **1.x → 1.5**
-   Spring Boot **1.5 → 2.0**
-   Spring Boot **2.0 → 2.4**
-   Spring Boot **2.4 → 2.7**
-   Spring Boot **2.7 → 3.x**
-   Spring Boot **3.x → 3.5**
-   Spring Boot **3.5 → 4.x**

Essa abordagem reduziu riscos de incompatibilidade e permitiu validações
progressivas ao longo do processo.

------------------------------------------------------------------------

# 2. Novas Exigências e Mudanças de Base

## 2.1 Atualização para Java 21

A nova versão do Java exigida pela plataforma é o **Java 21**, que traz
melhorias relevantes:

-   ⚡ Melhor desempenho geral da aplicação\
-   🧠 Otimização do uso de memória\
-   🔐 Melhorias de segurança\
-   🧵 Recursos modernos como Virtual Threads e Pattern Matching

------------------------------------------------------------------------

## 2.2 Migração para Jakarta EE

Uma das mudanças mais críticas foi a transição do ecossistema Java EE
para Jakarta EE.

Todo o namespace:

``` java
javax.*
```

foi substituído por:

``` java
jakarta.*
```

Essa mudança afetou:

-   Persistência (JPA)
-   Servlets
-   Validação
-   Segurança
-   Integrações externas

Foi necessário revisar manualmente grande parte do código-fonte e
dependências.

------------------------------------------------------------------------

## 2.3 Gestão de Propriedades de Configuração

Durante o processo de atualização, diversas propriedades utilizadas nas
versões anteriores foram:

-   Removidas
-   Renomeadas
-   Reestruturadas

Exemplos:

    server.servlet-path=/api -> spring.mvc.servlet.path=/api
	server.contextPath=/tc -> server.servlet.context-path=/tc

	logging.path -> logging.file.path

Essas mudanças exigiram a revisão manual dos arquivos de configuração da
aplicação.

------------------------------------------------------------------------

# 3. Impactos Críticos no Frontend

## 3.1 Alteração no Match de URL

Versões anteriores aceitavam URLs com barra final opcional:

    /api/clientes
    /api/clientes/

Agora, se a URL possuir **barra final não mapeada**, ocorre erro:

    NoResourceFoundException

------------------------------------------------------------------------

## 3.2 Correção Necessária

O frontend deve:

-   Garantir correspondência exata com os endpoints
-   Remover barras finais desnecessárias
-   Padronizar URLs consumidas

------------------------------------------------------------------------

# 4. Principais Desafios Técnicos e Soluções

## 4.1 Compatibilidade de Dependências

### 4.1.1 Remoção da Dependência Catnap

A biblioteca **Catnap** foi **removida do projeto** por não acompanhar a
evolução do Spring Boot e não possuir suporte adequado ao ecossistema
Jakarta.

------------------------------------------------------------------------

### 4.1.2 Solução Alternativa ao Catnap

Como substituição, foi implementada uma solução interna baseada em:

-   Interceptador de requisições HTTP
-   Anotação customizada `@FilterResponseBody`
-   Filtros dinâmicos aplicados diretamente na serialização da resposta

Funcionamento:

1.  Identificação de endpoints com `@FilterResponseBody`
2.  Interceptação da requisição HTTP
3.  Aplicação de filtros dinâmicos nos campos retornados

Benefícios:

-   Eliminação de dependência externa descontinuada\
-   Maior controle interno\
-   Compatibilidade total com Spring Boot 4\
-   Flexibilidade futura

------------------------------------------------------------------------

### 4.1.3 Fork do JSweet

A biblioteca JSweet apresentou incompatibilidade com Java 21.

Solução:

-   Criação de fork da biblioteca
-   Ajustes internos para possibilitar compatibilidade com Java 21
-   Correção da geração do bundle

------------------------------------------------------------------------

### 4.1.4 Compressão HTTP (Ziplet)

A biblioteca Ziplet não foi atualizada para Jakarta.

Solução:

-   Fork interno
-   Ajuste de namespaces
-   Manutenção da funcionalidade

------------------------------------------------------------------------

# 4.2 Segurança e Configuração

## 4.2.1 Configuração CORS

Agora é obrigatório declarar:

    CorsConfigurationSource

dentro do:

    SecurityFilterChain

------------------------------------------------------------------------

## 4.2.2 Registro de Servlets

Todo servlet precisa ser registrado manualmente.

Caso contrário:

    403 Forbidden

------------------------------------------------------------------------

## 4.3 Persistência de Dados (Hibernate 6)

### 4.3.1 Restrições de Anotação no Hibernate 6

Durante a atualização para o Hibernate 6, foi identificada uma restrição
estrutural importante relacionada ao uso de anotações de persistência.

Nas versões anteriores do Hibernate, era permitido utilizar
simultaneamente as anotações:

    @MappedSuperclass
    @Embeddable

Essa prática era utilizada para permitir o compartilhamento de atributos
comuns em classes reutilizáveis que também eram incorporadas em outras
entidades.

Entretanto, no Hibernate 6, essa combinação **não é mais permitida**,
resultando em falhas de mapeamento durante a inicialização da aplicação.

------------------------------------------------------------------------

### Problema Identificado

O padrão anteriormente utilizado seguia o modelo abaixo:

``` java
@Embeddable
@MappedSuperclass
public class ConfigIcms extends ConfigTrib implements ConfigRegrasImposto {

    @Column(name = "icms_cfop")
    private Integer cfop;

    @Column(name = "icms_usar_cfop_padrao")
    private Boolean usarCfopPadrao = false;

    @Column(name = "icms_cst", length = 2)
    @Convert(converter = CstIcmsConverter.class)
    private CstIcms cst;

    // etc...
}
```

Esse modelo permitia reutilização de atributos, porém tornou-se
incompatível com as novas regras do Hibernate.

------------------------------------------------------------------------

### Solução Arquitetural Adotada

Para atender às novas restrições, foi adotado um **modelo de herança
intermediária**, separando claramente as responsabilidades entre:

-   Classe base reutilizável (`@MappedSuperclass`)
-   Classe embutida (`@Embeddable`)

Foram criadas classes intermediárias abstratas, como:

-   `ConfigIcmsBase`
-   `ConfigIpiBase`

Essas classes atuam como **base de mapeamento comum**, permitindo a
reutilização segura dos atributos.

------------------------------------------------------------------------

### Novo Padrão Implementado

#### Classe Base Abstrata

``` java
@MappedSuperclass
public abstract class ConfigIcmsBase extends ConfigTrib {

    @Column(name = "icms_cfop")
    private Integer cfop;

    @Column(name = "icms_usar_cfop_padrao")
    private Boolean usarCfopPadrao = false;

    @Column(name = "icms_cst", length = 2)
    @Convert(converter = CstIcmsConverter.class)
    private CstIcms cst;

    // etc...
}
```

------------------------------------------------------------------------

#### Classe Embeddable Especializada

``` java
@Embeddable
public class ConfigIcms extends ConfigIcmsBase implements ConfigRegrasImposto {

    public ConfigIcms() {
    }

    @Override
    public TipoImposto getTipoImposto() {
        return TipoImposto.ICMS;
    }

    // etc...
}
```

------------------------------------------------------------------------

#### Uso no Objeto de Imposto

A configuração passou a ser utilizada como atributo embutido
(`@Embedded`), conforme o modelo abaixo:

``` java
@Embeddable
public class Icms implements Imposto<ConfigIcms> {

    @Embedded
    private ConfigIcms config = new ConfigIcms();

    @Column(name = "icms_v_bc")
    @JsonSerialize(using = BigDecimalSerializer.class)
    @JsonDeserialize(using = BigDecimalDeserializer.class)
    private BigDecimal vBc;

    // etc...
}
```

------------------------------------------------------------------------

### Benefícios Obtidos

A nova arquitetura trouxe os seguintes benefícios:

-   ✔ Compatibilidade total com Hibernate 6\
-   ✔ Eliminação de erros de inicialização do ORM\
-   ✔ Separação clara de responsabilidades entre classes\
-   ✔ Maior previsibilidade no mapeamento JPA\
-   ✔ Base preparada para futuras evoluções do framework

Além disso, o novo padrão permite reutilização segura dos atributos
comuns sem violar as restrições impostas pelas versões mais recentes do
Hibernate.

### Modificações Necessárias no Frontend

Será necessária a realização de ajustes nos formulários do frontend para adequação ao novo mapeamento estrutural dos objetos de configuração tributária.

Com a introdução da camada intermediária de configuração (`config`), os acessos diretos aos atributos foram alterados, exigindo a atualização dos bindings utilizados nas telas e componentes.

**Exemplo de alteração necessária:**


    icms.cfop -> icms.config.cfop



Essa modificação deve ser aplicada a todos os pontos do frontend que realizam leitura ou escrita de propriedades relacionadas às configurações dos impostos.

A não realização desses ajustes poderá resultar na exibição incorreta ou ausência de determinados atributos tributários nas interfaces do sistema.

------------------------------------------------------------------------

### 4.3.2 Geradores de Identificadores (SequenceStyleGenerator)

Durante a atualização para o Hibernate 6, foi necessário revisar e
reestruturar o mecanismo de geração de identificadores (IDs) das
entidades persistidas.

Essa revisão envolveu diretamente os seguintes componentes internos do
Hibernate:

-   `SequenceStyleGenerator`
-   `AnnotationBasedGenerator`
-   `allowAssignedIdentifiers`

------------------------------------------------------------------------

### Contexto Técnico

O Hibernate utiliza o componente `SequenceStyleGenerator` como seu
mecanismo padrão para geração de identificadores baseados em sequências
de banco de dados.

Esse gerador possui a capacidade de operar utilizando:

-   Sequências nativas do banco de dados
-   Tabelas que simulam sequências (quando o banco não possui suporte
    nativo)

Essa flexibilidade permite manter portabilidade entre diferentes bancos
de dados sem alterar a lógica da aplicação.

Além disso, o `SequenceStyleGenerator` é responsável por:

-   Criar e registrar objetos necessários no banco (sequências ou
    tabelas)
-   Determinar valores iniciais e incrementos
-   Gerar identificadores automaticamente durante operações de
    persistência

Essas configurações são realizadas durante a inicialização do gerador e
permanecem ativas durante todo o ciclo de vida da aplicação.

------------------------------------------------------------------------

### Problema Identificado

Durante o processo de migração, foi identificado que o comportamento
padrão do Hibernate 6 **não permite, por padrão, o uso de
identificadores previamente atribuídos manualmente** quando um gerador
automático está configurado.

Esse cenário impactava diretamente entidades que necessitavam:

-   Persistir registros com IDs já definidos
-   Manter compatibilidade com dados previamente existentes
-   Integrar registros oriundos de sistemas externos

Sem o devido ajuste, ocorria falha durante a persistência de entidades
com identificadores previamente preenchidos.

------------------------------------------------------------------------

### Solução Arquitetural Adotada

Foi implementado um novo padrão de geração de identificadores baseado
em:

-   `SequenceStyleGenerator` como base principal
-   Implementação personalizada utilizando `AnnotationBasedGenerator`
-   Ajuste explícito do método `allowAssignedIdentifiers`

O componente `AnnotationBasedGenerator` permite inicializar geradores de
identificadores a partir de anotações declaradas nas entidades,
recuperando metadados diretamente da configuração do modelo.

Essa abordagem permitiu adaptar o comportamento do gerador sem alterar o
modelo conceitual das entidades.

------------------------------------------------------------------------

### Ajuste do Método allowAssignedIdentifiers

Foi necessário sobrescrever o comportamento padrão relacionado ao
método:

``` java
allowAssignedIdentifiers()
```

Esse método determina se o gerador permite que um identificador já
definido manualmente seja utilizado durante a persistência da entidade.

Por padrão:

    false

Após a customização:

    true

Essa alteração permitiu que o Hibernate aceitasse valores previamente
definidos quando presentes, mantendo o comportamento automático quando
necessário.

------------------------------------------------------------------------

### Benefícios Obtidos

A nova implementação trouxe ganhos significativos para a arquitetura de
persistência:

-   ✔ Compatibilidade total com Hibernate 6
-   ✔ Suporte a persistência com IDs previamente definidos
-   ✔ Continuidade da compatibilidade com bases de dados existentes
-   ✔ Flexibilidade para integração com sistemas externos
-   ✔ Padronização do mecanismo de geração de identificadores
-   ✔ Maior controle sobre o ciclo de vida dos IDs

Além disso, a utilização do `SequenceStyleGenerator` mantém a
portabilidade entre diferentes bancos de dados, permitindo que o
Hibernate utilize sequências reais ou estruturas equivalentes
automaticamente.

------------------------------------------------------------------------

### Impacto Técnico

Essa alteração impactou diretamente:

-   Classes base responsáveis pela geração de identificadores
-   Estratégias de persistência utilizadas pelas entidades
-   Integrações que dependem da manutenção de identificadores externos
-   Configuração global do mecanismo de geração de IDs

Essa reestruturação foi essencial para garantir compatibilidade total
com as novas versões do Hibernate e assegurar a integridade dos dados
persistidos.

# 5. RFC 3986

### Ajuste Necessário no Encoding de URLs para Integração com Analytics

Foi identificado que, após a atualização para o **Spring Framework 6**, tornou-se necessário realizar ajustes no processo de codificação (encoding) das URLs utilizadas nas requisições direcionadas ao módulo **Analytics**.

Nas versões mais recentes do Spring Framework, houve um endurecimento nas validações relacionadas à construção e interpretação de URLs, passando a exigir conformidade estrita com a especificação definida pela **RFC 3986 (Uniform Resource Identifier – URI: Generic Syntax)**.

Como consequência, URLs que contenham caracteres não permitidos ou que não estejam devidamente codificados passaram a resultar em falhas durante o processamento da requisição, podendo gerar exceções em tempo de execução, tais como:

- `IllegalArgumentException`
- `URISyntaxException`
- Rejeição da requisição durante a resolução do endpoint
- Falha na construção de objetos `URI` ou `URL`
- Erros relacionados ao parsing de query parameters

Esse comportamento foi observado principalmente em cenários onde parâmetros da URL continham:

- Espaços em branco (` `)
- Caracteres reservados não codificados (ex.: `|`, `{`, `}`, `[`, `]`, `^`)
- Caracteres especiais utilizados em filtros ou expressões dinâmicas
- Strings contendo acentuação ou caracteres fora do conjunto ASCII
- Parâmetros contendo JSON ou estruturas complexas serializadas diretamente na query string

---

### Causa Técnica

O problema está diretamente relacionado ao novo comportamento dos componentes internos do Spring responsáveis pelo processamento de requisições HTTP, especialmente:

- `UriComponentsBuilder`
- `DefaultUriBuilderFactory`
- Mecanismos internos de parsing e normalização de URIs

Esses componentes passaram a validar de forma mais rigorosa os caracteres presentes nas URLs, rejeitando valores que não estejam devidamente codificados segundo o padrão **percent-encoding** definido pela RFC 3986.

---

### Solução

Será necessário garantir que todas as URLs geradas para comunicação com o módulo **Analytics** passem por um processo explícito de codificação (URL encoding), assegurando que:

- Todos os parâmetros de query sejam devidamente codificados
- Caracteres especiais sejam convertidos para sua representação segura
- A URL final esteja em conformidade com o padrão RFC 3986

Esse ajuste deve ser aplicado especialmente nos pontos onde URLs são montadas dinamicamente, incluindo:

- Construção manual de query parameters
- Serialização de filtros ou parâmetros compostos
- Integrações que utilizam concatenação direta de strings para formar URLs

---

### Impacto Técnico

Sem a aplicação correta do encoding, podem ocorrer:

- Falhas na comunicação com o módulo Analytics
- Rejeição silenciosa de requisições HTTP
- Interrupção de funcionalidades dependentes de relatórios e análises
- Inconsistências no envio de parâmetros complexos
- Aumento na ocorrência de erros em ambientes com dados contendo caracteres especiais

A padronização do encoding das URLs tornou-se um requisito obrigatório para garantir compatibilidade plena com o **Spring Framework 6** e assegurar a estabilidade das integrações existentes.

------------------------------------------------------------------------
# 6. Configuração `spring.jpa.open-in-view` — Avaliação Técnica e Decisão de Manutenção do Comportamento Legado

### Contexto

Durante o processo de modernização da aplicação para **Java 21** e versões mais recentes do **Spring Boot**, foi avaliado o impacto do parâmetro:

```properties
spring.jpa.open-in-view
```

Este parâmetro controla a ativação do padrão **Open Session in View (OSIV)** — também conhecido como **Open EntityManager in View** — responsável por manter o `EntityManager` aberto durante todo o ciclo de vida da requisição HTTP.

Historicamente, esse comportamento foi habilitado por padrão nas versões iniciais do Spring Boot, visando facilitar o acesso a propriedades com carregamento **LAZY** fora do escopo transacional, especialmente durante a serialização de objetos e renderização de views.

---

### Funcionamento Técnico do Open-In-View

Quando habilitado:

```properties
spring.jpa.open-in-view=true
```

O `EntityManager` permanece aberto durante toda a requisição HTTP, incluindo:

* Execução de serviços
* Processamento de controllers
* Serialização de respostas JSON
* Renderização de views (quando aplicável)

Fluxo típico:

```text
Requisição HTTP
   ├── Transação iniciada (@Transactional)
   ├── Execução do Service
   ├── Execução do Controller
   ├── Serialização (Jackson)
   └── Encerramento do EntityManager
```

Isso permite que associações configuradas com `FetchType.LAZY` sejam carregadas mesmo após o término da transação original.

---

### Mudanças nas Versões Recentes do Spring Boot

Ao longo das versões do Spring Boot, houve uma mudança significativa na recomendação arquitetural relacionada ao uso do Open-In-View:

| Versão          | Comportamento Padrão     | Direcionamento Arquitetural              |
| --------------- | ------------------------ | ---------------------------------------- |
| Spring Boot 1.x | `true`                   | Uso padrão                               |
| Spring Boot 2.x | `true`                   | Introdução de aviso (warning) no startup |
| Spring Boot 3.x | `true`                   | Uso desencorajado                        |
| Spring Boot 4.x | `false` (padrão)         | Recomendado desabilitar                  |

---

### Impactos Técnicos do Open-In-View

O uso do Open-In-View possui implicações importantes em termos de arquitetura, performance e previsibilidade do sistema.

#### 6.1. Execução de Queries Fora do Escopo Transacional

Quando habilitado, consultas adicionais podem ser disparadas durante a serialização JSON ou processamento tardio de entidades.

Isso pode resultar em:

* Execução de queries fora da camada de serviço
* Dificuldade de rastreamento do fluxo de acesso ao banco
* Baixa previsibilidade do comportamento da aplicação

---

#### 6.2. Ocorrência de Problemas de Performance (N+1 Queries)

Um dos principais riscos associados ao Open-In-View é a geração de múltiplas consultas implícitas ao banco de dados.

Exemplo típico:

```text
SELECT pedido
SELECT item WHERE pedido_id = ?
SELECT item WHERE pedido_id = ?
SELECT item WHERE pedido_id = ?
...
```

Esse padrão, conhecido como **N+1 Query Problem**, pode impactar severamente a performance sob carga.

---

#### 6.3. Aumento do Tempo de Vida da Sessão

Com o Open-In-View ativo:

* O `EntityManager` permanece aberto por mais tempo
* Conexões podem ser mantidas além do necessário
* Pode ocorrer maior consumo de recursos em cenários de alta concorrência

---

#### 6.4. Mascaramento de Problemas Arquiteturais

O Open-In-View permite que entidades com carregamento LAZY sejam utilizadas fora da transação, o que pode ocultar deficiências no design da camada de acesso a dados.

Quando desabilitado:

```properties
spring.jpa.open-in-view=false
```

É comum ocorrer o erro:

```text
LazyInitializationException:
could not initialize proxy - no Session
```

Esse erro indica que o carregamento LAZY foi tentado fora do escopo transacional — o que, do ponto de vista arquitetural, representa um comportamento esperado e desejável em arquiteturas modernas.

---

### Estratégias Modernas para Substituição do Open-In-View

Em arquiteturas modernas, recomenda-se desabilitar o Open-In-View e adotar abordagens explícitas para carregamento de dados.

As principais estratégias incluem:

---

#### Uso de `JOIN FETCH`

Permite carregar associações LAZY explicitamente na consulta.

```java
@Query("""
    SELECT p
    FROM Pedido p
    LEFT JOIN FETCH p.itens
    WHERE p.id = :id
""")
Pedido buscarComItens(Long id);
```

---

#### Uso de `EntityGraph`

Permite definir graficamente os relacionamentos a serem carregados.

```java
@EntityGraph(attributePaths = {"itens"})
Pedido findById(Long id);
```

---

#### Uso de DTOs (Data Transfer Objects)

Estratégia recomendada para APIs REST modernas.

```java
@Query("""
    SELECT new PedidoDTO(
        p.id,
        p.data,
        i.nome
    )
    FROM Pedido p
    JOIN p.itens i
""")
List<PedidoDTO> listar();
```

Essa abordagem evita o carregamento desnecessário de entidades completas e melhora significativamente a previsibilidade do acesso a dados.

---

### Impactos Esperados ao Desabilitar Open-In-View

A desativação do Open-In-View exige ajustes estruturais na aplicação.

Os principais impactos esperados incluem:

* Necessidade de revisão de consultas JPA
* Ajuste de endpoints que retornam entidades diretamente
* Implementação de DTOs ou fetch explícito
* Revisão de relacionamentos LAZY

Embora exija esforço inicial, essa abordagem resulta em:

* Melhor previsibilidade do acesso a dados
* Redução de problemas de performance
* Maior controle sobre o ciclo de vida das entidades
* Arquitetura mais robusta e sustentável

---

### Decisão Arquitetural Adotada Nesta Migração

Durante esta etapa de modernização, foi identificado que a aplicação possui forte dependência do comportamento legado proporcionado pelo Open-In-View.

Diversos endpoints realizam acesso indireto a propriedades LAZY durante:

* Serialização JSON
* Processamento de respostas REST
* Construção dinâmica de objetos retornados ao frontend

A desativação imediata do Open-In-View exigiria uma refatoração ampla envolvendo:

* Reestruturação da camada de persistência
* Revisão de consultas existentes
* Introdução sistemática de DTOs
* Revisão de múltiplos endpoints

Diante disso, foi adotada a seguinte decisão técnica:

```properties
spring.jpa.open-in-view=true
```

Essa configuração foi mantida explicitamente para:

* Preservar o comportamento legado existente
* Garantir estabilidade funcional durante a fase inicial da migração
* Reduzir o risco operacional associado à modernização simultânea de múltiplos componentes
* Permitir que outras etapas críticas da migração sejam concluídas com menor impacto funcional

---

### Planejamento Futuro

Embora o comportamento legado tenha sido mantido nesta etapa, recomenda-se que a desativação do Open-In-View seja tratada como evolução arquitetural futura.

Etapas sugeridas:

1. Identificar endpoints que retornam entidades diretamente
2. Introduzir DTOs progressivamente
3. Revisar consultas com `JOIN FETCH` ou `EntityGraph`
4. Validar comportamento sem Open-In-View em ambiente de homologação
5. Desabilitar definitivamente:

```properties
spring.jpa.open-in-view=false
```

Essa evolução permitirá alinhar a aplicação às boas práticas modernas do ecossistema Spring.

---

### Conclusão

A configuração `spring.jpa.open-in-view` representa um ponto crítico de compatibilidade entre arquiteturas legadas e modernas.

Nesta fase da migração para **Java 21** e versões mais recentes do **Spring Boot**, optou-se por manter:

```properties
spring.jpa.open-in-view=true
```

como medida de mitigação de risco e preservação funcional.

Contudo, sua desativação permanece recomendada como evolução arquitetural futura, visando:

* Melhor controle transacional
* Maior previsibilidade no acesso ao banco
* Otimização de performance
* Conformidade com boas práticas atuais do ecossistema Spring

------------------------------------------------------------------------

# 7. Atualizações e Mudanças no Logback — Compatibilidade e Ajustes Necessários

### Contexto

Durante o processo de modernização da aplicação para **Java 21** e versões mais recentes do **Spring Boot**, foram identificadas mudanças significativas relacionadas ao framework de logging **Logback**, especialmente em relação à estrutura de configuração e classes utilizadas em estratégias de rotação de arquivos.

As alterações observadas estão relacionadas principalmente à evolução das versões do **Logback Classic**, utilizadas implicitamente pelas versões modernas do Spring Boot.

Essas mudanças impactam diretamente arquivos de configuração `logback.xml` e `logback-spring.xml`, exigindo ajustes para garantir compatibilidade com as novas versões da biblioteca.

---

### Mudança na Configuração de `encoder`

Uma das alterações obrigatórias nas versões modernas do Logback refere-se ao uso do elemento `<encoder>` em substituição ao antigo `<layout>`.

#### Configuração Legada (Não Recomendada)

Em versões antigas, era comum utilizar:

```xml
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>logs/app.log</file>

    <layout class="ch.qos.logback.classic.PatternLayout">
        <pattern>%d{yyyy-MM-dd HH:mm:ss} %-5level %logger - %msg%n</pattern>
    </layout>
</appender>
```

Esse formato deixou de ser suportado nas versões modernas do Logback, podendo gerar falhas durante a inicialização da aplicação.

---

#### Configuração Atual (Recomendada)

O formato moderno exige o uso de `<encoder>`:

```xml
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>logs/app.log</file>

    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} %-5level %logger - %msg%n</pattern>
    </encoder>
</appender>
```

Esse ajuste é obrigatório para compatibilidade com versões recentes do Logback.

---

### Remoção da Classe `SizeAndTimeBasedFNATP`

Uma mudança relevante identificada foi a remoção da classe:

```text
ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP
```

Essa classe era utilizada em configurações antigas para controle combinado de rotação por tempo e tamanho de arquivo.

#### Configuração Legada

Exemplo antigo:

```xml
<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">

    <fileNamePattern>
        logs/app.%d{yyyy-MM-dd}.%i.log
    </fileNamePattern>

    <timeBasedFileNamingAndTriggeringPolicy
        class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">

        <maxFileSize>10MB</maxFileSize>

    </timeBasedFileNamingAndTriggeringPolicy>

</rollingPolicy>
```

Essa abordagem não é mais suportada nas versões atuais do Logback.

---

#### Configuração Atual Recomendada

A substituição deve ser feita utilizando:

```xml
ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy
```

Exemplo atualizado:

```xml
<rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">

    <fileNamePattern>
        logs/app.%d{yyyy-MM-dd}.%i.log
    </fileNamePattern>

    <maxFileSize>10MB</maxFileSize>

    <maxHistory>30</maxHistory>

    <totalSizeCap>3GB</totalSizeCap>

</rollingPolicy>
```

Essa nova abordagem simplifica a configuração e elimina a necessidade do componente intermediário `FNATP`.

---

### Impactos Observados Durante a Migração

Durante a atualização do ambiente, foram identificados erros de inicialização relacionados à configuração antiga do Logback.

Os sintomas mais comuns incluem:

```text
ERROR in ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP
ClassNotFoundException
```

Ou:

```text
ERROR in ch.qos.logback.core.FileAppender
No applicable action for [layout]
```

Esses erros indicam incompatibilidade direta entre a configuração legada e a versão atual do Logback.

---

### Outras Mudanças Relevantes

Além das alterações principais, foram observadas mudanças adicionais que devem ser consideradas.

---

#### Uso Obrigatório de `fileNamePattern` Compatível

Ao utilizar `SizeAndTimeBasedRollingPolicy`, o padrão de nome do arquivo deve conter:

```text
%d — Data
%i — Índice
```

Exemplo:

```xml
<fileNamePattern>
    logs/app.%d{yyyy-MM-dd}.%i.log
</fileNamePattern>
```

A ausência do índice `%i` pode impedir o funcionamento correto da rotação baseada em tamanho.

---

#### Compatibilidade com Java 21

Versões modernas do Logback são totalmente compatíveis com Java 21, porém configurações antigas podem falhar devido a:

* Remoção de classes internas
* Alteração de APIs internas
* Endurecimento da validação de configuração XML

Essas mudanças tornam obrigatória a atualização das configurações legadas.

---

### Benefícios das Configurações Modernas

A atualização para o modelo moderno de configuração do Logback traz benefícios técnicos relevantes:

* Configuração mais simples e direta
* Redução de componentes intermediários
* Melhor controle de retenção de logs
* Maior previsibilidade de rotação
* Compatibilidade com versões atuais do Spring Boot
* Melhor integração com ambientes containerizados

---

### Exemplo Completo Atualizado

Abaixo está um exemplo consolidado de configuração compatível com versões modernas:

```xml
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">

    <file>logs/application.log</file>

    <encoder>
        <pattern>
            %d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
        </pattern>
    </encoder>

    <rollingPolicy
        class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">

        <fileNamePattern>
            logs/application.%d{yyyy-MM-dd}.%i.log
        </fileNamePattern>

        <maxFileSize>10MB</maxFileSize>

        <maxHistory>30</maxHistory>

        <totalSizeCap>3GB</totalSizeCap>

    </rollingPolicy>

</appender>
```

Essa estrutura atende aos requisitos das versões modernas do Logback e mantém o comportamento esperado de rotação por tamanho e tempo.

---

### Conclusão

Durante o processo de modernização para **Java 21** e versões mais recentes do **Spring Boot**, foi necessária a atualização da configuração do Logback para compatibilidade com as versões atuais da biblioteca.

As principais alterações realizadas foram:

* Substituição de `<layout>` por `<encoder>`
* Remoção do uso da classe `SizeAndTimeBasedFNATP`
* Adoção de `SizeAndTimeBasedRollingPolicy`
* Ajuste dos padrões de nomeação (`fileNamePattern`)

Essas alterações garantem compatibilidade com as versões modernas do Logback, preservando o comportamento funcional esperado e mantendo a confiabilidade do mecanismo de logging da aplicação.

------------------------------------------------------------------------

# 8. Versões finais homologadas
- **Java:** 21.0.2-open
- **Maven:** 3.9.12
- **Spring:** 4.0.4
- **Spring Data:** 4.0.4
- **Spring Security:** 7.0.4
- **QueryDSL:** 5.1.0
- **Hibernate:** 7.2.7
- **Jackson:** 2.21.1
- **Tomcat:** 11.0.18
- **Redis:** 6.0.0

------------------------------------------------------------------------
# 9. Benefícios Técnicos Obtidos

-   🚀 Melhor desempenho
-   🔒 Maior segurança
-   🧩 Melhor manutenibilidade
-   🛠️ Maior compatibilidade tecnológica

------------------------------------------------------------------------

# 10. Conclusão

A migração para **Spring Boot 4.0.4** e **Java 21** posiciona a
aplicação em um patamar tecnológico moderno, seguro e sustentável,
preparado para evoluções futuras.

------------------------------------------------------------------------

# 11. Testes e liberação de VCAN

Foram realizados testes integrados com o Frontend em cada etapa do processo de atualização nos projetos **Loyalty**, **Analytics** e **Truecloud**, com o objetivo de assegurar a ausência de impactos funcionais aos clientes durante a utilização das aplicações. Os erros identificados que demandavam ajustes no Frontend foram tratados de forma pontual durante a execução dos testes, com o objetivo de viabilizar a continuidade do processo de validação das funcionalidades.

Essas correções foram aplicadas temporariamente e devidamente registradas para posterior revisão e implementação definitiva por parte de Nielson, garantindo a adequada formalização das alterações necessárias no código-fonte.

Entretanto, durante o processo de atualização dos ambientes dos clientes, recomenda-se a adoção de medidas adicionais de cautela. É altamente recomendável utilizar inicialmente as máquinas **vcan** para disponibilização desta nova versão, bem como selecionar um grupo controlado de clientes para execução do processo de homologação, garantindo maior segurança antes da liberação em larga escala.

------------------------------------------------------------------------

# 12. Referência Técnica Oficial

https://docs.spring.io/spring-boot/upgrading.html