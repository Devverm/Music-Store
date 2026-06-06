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

## Database Schema

The database consists of multiple related tables including `customer`, `invoice`, `invoice_line`, `track`, `album`, `artist`, `genre`, and `employee`.

![Schema Diagram](<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Music Store – Database Schema</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    background: #f5f4f0;
    color: #1a1a18;
    padding: 2rem;
    min-height: 100vh;
  }
  header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 1.5rem;
  }
  header h1 {
    font-size: 1.3rem;
    font-weight: 500;
    color: #1a1a18;
  }
  header span {
    font-size: 0.85rem;
    color: #888780;
    background: #e8e6df;
    padding: 3px 10px;
    border-radius: 20px;
  }
  #erd {
    background: #fff;
    border-radius: 12px;
    border: 1px solid #d3d1c7;
    padding: 1.5rem;
    overflow: auto;
  }
  #erd svg { width: 100%; }

  @media (prefers-color-scheme: dark) {
    body { background: #1e1e1c; color: #c2c0b6; }
    header h1 { color: #e8e6df; }
    header span { background: #2c2c2a; color: #888780; }
    #erd { background: #252523; border-color: #3a3a37; }
  }
</style>
</head>
<body>
<header>
  <h1>Music Store – Database Schema</h1>
  <span>PostgreSQL · 11 tables</span>
</header>
<div id="erd"></div>

<script type="module">
import mermaid from 'https://esm.sh/mermaid@11/dist/mermaid.esm.min.mjs';
const dark = matchMedia('(prefers-color-scheme: dark)').matches;
mermaid.initialize({
  startOnLoad: false,
  theme: 'base',
  fontFamily: '-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif',
  themeVariables: {
    darkMode: dark,
    fontSize: '13px',
    fontFamily: '-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif',
    lineColor: dark ? '#9c9a92' : '#73726c',
    textColor: dark ? '#c2c0b6' : '#3d3d3a',
    primaryColor: dark ? '#2d2b4e' : '#EEEDFE',
    primaryBorderColor: dark ? '#534AB7' : '#7F77DD',
    primaryTextColor: dark ? '#c2c0b6' : '#26215C',
    secondaryColor: dark ? '#1a2e28' : '#E1F5EE',
    tertiaryColor: dark ? '#2c1f1a' : '#FAECE7',
  },
});

const diagram = `erDiagram
  ARTIST ||--o{ ALBUM : "has"
  ALBUM ||--o{ TRACK : "contains"
  TRACK }o--|| MEDIATYPE : "is"
  TRACK }o--|| GENRE : "belongs to"
  TRACK ||--o{ INVOICELINE : "purchased via"
  TRACK }o--o{ PLAYLISTTRACK : "listed in"
  PLAYLIST ||--o{ PLAYLISTTRACK : "has"
  INVOICE ||--o{ INVOICELINE : "has"
  CUSTOMER ||--o{ INVOICE : "places"
  EMPLOYEE ||--o{ CUSTOMER : "supports"
  EMPLOYEE ||--o| EMPLOYEE : "reports to"

  ARTIST {
    int ArtistId PK
    varchar Name
  }
  ALBUM {
    int AlbumId PK
    varchar Title
    int ArtistId FK
  }
  TRACK {
    int TrackId PK
    varchar Name
    int AlbumId FK
    int MediaTypeId FK
    int GenreId FK
    varchar Composer
    int Milliseconds
    int Bytes
    decimal UnitPrice
  }
  MEDIATYPE {
    int MediaTypeId PK
    varchar Name
  }
  GENRE {
    int GenreId PK
    varchar Name
  }
  PLAYLIST {
    int PlaylistId PK
    varchar Name
  }
  PLAYLISTTRACK {
    int PlaylistId PK
    int TrackId PK
  }
  INVOICELINE {
    int InvoiceLineId PK
    int InvoiceId FK
    int TrackId FK
    decimal UnitPrice
    int Quantity
  }
  INVOICE {
    int InvoiceId PK
    int CustomerId FK
    date InvoiceDate
    varchar BillingAddress
    varchar BillingCity
    varchar BillingState
    varchar BillingCountry
    decimal Total
  }
  CUSTOMER {
    int CustomerId PK
    varchar FirstName
    varchar LastName
    varchar Company
    varchar Email
    int SupportRepId FK
  }
  EMPLOYEE {
    int EmployeeId PK
    varchar LastName
    varchar FirstName
    varchar Title
    int ReportsTo FK
    date HireDate
    varchar Email
  }
`;

const { svg } = await mermaid.render('erd-svg', diagram);
document.getElementById('erd').innerHTML = svg;

document.querySelectorAll('#erd svg .node').forEach(node => {
  const firstPath = node.querySelector('path[d]');
  if (!firstPath) return;
  const d = firstPath.getAttribute('d');
  const nums = d.match(/-?[\d.]+/g)?.map(Number);
  if (!nums || nums.length < 8) return;
  const xs = [nums[0], nums[2], nums[4], nums[6]];
  const ys = [nums[1], nums[3], nums[5], nums[7]];
  const x = Math.min(...xs), y = Math.min(...ys);
  const w = Math.max(...xs) - x, h = Math.max(...ys) - y;
  const rect = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
  rect.setAttribute('x', x); rect.setAttribute('y', y);
  rect.setAttribute('width', w); rect.setAttribute('height', h);
  rect.setAttribute('rx', '8');
  for (const a of ['fill', 'stroke', 'stroke-width', 'class', 'style']) {
    if (firstPath.hasAttribute(a)) rect.setAttribute(a, firstPath.getAttribute(a));
  }
  firstPath.replaceWith(rect);
});

document.querySelectorAll('#erd svg .row-rect-odd path, #erd svg .row-rect-even path').forEach(p => {
  p.setAttribute('stroke', 'none');
});
</script>
</body>
</html>)

---

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
