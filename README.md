# Kalkulator_Macierzy

Projekt powstał jedynie w celu utrwalenia wiedzy matematycznej. W związku z tym nie są tu wykorzystywane bilbioteki jak NumPy.

Do zrobienia:
 - [x] Dodawanie macierzy
 - [x] Odejmowanie macierzy
 - [x] Mnożenie macierzy
 - [x] Reguła Sarrusa
 - [x] Transponowanie macierzy
 - [x] Rozwinięcie Laplace'a
 - [ ] Sprawdzanie czy wektory są liniowo niezależne (rząd macierzy)
 - [ ] Obliczanie macierzy odwrotnej
 - [ ] Obliczanie macierzy dopełnień
 - [ ] Rozwiązywanie równań macierzowych
 - [ ] Przepisać na obiektowy, ogarnąć obsługę błędów, poprawić błędy, zoptymalizować kod, zintegrować z flaskiem

```py
import re
from operator import add, sub


def input_to_list(x):
    return list(map(int, re.sub('\s+', ',', x).split(",")))


def matrix_input(mode):
    if mode == "default":
        first_matrix, first_matrix_size = [], input_to_list(input(f'\nPodaj wymiary macierzy (y x): '))

        for i in range(first_matrix_size[0]):
            first_matrix.append(input_to_list(input(f'\nPodaj {i + 1} wiersz: ')))

        if any(len(x) != first_matrix_size[1] for x in first_matrix):
            print(f'\n!Wprowadzone wymiary macierzy nie zgadzają się z oczekiwanymi wymiarami! Spróbuj jeszcze raz.\n')
            matrix_input("default")

        return first_matrix, first_matrix_size

    elif mode == "basic_operations":
        first_matrix, first_matrix_size = matrix_input("default")

        second_matrix, second_matrix_size = [], input_to_list(input(f'\nPodaj wymiary drugiej macierzy (y x): '))

        if first_matrix_size != second_matrix_size:
            print(f'\n!Nie można dodawać lub odejmować macierzy o różnych rozmiarach! Spróbuj jeszcze raz.\n')
            matrix_input("basic_operations")

        for i in range(second_matrix_size[0]):
            second_matrix.append(input_to_list(input(f'\nPodaj {i + 1} wiersz: ')))

        if any(len(x) != second_matrix_size[1] for x in second_matrix):
            print(f'\n!Wprowadzone wymiary macierzy nie zgadzają się z oczekiwanymi wymiarami! Spróbuj jeszcze raz.\n')
            matrix_input("basic_operations")

        return first_matrix, second_matrix

    elif mode == "sarrus":
        matrix = matrix_input("default")[0]

        if any(len(x) != 3 for x in matrix):
            print(
                "\n!Wyznaczniki metodą Sarrusa można wyznaczać jedynie z macierzy wymiaru 3x3! Spróbuj jeszcze raz.\n")
            matrix_input("sarrus")

        return matrix

    elif mode == "laplace":
        matrix = matrix_input("default")[0]

        if any(len(x) != 4 for x in matrix):
            print(
                "\n!Wyznaczniki metodą Laplace'a można wyznaczać jedynie z macierzy wymiaru 4x4! Spróbuj jeszcze raz.\n")
            matrix_input("laplace")

        return matrix


def dodawanie():
    first_matrix, second_matrix = matrix_input("basic_operations")
    for i, j in zip(first_matrix, second_matrix):
        print(list(map(add, i, j)))


def odejmowanie():
    first_matrix, second_matrix = matrix_input("basic_operations")
    for i, j in zip(first_matrix, second_matrix):
        print(list(map(sub, i, j)))


def mnozenie():
    first_matrix, second_matrix = matrix_input("basic_operations")
    return print(
        [[sum(a * b for a, b in zip(X_row, Y_col)) for Y_col in zip(*second_matrix)] for X_row in first_matrix])


def sarrus(x, type=0):
    if type == 0:
        x = matrix_input("sarrus")

    for i in range(2):
        x.append(x[i])

    return print(
        x[0][0] * x[1][1] * x[2][2] +
        x[1][0] * x[2][1] * x[3][2] +
        x[2][0] * x[3][1] * x[4][2] +
        (x[0][2] * x[1][1] * x[2][0]) * -1 +
        (x[1][2] * x[2][1] * x[3][0]) * -1 +
        (x[2][2] * x[3][1] * x[4][0]) * -1
    )


def transponowanie():
    matrix, rozmiar = matrix_input()

    new_matrix = []

    for i in range(rozmiar[1]):
        row = []
        for j in range(rozmiar[0]):
            row.append(matrix[j][i])
        new_matrix.append(row)

    return new_matrix


def laplace():
    matrix = matrix_input("laplace")
    mnozniki = []

    for i in range(1, 4):
        if matrix[0][i] != 0:
            mnoznik = (matrix[0][i] / matrix[0][0]) * -1
        else:
            mnoznik = 0
        mnozniki.append(mnoznik)

    chosen_number = matrix[0][0]

    matrix[0] = [matrix[0][0], 0, 0, 0]

    matrix_2 = []

    for i in range(1, 4):
        row = []
        for j in range(1, 4):
            row.append(matrix[i][0] * mnozniki[j - 1] + matrix[i][j])
        matrix_2.append(row)

    return print(sarrus(matrix_2, 1) * chosen_number)


def menu():
    print("""
            MENU - kalkulator macierzy

                1. Dodawanie macierzy
                2. Odejmowanie macierzy
                3. Mnożenie macierzy
                4. Wyznacznik metodą Sarrusa dla macierzy wymiarów 3x3
                5. Transponowanie macierzy
                6. Wyznacznik metodą Leplace'a dla macierzy wymiarów 4x4
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
            sarrus([], 0)
        case 5:
            transponowanie()
        case 6:
            laplace()
        case _:
            print("Nie ma takiej opcji wyboru! Spróbuj jeszcze raz.")
            menu()


menu()

```
