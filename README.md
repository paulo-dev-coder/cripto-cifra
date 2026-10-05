Uma criptografia em linguagem C com Cifra de César e sequências matemáticas como PA, PG, Primos e Fibonacci, menu interativo, ponteiros e log em arquivo.

# 🔐 Atividade Avaliativa 02 – Criptografia em C

Repositório desenvolvido para a disciplina **Algoritmo e Pensamento Computacional**, ministrada pelo professor **Francisco de Assis Cavallaro**.

Nome dos **alunos**: Alessa Araújo Ctvrtnik, João Vitor Mendes, Matheus Ferreira da Silva, Otávio Teixeira Lopes da Silva e Paulo Sergio Olivastro.

O projeto implementa, em **Linguagem C**, um sistema unificado de **criptografia em duas camadas**:

1. **Camada 1 – Cifra de César:** aplica um deslocamento fixo (`SHIFT`) sobre cada letra.
2. **Camada 2 – Deslocamento Dinâmico:** soma o termo correspondente de uma sequência matemática escolhida pelo usuário:
   - Progressão Aritmética (PA)
   - Progressão Geométrica (PG)
   - Números Primos
   - Série de Fibonacci

O sistema possui menu interativo, funções modularizadas, uso de ponteiros, tratamento do estouro circular do alfabeto (`'z' → 'a'`) e gravação do resultado final no arquivo `resultado_criptografia.txt`.

## Finalidades

- Aplicar conceitos de lógica, matemática e programação em C.
- Compreender técnicas clássicas de criptografia.
- Desenvolver pensamento crítico sobre segurança e padrões matemáticos.
- Praticar modularização, manipulação de arquivos e versionamento com Git/GitHub.

## Exemplo de uso

```text
Palavra: coracao
SHIFT: 3
Sequência: Fibonacci (Tipo 4)
