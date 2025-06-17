#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX_PAISES 6
#define TAM_NOME 30
#define TAM_CONTINENTE 15

typedef struct {
    char continente[TAM_CONTINENTE];
    int codigo;
    char nome[TAM_NOME];
    long populacao;
    double pib;             // em bilhões de dólares
    double area;            // em km²
    int pontos_turisticos;
    double densidade;       // hab/km²
    double pib_per_capita;  // dólares por habitante
} Pais;

void limparBuffer() {
    while (getchar() != '\n');
}

void cadastrarPais(Pais paises[], int *total) {
    if (*total >= MAX_PAISES) {
        printf("\nLimite de 6 países atingido!\n");
        return;
    }

    printf("\n=== Cadastro do País %d/%d ===\n", *total + 1, MAX_PAISES);
    
    // Continente
    printf("Continente: ");
    fgets(paises[*total].continente, TAM_CONTINENTE, stdin);
    paises[*total].continente[strcspn(paises[*total].continente, "\n")] = '\0';
    
    // Código
    printf("Código (numérico): ");
    scanf("%d", &paises[*total].codigo);
    
    // Nome
    limparBuffer();
    printf("Nome do país: ");
    fgets(paises[*total].nome, TAM_NOME, stdin);
    paises[*total].nome[strcspn(paises[*total].nome, "\n")] = '\0';
    
    // População
    printf("População (habitantes): ");
    scanf("%ld", &paises[*total].populacao);
    
    // PIB
    printf("PIB (bilhões de US$): ");
    scanf("%lf", &paises[*total].pib);
    
    // Área
    printf("Área (km²): ");
    scanf("%lf", &paises[*total].area);
    
    // Pontos turísticos
    printf("Pontos turísticos: ");
    scanf("%d", &paises[*total].pontos_turisticos);
    
    // Calcular propriedades derivadas
    paises[*total].densidade = (double)paises[*total].populacao / paises[*total].area;
    paises[*total].pib_per_capita = (paises[*total].pib * 1e9) / paises[*total].populacao;
    
    (*total)++;
    printf("País registrado com sucesso!\n");
}

void mostrarResumo(const Pais *p) {
    printf("\n--- %s [%s] ---\n", p->nome, p->continente);
    printf("Código: %d\n", p->codigo);
    printf("População: %ld hab\n", p->populacao);
    printf("Área: %.2lf km²\n", p->area);
    printf("Densidade: %.2lf hab/km²\n", p->densidade);
    printf("PIB: US$ %.2lf bilhões\n", p->pib);
    printf("PIB per capita: US$ %.2lf\n", p->pib_per_capita);
    printf("Pontos turísticos: %d\n", p->pontos_turisticos);
}

void listarPaises(Pais paises[], int total) {
    if (total == 0) {
        printf("\nNenhum país registrado!\n");
        return;
    }
    
    printf("\n===== PAÍSES REGISTRADOS =====\n");
    for (int i = 0; i < total; i++) {
        mostrarResumo(&paises[i]);
    }
    printf("\nTotal: %d países\n", total);
}

int main() {
    Pais paises[MAX_PAISES];
    int totalPaises = 0;
    int opcao;
    
    printf("Sistema de Registro de Países - Capacidade: %d países\n", MAX_PAISES);
    
    do {
        printf("\n=== MENU PRINCIPAL ===\n");
        printf("1. Adicionar novo país\n");
        printf("2. Listar todos os países\n");
        printf("3. Sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);
        
        switch (opcao) {
            case 1:
                cadastrarPais(paises, &totalPaises);
                break;
            case 2:
                listarPaises(paises, totalPaises);
                break;
            case 3:
                printf("\nEncerrando o programa...\n");
                break;
            default:
                printf("\nOpção inválida! Tente novamente.\n");
        }
        limparBuffer();
    } while (opcao != 3);
    
    return 0;
}
