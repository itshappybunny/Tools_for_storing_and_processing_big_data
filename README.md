# Tools_for_storing_and_processing_big_data

## PRACTICAL WORKS 

## **[Лабораторная работа 2.1. Изучение методов хранения данных на основе NoSQL](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/2.1_%D0%9B%D0%B0%D0%B1%D0%BE%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BD%D0%B0%D1%8F_%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0.pdf)**


## **[Лабораторная работа 3.1. Проектирование архитектуры хранилища больших данных](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/3.1_%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D1%8B_%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D0%B0_%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D1%85_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.pdf)**


## **Лабораторная работа 4.1. Сравнение подходов хранения больших данных**


## **Лабораторная работа 5.1. Развертывание и настройка кластера Hadoop**


## **Лабораторная работа 6.1. Обработка данных с использованием Apache Spark**


import seaborn as sns

print("📊 Сравнение производительности запросов на срочные проекты")
print("=" * 60)

# Проверяем подключение к MongoDB
if not mongo_client:
    print("❌ MongoDB недоступен, пропускаем сравнение")
else:
    # Тестирование на нескольких проектах/итерациях (можно просто повторить запрос несколько раз)
    test_iterations = [1, 2, 3, 4, 5]  # можно увеличить, если нужно больше данных
    postgres_times = []
    mongodb_times = []

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
        
        # Сравнение
        if pg_time < mongo_time:
            faster = "PostgreSQL"
            speedup = mongo_time / pg_time
        else:
            faster = "MongoDB"
            speedup = pg_time / mongo_time
        
        print(f"  🏆 Быстрее: {faster} (в {speedup:.2f} раз)")

    # Визуализация через seaborn
    sns.set_theme(style="whitegrid")
    
    # Время выполнения
    plt.figure(figsize=(14, 8))
    sns.lineplot(x=test_iterations, y=postgres_times, marker='o', label='PostgreSQL', linewidth=2)
    sns.lineplot(x=test_iterations, y=mongodb_times, marker='s', label='MongoDB', linewidth=2)
    plt.xlabel("Итерация")
    plt.ylabel("Время выполнения (сек)")
    plt.title("Время выполнения запросов на срочные проекты")
    plt.legend()
    plt.show()
    
    # Соотношение производительности
    speedup_ratio = [mongo_time / pg_time for pg_time, mongo_time in zip(postgres_times, mongodb_times)]
    plt.figure(figsize=(8,5))
    sns.barplot(x=[str(i) for i in test_iterations], y=speedup_ratio, palette=["green" if x>1 else "red" for x in speedup_ratio])
    plt.axhline(1, color='black', linestyle='--', alpha=0.5)
    plt.xlabel("Итерация")
    plt.ylabel("Соотношение времени (MongoDB/PostgreSQL)")
    plt.title("Сравнение производительности")
    plt.show()
    
    # Статистика
    avg_pg_time = np.mean(postgres_times)
    avg_mongo_time = np.mean(mongodb_times)
    std_pg_time = np.std(postgres_times)
    std_mongo_time = np.std(mongodb_times)
    
    print("\n📋 ДЕТАЛЬНАЯ СТАТИСТИКА:")
    print(f"PostgreSQL - Среднее: {avg_pg_time:.4f}с, Стд. отклонение: {std_pg_time:.4f}с")
    print(f"MongoDB - Среднее: {avg_mongo_time:.4f}с, Стд. отклонение: {std_mongo_time:.4f}с")
    print(f"Общее ускорение PostgreSQL: {avg_mongo_time/avg_pg_time:.2f}x")
