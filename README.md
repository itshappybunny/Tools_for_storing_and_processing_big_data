# Tools_for_storing_and_processing_big_data

## PRACTICAL WORKS 

## **[Лабораторная работа 2.1. Изучение методов хранения данных на основе NoSQL](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/2.1_%D0%9B%D0%B0%D0%B1%D0%BE%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BD%D0%B0%D1%8F_%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0.pdf)**


## **[Лабораторная работа 3.1. Проектирование архитектуры хранилища больших данных](https://github.com/itshappybunny/Tools_for_storing_and_processing_big_data/blob/main/3.1_%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D1%8B_%D1%85%D1%80%D0%B0%D0%BD%D0%B8%D0%BB%D0%B8%D1%89%D0%B0_%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D1%85_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.pdf)**


## **Лабораторная работа 4.1. Сравнение подходов хранения больших данных**


## **Лабораторная работа 5.1. Развертывание и настройка кластера Hadoop**


## **Лабораторная работа 6.1. Обработка данных с использованием Apache Spark**




import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

sns.set_theme(style="whitegrid")
width = 0.35  # ширина столбцов

fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(16, 12))

# 1. Сложность реализации
categories = ['Строки кода / Этапы', 'Читаемость', 'Поддерживаемость']
postgres_scores = [postgres_query_lines, 8, 8]
mongo_scores = [mongodb_pipeline_steps, 6, 6]

x = np.arange(len(categories))
bars1 = ax1.bar(x - width/2, postgres_scores, width, label='PostgreSQL', color='blue', alpha=0.7)
bars2 = ax1.bar(x + width/2, mongo_scores, width, label='MongoDB', color='orange', alpha=0.7)
ax1.set_xticks(x)
ax1.set_xticklabels(categories, rotation=15, ha='right')
ax1.set_ylabel('Оценка / строки')
ax1.set_title('Сравнение сложности реализации')
ax1.legend()
ax1.grid(True, alpha=0.3)

# Добавляем подписи на оба столбца
for bar in bars1:
    height = bar.get_height()
    ax1.text(bar.get_x() + bar.get_width()/2., height + 0.2,
             f'{int(height)}', ha='center', va='bottom', color='blue', fontsize=10)
for bar in bars2:
    height = bar.get_height()
    ax1.text(bar.get_x() + bar.get_width()/2., height + 0.2,
             f'{int(height)}', ha='center', va='bottom', color='orange', fontsize=10)

# 2. Производительность операций
operations = ['Поиск проектов', 'Поиск задач', 'Фильтр по статусу', 'JOIN / Aggregation']
pg_perf = [9, 9, 9, 8]
mongo_perf = [7, 7, 8, 7]

x_ops = np.arange(len(operations))
bars3 = ax2.bar(x_ops - width/2, pg_perf, width, label='PostgreSQL', color='blue', alpha=0.7)
bars4 = ax2.bar(x_ops + width/2, mongo_perf, width, label='MongoDB', color='orange', alpha=0.7)
ax2.set_xticks(x_ops)
ax2.set_xticklabels(operations, rotation=15, ha='right')
ax2.set_ylabel('Оценка (1-10)')
ax2.set_title('Производительность по операциям')
ax2.set_ylim(0, 10)
ax2.legend()
ax2.grid(True, alpha=0.3)

# Подписи на столбцах
for bar in bars3:
    height = bar.get_height()
    ax2.text(bar.get_x() + bar.get_width()/2., height + 0.2, f'{height}', ha='center', va='bottom', color='blue', fontsize=10)
for bar in bars4:
    height = bar.get_height()
    ax2.text(bar.get_x() + bar.get_width()/2., height + 0.2, f'{height}', ha='center', va='bottom', color='orange', fontsize=10)

# 3. Гибкость системы
aspects = ['Схема данных', 'Масштабирование', 'Типы данных', 'Индексирование']
pg_flex = [6, 7, 8, 8]
mongo_flex = [9, 9, 7, 8]

x_flex = np.arange(len(aspects))
bars5 = ax3.bar(x_flex - width/2, pg_flex, width, label='PostgreSQL', color='blue', alpha=0.7)
bars6 = ax3.bar(x_flex + width/2, mongo_flex, width, label='MongoDB', color='orange', alpha=0.7)
ax3.set_xticks(x_flex)
ax3.set_xticklabels(aspects, rotation=15, ha='right')
ax3.set_ylabel('Оценка (1-10)')
ax3.set_title('Гибкость системы')
ax3.legend()
ax3.grid(True, alpha=0.3)

for bar in bars5:
    height = bar.get_height()
    ax3.text(bar.get_x() + bar.get_width()/2., height + 0.2, f'{height}', ha='center', va='bottom', color='blue', fontsize=10)
for bar in bars6:
    height = bar.get_height()
    ax3.text(bar.get_x() + bar.get_width()/2., height + 0.2, f'{height}', ha='center', va='bottom', color='orange', fontsize=10)

# 4. Общая оценка
overall = ['Производительность', 'Гибкость', 'Простота', 'Масштабируемость']
pg_overall = [8.5, 6.5, 8, 7.5]
mongo_overall = [7, 8.5, 6, 8.5]

x_overall = np.arange(len(overall))
bars7 = ax4.bar(x_overall - width/2, pg_overall, width, label='PostgreSQL', color='blue', alpha=0.7)
bars8 = ax4.bar(x_overall + width/2, mongo_overall, width, label='MongoDB', color='orange', alpha=0.7)
ax4.set_xticks(x_overall)
ax4.set_xticklabels(overall, rotation=15, ha='right')
ax4.set_ylabel('Оценка (1-10)')
ax4.set_title('Общая оценка систем')
ax4.legend()
ax4.grid(True, alpha=0.3)

for bar in bars7:
    height = bar.get_height()
    ax4.text(bar.get_x() + bar.get_width()/2., height + 0.1, f'{height:.1f}', ha='center', va='bottom', color='blue', fontsize=10)
for bar in bars8:
    height = bar.get_height()
    ax4.text(bar.get_x() + bar.get_width()/2., height + 0.1, f'{height:.1f}', ha='center', va='bottom', color='orange', fontsize=10)

plt.tight_layout()
plt.show()

