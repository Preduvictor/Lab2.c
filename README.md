#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_SENHAS 100

typedef struct {
    int numero;
    char tipo[10];
} Senha;

Senha senhas[MAX_SENHAS];
int countNormal = 0;
int countPrioridade = 0;
int totalSenhas = 0;

void inserirSenha(const char *tipo) {
    if (totalSenhas >= MAX_SENHAS) {
        printf("Limite máximo de senhas atingido!\n");
        return;
    }

    Senha novaSenha;
    if (strcmp(tipo, "NORMAL") == 0) {
        novaSenha.numero = ++countNormal;
        strcpy(novaSenha.tipo, "NORMAL");
    } else {
        novaSenha.numero = ++countPrioridade;
        strcpy(novaSenha.tipo, "PRIORIDADE");
    }

    int pos = totalSenhas;
    if (strcmp(novaSenha.tipo, "PRIORIDADE") == 0) {
        while (pos > 0 && strcmp(senhas[pos - 1].tipo, "NORMAL") == 0) {
            senhas[pos] = senhas[pos - 1];
            pos--;
        }
    }
    senhas[pos] = novaSenha;
    totalSenhas++;
    printf("Senha %s gerada: %s-%d\n", novaSenha.tipo, novaSenha.tipo[0] == 'P' ? "P" : "N", novaSenha.numero);
}

void consumirSenha() {
    if (totalSenhas == 0) {
        printf("Não há senhas para consumir.\n");
        return;
    }

    printf("Consumindo senha: %s-%d\n", senhas[0].tipo, senhas[0].numero);

    for (int i = 0; i < totalSenhas - 1; i++) {
        senhas[i] = senhas[i + 1];
    }
    totalSenhas--;
}

void visualizar() {
    if (totalSenhas == 0) {
        printf("Nenhuma senha para exibir.\n");
        return;
    }

    printf("Senhas Prioridade:\n");
    for (int i = 0; i < totalSenhas; i++) {
        if (strcmp(senhas[i].tipo, "PRIORIDADE") == 0) {
            printf("Senha %s-%d\n", senhas[i].tipo, senhas[i].numero);
        }
    }

    printf("Senhas Normais:\n");
    for (int i = 0; i < totalSenhas; i++) {
        if (strcmp(senhas[i].tipo, "NORMAL") == 0) {
            printf("Senha %s-%d\n", senhas[i].tipo, senhas[i].numero);
        }
    }
}

void menu_interativo() {
    char opcao;

    do {
        printf("\n--- Menu Interativo ---\n");
        printf("Digite uma opção:\n");
        printf("N - Inserir senha normal\n");
        printf("P - Inserir senha com prioridade\n");
        printf("C - Consumir senha\n");
        printf("S - Sair do programa\n");
        printf("V - Visualizar todas as senhas não consumidas\n");

        printf("Escolha uma opção: ");
        scanf(" %c", &opcao);
        opcao = toupper(opcao);

        switch(opcao) {
            case 'N':
                inserirSenha("NORMAL");
                break;
            case 'P':
                inserirSenha("PRIORIDADE");
                break;
            case 'C':
                consumirSenha();
                break;
            case 'S':
                printf("Saindo do programa...\n");
                break;
            case 'V':
                visualizar();
                break;
            default:
                printf("Opção inválida! Tente novamente.\n");
                break;
        }
    } while(opcao != 'S');
}

int main() {
    menu_interativo();
    return 0;
}
