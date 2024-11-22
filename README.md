# Tipos de Exchanges no RabbitMQ

### 1. Direct Exchange

* Descrição:
  * A exchange roteia mensagens para as queues que têm uma binding key exatamente igual à routing key da mensagem.
* Exemplo de Cenário:
  * Aplicação de envio de notificações onde diferentes tipos de mensagens (como "email" ou "sms") são roteadas para filas específicas.
* Características:
  * Alta precisão no roteamento.
  * Útil para sistemas onde cada mensagem tem um destinatário específico.
  
# 
```mermaid
graph TD
    Producer -->|Routing Key: email| Direct_Exchange
    Direct_Exchange -->|Binding Key: email| Queue_Email
    Direct_Exchange -->|Binding Key: sms| Queue_SMS
```
### 2. Fanout Exchange
* Descrição:
  * A exchange ignora a routing key e entrega a mensagem para todas as queues vinculadas.
* Exemplo de Cenário:
  * Sistema de publicação de eventos, como notificações de novas postagens ou atualizações, onde todas as partes interessadas precisam receber a mensagem.
* Características:
  * Roteamento sem filtros.
  * Útil para sistemas de broadcast.
 
# 
```mermaid
graph TD
    Producer --> Fanout_Exchange
    Fanout_Exchange --> Queue_1
    Fanout_Exchange --> Queue_2
    Fanout_Exchange --> Queue_3
```
### 3. Topic Exchange

* Descrição:
  * A exchange roteia mensagens com base em padrões na routing key. Suporta caracteres especiais como:
  * `*` (um único nível no caminho).
  * `#` (zero ou mais níveis no caminho).
* Exemplo de Cenário:
  * Sistema de logs, onde as mensagens são roteadas com base em categorias como "error.*", "info.#", etc.
* Características:
  * Flexibilidade no roteamento.
  * Útil para sistemas complexos com várias regras de roteamento.

#
```mermaid
graph TD
    Producer -->|Routing Key: error.critical| Topic_Exchange
    Topic_Exchange -->|Binding Key: error.*| Queue_Error
    Topic_Exchange -->|Binding Key: info.#| Queue_Info
```
### 4. Headers Exchange
* Descrição:
  * A exchange roteia mensagens com base nos cabeçalhos (headers) da mensagem em vez de usar a routing key.
* Exemplo de Cenário:
  * Aplicações onde o roteamento depende de informações específicas como "formato de arquivo" ou "prioridade".
* Características:
  * Muito flexível, mas mais pesado em termos de desempenho.
  * Útil para sistemas que dependem de metadados detalhados.

#
```mermaid
graph TD
    Producer -->|Headers: format=pdf| Headers_Exchange
    Headers_Exchange -->|Header Match: format=pdf| Queue_PDF
    Headers_Exchange -->|Header Match: format=xml| Queue_XML
```
### Resumo dos Cenários
<table>
  <thead>
    <tr align="left">
      <th>Tipo de Exchange</th>
      <th>Cenário Ideal</th>
    </tr>
  </thead>
  <tbody align="left">
    <tr>
      <td>Direct</td>
      <td>Mensagens direcionadas com base em routing keys exatas.</td>
    </tr>
    <tr>
      <td>Fanout</td>
      <td>Mensagens para todas as filas vinculadas (broadcast).</td>
    </tr>
    <tr>
      <td>Topic</td>
      <td>Mensagens filtradas com base em padrões complexos.</td>  
    </tr>
    <tr>
      <td>Headers</td>
      <td>Mensagens filtradas com base em cabeçalhos específicos, ignorando a routing key.</td>    
    </tr>
  </tbody>
</table>

# 
```mermaid
graph TD
    Producer -->|Routing Key| Exchange_Type
    Exchange_Type -->|Binding Key or Rule| Queue_A
    Exchange_Type -->|Binding Key or Rule| Queue_B
    Exchange_Type -->|Binding Key or Rule| Queue_C
```





