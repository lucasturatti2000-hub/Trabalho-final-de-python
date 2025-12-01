import csv
import os

ARQUIVO = "estoque.csv"


def inicializar_csv():
    """Cria o arquivo CSV caso não exista."""
    if not os.path.exists(ARQUIVO):
        with open(ARQUIVO, "w", newline="", encoding="utf-8") as f:
            writer = csv.writer(f)
            writer.writerow(["id", "produto", "quantidade"])  


def carregar_estoque():
    estoque = []
    with open(ARQUIVO, "r", encoding="utf-8") as f:
        reader = csv.DictReader(f)
        for linha in reader:
            estoque.append(linha)
    return estoque


def salvar_estoque(estoque):
    with open(ARQUIVO, "w", newline="", encoding="utf-8") as f:
        writer = csv.writer(f)
        writer.writerow(["id", "produto", "quantidade"])
        for item in estoque:
            writer.writerow([item["id"], item["produto"], item["quantidade"]])


def adicionar_produto():
    estoque = carregar_estoque()
    novo_id = len(estoque) + 1

    produto = input("Nome do produto: ")
    quantidade = int(input("Quantidade inicial: "))

    estoque.append({
        "id": str(novo_id),
        "produto": produto,
        "quantidade": str(quantidade)
    })

    salvar_estoque(estoque)
    print("\nProduto adicionado com sucesso!\n")


def listar_estoque():
    estoque = carregar_estoque()
    print("\n---- ESTOQUE ----")
    for item in estoque:
        print(f"{item['id']}. {item['produto']} — {item['quantidade']} unidades")
    print("-----------------\n")


def movimentar_estoque():
    estoque = carregar_estoque()
    listar_estoque()

    id_produto = input("ID do produto para movimentar: ")

    for item in estoque:
        if item["id"] == id_produto:
            movimento = int(input("Quantidade (positiva entra, negativa sai): "))
            nova_qtd = int(item["quantidade"]) + movimento

            if nova_qtd < 0:
                print("Erro: quantidade insuficiente no estoque!\n")
                return

            item["quantidade"] = str(nova_qtd)
            salvar_estoque(estoque)
            print("\nMovimentação registrada!\n")
            return

    print("Produto não encontrado.\n")


def menu():
    inicializar_csv()

    while True:
        print("=== CONTROLE DE ESTOQUE ===")
        print("1 - Adicionar produto")
        print("2 - Listar estoque")
        print("3 - Movimentar estoque")
        print("0 - Sair")

        opcao = input("Escolha: ")

        if opcao == "1":
            adicionar_produto()
        elif opcao == "2":
            listar_estoque()
        elif opcao == "3":
            movimentar_estoque()
        elif opcao == "0":
            print("Encerrando...")
            break
        else:
            print("Opção inválida!\n")


if __name__ == "__main__":
    menu()
