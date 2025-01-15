# README

## Описание

Данный проект предназначен для инкрементальной загрузки данных из нескольких источников (разные схемы в PostgreSQL) в корпоративное хранилище данных (DWH) при помощи Apache Spark. Итоговая витрина — `dwh.craftsman_report_datamart`, в которой формируется аналитическая информация о продажах, мастерах и заказах в маркетплейсе товаров ручной работы.

## Состав проекта

1. **Docker** для запуска и настройки окружения с PostgreSQL и Spark.  
2. **Jupyter/Spark Notebook** - ноутбук с кодом, который считывает данные из PostgreSQL, формирует измерения, факты и витрину, и обновляет дату загрузки инкремента.  
3. **DDL-скрипты** (исходные данные) по созданию таблиц DWH и витрины данных в PostgreSQL.  
4. **CSV файлы** (исходные данные) для заполнения таблиц-источников.


## Шаги по запуску
1. Установить себе Docker Desktop - https://www.docker.com/products/docker-desktop/ 
2. Установить себе DBeaver Community - https://dbeaver.io/
3. Запустить в docker СУБД PostgrSQL
   - https://habr.com/ru/articles/823816/
   - https://habr.com/ru/articles/578744/
   - https://www.docker.com/blog/how-to-use-the-postgres-docker-official-image/
   - https://geshan.com.np/blog/2021/12/docker-postgres/
   - https://hub.docker.com/_/postgres
  
   `docker run -d --name postgres_container -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=postgres -p 5432:5432 postgres`
      
5. Запустить DBeaver и с помощью скриптов из папки "Cкрипты создания таблиц источников" - создать пустые таблицы источников в PostgreSQL
6. С помощью DBeaver импортировать данные из csv файлов в папке "Данные для источников" в пустые таблицы, созданные на предыдущем шаге
7. С помощью DBeaver запустить скрипты из папки "Скрипты по созданию таблиц DWH и витрин". Скрипты создадут таблицы фактов, измерений и таблицы для витрины данных.
8. Запустите Spark в Docker, чтобы он имел доступ к PostgreSQL в docker (нужно настроить сетевую связанность между ними)
   - https://hub.docker.com/r/jupyter/all-spark-notebook
   - https://stackoverflow.com/questions/37694987/connecting-to-postgresql-in-a-docker-container-from-outside

    `docker run -d -p 8888:8888 -p 4040:4040 --name spark-notebook --user root -v D:\tmp\postgresql-42.6.0.jar:/opt/spark/jars/postgresql-42.6.0.jar jupyter/all-spark-notebook`
   
    `Invoke-WebRequest -Uri https://jdbc.postgresql.org/download/postgresql-42.6.0.jar -OutFile D:\tmp\spark_jars\postgresql-42.6.0.jar` (windows)
   
    `docker network create spark-postgres-network`
   
    `docker network connect spark-postgres-network postgres_container`
    
    `docker network connect spark-postgres-network spark-notebook`
   
10. **Запустить скрипт DWH.ipynb**

