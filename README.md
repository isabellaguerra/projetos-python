# projetos-python editor de tarefas
import sqlite3

def criar_banco():
    conexao = sqlite3.connect('tarefas.db')
    cursor = conexao.cursor()
    
    cursor.execute('''
    CREATE TABLE IF NOT EXISTS tarefas (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        titulo TEXT NOT NULL,
        descricao TEXT,
        data_criacao TEXT DEFAULT (datetime('now', 'localtime')),
        concluida INTEGER DEFAULT 0
    )
    ''')
    
    conexao.commit()
    conexao.close()

if __name__ == "__main__":
    criar_banco()
    print("Banco de dados 'tarefas.db' criado com sucesso!")

import sqlite3

class GerenciadorTarefas:
    def __init__(self):
        self.conexao = sqlite3.connect('tarefas.db')
        self.cursor = self.conexao.cursor()
    
    def adicionar_tarefa(self, titulo, descricao=""):
        self.cursor.execute(
            "INSERT INTO tarefas (titulo, descricao) VALUES (?, ?)",
            (titulo, descricao)
        )
        self.conexao.commit()
        print(f"Tarefa '{titulo}' adicionada!")
    
    def listar_tarefas(self):
        self.cursor.execute("SELECT * FROM tarefas")
        tarefas = self.cursor.fetchall()
        
        if not tarefas:
            print("Nenhuma tarefa encontrada.")
            return
        
        print("\n--- SUAS TAREFAS ---")
        for tarefa in tarefas:
            status = "✓" if tarefa[4] else " "
            print(f"{tarefa[0]}. [{status}] {tarefa[1]} - {tarefa[2]}")
    
    def marcar_concluida(self, id_tarefa):
        self.cursor.execute(
            "UPDATE tarefas SET concluida = 1 WHERE id = ?",
            (id_tarefa,)
        )
        self.conexao.commit()
        print("Tarefa concluída!")
    
    def remover_tarefa(self, id_tarefa):
        self.cursor.execute(
            "DELETE FROM tarefas WHERE id = ?",
            (id_tarefa,)
        )
        self.conexao.commit()
        print("Tarefa removida!")
    
    def __del__(self):
        self.conexao.close()


       
        GerenciadorTarefas

def menu():
    print("\n=== Gerenciador de Tarefas ===")
    print("1. Adicionar Tarefa")
    print("2. Listar Tarefas")
    print("3. Marcar como Concluída")
    print("4. Remover Tarefa")
    print("5. Sair")
    return input("Escolha uma opção: ")

def main():
    gerenciador = GerenciadorTarefas()
    
    while True:
        opcao = menu()
        
        if opcao == "1":
            titulo = input("Título da tarefa: ")
            descricao = input("Descrição (opcional): ")
            gerenciador.adicionar_tarefa(titulo, descricao)
        
        elif opcao == "2":
            gerenciador.listar_tarefas()
        
        elif opcao == "3":
            id_tarefa = input("ID da tarefa concluída: ")
            gerenciador.marcar_concluida(id_tarefa)
        
        elif opcao == "4":
            id_tarefa = input("ID da tarefa a remover: ")
            gerenciador.remover_tarefa(id_tarefa)
        
        elif opcao == "5":
            print("Até logo! 👋")
            break
        
        else:
            print("Opção inválida!")

if __name__ == "__main__":
    main()

    input()
