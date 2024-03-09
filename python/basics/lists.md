##Manipulação de Elementos:

append(elemento): Adiciona um elemento ao final da lista.
extend(outra_lista): Junta duas listas, adicionando os elementos da 'outra_lista' ao final da primeira.
insert(indice, elemento): Insere um elemento na lista em uma posição específica.
remove(elemento): Remove a primeira ocorrência do elemento da lista.
pop(indice): Remove o elemento da lista na posição específica e o retorna.
clear(): Limpa a lista, removendo todos os elementos.
Exemplo:

```Python
#  Cria uma lista
lista = ["banana", "maçã", "laranja"]

# Adiciona um elemento ao final da lista
lista.append("uva")
print(lista)  Saída: ['banana', 'maçã', 'laranja', 'uva']

# Junta duas listas
outra_lista = ["kiwi", "morango"]
lista.extend(outra_lista)
print(lista)  Saída: ['banana', 'maçã', 'laranja', 'uva', 'kiwi', 'morango']

# Insere um elemento na lista em uma posição específica
lista.insert(2, "abacaxi")
print(lista)  Saída: ['banana', 'maçã', 'abacaxi', 'laranja', 'uva', 'kiwi', 'morango']

# vRemove a primeira ocorrência do elemento da lista
lista.remove("laranja")
print(lista)  Saída: ['banana', 'maçã', 'abacaxi', 'uva', 'kiwi', 'morango']

# Remove o elemento da lista na posição específica e o retorna
elemento_removido = lista.pop(3)
print(elemento_removido)  Saída: 'uva'
print(lista)  Saída: ['banana', 'maçã', 'abacaxi', 'kiwi', 'morango']

# Limpa a lista
lista.clear()
print(lista)  Saída: []
```

Acesso a Elementos:

len(lista): Retorna o tamanho da lista (número de elementos).
index(elemento): Retorna o índice da primeira ocorrência do elemento na lista.
count(elemento): Retorna o número de ocorrências do elemento na lista.
get(indice, valor_padrao): Retorna o elemento na posição 'indice' ou o valor 'valor_padrao' se o índice estiver fora da lista.
Exemplo:

Python```
# Cria uma lista
lista = [10, 20, 30, 40, 50]

# Retorna o tamanho da lista
tamanho_lista = len(lista)
print(tamanho_lista)  Saída: 5

# Retorna o índice da primeira ocorrência do elemento na lista
indice_elemento = lista.index(30)
print(indice_elemento)  Saída: 2

# Retorna o número de ocorrências do elemento na lista
numero_ocorrencias = lista.count(20)
print(numero_ocorrencias)  Saída: 1

# Retorna o elemento na posição 'indice' ou o valor 'valor_padrao' se o índice estiver fora da lista
elemento_ou_padrao = lista.get(7, "Índice fora da lista")
print(elemento_ou_padrao)  Saída: "Índice fora da lista"
```

Operações com Listas:

+ (lista1, lista2): Concatena duas listas, retornando uma nova lista com os elementos de ambas.
*** n**: Repete a lista 'n' vezes, retornando uma nova lista com 'n' cópias da lista original.
in (elemento, lista): Verifica se o elemento está presente na lista, retornando True se estiver e False se não estiver.
Exemplo:

```Python
# Cria duas listas
lista1 = [1, 2, 3]
lista2 = [4, 5, 6]

# Concatena duas listas
lista_concatenada = lista1 + lista2
print(lista_concatenada)  Saída: [1, 2, 3, 4, 5, 6]

# Repete a lista 'n' vezes
lista_repetida = lista1 * 3
print(lista_repetida)  
```
