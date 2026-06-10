# Experiment Report: Data Quality Impact on AI Agent

**Student Email:** zantlytm@gmail.com
**Name:** Nguyễn Thị Bảo Trân
**Student ID:** AI20K-2A202600917

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Agent: Based on my data, the best choice is Laptop at $1200." | 10 | Tra loi dung. Laptop la san pham electronics co gia cao nhat trong du lieu da duoc validate va transform. |
| Garbage Data (`garbage_data.csv`) | "Agent: Based on my data, the best choice is Nuclear Reactor at $999999." | 1 | Tra loi vo nghia. Agent goi y mot "Nuclear Reactor" gia $999,999 — mot outlier ro rang khong phai san pham that. |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Agent tra loi sai vi no hoan toan tin tuong vao du lieu dau vao ma khong co bat ky buoc kiem tra chat luong nao. Cu the, file `garbage_data.csv` chua 4 loai loi dien hinh:

1. **Extreme Outlier:** Record "Nuclear Reactor" co gia $999,999 thuoc category "electronics". Vi logic cua Agent la chon san pham co `price` cao nhat (`idxmax`), outlier nay lap tuc tro thanh "best choice", day ket qua dung (Laptop $1200) ra khoi cau tra loi. Mot record rac duy nhat da lam sai lech toan bo output.

2. **Duplicate IDs:** Hai record cung `id = 1` (Laptop va Banana). Trong he thong thuc te, duplicate keys gay ra dem trung, join sai, va khien Agent co the tham chieu nham san pham khi truy van theo ID.

3. **Wrong Data Types:** Gia tri `price = "ten dollars"` la chuoi thay vi so. Dieu nay khien cot price khong con thuan kieu numeric, cac phep so sanh/sap xep tro nen khong dang tin cay va co the lam crash pipeline o cac buoc tinh toan.

4. **Null Values:** Record "Ghost Item" co `id` va `category` la null. Du lieu thieu khien cac phep loc theo category bo sot hoac dem sai, va lam giam do tin cay cua bat ky thong ke nao tinh tren cot do.

Ban chat van de la **"Garbage In, Garbage Out"**: Agent (giong nhu mo hinh RAG that) chi truy xuat va tong hop tu knowledge base. Neu knowledge base bi nhiem doc, cau tra loi se sai du prompt co tot den dau. Trong khi do, voi `processed_data.csv` — du lieu da di qua pipeline validate (loai gia <= 0, category rong) va transform (chuan hoa kieu, Title Case) — Agent tra loi chinh xac ngay lap tuc.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** **Dong y.**

Thi nghiem cho thay cung mot Agent, cung mot prompt ("What is the best electronic product?"), nhung ket qua khac nhau hoan toan chi vi chat luong du lieu. Prompt tot khong the cuu duoc du lieu rac: Agent van tu tin goi y "Nuclear Reactor" gia $999,999. Nguoc lai, voi du lieu sach, mot logic don gian cung cho ra cau tra loi dung. Vi vay, dau tu vao data pipeline (validation, cleaning, observability) la nen tang bat buoc truoc khi toi uu prompt — du lieu sach la dieu kien can de Agent dang tin cay.
