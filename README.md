# 🏥 Hospital Management Data Analytics

End-to-end data analytics project simulating a hospital's operations data — covering **database design (MySQL)**, **ETL automation (Python/Flask)**, and **business intelligence reporting (Power BI)**. Built to demonstrate a complete analyst workflow: raw data → relational database → cleaned model → interactive dashboard.

---

## 📌 Project Overview

This project models a hospital's day-to-day operations — patients, doctors, appointments, admissions, billing, surgeries, medical tests, medicine stock, staff, and patient satisfaction — across **14 relational tables**, and turns that data into a multi-page Power BI dashboard for operational and financial insight.

**Business questions this project is designed to answer:**
- How is the hospital performing on revenue, billing, and outstanding payments?
- Which departments and doctors are handling the most patient load?
- What does bed/room occupancy and utilization look like?
- How satisfied are patients, and does it vary by doctor/department?
- Are medicine stock levels healthy against reorder thresholds?

---

## 🧰 Tech Stack

| Layer | Tools |
|---|---|
| Data Source | Excel (.xlsx) |
| Data Ingestion / ETL | Python, Pandas, Flask (custom-built Excel-to-MySQL uploader app) |
| Database | MySQL |
| Reporting & Visualization | Power BI (Power Query, Data Modelling, DAX) |
| Version Control | Git & GitHub |

---

## 🔁 Project Workflow

```
Raw Excel Files  →  Python/Flask Uploader App  →  MySQL Database  →  Power BI (Power Query + DAX)  →  Interactive Dashboard
```

1. **Source data** was prepared as Excel files, one per entity (patients, doctors, appointments, bills, etc.).
2. A **custom Flask web app** (`Python App - Excel Uploader_SQL DB/`) was built to upload any Excel/CSV file directly into MySQL — it dynamically detects column types and creates tables on the fly, so new datasets can be ingested without writing manual `CREATE TABLE` scripts each time.
3. Data lives in **MySQL** as the single source of truth (dump provided in `MySQL Database - Dump File/`).
4. **Power BI** connects to MySQL, models the relationships, and builds DAX measures for KPIs across 5 report pages: **Overview, Hospital, Patient, Doctor, and Finance**.

---

## 🗂️ Repository Structure

```
hospital-management-data-analytics/
│
├── README.md
├── Hospital_Analytics_Dashboard-v1.pbix              # Power BI dashboard (Overview, Hospital, Patient, Doctor, Finance pages)
│
├── Excel Files_Direct Import/              # Final cleaned Excel source files (imported directly into Power BI / MySQL)
│   ├── patient.xlsx, Doctor.xlsx, Appointment.xlsx, Hospital Bills.xlsx
│   ├── Rooms.xlsx, Beds.xlsx, Department.xlsx, Staff.xlsx, Supplier.xlsx
│   ├── Surgery.xlsx, Medical Tests.xlsx, Patient_Tests.xlsx
│   ├── Medical Stock.xlsx, medicine_patient.xlsx, Satisfaction Score.xlsx
│
├── Pre Setup Excel Files/                  # Earlier/raw versions of source files before cleanup
│
├── MySQL Database - Dump File/
│   └── hospital_data_dump.sql              # Full database dump (14 tables) for restoring the DB locally
│
├── Python App - Excel Uploader_SQL DB/
│   └── excel_uploader/
│       ├── app.py                          # Flask app: uploads Excel/CSV files into MySQL dynamically
│       └── templates/ (index.html, upload.html)
│
└── Images/                                 # Assets used inside the Power BI dashboard (doctor photos, backgrounds)
```



---

## 🗃️ Database Schema (14 Tables)

`patient` · `doctor` · `department` · `appointment` · `surgery` · `rooms` · `beds` · `staff` · `supplier` · `medical_stock` · `medical_tests` · `patient_tests` · `medicine_patient` · `hospital_bills` · `satisfaction_score`

Entity relationships (by shared keys):
- `patient` ↔ `appointment`, `surgery`, `beds`, `patient_tests`, `medicine_patient`, `hospital_bills`, `satisfaction_score` (via `patient_id`)
- `doctor` ↔ `appointment`, `surgery`, `patient_tests`, `satisfaction_score` (via `doctor_id`), and ↔ `department`
- `department` ↔ `rooms`, `staff`, `medical_tests`
- `rooms` ↔ `beds` (via `room_id`)
- `medical_stock` ↔ `medicine_patient`, `supplier`

## 📊 Power BI Dashboard

The `.pbix` file contains 5 report pages:
- **Overview** – top-level hospital KPIs at a glance
- **Hospital** – bed/room occupancy, department & staff view
- **Patient** – admissions, demographics, satisfaction
- **Doctor** – doctor-wise appointments, specialization, workload
- **Finance** – billing, revenue, payment status, discounts

*(Screenshot placeholders — export images from the .pbix and add them here, e.g. `![Overview Page](Images/screenshots/overview.png)`)*

---

## ⚙️ How to Reproduce This Project Locally

1. **Restore the database**
   ```bash
   mysql -u root -p < "MySQL Database - Dump File/hospital_data_dump.sql"
   ```
2. **(Optional) Run the Excel-to-MySQL uploader app**
   ```bash
   cd "Python App - Excel Uploader_SQL DB/excel_uploader"
   pip install flask pandas mysql-connector-python openpyxl
   python app.py
   ```
   Open `http://localhost:5000`, connect to your MySQL instance, and upload any Excel file from `Excel Files_Direct Import/`.
3. **Open the dashboard**
   Open `Hospital_Dashboard-v1.pbix` in Power BI Desktop, and point the MySQL connector at your local database if prompted to refresh.

---

## 🧠 Key Learnings / Skills Demonstrated

- Designing a multi-entity relational schema for a real-world domain (14 interrelated tables)
- Building a lightweight Python/Flask ETL tool to automate Excel → MySQL ingestion
- Data modelling and DAX measure creation in Power BI across relationship-heavy data
- Structuring a multi-page dashboard around distinct stakeholder views (operations, clinical, finance)

## 🔧 Known Limitations & Next Steps

- The current MySQL dump has **no foreign key constraints** and most columns are stored as `VARCHAR`/generic types (a side effect of the dynamic uploader tool, which infers types on upload rather than following a fixed schema). A production version would define explicit PKs/FKs and proper data types.
- Dataset is small/synthetic (15–30 rows per table for most entities), so it's meant to demonstrate structure and workflow rather than large-scale analysis.
- Planned improvements: add explicit FK constraints, write SQL analysis queries (revenue trends, doctor workload, bed occupancy rate) as a companion `/sql` folder, and add dashboard screenshots to this README.

---

## 👤 Author

**Aditya Bhattacharjee**
Academic Operations Professional transitioning into Data Analytics | Excel · SQL/MySQL · Power BI · Python
