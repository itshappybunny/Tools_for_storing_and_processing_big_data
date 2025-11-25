# Tools_for_storing_and_processing_big_data

## PRACTICAL WORKS 

## **[Лабораторная работа 2.1. Изучение методов хранения данных на основе NoSQL](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/2.1_%D0%9B%D0%B0%D0%B1%D0%BE%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BD%D0%B0%D1%8F_%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0.pdf)**


## **[Лабораторная работа 3.1. Проектирование архитектуры хранилища больших данных](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/3.1_%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D1%8B_%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D0%B0_%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D1%85_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.pdf)**


## **Лабораторная работа 4.1. Сравнение подходов хранения больших данных**


## **Лабораторная работа 5.1. Развертывание и настройка кластера Hadoop**


## **Лабораторная работа 6.1. Обработка данных с использованием Apache Spark**


# ---------------------------------------------------------
# ГЕНЕРАЦИЯ ДАННЫХ ДЛЯ ПРОЕКТОВ И ЗАДАЧ (PostgreSQL + MongoDB)
# ---------------------------------------------------------

faker = Faker()
np.random.seed(42)

# Объёмы данных
n_projects = 10000      # обычные данные
n_tasks = 100000        # большие данные

print("Генерация данных:")
print(f"- Проектов: {n_projects:,}")
print(f"- Задач: {n_tasks:,}")

# ---------------------------------------------------------
# 1. Генерация проектов
# ---------------------------------------------------------

projects_data = []
for i in range(n_projects):
    projects_data.append({
        "project_id": i,
        "name": f"Project_{i:05d}",
        "description": faker.sentence(),
        "created_at": faker.date_time_between(start_date="-2y", end_date="now")
    })

projects_df = pd.DataFrame(projects_data)

# ---------------------------------------------------------
# 2. Генерация задач
# ---------------------------------------------------------

statuses = ["новая", "в работе", "срочно", "завершена"]

tasks_data = []
for i in range(n_tasks):
    tasks_data.append({
        "task_id": i,
        "project_id": np.random.randint(0, n_projects),
        "title": f"Task_{i:06d}",
        "status": np.random.choice(statuses, p=[0.4, 0.3, 0.05, 0.25]),  # 5% задач — "срочно"
        "deadline": faker.date_time_between(start_date="now", end_date="+30d")
    })

tasks_df = pd.DataFrame(tasks_data)

print("\nСозданы DataFrame:")
print(f"- projects_df: {len(projects_df):,} записей")
print(f"- tasks_df: {len(tasks_df):,} записей")

display(projects_df.head())
display(tasks_df.head())

# ---------------------------------------------------------
# 3. ПРЕОБРАЗОВАНИЕ ДАННЫХ ДЛЯ MONGODB (вложенные документы)
# ---------------------------------------------------------

print("\nПреобразование данных для MongoDB...")

# Группировка задач по проектам
tasks_grouped = tasks_df.groupby("project_id")

projects_mongo = []

for _, project in projects_df.iterrows():
    pid = project["project_id"]

    project_doc = {
        "project_id": int(pid),
        "name": project["name"],
        "description": project["description"],
        "created_at": project["created_at"],
        "tasks": tasks_grouped.get_group(pid)
                 .drop(columns=["project_id"])
                 .to_dict("records") if pid in tasks_grouped.groups else []
    }

    projects_mongo.append(project_doc)

print(f"✔ Подготовлено документов для MongoDB: {len(projects_mongo):,}")
print("Пример документа:")
projects_mongo[0]
