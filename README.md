# laba6
code
Задание состоит из двух частей. 
1 часть – написать программу в соответствии со своим вариантом задания. Написать 2 варианта формирования (алгоритмический и с помощью функций Питона), сравнив по времени их выполнение.
2 часть – усложнить написанную программу, введя по своему усмотрению в условие минимум одно ограничение на характеристики объектов (которое будет сокращать количество переборов)  и целевую функцию для нахождения оптимального  решения.

Вариант 30. Имеется К книг. Сформируйте разные варианты их размещения по Т бандеролям.


import time

def place_books_recursive(K, T):
    # Рекурсивная функция для размещения книг по бандеролям
    def helper(books, bundles):
        if not books:
            yield bundles
        else:
            for i in range(len(bundles)):
                new_bundles = [list(bundle) for bundle in bundles]
                new_bundles[i].append(books[0])
                yield from helper(books[1:], new_bundles)

    return list(helper(list(range(1, K + 1)), [[] for _ in range(T)]))

# Пример использования
K = 3
T = 2
start_time = time.time()
result_recursive = place_books_recursive(K, T)
end_time = time.time()
print(f"Алгоритмический метод: {end_time - start_time:.6f} секунд")
print(result_recursive)
import itertools

def place_books_python(K, T):
    books = range(1, K + 1)
    all_combinations = itertools.product(range(T), repeat=K)
    results = []
    for combination in all_combinations:
        bundles = [[] for _ in range(T)]
        for book, bundle_index in zip(books, combination):
            bundles[bundle_index].append(book)
        results.append(bundles)
    return results

# Пример использования
start_time = time.time()
result_python = place_books_python(K, T)
end_time = time.time()
print(f"Функции Python: {end_time - start_time:.6f} секунд")
print(result_python)
def place_books_with_constraint(K, T, max_books_per_bundle):
    books = range(1, K + 1)
    all_combinations = itertools.product(range(T), repeat=K)
    valid_results = []
    
    for combination in all_combinations:
        bundles = [[] for _ in range(T)]
        for book, bundle_index in zip(books, combination):
            bundles[bundle_index].append(book)
        
        if all(len(bundle) <= max_books_per_bundle for bundle in bundles):
            valid_results.append(bundles)
    
    return valid_results

def evaluate_bundles(bundles):
    bundle_sizes = [len(bundle) for bundle in bundles]
    return max(bundle_sizes) - min(bundle_sizes)

def find_optimal_placement(K, T, max_books_per_bundle):
    valid_results = place_books_with_constraint(K, T, max_books_per_bundle)
    optimal_placement = min(valid_results, key=evaluate_bundles)
    return optimal_placement

# Пример использования
max_books_per_bundle = 2
start_time = time.time()
optimal_result = find_optimal_placement(K, T, max_books_per_bundle)
end_time = time.time()
print(f"Оптимальное размещение с ограничением: {end_time - start_time:.6f} секунд")
print(optimal_result)
