# Kalkulator_Macierzy
Do zrobienia:
 - [ ] Rozwinięcie Laplace'a
 - [ ] Transponowanie macierzy
 - [ ] Sprawdzanie czy wektory są liniowo niezależne (rząd macierzy)
 - [ ] Obliczanie macierzy odwrotnej
 - [ ] Obliczanie macierzy dopełnień
 - [ ] Rozwiązywanie równań macierzowych
 - [ ] Przepisać na obiektowy, ogarnąć obsługę błędów, poprawić błędy, zoptymalizować kod

```py
import re
from operator import add, sub

def input_to_list(x):
    return list(map(int, re.sub('\s+', ',', x).split(",")))

def matrix_input(choise):

    if choise == 1:
        first_matrix = []
        second_matrix = []

        first_matrix_size = input_to_list(input(f'\nPodaj wymiary pierwszej macierzy (x y): '))

        for i in range(first_matrix_size[0]):
            first_matrix.append(input_to_list(input(f'\nPodaj {i+1} wiersz: ')))

        if any(len(x) != first_matrix_size[1] for x in first_matrix):
            print("\n!Wprowadzone wymiary macierzy nie zgadzają się z oczekiwanymi wymiarami! Spróbuj jeszcze raz.\n")
            matrix_input()

        second_matrix_size = input_to_list(input(f'\nPodaj wymiary drugiej macierzy (x y): '))

        if second_matrix_size != second_matrix_size:
            print("\n!Nie można dodawać macierzy o różnych rozmiarach! Spróbuj jeszcze raz.\n")
            matrix_input()

        for i in range(second_matrix_size[0]):
            second_matrix.append(input_to_list(input(f'\nPodaj {i+1} wiersz: ')))

        if any(len(x) != second_matrix_size[1] for x in second_matrix):
            print("\n!Wprowadzone wymiary macierzy nie zgadzają się z oczekiwanymi wymiarami! Spróbuj jeszcze raz.\n")
            matrix_input()

        return first_matrix, second_matrix
        
    elif choise == 0:
        matrix = []

        for i in range(3):
            matrix.append(input_to_list(input(f'\nPodaj {i+1} wiersz: ')))

        if any(len(x) != 3 for x in matrix):
            for x in matrix:
                print(x,len(x),any(len(x) != 3 for x in matrix))
            print("\n!Wyznaczniki metodą Sarrusa można wyznarzać jedynie z macierzy wymiaru 3x3! Spróbuj jeszcze raz.\n")
            matrix_input(0)

        return matrix
    

def dodawanie():
    first_matrix, second_matrix = matrix_input(1)
    for i,j in zip(first_matrix, second_matrix):
        print(list(map(add, i, j)))

def odejmowanie():
    first_matrix, second_matrix = matrix_input(1)
    for i,j in zip(first_matrix, second_matrix):
        print(list(map(sub, i, j)))

def mnozenie():
    matrix = matrix_input(1)
    print([[sum(a*b for a,b in zip(X_row,Y_col)) for Y_col in zip(*second_matrix)] for X_row in first_matrix])

def sarrus():
    x = matrix_input(0)

    for i in range(2):
        x.append(x[i])

    print(
        x[0][0] * x[1][1] * x[2][2] + 
        x[1][0] * x[2][1] * x[3][2] + 
        x[2][0] * x[3][1] * x[4][2] + 
        (x[0][2] * x[1][1] * x[2][0])*-1 + 
        (x[1][2] * x[2][1] * x[3][0])*-1 + 
        (x[2][2] * x[3][1] * x[4][0])*-1
        )


def menu():
    print("""
            MENU - kalkulator macierzy

                1. Dodawanie macierzy
                2. Odejmowanie macierzy
                3. Mnożenie macierzy
                4. Wyznacznik metodą Sarrusa
    """)

    choise = int(input("Wybieram: "))

    match choise:
        case 1:
            dodawanie()
        case 2:
            odejmowanie()
        case 3:
            mnozenie()
        case 4:
            sarrus()
        case _:
            print("Nie ma takiej opcji wyboru! Spróbuj jeszcze raz.")
            menu()

menu()
```
