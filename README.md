# 📊 School Dashboard — Power BI

> A Power BI decision-making report for analyzing academic performance in an educational institution.

---

## 🎯 Project Overview

The school administration wanted to better understand academic performance, absenteeism, teacher workload, and course effectiveness. This project delivers an interactive Power BI report to support strategic decision-making.

---

## 📁 Project Structure

```
tableau-bord-scolaire-powerbi/
│
├── data/
│   ├── students.csv          # 120 students
│   ├── teachers.csv          # 20 teachers
│   └── courses.csv           # 500 courses
│
├── presentation/
│   └── tableau_bord_scolaire.pptx
│
├── rapport/
│   └── projet_etude.pbix     # Power BI report
│
└── README.md
```

---

## 🗄️ Dataset Description

### students.csv — 120 students
| Column | Description |
|---|---|
| `student_id` | Unique identifier |
| `full_name` | Full name |
| `birth_date` | Date of birth |
| `gender` | Gender (M / F) |
| `enrollment_date` | Enrollment date |
| `class_level` | Grade level |
| `section` | Section / stream |
| `status` | Status (Active, Graduate, Repeating, Transferred) |
| `average_grade` | Annual average grade / 20 |
| `absences_count` | Number of absence days |
| `teacher_id` | Reference to main teacher |

### teachers.csv — 20 teachers
| Column | Description |
|---|---|
| `teacher_id` | Unique identifier |
| `full_name` | Full name |
| `hire_date` | Hiring date |
| `subject` | Subject taught |
| `department` | Teaching department |
| `contract_type` | Contract type (Permanent, Contractual, Part-time) |
| `weekly_hours` | Teaching hours per week |
| `performance_rating` | Internal evaluation score (1 to 5) |

### courses.csv — 500 courses
| Column | Description |
|---|---|
| `course_id` | Unique identifier |
| `course_name` | Course title |
| `teacher_id` | Reference to teacher |
| `student_id` | Reference to student |
| `semester` | Semester (S1 / S2) |
| `year` | School year |
| `scheduled_hours` | Planned hours |
| `completed_hours` | Actual completed hours |
| `grade` | Grade obtained / 20 |
| `pass_fail` | Result (Passed / Failed) |

---

## 🏗️ Data Architecture & Modeling

### Star Schema

```
DimDate ────────────────────────────────────────────┐
                                                     │
students ──── (teacher_id) ──── teachers             │
    │                                                │
    └── (enrollment_date) ──────────────────── DimDate

courses ──── (student_id) ──── students
        └─── (teacher_id) ──── teachers
```

### Relationships
| From | To | Cardinality |
|---|---|---|
| `students[teacher_id]` | `teachers[teacher_id]` | Many-to-One |
| `courses[student_id]` | `students[student_id]` | Many-to-One |
| `courses[teacher_id]` | `teachers[teacher_id]` | Many-to-One |
| `students[enrollment_date]` | `DimDate[Date]` | Many-to-One |

---

## 🔧 Data Preparation (Power Query)

- ✅ Data type correction (dates, decimals, text)
- ✅ Missing values handling (median for grades, "Unknown" for text)
- ✅ Duplicate removal
- ✅ Format standardization (title case, extra spaces)
- ✅ Calculated columns:
  - `age_eleve` — calculated from `birth_date`
  - `anciennete_enseignant` — calculated from `hire_date`
  - `taux_realisation_cours` — `completed_hours / scheduled_hours × 100`
  - `annee_inscription` — extracted from `enrollment_date`

---

## 📐 DAX Measures

```dax
Total Students = DISTINCTCOUNT(students[student_id])

Overall Average = AVERAGE(students[average_grade])

Pass Rate =
DIVIDE(
    COUNTROWS(FILTER(courses, courses[pass_fail] = "Réussi")),
    COUNTROWS(courses), 0
) * 100

At-Risk Students =
COUNTROWS(FILTER(students,
    students[average_grade] < 10 &&
    students[absences_count] > 15
))

Total Teachers = DISTINCTCOUNT(teachers[teacher_id])

Average Seniority = AVERAGE(teachers[anciennete_enseignant])

Avg Hours per Teacher = AVERAGE(teachers[weekly_hours])

Avg Evaluation Score = AVERAGE(teachers[performance_rating])

Total Courses = COUNTROWS(courses)

Avg Completion Rate = AVERAGE(courses[taux_realisation_cours])
```

---

## 📊 Report Pages

### Page 1 — Students View 🔵
| Visual | Description |
|---|---|
| 4 KPI Cards | Total students, Overall average, Pass rate, At-risk students |
| Stacked bar chart | Distribution by grade level and section |
| Line chart | Enrollment trend by year |
| Scatter plot | Correlation between absences and average grade |
| Donut chart | Distribution by student status |

### Page 2 — Teachers View 🟢
| Visual | Description |
|---|---|
| 4 KPI Cards | Total teachers, Avg seniority, Weekly hours, Eval. score |
| Grouped bar chart | Distribution by subject and contract type |
| Histogram | Distribution of evaluation scores |
| Top/Flop Table | Top 5 and Flop 5 teachers by performance_rating |

### Page 3 — Courses & Results View 🟣
| Visual | Description |
|---|---|
| 4 KPI Cards | Total courses, Completion rate, Pass rate S1, Pass rate S2 |
| Stacked bar chart | Pass / Fail by subject |
| Line chart | Average grade trend by semester and year |
| Sorted table | Subjects with average >= 12 |

### Page 4 — Conclusions
Strategic summary and recommendations based on data analysis.

---

## 💡 Key Insights

- **29 at-risk students** (grade < 10 and absences > 15 days) → personalized follow-up recommended
- **Overall pass rate: 52%** → reinforcement needed in science subjects
- **Average evaluation score: 3.16/5** → pedagogical support needed for teachers scoring < 2/5
- **Negative correlation** confirmed between absenteeism and academic performance

---

## 🛠️ Technologies Used

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-185FA5?style=for-the-badge&logo=microsoft&logoColor=white)
![Power Query](https://img.shields.io/badge/Power%20Query-0F6E56?style=for-the-badge&logo=microsoft&logoColor=white)

| Tool | Usage |
|---|---|
| Power BI Desktop | Report development |
| Power Query (M) | Data cleaning and transformation |
| DAX | Calculated measures and columns |
| Star Schema | Data architecture |

---

## 👩‍💻 Author

**Ouiam El Khalfi** — Data Engineering Student
Academic Project — 2026
