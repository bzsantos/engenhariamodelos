# MANUAL DE INTEGRAÇÃO: DO PROCESSO AO CÓDIGO
**Disciplina:** Engenharia de Software II  
**Professor:** Bruno Zolotareff
**Objetivo:** Garantir a consistência entre o BPMN (Comportamento) e o Diagrama de Classes (Estrutura).

---

## 1. INTRODUÇÃO
Na engenharia de software, diagramas não são desenhos isolados. O **BPMN** descreve o "como o negócio funciona", enquanto o **Diagrama de Classes** descreve a "estrutura que sustenta o negócio". Se um elemento aparece no processo, ele deve ter um "lar" na estrutura.

---

## 2. CHECKLIST DE CONSISTÊNCIA (Os 3 Pilares)

Antes de entregar seu projeto, verifique se seus diagramas passam no teste abaixo:

### A. Rastreabilidade de Dados (Substantivos)
* **Foco:** *Data Objects* (ícones de folha) e *Data Stores* no BPMN.
* [ ] Cada objeto de dado que flui no BPMN possui uma **Classe** correspondente?
* [ ] Os dados preenchidos em tarefas (ex: Nome, Valor) existem como **Atributos** nas classes?

### B. Análise de Verbos (Ações vs. Métodos)
* **Foco:** *Tasks* (Tarefas) dentro das raias (Lanes) do sistema.
* [ ] O verbo da tarefa (ex: *Validar Estoque*) existe como um **Método** em alguma classe?
* [ ] O método está na classe correta? (Evite colocar tudo em uma classe "Sistema").

### C. Ciclo de Vida e Estados (Status)
* **Foco:** *Gateways* (Decisões) e fluxos condicionais.
* [ ] A classe principal possui um atributo de **status**?
* [ ] Os nomes das saídas dos Gateways (ex: "Aprovado", "Recusado") são valores previstos para esse atributo de status?

---

## 3. EXEMPLO PRÁTICO: PROCESSO DE PEDIDO

Para entender como aplicar o checklist, observe a integração abaixo para um sistema simples de vendas:

### Passo 1: O Fluxo (BPMN)
Imagine o seguinte processo:
1.  O cliente solicita um **Pedido** (*Data Object*).
2.  O sistema executa a tarefa **Verificar Disponibilidade**.
3.  Um **Gateway** decide: se houver estoque, o pedido é **Confirmado**; caso contrário, é **Cancelado**.

### Passo 2: A Estrutura (Diagrama de Classes)
Para sustentar o processo acima, o Diagrama de Classes **precisa** conter:

| Elemento no BPMN | Elemento na Classe | Por que? |
| :--- | :--- | :--- |
| Objeto de Dado `Pedido` | Classe `Pedido` | Para armazenar os dados da venda. |
| Tarefa `Verificar Disponibilidade` | Método `Produto.checarEstoque()` | Toda ação do processo deve ser um método no código. |
| Gateway `Disponível?` | Atributo `Pedido.status` | Para registrar o resultado da decisão do processo. |

---

## 4. ROTEIRO PARA EXECUÇÃO

Siga estes passos para construir seus modelos de forma integrada:

1.  **Mapeie os Substantivos:** Liste todos os documentos e informações que circulam no seu BPMN. Transforme-os em **Classes e Atributos**.
2.  **Mapeie os Verbos:** Liste todas as ações que o sistema faz nas tarefas do BPMN. Transforme-as em **Métodos** dentro das classes.
3.  **Mapeie as Decisões:** Observe seus Gateways. Eles alteram o rumo do processo? Então sua classe precisa de um atributo (como um `Enum`) para rastrear esse **Estado**.

---

## 🚩 SINAIS DE ALERTA (RED FLAGS)

* **O Objeto Mágico:** Um dado aparece no BPMN, mas não há classe para ele.
* **Ação sem Dono:** Existe uma tarefa no processo, mas nenhuma classe possui um método para realizá-la.
* **Lane Fantasma:** Existe uma raia (Lane) para um sistema, mas o diagrama de classes não possui classes que representem as funções dessa raia.

---

