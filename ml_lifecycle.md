# Problemática no Ciclo de Vida de Modelos de Machine Learning

**O modelo deve ser reprodutível**

- Qual versão gerou o atual modelo/
- Quais hiper parâmetros foram utilizados na criação de um modelo?
- Moduralização: Código genérico, funciona em todos os casos(ex novos atributos)


**É preciso versionar, não só codar**
- Código
- Hiper parâmetros
- Dados utilizados

**Testes**

- Quais métricas serão avaliadas?
- Quais são os limites de aceitação?

**Dados**

- Gerenciamento de Metadados
- Composto por 
  - atributos
  - tipos
- Podem conter
  - Nulos ou em branco
  - Valores fora do domínio
  - Novos domínios

**Monitoramento**
- Modelos tenden a ter performance degradada
- Existem oportunidades para melhor o modelo?
- Performance
- Computacional
  - Error, latência, time-out
  - Modelo