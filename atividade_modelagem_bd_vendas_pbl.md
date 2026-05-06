# Atividade PBL - Modelagem de Banco de Dados

**Curso:** Análise e Desenvolvimento de Sistemas  
**Disciplina:** Banco de Dados  
**Tema:** Sistema de vendas simples  

---

## Contexto do problema

Uma loja de bairro chamada **Ponto Certo** começou vendendo por WhatsApp e em caderno.  
Agora a equipe quer organizar as informações em um banco de dados para controlar melhor as vendas.

O dono da loja pediu para a equipe de tecnologia propor como os dados devem ser armazenados, pois hoje existem anotações espalhadas e muitos erros:

- cliente com nome repetido e telefone diferente;
- produto vendido com preço antigo;
- venda sem identificação clara de quem vendeu;
- dificuldade para descobrir quanto foi vendido por dia;
- dificuldade para saber quais produtos mais saem.

Seu grupo foi contratado para analisar o problema e propor as tabelas necessárias para esse sistema.

---

## Objetivo da atividade

Com base no cenário, seu time deve propor uma estrutura de tabelas para um sistema de vendas simples.
 
Foquem em:

- quais informações precisam ser guardadas;
- como separar essas informações em tabelas;
- como as tabelas se relacionam;
- quais campos parecem essenciais em cada tabela.

---

## Regras de negócio levantadas com o cliente

Use estas regras como base da análise:

1. A loja vende vários produtos.
2. Cada produto possui código interno, nome, preço atual e quantidade em estoque.
3. Produtos podem ser agrupados por tipo (ex.: bebida, limpeza, mercearia).
4. Cada venda é registrada com data e hora.
5. Uma venda pode ter um ou mais produtos.
6. Um mesmo produto pode aparecer em muitas vendas diferentes.
7. Em cada item vendido, deve ser guardada a quantidade e o preço praticado no momento da venda.
8. Cada venda pode estar em um status (ex.: aberta, finalizada, cancelada).
9. A loja pode identificar o cliente na venda, mas em alguns casos a venda acontece sem cadastro de cliente.
10. Quando houver cliente cadastrado, devem existir pelo menos nome e um contato.
11. Um vendedor atende várias vendas ao longo do dia.
12. Toda venda finalizada tem uma forma de pagamento (ex.: dinheiro, pix, cartão).
13. Uma venda pode ser paga em mais de uma parte (ex.: parte no pix e parte no cartão).
14. O gerente quer consultar o total vendido por período e por vendedor.
15. O gerente quer descobrir os produtos mais vendidos e clientes que mais compram.

---

## Entrega esperada do grupo

Produzam um documento curto com:

1. Lista das tabelas que o grupo considera necessárias.
2. Para cada tabela, lista de campos principais.
3. Indicação do que pode funcionar como identificador único (chave principal).
4. Explicação textual dos relacionamentos entre tabelas (ex.: uma venda tem vários itens).
5. Duas consultas SQL que vocês gostariam de conseguir responder com o modelo proposto.

Não se preocupem em acertar tudo. O foco da atividade é justificar as decisões com base no problema.

---

## Perguntas-guia para discussão (PBL)

Use estas perguntas para orientar o raciocínio do grupo:

1. Se colocarmos tudo em uma única tabela, quais problemas podem aparecer?
2. Quais dados mudam com frequência e quais dados são mais estáveis?
3. Em que pontos pode haver repetição desnecessária de informações?
4. O que precisa ser guardado para manter o histórico correto de uma venda?
5. Quais relacionamentos parecem ser de um para muitos?
6. Existe algum relacionamento de muitos-para-muitos? Como representar isso?
7. Como garantir que uma venda sem itens não seja considerada válida?
8. Como representar pagamentos parciais sem perder rastreabilidade?
9. Quais campos seriam obrigatórios para evitar registros incompletos?
10. Como o modelo ajudaria nas consultas de gestão pedidas pelo gerente?

---

## Combinado da atividade

Nesta fase, não existe resposta única.  
Cada time deve defender suas escolhas com argumentos técnicos simples, baseados no cenário.

Depois desta atividade, a turma verá técnicas formais de modelagem para comparar e evoluir as propostas.
