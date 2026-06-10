[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112733&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** zantlytm@gmail.com
**Name:** Nguyễn Thị Bảo Trân
**Student ID:** AI20K-2A202600917

---

## Mo ta

Bai lab xay dung mot **ETL Pipeline tu dong** (Extract → Validate → Transform → Load) bang Python va pandas, ket hop voi cac yeu to **Data Observability** (logging so record processed/dropped, timestamp `processed_at`).

Cac buoc da thuc hien:
- **Extract:** Doc du lieu tu `raw_data.json`, co xu ly loi file khong ton tai / JSON hong.
- **Validate:** Loai bo record co `price <= 0` (hoac khong phai so) va record co `category` rong, dong thoi log ro tung record bi loai.
- **Transform:** Tinh `discounted_price = price * 0.9`, chuan hoa `category` ve Title Case, them cot `processed_at` (ISO timestamp).
- **Load:** Luu ket qua ra `processed_data.csv`.
- **Stress Test:** Chay `agent_simulation.py` voi du lieu sach va du lieu rac (`garbage_data.csv`), so sanh ket qua va phan tich trong `experiment_report.md`.

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chay ETL Pipeline
```bash
python solution.py
```
Sau khi chay, kiem tra file `processed_data.csv` da duoc tao.

### Chay Agent Simulation (Stress Test)
```bash
# 1. Tao du lieu rac
python generate_garbage.py

# 2. Chay Agent voi ca 2 bo du lieu (Clean vs Garbage)
python -c "from agent_simulation import simulate_agent_response; print(simulate_agent_response('What is the best electronic product?', 'processed_data.csv')); print(simulate_agent_response('What is the best electronic product?', 'garbage_data.csv'))"
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── raw_data.json            # Du lieu nguon
├── processed_data.csv       # Output cua pipeline
├── generate_garbage.py      # Script tao du lieu rac
├── agent_simulation.py      # Mo phong Agent (RAG-like)
├── experiment_report.md     # Bao cao thi nghiem
└── README.md                # File nay
```

---

## Ket qua

- **Extract:** 5 records doc tu `raw_data.json`.
- **Validate:** 2 records bi loai (1 record gia am `-10`, 1 record category rong) → con **3 records hop le**.
- **Load:** 3 records duoc luu vao `processed_data.csv` voi day du cot `discounted_price`, `category` (Title Case), `processed_at`.
- **Stress Test:** Voi du lieu sach, Agent tra loi dung ("Laptop at $1200"). Voi du lieu rac, Agent bi outlier danh lua va goi y "Nuclear Reactor at $999999". Chi tiet trong `experiment_report.md`.
