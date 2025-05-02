# Prompts da Indexação
{apend:1

## Prompt para Extração de Entidades do Tipo Documento**
{apend:1.1**

\setlength{\parindent**{0pt**


**-Objetivo-**



Dado um documento de texto, descubra qual o nome do documento e resuma-o em no máximo 100 palavras. Caso o documento cite algum outro documento, coloque isso no resumo.

Coloque no nome do documento somente o tipo do documento e o identificador dele. Exemplo: ``PORTARIA Nº 13/1998 - SEC-CEXTERNO'' terá o nome de ``PORTARIA 13/1998''

A saída será no formato: \{\{initial\_delimiter\**\**nome|||resumo\{\{completion\_delimiter\**\**. 

Retorne a saída em português, use **\{\{initial\_delimiter\**\**** como token inicial antes do resumo e **\{\{completion\_delimiter\**\**** como o último token depois do resumo.



**-Exemplos-**



\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#
Exemplo 1:

Texto: 


RESOLUÇÃO Nº 68/1965




			-Vide Resolução nº 99/1965, de 18-03-1965.



          
			Instruções sobre Fundos Rotativos, mandadas observar no Tribunal de Contas.

            


            
O TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS, usando de atribuição que lhe confere o inciso II do § 4° do artigo 31 da Constituição Estadual, resolve consolidar, como de natureza regimental, as seguintes normas legais, para serem rigorosamente observadas por esta Casa na informação e decisão dos pagamentos a Fundos Rotativos e na instrução e julgamento das prestações trimestrais das contas de movimentação daqueles recursos peculiares:


---------------------


Saída:




\{\{initial\_delimiter\**\**RESOLUÇÃO 68/1965|||A Resolução nº 68/1965, do Tribunal de Contas do Estado de Goiás, estabelece normas regimentais para a gestão e fiscalização de Fundos Rotativos, incluindo instruções para pagamentos, movimentação e julgamento das prestações de contas trimestrais, conforme o artigo 31 da Constituição Estadual.\{\{completion\_delimiter\**\**
\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#



**-Dados Reais-**



\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#


Texto: \{doc\_str\**


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#


Saída:


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


## Prompt para o Pré-Processamento de Texto**
{apend:1.2**

\setlength{\parindent**{0pt**


**-Objetivo-**


Dado um texto jurídico, identifique as seções do texto e altere o nome dos artigos especificando a qual documento eles pertencem.


**-Etapas-**


1. Identifique no texto as seguintes seções: Introdução, Artigo 1, Artigo 2, ..., Artigo N e Conclusão.  


2. Nas seções de artigos, troque o nome do artigo para: nome do artigo, seguido pelo nome do documento a que pertence, seguido por ``: ''.


3. Retorne o texto em português, use **\{\{initial\_delimiter\**\**** como token inicial do texto, e **\{\{completion\_delimiter\**\**** como o último token do texto.


**-Exemplo-**


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#
Exemplo 1:

Texto:

TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS  




PORTARIA Nº 1/2024 - SEC-CEXTERNO  


Designa equipe de fiscalização para realização de Acompanhamento da concessão do Parque Estadual da Serra de Caldas Novas – PESCaN.  


Art.1º Designar os servidores Letícia da Silva Manchini e Valdo de Sousa Filho, sob a coordenação de Vânia Mara de Souza e Silva, para comporem equipe de fiscalização.  


Art. 2º Estabelecer a data de 30/04/2024 para entrega do Relatório final de fiscalização pela equipe designada no art. 1º desta Portaria.  


CUMPRA-SE E PUBLIQUE-SE.  


SECRETARIA DE CONTROLE EXTERNO DO TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS em Goiânia aos 15 de fevereiro de 2024.  


---------------------  


Saída:


\{\{initial\_delimiter\**\**  
TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS  


PORTARIA Nº 1/2024 - SEC-CEXTERNO  


Designa equipe de fiscalização para realização de Acompanhamento da concessão do Parque Estadual da Serra de Caldas Novas – PESCaN.  


Art.1º da Portaria Nº 1/2024: Designar os servidores Letícia da Silva Manchini e Valdo de Sousa Filho, sob a coordenação de Vânia Mara de Souza e Silva, para comporem equipe de fiscalização.  


Art. 2º da Portaria Nº 1/2024: Estabelecer a data de 30/04/2024 para entrega do Relatório final de fiscalização pela equipe designada no art. 1º desta Portaria.  


CUMPRA-SE E PUBLIQUE-SE.  


SECRETARIA DE CONTROLE EXTERNO DO TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS em Goiânia aos 15 de fevereiro de 2024.  


\{\{completion\_delimiter\**\**  


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#


**-Dados Reais-**


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#  


Texto: \{doc\_str\**  


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#  


Saída:  


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

## Prompt para Extração de Entidades do Tipo Artigo**
{apend:1.3**

\setlength{\parindent**{0pt**


**-Objetivo-**


Dado um documento de texto potencialmente relevante para essa atividade, identifique entidades que são do tipo artigo. Identifique também os artigos que são citados por outros artigos, caso haja.  
Caso não identifique um artigo, não invente.


**-Etapas-**


1. Identifique todas as entidades. Para cada entidade identificada, extraia as seguintes informações:


   - entity\_name: Nome da entidade.
   

   - entity\_type: Um dos seguintes tipos: [artigo].
   

   - entity\_description: Descrição abrangente dos atributos e atividades da entidade, acrescentando o nome detalhado das entidades com que ela se relaciona no texto.  
   

   Formate cada entidade como: entity|||<entity\_name>|||<entity\_type>|||<entity\_description> \{\{record\_delimiter\**\**.  
   Normalmente o artigo está descrito no texto como sendo ``Art. N da Portaria/Normativa XX/YYYY''.


2. Identificar de qual documento é esse artigo no nome da entidade. Exemplo: ``Artigo 1 da Portaria 6662/2021''. Escrever o <entity\_name> no formato ``Artigo N do Documento XX/YYYY'', onde N é o número do artigo e XX/YYYY o número do documento.


3. Retorne a saída em português como uma lista única de todas as entidades identificadas nas etapas 1 e 2. Use **\{\{initial\_delimiter\**\**** como token inicial da lista, **\{\{record\_delimiter\**\**** como o delimitador de cada item da lista e **\{\{completion\_delimiter\**\**** como o último token da lista.



**-Exemplos-**


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#
Exemplo 1:

entity\_types: [artigo]

NOME DO DOCUMENTO: PORTARIA Nº 554/2024

Texto:

Art. 1º Instituir processo para seleção de 2 Analistas de Controle Externo do Tribunal de Contas do Estado de Goiás, visando a integrar a lista a ser submetida pela ATRICON ao TCU, destinada à formação das equipes de auditoria externa das finanças da ONU.


Parágrafo único. O processo a que se refere o caput deste artigo será conduzido pela seguinte comissão:  


I - Nádia Rezende Faria (coordenadora); 


II - Sérvio Túlio Teixeira e Silva;  


III - Vera Núbia Zandonadi Gomes.  


Art. 2º São pré-requisitos para participação do processo seletivo a que se refere o art. 1º:


I - Obrigatórios:  


a) Treinamento em auditoria financeira (exemplos: certificação CIPFA em IPSAS, Pós-graduação em Auditoria Financeira; curso sobre Auditoria de Contas, Participação na tradução do Manual de Auditoria Financeira da IDI, Registro no Cadastro Nacional de Auditores Independentes, entre outros);  


---------------------  


Saída:  
\{\{initial\_delimiter\**\**  


entity|||Artigo 1 da Portaria 554/2024|||artigo|||A Portaria nº 554/2024 institui o processo para selecionar dois Analistas de Controle Externo do TCE-GO, visando integrar equipes de auditoria externa das finanças da ONU, com coordenação de Nádia Rezende Faria e apoio de uma comissão.


\{\{record\_delimiter\**\**  


entity|||Artigo 2 da Portaria 554/2024|||artigo|||Conforme a Portaria nº 554/2024, o TCE-GO realizará seleção de Analistas para compor a lista da ATRICON ao TCU, destinada à auditoria da ONU, conduzida por comissão designada. 


\{\{completion\_delimiter\**\**  


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#


Exemplo 2:


entity\_types: [artigo]


NOME DO DOCUMENTO: PORTARIA Nº 19/2024


Texto:


Art. 1º O art. 1º da Portaria nº 15/2024 SEC-CEXTERNO, de 23 de fevereiro de 2024, publicada no Diário Eletrônico de Contas do Tribunal de Contas do Estado de Goiás, Ano XIII, Número 33, folha 2, no dia 23 de fevereiro de 2024, passa a vigorar com a seguinte redação:  


“Designar o servidor Filipe Pires Correia da Fonseca, sob a coordenação de Celso Hiroki Sakuma, para comporem equipe de fiscalização que realizará Inspeção, junto à Secretaria de Estado da Economia – ECONOMIA e Agência Goiana de Infraestrutura e Transportes – GOINFRA, no TARE n.º 001-1024/2022-GSE, referente a construção do anel viário do Contorno Oeste, localizado no município de Pires do Rio, com o objetivo de verificar se a qualidade e quantidade dos serviços executados quanto ao cumprimento e obediência das cláusulas contratuais dos projetos, das normas técnicas, das medições e dos pagamentos, e também como se deu a atuação da GOINFRA em relação às suas obrigações contidas nos decretos estaduais nº 9.622/2020 e nº 10.015/2021, no que couber.”  


Art. 2º O art. 4º da Portaria nº 15/2024 SEC-CEXTERNO, de 23 de fevereiro de 2024, passa a vigorar com a seguinte redação:  


“Art. 4º O anel viário deverá ser concluído em um período de 14 dias”.  


---------------------  


Saída:  


\{\{initial\_delimiter\**\**  


entity|||Artigo 1 da Portaria 19/2024|||artigo|||Altera o artigo 1 da Portaria 15/2024 para designar Filipe Pires Correia da Fonseca e Celso Hiroki Sakuma para fiscalizar a construção do Contorno Oeste em Pires do Rio, verificando conformidade contratual, qualidade, medições, pagamentos e atuação da GOINFRA, conforme decretos estaduais nº 9.622/2020 e nº 10.015/2021.  


\{\{record\_delimiter\**\**  


entity|||Artigo 1 da Portaria 15/2024|||artigo|||Artigo 1 da Portaria 15/2024  
\{\{record\_delimiter\**\**  


entity|||Artigo 2 da Portaria 19/2024|||artigo|||Altera o artigo 4 da Portaria 15/2024 e designa 14 dias para o término do anel viário.  


\{\{record\_delimiter\**\**  


entity|||Artigo 4 da Portaria 15/2024|||artigo|||Artigo 4 da Portaria 15/2024  
\{\{completion\_delimiter\**\**  


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#



**-Dados Reais-**

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#


entity\_types: \{entity\_types\**  


NOME DO DOCUMENTO: \{doc\_name\**  


Texto:  


\{input\_text\**  


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#  


Saída:  


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


## Prompt para Concatenação de Entidades**
{apend:1.4**

\setlength{\parindent**{0pt**


**-Objetivo-**

É fornecida uma lista de entidades estruturada como ``classe|||id|||nome|||tipo|||descrição'', onde:

classe refere-se a se é entidade ou relação;

id é o identificador da entidade;

nome é o nome da entidade;

tipo é o tipo da entidade;

descrição é a descrição detalhada da entidade.

Dada essa lista, faça um resumo da descrição de cada entidade e forneça uma nova lista dessas entidades com as descrições resumidas.



**-Etapas-**


1. Resuma a descrição da entidade em no máximo \{entity\_extraction\_char\_limit\** caracteres, retirando informações duplicadas e preservando nomes e relações.

2. Estruture cada entidade como: 

<classe>|||<id>|||<nome>|||<tipo>|||<nova\_descrição>\{\{record\_delimiter\**\**.

3. Retorne a saída em português como uma lista única de todas as entidades com as novas descrições feitas nas etapas 1 e 2. Use **\{\{initial\_delimiter\**\**** como token inicial da lista, **\{\{record\_delimiter\**\**** como o delimitador de cada item da lista e **\{\{completion\_delimiter\**\**** como o último token da lista.



**-Exemplo-**


Lista de Entidades

classe|||id|||nome|||tipo|||descrição

entity|||22\_0\_1\_2024|||TCE|||organização||| TCE: O Tribunal de Contas do Estado de Goiás é responsável pela fiscalização de administração pública. Órgão fiscalizador que controla gastos e gestão de recursos públicos.\{\{record\_delimiter\**\**

entity|||11\_2\_3\_2023|||Arturo Moraes|||pessoa|||Arturo Moraes: Membro de fiscalização do anel viário. Conselheiro Chefe do Conselho Regional Goiano.\{\{record\_delimiter\**\**

---------------------

saída:

\{\{initial\_delimiter\**\**

entity|||22\_0\_1\_2024|||TCE|||organização||| O Tribunal de Contas do Estado de Goiás (TCE) é responsável pela fiscalização de administração pública, controlando gastos e fazendo a gestão de recursos públicos.\{\{record\_delimiter\**\**

entity|||11\_2\_3\_2023|||Arturo Moraes|||pessoa|||Arturo Moraes é membro da fiscalização do anel viário, e exerce o cargo de Conselheiro Chefe do Conselho Regional Goiano.\{\{record\_delimiter\**\**

\{\{completion\_delimiter\**\**



**-Dados Reais-**


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Lista de Entidades

classe|||id|||nome|||tipo|||descrição

\{entities\_str\_list\**

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Saída:



%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

## Prompt para Extração de Triplas**
{apend:1.5**

\setlength{\parindent**{0pt**


**-Objetivo-**


Dado um documento de texto potencialmente relevante para essa atividade e uma lista de entidades, identifique todas as relações dessas entidades no texto.



**-Etapas-**


1. A partir do texto e das entidades fornecidas, identificar todos os pares de (entidade\_fonte, entidade\_alvo) que estejam *claramente relacionados* entre si.

Para cada par de entidades relacionadas, extrair as seguintes informações:

- entidade\_fonte: nome da entidade de origem

- entidade\_alvo: nome da entidade de destino

- descrição\_relacionamento: explicação de como a entidade de origem se relaciona com a entidade de destino.

Formatar cada relação como: 

relação|||<id\_entidade\_fonte>|||<entidade\_fonte>|||<id\_entidade\_alvo>|||<entidade\_alvo>

|||<descrição\_relacionamento>\{\{record\_delimiter\**\**.


2. Retorne a saída em português como uma lista única de todas as relações identificadas nas etapas 1.

Use **\{\{initial\_delimiter\**\**** como token inicial da lista, **\{\{record\_delimiter\**\**** como o delimitador de cada item da lista e **\{\{completion\_delimiter\**\**** como o último token da lista.



**-Exemplo-**


Texto:

PORTARIA Nº 39/2023 - SEC-CEXTERNO



CONSIDERANDO edição da Portaria nº 35/2023-SEC-CEXTERNO, de 11 de setembro de 2023, publicada no Diário Eletrônico de Contas – Ano XII, nº 163, em 12 de setembro de 2023, contendo erro material,



RESOLVE:



Artigo 1 da Portaria 39/2023: O artigo 1 da Portaria n.º 24/2023 SEC-CEXTERNO, de 22 de junho de 2023, passa a vigorar com a seguinte redação:



“Art. 1º Designar os servidores Hayk Carvalho Silva, Marcelo Bisinoto Higino de Cuba e Celso Hiroki Sakuma, sob a coordenação deste último, com a assessoria dos servidores Daniel Menezes Brandão, Jonas Rodrigues de Cerqueira Neto, Waldir Araújo Mármore e Thiago Costa Campos, para comporem equipe de fiscalização junto à Agência Goiana de Infraestrutura e Transportes – GOINFRA, para realização de Inspeção com o objetivo de verificar a quantidade e qualidade dos serviços executados de revestimento asfáltico dos contratos.



Entidades:

entity|||id|||nome|||tipo|||descrição

entity|||3\_3\_2\_2023|||PORTARIA 39/2023|||Portaria corrige erro da antiga portaria, designa nova equipe para fiscalizar contratos de obras asfálticas.\{\{record\_delimiter\**\**

entity|||4\_2\_7\_2023|||PORTARIA 24/2023|||Portaria designa equipe para fiscalizar contratos de obras asfálticas.\{\{record\_delimiter\**\**

entity|||5\_2\_1\_2023|||Artigo 1 da portaria 39/2023|||Designar os servidores Hayk Carvalho Silva, Marcelo Bisinoto Higino de Cuba e Celso Hiroki Sakuma, sob a coordenação deste último, com a assessoria dos servidores Daniel Menezes Brandão, Jonas Rodrigues de Cerqueira Neto, Waldir Araújo Mármore e Thiago Costa Campos, para comporem equipe de fiscalização junto à Agência Goiana de Infraestrutura e Transportes – GOINFRA.\{\{record\_delimiter\**\**

entity|||2\_3\_5\_2023|||Artigo 1 da portaria 24/2023|||Designar os servidores Hayk Carvalho Silva, Eugênio Eusébio Souza e Filipe Gomes Ramos, sob a coordenação deste último, com a assessoria dos servidores Roberto Carvalho Oliveira, Isabella Cruvinel Hemza, Waldir Araújo Mármore e Thiago Costa Campos, para comporem equipe de fiscalização junto à Agência Goiana de Infraestrutura e Transportes – GOINFRA.\{\{record\_delimiter\**\**

---------------------

saída:

\{\{initial\_delimiter\**\**

relação|||5\_2\_1\_2023|||Artigo 1 da PORTARIA 39/2023|||2\_3\_5\_2023|||Artigo 1 da PORTARIA 24/2023|||O artigo 1 da PORTARIA 39/2023 altera a PORTARIA 24/2023 designando novos integrantes para o time de fiscalização junto à GOINFRA.\{\{record\_delimiter\**\**

relação|||3\_3\_2\_2023|||PORTARIA 39/2023|||5\_2\_1\_2023|||Artigo 1 da portaria 39/2023|||A portaria 39/2023 contém o artigo 1 da portaria 39/2023.\{\{record\_delimiter\**\**

relação|||4\_2\_7\_2023|||PORTARIA 24/2023|||2\_3\_5\_2023|||Artigo 1 da portaria 24/2023|||A Portaria 24/2023 contém o Artigo 1 da portaria 24/2023.\{\{completion\_delimiter\**\**




**-Dados Reais-**


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Texto: \{input\_text\**

Entidades: \{entities\**

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Saída:



%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

## Filtragem e Classificação de Triplas**
{apend:1.6**

\setlength{\parindent**{0pt**


**-Objetivo-**


Sabendo que um documento jurídico é dividido em vários artigos, e que um artigo pode trazer uma informação nova ou referenciar um artigo de outro documento,

filtre e classifique uma lista de relações de entidades jurídicas.



**-Etapas-**


1. Retire da lista relações duplicadas. Relações duplicadas são identificadas caso possuam o mesmo par de entidades na relação.

2. Da lista de relações feita na etapa 1, retire da lista as relações que possuam um documento se relacionando com um artigo que não é dele.

O nome do artigo normalmente é ``Artigo N do DOCUMENTO YY/ZZ'', então para o artigo ser do documento, o nome do documento deverá ser ``DOCUMENTO XX/YYYY''.

3. Da lista de relações resultante dos passos 1 e 2, classifique as relações em: contém, altera, revoga ou retifica. A seguir, uma descrição de cada classificação.

     - contém: De documento para artigo, é uma relação que diz que um DOCUMENTO XX/YYYY contém um ARTIGO N do DOCUMENTO XX/YYYY.

     - revoga: De artigo para artigo, é uma relação que diz que um artigo N de um documento XX/YYYY revoga um artigo M de um documento AA/BBBB.

     - altera: De artigo para artigo, é uma relação que diz que um artigo N de um documento XX/YYYY altera um artigo M de um documento AA/BBBB.

     - retifica: De artigo para artigo, é uma relação que diz que um artigo N de um documento XX/YYYY retifica um artigo M de um documento AA/BBBB.

4. Dado a lista resultante produzida nos passos 1, 2, 3 e as classificações de cada relação, forneça uma nova lista trocando a descrição da relação pela sua classificação.

A relação ficará no seguinte formato:

relação|||<id\_entidade\_fonte>|||<entidade\_fonte>|||<id\_entidade\_alvo>|||<entidade\_alvo>

|||<classificação\_relacionamento>

5. Retorne a saída em português como uma lista única de todas as entidades identificadas nas etapas 1, 2, 3 e 4.

Use **\{\{initial\_delimiter\**\**** como token inicial da lista, **\{\{record\_delimiter\**\**** como o delimitador de cada item da lista e **\{\{completion\_delimiter\**\**** como o último token da lista.



**-Exemplo-**


Lista de relacionamentos:

relação|||1\_3\_2\_2013|||TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS

|||7\_3\_2\_2021|||RESOLUÇÃO Nº 99/1965|||O Tribunal de Contas do Estado de Goiás é o autor e responsável pela publicação da Resolução nº 99/1965.\{\{record\_delimiter\**\**
  
relação|||5\_2\_1\_2023|||Artigo 1 da PORTARIA 39/2023|||2\_3\_5\_2023|||Artigo 1 da PORTARIA 24/2023||| O artigo 1 da PORTARIA 3/2023 altera a PORTARIA 24/2024 designando novos integrantes para o time de fiscalização junto a GOINFRA.\{\{record\_delimiter\**\**

relação|||7\_3\_2\_2021|||RESOLUÇÃO Nº 99/1965|||6\_3\_2\_1999|||RESOLUÇÃO Nº 68, DE 18 DE FEVEREIRO DE 1965|||A Resolução nº 99/1965 altera dispositivos definidos na Resolução nº 68, transferindo competências descritas.\{\{record\_delimiter\**\**
  
relação|||4\_2\_7\_2023|||PORTARIA 24/2023|||2\_3\_5\_2023|||Artigo 1 da portaria 24/2023|||A Portaria 24/2023 contém o Artigo 1 da portaria 24/2023.


---------------------

saída:

\{\{initial\_delimiter\**\**

relação|||4\_2\_7\_2023|||PORTARIA 24/2023|||2\_3\_5\_2023|||Artigo 1 da portaria 24/2023|||contém\{\{record\_delimiter\**\**

relação|||5\_2\_1\_2023|||Artigo 1 da PORTARIA 39/2023|||2\_3\_5\_2023|||Artigo 1 da PORTARIA 24/2023|||altera

\{\{completion\_delimiter\**\**



**-Dados Reais-**


\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Lista de relacionamentos:
 
\{relationship\_list\**

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Saída:


