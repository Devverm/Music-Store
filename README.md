# 🎵 Music Store

A SQL-based data analysis project that explores the business performance of an online music store. By writing queries against a relational PostgreSQL database, this project uncovers insights about customers, sales, artists, and genres to support data-driven decision-making.

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-informational?style=for-the-badge&logo=databricks&logoColor=white)

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Database Schema](#database-schema)
- [Installation & Setup](#installation--setup)
- [Analysis Questions](#analysis-questions)
- [Key Findings](#key-findings)
- [Tools Used](#tools-used)
- [Project Structure](#project-structure)
- [License](#license)

---

## Project Overview

This project uses SQL to answer 15 business questions about an online music store's dataset. The goal is to identify trends in customer purchases, popular genres and artists, top-spending customers by country, and more — all of which can help the store improve its marketing and product strategy.

It is also a great starting point for anyone looking to strengthen their SQL skills through a real-world scenario.

---

## Database Schema <img width="2880" height="2982" alt="music_store_erd" src="https://github.com/user-attachments/assets/90365407-493c-4210-ada4-92dbe1b9f93b" />


The database consists of multiple related tables including `customer`, `invoice`, `invoice_line`, `track`, `album`, `artist`, `genre`, and `employee`.

![Schema Diagram]()


## Installation & Setup

To run this project on your machine, you need any SQL-supported DBMS. Follow the steps below:

**General Setup (any SQL DBMS)**

1. Create a new database.
2. Create tables using the schema diagram provided above.
3. Import the CSV files found in the [`dataset/`](./dataset) folder into the corresponding tables.

**Quick Setup (PostgreSQL only)**

If you are using PostgreSQL, you can skip manual table creation by restoring the included database backup:

```bash
pg_restore -U <your_username> -d <your_database_name> music_store_db_backup
```

Alternatively, use the `music_store_db_query.txt` file to recreate the schema via `psql` or PgAdmin4.

---

## Analysis Questions

The `analysis.sql` file contains queries for all 15 business questions:

| # | Question |
|---|----------|
| Q1 | Who is the senior most employee based on job title? |
| Q2 | Which countries have the most invoices? |
| Q3 | What are the top 3 invoice totals? |
| Q4 | Which city has the best customers (highest sum of invoice totals)? |
| Q5 | Who is the best customer (most money spent)? |
| Q6 | Who are all the Rock Music listeners? (email, name, genre) |
| Q7 | Which are the top 10 rock bands by track count? |
| Q8 | Which tracks have a length longer than the average? |
| Q9 | How much has each customer spent per artist? |
| Q10 | What is the most popular genre in each country? |
| Q11 | Who is the top-spending customer in each country? |
| Q12 | Who are the most popular artists by purchase count? |
| Q13 | Which is the most popular song by purchase count? |
| Q14 | What are the average prices across different music genres? |
| Q15 | Which countries make the most music purchases? |

SQL techniques used include `JOIN`, `GROUP BY`, `ORDER BY`, subqueries, `WITH` (CTEs), and window functions (`ROW_NUMBER() OVER PARTITION BY`).

---

## Key Findings

- **Most popular genre:** Rock
- **Most popular artist:** Queens
- **Most popular song:** War Pigs
- **Average price of an album:** $1
- **Top country for purchases:** United States

---

## Tools Used

- **PostgreSQL** — Relational database management system
- **PgAdmin4** — GUI for managing and querying the database

---

## Project Structure

```
Music-Store-Analysis/
├── dataset/                    # CSV files for all database tables
├── MusicDatabaseSchema.png     # Entity-relationship diagram
├── analysis.sql                # All 15 SQL queries with comments
├── music_store_db_backup       # PostgreSQL database backup file
└── music_store_db_query.txt    # SQL script for schema creation

## Credits

Inspired by: [SQL Music Store Analysis Tutorial](https://youtu.be/VFIuIjswMKM)
