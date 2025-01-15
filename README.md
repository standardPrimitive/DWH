# README

## Описание

Данный проект предназначен для инкрементальной загрузки данных из нескольких источников (разные схемы в PostgreSQL) в корпоративное хранилище данных (DWH) при помощи Apache Spark. Итоговая витрина — `dwh.craftsman_report_datamart`, в которой формируется аналитическая информация о продажах, мастерах и заказах в маркетплейсе товаров ручной работы.

## Состав проекта

1. **Docker** для запуска и настройки окружения с PostgreSQL и Spark.  
2. **Jupyter/Spark Notebook** - ноутбук с кодом, который считывает данные из PostgreSQL, формирует измерения, факты и витрину, и обновляет дату загрузки инкремента.  
3. **DDL-скрипты** (исходные данные) по созданию таблиц DWH и витрины данных в PostgreSQL.  
4. **CSV файлы** (исходные данные) для заполнения таблиц-источников.


## Шаги по запуску

1. **Запустить контейнер с PostgreSQL**  
  `docker run -d --name postgres_container -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=postgres -p 5432:5432 postgres`
2. **Запустить контейнер с Spark**  
  `docker run -d -p 8888:8888 -p 4040:4040 --name spark-notebook --user root -v D:\tmp\postgresql-42.6.0.jar:/opt/spark/jars/postgresql-42.6.0.jar jupyter/all-spark-notebook`
  (файл postgresql-42.6.0.jar скачивается с помощью `Invoke-WebRequest -Uri https://jdbc.postgresql.org/download/postgresql-42.6.0.jar -OutFile D:\tmp\spark_jars\postgresql-42.6.0.jar` (windows)
3. **Настроить связь подключения**
  docker network create spark-postgres-network
  docker network connect spark-postgres-network postgres_container       
  docker network connect spark-postgres-network spark-notebook
4. **Запустить скрипт**

