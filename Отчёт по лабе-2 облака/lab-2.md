# Отчёт по лабораторной работе 2 (Azure)
## Цель работы:
Получение навыков аналитики и понимания спектра публичных облачных сервисов. Формирование у студентов комплексного видения Облака. 

## Дано:
- Слепок данных биллинга от провайдера после небольшой обработки в виде SQL-параметров. Символ % в начале/конце означает, что перед/после него может стоять любой набор символов
- Образец итогового соответствия, что желательно получить в конце. В этом же документе  

## Необходимо:
1. Импортировать файл .csv в Excel или любую другую программу работы с таблицами. Для Excel делается на вкладке Данные – Из текстового / csv файла – выбрать файл, разделитель – точка с запятой.
2. Распределить потребление сервисов по иерархии, чтобы можно было провести анализ от большего к меньшему (напр. От всех вычислительных ресурсов Compute дойти до конкретного типа использования - Выделенной стойка в датацентре Dedicated host usage).
3. Сохранить файл и залить в соответствующую папку на Google Drive.

## Выполнение лабораторной работы
Был дан следующий файл, содержащий часть данных биллинга провайдера:
![image](https://github.com/user-attachments/assets/014ea3ab-d3f1-415b-bbf0-9981cf3dcc48)

Необходимо, отталкиваясь от уже известных данных, заполнить первые 5 столбцов таблицы, используя документацию Azure.

В ходе работы были встречены и изучены следующие сервисы:
- **Azure Data Management** -- набор сервисов и инструментов для сбора, хранения, обработки и анализа данных
- **Azure Machine Learning Studio** -- сервис для работы с искусственным интеллектом: создания, обучения и развёртывания моделей машинного обучения
- **Azure Bastion** -- сервис для безопасного подключения к виртуальным машинам по протоколам RDP и SSH
- **Azure API Management** -- сервис для создания, контроля и публикации API.
- **Azure Monitor** -- сервис для мониторинга, анализа и контроля приложений, виртуальных машин, контейнеров и прочих ресурсов
- **Azure Backup Service** -- сервис, обеспечивающий резервное копирование и безопасное хранение данных
- **Microsoft HDInsight** -- сервис для работы с большими данными, позволяющие использовать проекты и кластеры с открытым кодом
- **Azure Data Factory** -- сервис для создания конвейеров данных, т.е. перемещения и обработки данных из разных источников
- **Azure Data Lake** -- хранилище для работы с большими данными
- **Azure Virtual Machines** -- сервис для создания и запуска виртуальных машин


В результате работы была получена следующая [таблица](https://docs.google.com/spreadsheets/d/1sgDEFPPv7C4fOYxGYoo8N-tVAIZFJFgvFaRR0Bv8arc/edit?gid=0#gid=0):
![image](https://github.com/user-attachments/assets/5dca8d66-5391-48ce-8297-155206a35e9a)




## Вывод
В ходе выполнения лабораторной работы были получены знания о ряде сервисов Azure. Процесс оказался труднее и времязатратнее, чем работа над первой лабораторной, так как документация Azure оказалась более запутанной и было необходимо время, чтобы освоиться. Однако в итоге работа была выполнена,  результате чего получилось расширить свои знания об облачных сервисах.
