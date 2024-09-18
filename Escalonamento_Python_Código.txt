import numpy as np

# Função para exibir a matriz original
def exibirMatrizOriginal(matrizA):
    for i in matrizA:
        print(i)

# Função para exibir a matriz escalonada
def exibirMatrizEscalonada(matrizA):
    for i in matrizA:
        print(i)

# Função para ler os valores da matriz
def lerMatriz(matriz):
    for c in range(len(matriz)):
        matriz[c] = int(input(f'Digite um valor para [{c}]: '))

# Função para criar uma matriz com o tamanho especificado
def criarMatriz(tamanho):
    return np.zeros((tamanho, tamanho), dtype=int)

# Lendo o tamanho da matriz
tamanho = int(input("Digite o tamanho da matriz (entre 3 e 10): "))
if tamanho < 3 or tamanho > 10:
    print("Tamanho inválido! O tamanho da matriz deve ser entre 3 e 10.")
    exit()

# Definindo a matriz A
matrizA = criarMatriz(tamanho)

# Lendo os valores da matriz A
print('Digite os valores da matriz A:')
for l in range(matrizA.shape[0]):
    for c in range(matrizA.shape[1]):
        matrizA[l][c] = int(input(f'Digite um valor para [{l}, {c}]: '))

print('Matriz Original:')
exibirMatrizOriginal(matrizA)

# Definindo a matriz B como um vetor unidimensional
matrizB = np.zeros(tamanho, dtype=int)

# Lendo os valores da matriz B
print('Digite os valores da matriz B:')
lerMatriz(matrizB)

# Função de substituição e somatória
def subs_somatoria(matrizA, matrizB):
    n = len(matrizA)
    x = n * [0]
    for i in range(n - 1, -1, -1):
        S = 0
        for j in range(n - 1, -1, -1):
            S = S + matrizA[i][j] * x[j]
        x[i] = (matrizB[i] - S) / matrizA[i][i]
    return x

# Função de eliminação de Gauss
def Gauss(matrizA, matrizB):
    n = len(matrizA)
    for k in range(0, n - 1):
        if matrizA[k][k] == 0:
            continue
        for i in range(k + 1, n):
            F = -matrizA[i][k] / matrizA[k][k]
            for j in range(k + 1, n):
                matrizA[i][j] = F * matrizA[k][j] + matrizA[i][j]
            matrizB[i] = F * matrizB[k] + matrizB[i]
            matrizA[i][k] = 0
    det = 1
    for i in range(n):
        det = det * matrizA[i][i]
    if det != 0:
        x = subs_somatoria(matrizA, matrizB)
        return (x, det)
    else:
        print('Matriz dos coeficientes é singular, ou seja, possui determinante igual a 0')
        return ([], det)

# Resolvendo o sistema
(x, det) = Gauss(matrizA, matrizB)

# Exibindo a matriz escalonada e a solução
print('Matriz A escalonada:')
exibirMatrizEscalonada(matrizA)
print('\n')
print('Vetor B equivalente:')
print(matrizB)  # Como é um vetor, não precisa da função exibirMatrizEscalonada
print('\n')
print('A matriz possui determinante igual a {} e o valor da solução {}'.format(det, x))
