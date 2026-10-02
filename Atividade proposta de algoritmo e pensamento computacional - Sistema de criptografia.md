%%writefile cripto.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define TAM_MAX_PALAVRA     16          /* 15 letras + '\0' */
#define ARQUIVO_SAIDA       "resultado_criptografia.txt"
#define TAM_ALFABETO        26

// Nome dos alunos: Alessa Araújo Ctvrtnik, João Vitor Mendes, Matheus Ferreira da Silva, Otávio Teixeira Lopes da Silva e Paulo Sergio Olivastro

int  menuSequencia(void);
void limparBuffer(void);

int  termoPA         (int i, int a1, int r);
int  termoPG         (int i, int a1, int q);
int  ehPrimo         (int n);
int  termoPrimo      (int i);
int  termoFibonacci  (int i);
int  termoSequencia  (int tipo, int i);

void criptografar    (const char *entrada, char *saida,
                      int shift, int tipo);

void gravarLog       (const char *palavraCodificada,
                      int shift, int tipo, int letras);

// Utilitários

void limparBuffer(void) {
    int c;
    while ((c = getchar()) != '\n' && c != EOF) { /* descarta */ }
}

// Sequências matemáticas

/* PA: a_n = a1 + (n-1)*r  (índice i 0-based -> n = i+1) */
int termoPA(int i, int a1, int r) {
    return a1 + i * r;
}

/* PG: a_n = a1 * q^(n-1)  (com proteção contra overflow simples) */
int termoPG(int i, int a1, int q) {
    long long v = a1;
    for (int k = 0; k < i; k++) v *= q;
    return (int)(v % 1000);        /* mantém valor utilizável */
}

// Verificação de primalidade
int ehPrimo(int n) {
    if (n < 2) return 0;
    for (int d = 2; (long long)d * d <= n; d++)
        if (n % d == 0) return 0;
    return 1;
}

// Retorna o (i)-ésimo número primo em base 0: 2,3,5,7,11,...
int termoPrimo(int i) {
    int cont = -1, n = 1;
    while (cont < i) {
        n++;
        if (ehPrimo(n)) cont++;
    }
    return n;
}

// Fibonacci 0-based: 1,1,2,3,5,8,13,...
int termoFibonacci(int i) {
    if (i == 0 || i == 1) return 1;
    int a = 1, b = 1, c = 1;
    for (int k = 2; k <= i; k++) {
        c = a + b;
        a = b;
        b = c;
    }
    return b;
}

// Dispatcher: escolhe a sequência pelo tipo (1..4)
int termoSequencia(int tipo, int i) {
    switch (tipo) {
        case 1: return termoPA(i, 5, 3);      /* PA: 5, 8, 11, 14... */
        case 2: return termoPG(i, 2, 2);      /* PG: 2, 4, 8, 16...  */
        case 3: return termoPrimo(i);         /* Primos: 2, 3, 5, 7... */
        case 4: return termoFibonacci(i);     /* Fib: 1, 1, 2, 3, 5... */
        default: return 0;
    }
}

// Núcleo da criptografia

void criptografar(const char *entrada, char *saida,
                  int shift, int tipo) {
    int n = (int)strlen(entrada);

    for (int i = 0; i < n; i++) {
        int base     = tolower((unsigned char)entrada[i]) - 'a';
        int desloc   = shift + termoSequencia(tipo, i);
        int novo     = ((base + desloc) % TAM_ALFABETO
                        + TAM_ALFABETO) % TAM_ALFABETO;
        saida[i] = (char)('a' + novo);
    }
    saida[n] = '\0';
}

void gravarLog(const char *palavraCodificada,
               int shift, int tipo, int letras) {
    FILE *fp = fopen(ARQUIVO_SAIDA, "a");
    if (fp == NULL) {
        perror("Erro ao abrir o arquivo de log");
        return;
    }
    fprintf(fp,
        "Palavra codificada: %s | SHIFT: %d | Tipo: %d | Letras: %d\n",
        palavraCodificada, shift, tipo, letras);
    fclose(fp);
}

// Menu das sequências númericas
int menuSequencia(void) {
    int op;
    printf("\n Escolha a sequência matemática \n");
    printf(" 1 - Progressao Aritmetica (PA: 5, 8, 11, 14, ...)\n");
    printf(" 2 - Progressao Geometrica (PG: 2, 4, 8, 16, ...)\n");
    printf(" 3 - Numeros Primos          (2, 3, 5, 7, ...)\n");
    printf(" 4 - Serie de Fibonacci      (1, 1, 2, 3, 5, ...)\n");
    printf("Opcao: ");
    if (scanf("%d", &op) != 1) {
        limparBuffer();
        return -1;
    }
    limparBuffer();
    return op;
}

// Programa principal

int main(void) {
    char palavra[TAM_MAX_PALAVRA];
    char codificada[TAM_MAX_PALAVRA];
    int  shift = 0;
    int  tipo  = 0;

    printf("Insira a palavra secreta (máx. 15 letras, sem acentos): ");
    if (scanf("%15s", palavra) != 1) {
        printf("Erro na leitura da palavra.\n");
        return 1;
    }
    limparBuffer();

    for (int i = 0; palavra[i] != '\0'; i++) {
        if (!isalpha((unsigned char)palavra[i])) {
            printf("Erro: use apenas letras (a-z).\n");
            return 1;
        }
    }

    printf("Digite o valor de SHIFT (ex: 1, 2, 3 ou 4): ");
    if (scanf("%d", &shift) != 1) {
        printf("Erro: SHIFT invalido.\n");
        return 1;
    }
    limparBuffer();

    do {
        tipo = menuSequencia();
        if (tipo < 1 || tipo > 4)
            printf("Opcao invalida. Escolha entre 1 e 4.\n");
    } while (tipo < 1 || tipo > 4);

    criptografar(palavra, codificada, shift, tipo);

    printf("Palavra original  : %s\n", palavra);
    printf("SHIFT aplicado    : %d\n", shift);
    printf("Tipo de sequencia  : %d\n", tipo);
    printf("Palavra codificada: %s\n", codificada);

    gravarLog(codificada, shift, tipo, (int)strlen(palavra));
    printf("\nLog gravado em '%s'.\n", ARQUIVO_SAIDA);

    return 0;
}