# projeto-bd-agregação





**Sistema de Gerenciamento de Alocações e Recursos**



Este projeto apresenta uma modelagem de banco de dados SQL desenvolvida para gerenciar funcionários, suas hierarquias, dependentes e a alocação de equipamentos dentro de projetos específicos.



**1. Diagrama de Entidade-Relacionamento (ER)**



O diagrama abaixo utiliza a sintaxe Mermaid para representar a estrutura do banco de dados:   



###### &#x20;*%% Relacionamentos*

###### &#x20;   *FUNCIONARIO ||--o{ FUNCIONARIO : "gerencia (supervisor\_id)"*

###### &#x20;   *FUNCIONARIO ||--o{ DEPENDENTE : "possui"*

###### &#x20;   *FUNCIONARIO ||--o{ ALOCACAO\_EQUIPAMENTO : "alocado em"*

###### &#x20;   *PROJETO ||--o{ ALOCACAO\_EQUIPAMENTO : "utiliza"*

###### 

###### &#x20;   *%% Entidades e Atributos*

###### &#x20;   *FUNCIONARIO {*

###### &#x20;       *int id PK*

###### &#x20;       *string nome*

###### &#x20;       *int supervisor\_id FK*

###### &#x20;   *}*

###### 

###### &#x20;   *DEPENDENTE {*

###### &#x20;       *int id PK*

###### &#x20;       *string nome*

###### &#x20;       *int funcionario\_id FK*

###### &#x20;   *}*

###### 

###### &#x20;   *PROJETO {*

###### &#x20;       *int id PK*

###### &#x20;       *string nome\_projeto*

###### &#x20;   *}*

###### 

###### &#x20;   *ALOCACAO\_EQUIPAMENTO {*

###### &#x20;       *int id PK*

###### &#x20;       *int funcionario\_id FK*

###### &#x20;       *int projeto\_id FK*

###### &#x20;       *string nome\_equipamento*

###### &#x20;   *}*



**2. Implementação do Autorelacionamento (Hierarquia)**



O autorelacionamento foi aplicado na tabela funcionario através da coluna supervisor\_id.



Um supervisor também é um funcionário. Portanto, a chave estrangeira aponta para a chave primária da própria tabela.

Para listar quem reporta a quem, utilizamos um LEFT JOIN. Isso garante que mesmo os diretores (que não possuem supervisor) apareçam na lista:



###### *f.nome AS "Nome do Funcionário",* 

###### *s.nome AS "Nome do Supervisor"*

###### *FROM funcionario f*

###### *LEFT JOIN funcionario s ON f.supervisor\_id = s.id;*



**3. Resolução da Agregação (Equipamentos em Projetos)**



No modelo conceitual, a Agregação ocorre quando um relacionamento precisa ser tratado como uma entidade para se relacionar com outra.



O equipamento não está ligado apenas ao "Projeto" ou apenas ao "Funcionário", mas sim ao fato de um funcionário específico estar trabalhando em um projeto específico.

Criamos a tabela alocacao\_equipamento. Ela "envelopa" o relacionamento entre Funcionário e Projeto e adiciona o atributo do Equipamento. Assim, conseguimos rastrear exatamente qual recurso está vinculado a qual par de (Pessoa + Tarefa).



