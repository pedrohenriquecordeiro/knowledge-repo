# **DAG no Apache Airflow?**

Se você já trabalha com o Apache Airflow, sabe que tudo começa pelas **DAGs (Directed Acyclic Graphs)**. Elas são a base da ferramenta e ajudam a organizar o que será feito, quando e em que ordem. Vamos entender o que é uma DAG, os principais parâmetros que você pode configurar, como os decorators se relacionam com a criação de DAGs e, claro, conferir um exemplo prático de código.



## **O que é uma DAG?**

Uma DAG nada mais é do que um gráfico que organiza tarefas dentro de um fluxo de trabalho. Ela define as dependências entre as tarefas e garante que elas sejam executadas na ordem certa. O "Acyclic" do nome significa que não pode haver ciclos, ou seja, uma tarefa não pode depender de si mesma.

Na arquitetura do Airflow:
- O **Scheduler** lê as DAGs e decide quais tarefas precisam ser executadas.
- O **Database** armazena informações como dependências, histórico e status das execuções.
- O **Web Server** exibe as DAGs, mostrando o que está rodando e permitindo interação com eles.



## **Principais Parâmetros de Configuração de um DAG**

Ao criar uma DAG, você pode configurar vários parâmetros importantes. Aqui estão os principais:

1. **`dag_id`**  
   - Nome único para identificar a DAG.

2. **`schedule_interval`**  
   - Define quando a DAG será executada.
   - Exemplos: `"@daily"` (uma vez por dia), `"@hourly"` (uma vez por hora), ou expressões cron como `"0 12 * * *"`.

3. **`start_date`**  
   - Data de início da DAG. Ela só começará a rodar após essa data.

4. **`verbose`**  
   - Define se a DAG terá mais detalhes no log. Geralmente é usado para depuração.

5. **`tags`**  
   - Lista de palavras-chave que ajudam a categorizar as DAGs.
   - Exemplo: `tags=["data-science", "treinamento"]`.

6. **`orientation`**  
   - Controla o layout visual da DAG na interface.
   - Exemplo: `"LR"` (da esquerda para a direita) ou `"TB"` (de cima para baixo).

7. **`catchup`**  
   - Define se a Airflow deve "compensar" execuções pendentes desde o `start_date`.
   - Por padrão, é `True`.

8. **`max_active_runs`**  
   - É usado para controlar o número máximo de execuções paralelas de um fluxo de trabalho.



### **O Que é um Decorator?**

No Python, um **decorator** é uma função que modifica o comportamento de outra função ou método. No Airflow, decorators simplificam a criação de DAGs e tarefas. Por exemplo, ao usar o `@dag` decorator, você pode transformar uma função Python em uma DAG, o que torna o código mais limpo e intuitivo.



### **Exemplo de Código**

Aqui está um exemplo simples de uma DAG no Airflow, usando operadores Bash e Python.

```python
from airflow import DAG  # Importa a classe DAG para criar o fluxo de trabalho
from airflow.decorators import dag, task  # Importa o decorator @dag para simplificar a criação do DAG
from airflow.operators.bash import BashOperator  # Operador para executar comandos Bash
from airflow.operators.python import PythonOperator  # Operador para usar funções Python
from datetime import datetime, timedelta  # Gerencia datas e intervalos de tempo

# Configura os argumentos padrão para todas as tarefas do DAG
default_args = {
    "owner": "airflow",  # Indica o responsável pelo DAG
    "retries": 2,  # Número de vezes que a tarefa será reexecutada em caso de falha
    "retry_delay": timedelta(minutes=5),  # Tempo de espera entre tentativas
}

# Criação do DAG com o decorator
@dag(
    dag_id="dag_name",  # Nome único do DAG
    description="Exemplo de DAG com operadores Python e Bash",
    schedule_interval="@daily",  # Frequência de execução (uma vez por dia)
    start_date=datetime(2023, 11, 1),  # Data inicial para execução
    catchup=False,  # Não compensa execuções pendentes
    tags=["data-science", "treinamento"],  # Categoriza o DAG
    orientation="LR",  # Layout da visualização (esquerda para direita)
)
def exemplo_dag():

    # Tarefa Bash
    tarefa_bash = BashOperator(
        task_id="mostra_data",  # Nome da tarefa
        bash_command="date",  # Comando Bash que será executado
    )

    # Tarefa Python
    def funcao_python():
        print("Executando tarefa Python no Airflow!")

    tarefa_python = PythonOperator(
        task_id="tarefa_python",  # Nome da tarefa
        python_callable=funcao_python,  # Função Python que será executada
    )

    # Define a ordem das tarefas: tarefa_bash -> tarefa_python
    tarefa_bash >> tarefa_python

# Instancia o DAG
dag = exemplo_dag()
```



## **Como os DAGs se Integram com o Airflow**

1. O código do DAG é salvo na pasta de DAGs, que o Scheduler monitora.
2. O **Scheduler** lê o DAG e organiza as tarefas com base no intervalo configurado.
3. As tarefas são enviadas para a fila e executadas pelos **Workers**.
4. O **Web Server** mostra o status do DAG e das tarefas em tempo real.

