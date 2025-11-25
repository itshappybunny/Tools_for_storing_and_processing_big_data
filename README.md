# Tools_for_storing_and_processing_big_data

## PRACTICAL WORKS 

## **[Лабораторная работа 2.1. Изучение методов хранения данных на основе NoSQL](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/2.1_%D0%9B%D0%B0%D0%B1%D0%BE%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BD%D0%B0%D1%8F_%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0.pdf)**


## **[Лабораторная работа 3.1. Проектирование архитектуры хранилища больших данных](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/3.1_%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D1%8B_%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D0%B0_%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D1%85_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.pdf)**


## **Лабораторная работа 4.1. Сравнение подходов хранения больших данных**


## **Лабораторная работа 5.1. Развертывание и настройка кластера Hadoop**


## **Лабораторная работа 6.1. Обработка данных с использованием Apache Spark**




# 🔹 Анализ сложности реализации
print("🔍 АНАЛИЗ СЛОЖНОСТИ РЕАЛИЗАЦИИ")
print("=" * 50)

# Подсчёт строк кода PostgreSQL запроса для срочных проектов
postgres_query_lines = """
SELECT DISTINCT p.project_id, p.name, p.description, p.created_at
FROM projects p
JOIN tasks t ON p.project_id = t.project_id
WHERE t.status = 'срочно'
""".strip().count('\n') + 1

# MongoDB — количество этапов агрегации
mongodb_pipeline_steps = 4  # match, group, find, project

print(f"📊 Сложность реализации:")
print(f"• PostgreSQL SQL запрос: {postgres_query_lines} строк")
print(f"• MongoDB агрегационный пайплайн: {mongodb_pipeline_steps} этапа(ов)")

print(f"\n📖 Читаемость кода:")
print(f"• PostgreSQL: Высокая (стандартный SQL)")
print(f"• MongoDB: Средняя (требует знания агрегационных операций)")

print(f"\n🔧 Поддерживаемость:")
print(f"• PostgreSQL: Легко модифицировать")
print(f"• MongoDB: Изменение пайплайна сложнее")

print(f"\n⚡ Производительность:")
print(f"• PostgreSQL: JOIN операции, индексы")
print(f"• MongoDB: Множественные проходы по документам")

# 🔹 Визуализация через seaborn
sns.set_theme(style="whitegrid")
width = 0.35

fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(16, 12))

# 1. Сравнение сложности реализации
categories = ['Строки кода / Этапы', 'Читаемость', 'Поддерживаемость']
postgres_scores = [postgres_query_lines, 8, 8]  # оценка по 10-балльной шкале
mongo_scores = [mongodb_pipeline_steps, 6, 6]

x = np.arange(len(categories))
sns.barplot(ax=ax1, x=x - width/2, y=postgres_scores, color='blue', alpha=0.7, label='PostgreSQL')
sns.barplot(ax=ax1, x=x + width/2, y=mongo_scores, color='orange', alpha=0.7, label='MongoDB')
ax1.set_xticks(x)
ax1.set_xticklabels(categories)
ax1.set_ylabel('Оценка / строки')
ax1.set_title('Сравнение сложности реализации')
ax1.legend()

# 2. Производительность операций
operations = ['Поиск проектов', 'Поиск задач', 'Фильтр по статусу', 'JOIN / Aggregation']
pg_perf = [9, 9, 9, 8]
mongo_perf = [7, 7, 8, 7]

ax2.plot(operations, pg_perf, 'o-', label='PostgreSQL', linewidth=2, markersize=8, color='blue')
ax2.plot(operations, mongo_perf, 's-', label='MongoDB', linewidth=2, markersize=8, color='orange')
ax2.set_ylabel('Оценка (1-10)')
ax2.set_title('Производительность по операциям')
ax2.set_ylim(0, 10)
ax2.legend()
ax2.grid(True, alpha=0.3)

# 3. Гибкость системы
aspects = ['Схема данных', 'Масштабирование', 'Типы данных', 'Индексирование']
pg_flex = [6, 7, 8, 8]
mongo_flex = [9, 9, 7, 8]

x_flex = np.arange(len(aspects))
sns.barplot(ax=ax3, x=x_flex - width/2, y=pg_flex, color='blue', alpha=0.7, label='PostgreSQL')
sns.barplot(ax=ax3, x=x_flex + width/2, y=mongo_flex, color='orange', alpha=0.7, label='MongoDB')
ax3.set_xticks(x_flex)
ax3.set_xticklabels(aspects)
ax3.set_ylabel('Оценка (1-10)')
ax3.set_title('Гибкость системы')
ax3.legend()
ax3.grid(True, alpha=0.3)

# 4. Общая оценка
overall = ['Производительность', 'Гибкость', 'Простота', 'Масштабируемость']
pg_overall = [8.5, 6.5, 8, 7.5]
mongo_overall = [7, 8.5, 6, 8.5]

x_overall = np.arange(len(overall))
sns.barplot(ax=ax4, x=x_overall - width/2, y=pg_overall, color='blue', alpha=0.7, label='PostgreSQL')
sns.barplot(ax=ax4, x=x_overall + width/2, y=mongo_overall, color='orange', alpha=0.7, label='MongoDB')
ax4.set_xticks(x_overall)
ax4.set_xticklabels(overall)
ax4.set_ylabel('Оценка (1-10)')
ax4.set_title('Общая оценка систем')
ax4.legend()
ax4.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Итоговые выводы
print("\n🎯 ИТОГОВЫЕ ВЫВОДЫ:")
print("="*50)
print("🏆 PostgreSQL лучше для:")
print("  • Аналитических запросов с JOIN")
print("  • Сложных агрегаций")
print("  • Транзакционных операций")
print("  • Систем с фиксированной схемой")

print("\n🏆 MongoDB лучше для:")
print("  • Гибких схем данных")
print("  • Горизонтального масштабирования")
print("  • Документно-ориентированных данных")
print("  • Быстрой разработки прототипов")

print("\n💡 Рекомендации:")
print("  • Для вложенных структур и сложной аналитики: PostgreSQL")
print("  • Для гибких и быстро меняющихся схем: MongoDB")
