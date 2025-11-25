# Tools_for_storing_and_processing_big_data

## PRACTICAL WORKS 

## **[Лабораторная работа 2.1. Изучение методов хранения данных на основе NoSQL](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/2.1_%D0%9B%D0%B0%D0%B1%D0%BE%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BD%D0%B0%D1%8F_%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0.pdf)**


## **[Лабораторная работа 3.1. Проектирование архитектуры хранилища больших данных](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/3.1_%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D1%8B_%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D0%B0_%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D1%85_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.pdf)**


## **Лабораторная работа 4.1. Сравнение подходов хранения больших данных**


## **Лабораторная работа 5.1. Развертывание и настройка кластера Hadoop**


## **Лабораторная работа 6.1. Обработка данных с использованием Apache Spark**


def get_urgent_projects_pg():
    """Получить проекты, где есть хотя бы одна срочная задача (PostgreSQL)."""
    
    try:
        conn = psycopg2.connect(**pg_conn_params)
        with conn.cursor() as cur:
            query = """
                SELECT DISTINCT p.project_id, p.name, p.description, p.created_at
                FROM projects p
                JOIN tasks t ON p.project_id = t.project_id
                WHERE t.status = 'срочно'
            """
            cur.execute(query)
            rows = cur.fetchall()

            # Преобразуем в словари — как в MongoDB
            projects = []
            for r in rows:
                projects.append({
                    "project_id": r[0],
                    "name": r[1],
                    "description": r[2],
                    "created_at": r[3]
                })

            return projects

    except Exception as e:
        print(f"❌ Ошибка запроса PostgreSQL: {e}")
        return []
    finally:
        conn.close()


# Выполнение и вывод
urgent_projects_pg, time_pg = measure_time(get_urgent_projects_pg)

if urgent_projects_pg:
    print("📌 Срочные проекты (PostgreSQL):")
    print(f"⏱ Выполнено за {time_pg:.5f} секунд")
    print(f"📊 Найдено {len(urgent_projects_pg)} проектов:")
    
    for proj in urgent_projects_pg[:5]:
        print(f"- Project ID: {proj['project_id']}, Name: {proj['name']}")
else:
    print("❌ Проекты не найдены")

