# Tools_for_storing_and_processing_big_data

## PRACTICAL WORKS 

## **[Лабораторная работа 2.1. Изучение методов хранения данных на основе NoSQL](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/2.1_%D0%9B%D0%B0%D0%B1%D0%BE%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BD%D0%B0%D1%8F_%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0.pdf)**


## **[Лабораторная работа 3.1. Проектирование архитектуры хранилища больших данных](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/3.1_%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D1%8B_%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D0%B0_%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D1%85_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.pdf)**


## **Лабораторная работа 4.1. Сравнение подходов хранения больших данных**


## **Лабораторная работа 5.1. Развертывание и настройка кластера Hadoop**


## **Лабораторная работа 6.1. Обработка данных с использованием Apache Spark**


# 🔹 Сравнение производительности запросов на срочные проекты (10 итераций)
test_iterations = list(range(1, 11))  # 10 тестов
postgres_times = []
mongodb_times = []

print("📊 Сравнение производительности запросов на срочные проекты")
print("=" * 60)

for i in test_iterations:
    print(f"\n🧪 Тестирование итерации {i}:")
    
    # PostgreSQL
    _, pg_time = measure_time(get_urgent_projects_pg)
    postgres_times.append(pg_time)
    print(f"  PostgreSQL: {pg_time:.4f} сек")
    
    # MongoDB
    _, mongo_time = measure_time(get_mongodb_urgent_projects)
    mongodb_times.append(mongo_time)
    print(f"  MongoDB: {mongo_time:.4f} сек")
    
    # Быстрее
    if pg_time < mongo_time:
        faster = "PostgreSQL"
        speedup = mongo_time / pg_time
    else:
        faster = "MongoDB"
        speedup = pg_time / mongo_time
    print(f"  🏆 Быстрее: {faster} (в {speedup:.2f} раз)")

# 🔹 Визуализация через seaborn
sns.set_theme(style="whitegrid")

# График времени выполнения по итерациям
plt.figure(figsize=(14, 6))
sns.lineplot(x=test_iterations, y=postgres_times, marker='o', label='PostgreSQL', linewidth=2)
sns.lineplot(x=test_iterations, y=mongodb_times, marker='s', label='MongoDB', linewidth=2)
plt.xlabel("Итерация")
plt.ylabel("Время выполнения (сек)")
plt.title("Время выполнения запросов на срочные проекты")
plt.legend()
plt.show()

# График сравнительных столбцов по каждой итерации
plt.figure(figsize=(14, 6))
data = pd.DataFrame({
    'Итерация': test_iterations * 2,
    'Время (сек)': postgres_times + mongodb_times,
    'СУБД': ['PostgreSQL']*len(test_iterations) + ['MongoDB']*len(test_iterations)
})

sns.barplot(x='Итерация', y='Время (сек)', hue='СУБД', data=data)
plt.title("Сравнительное время выполнения запросов (PostgreSQL vs MongoDB)")
plt.show()

# 🔹 Статистика
avg_pg_time = np.mean(postgres_times)
avg_mongo_time = np.mean(mongodb_times)
std_pg_time = np.std(postgres_times)
std_mongo_time = np.std(mongodb_times)

print("\n📋 ДЕТАЛЬНАЯ СТАТИСТИКА:")
print(f"PostgreSQL - Среднее: {avg_pg_time:.4f}с, Стд. отклонение: {std_pg_time:.4f}с")
print(f"MongoDB - Среднее: {avg_mongo_time:.4f}с, Стд. отклонение: {std_mongo_time:.4f}с")
print(f"Общее ускорение PostgreSQL: {avg_mongo_time/avg_pg_time:.2f}x")
