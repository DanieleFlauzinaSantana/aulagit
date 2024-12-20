# aulagit
import json
import os

# Caminho do arquivo JSON para armazenar as tarefas
FILE_PATH = "tasks.json"

# Função para carregar as tarefas
def load_tasks():
    if os.path.exists(FILE_PATH):
        with open(FILE_PATH, "r") as file:
            return json.load(file)
    return []

# Função para salvar as tarefas
def save_tasks(tasks):
    with open(FILE_PATH, "w") as file:
        json.dump(tasks, file, indent=4)

# Função para adicionar uma nova tarefa
def add_task(title, description):
    tasks = load_tasks()
    tasks.append({"title": title, "description": description, "completed": False})
    save_tasks(tasks)
    print(f"Tarefa '{title}' adicionada com sucesso!")

# Função para listar todas as tarefas
def list_tasks():
    tasks = load_tasks()
    if not tasks:
        print("Nenhuma tarefa encontrada.")
        return
    for i, task in enumerate(tasks, start=1):
        status = "✅" if task["completed"] else "❌"
        print(f"{i}. {task['title']} - {task['description']} [{status}]")

# Função para marcar uma tarefa como concluída
def complete_task(index):
    tasks = load_tasks()
    if 0 <= index < len(tasks):
        tasks[index]["completed"] = True
        save_tasks(tasks)
        print(f"Tarefa '{tasks[index]['title']}' marcada como concluída!")
    else:
        print("Índice inválido.")

# Função para excluir uma tarefa
def delete_task(index):
    tasks = load_tasks()
    if 0 <= index < len(tasks):
        removed = tasks.pop(index)
        save_tasks(tasks)
        print(f"Tarefa '{removed['title']}' excluída com sucesso!")
    else:
        print("Índice inválido.")

# Menu principal
def main():
    while True:
        print("\n--- Gerenciador de Tarefas ---")
        print("1. Adicionar Tarefa")
        print("2. Listar Tarefas")
        print("3. Concluir Tarefa")
        print("4. Excluir Tarefa")
        print("5. Sair")

        choice = input("Escolha uma opção: ")

        if choice == "1":
            title = input("Título da tarefa: ")
            description = input("Descrição da tarefa: ")
            add_task(title, description)
        elif choice == "2":
            list_tasks()
        elif choice == "3":
            try:
                index = int(input("Digite o índice da tarefa para concluir: ")) - 1
                complete_task(index)
            except ValueError:
                print("Por favor, insira um número válido.")
        elif choice == "4":
            try:
                index = int(input("Digite o índice da tarefa para excluir: ")) - 1
                delete_task(index)
            except ValueError:
                print("Por favor, insira um número válido.")
        elif choice == "5":
            print("Saindo... Até mais!")
            break
        else:
            print("Opção inválida. Tente novamente.")

if __name__ == "__main__":
    main()

