import random

# Задаем целевую сумму
# x1 + x2 + x3 + x4 + x5 = S Решаем это диофантово уравнение с пятью неизвестными где S — это некоторое целое число. Мы будем искать целые решения для данного уравнения в заданном диапазоне.

S = 20  # Измените это значение для поиска других решений

# Определяем параметры генетического алгоритма
POPULATION_SIZE = 100  # Размер популяции
NUM_GENERATIONS = 500  # Количество поколений
MUTATION_RATE = 0.1  # Вероятность мутации
NUM_UNKNOWN = 5  # Количество неизвестных


# Функция для оценки "пригодности" решения
def fitness(solution):
    return abs(S - sum(solution))


# Создание начальной популяции
def create_population(size):
    return [[random.randint(0, S) for _ in range(NUM_UNKNOWN)] for _ in range(size)]


# Операция мутации
def mutate(solution):
    if random.random() < MUTATION_RATE:
        idx = random.randint(0, NUM_UNKNOWN - 1)
        solution[idx] = random.randint(0, S)


# Операция кроссовера
def crossover(parent1, parent2):
    crossover_point = random.randint(1, NUM_UNKNOWN - 1)
    child = parent1[:crossover_point] + parent2[crossover_point:]
    return child


# Генетический алгоритм
def genetic_algorithm():
    population = create_population(POPULATION_SIZE)
    best_solution = None
    best_fitness = float('inf')

    for generation in range(NUM_GENERATIONS):
        population = sorted(population, key=fitness)

        if fitness(population[0]) < best_fitness:
            best_fitness = fitness(population[0])
            best_solution = population[0]

        new_population = population[:2]  # Сохраняем лучших 2
        while len(new_population) < POPULATION_SIZE:
            parent1 = random.choice(population[:POPULATION_SIZE // 2])
            parent2 = random.choice(population[:POPULATION_SIZE // 2])
            child = crossover(parent1, parent2)
            mutate(child)
            new_population.append(child)

        population = new_population

    return best_solution


# Запуск алгоритма
solution = genetic_algorithm()

if solution:
    print("Найденное решение:", solution)
    print("Соответствующая сумма:", sum(solution))
else:
    print("Решение не найдено.")
