# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-XXXX
**Name:** Winston Nguyen
**Date:** April 15, 2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Based on my data, the best choice is Laptop at $1200." | 10 | Correct product, correct price, valid category match |
| Garbage Data (`garbage_data.csv`) | "Based on my data, the best choice is Nuclear Reactor at $999999." | 1 | Extreme outlier skewed the result; response is completely unreliable |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Khi su dung Garbage Data, Agent chon san pham "Nuclear Reactor" voi gia $999,999 — mot outlier cuc doan khong phan anh thuc te thi truong. Day la hau qua truc tiep cua cac van de chat luong du lieu trong `garbage_data.csv`:

- **Extreme Outlier**: Ban ghi "Nuclear Reactor" co gia $999,999, cao bat thuong so voi cac san pham thong thuong. Vi Agent dung logic `idxmax()` de tim gia cao nhat, outlier nay luon duoc chon, khien ket qua hoan toan sai lech.
- **Duplicate IDs**: Co 2 ban ghi voi `id = 1` (Laptop va Banana). Dieu nay co the gay nham lan khi truy van du lieu hoac join voi bang khac, vi AI khong biet ban ghi nao la chinh xac.
- **Wrong Data Types**: "Broken Chair" co truong `price = "ten dollars"` — mot chuoi ky tu thay vi so. Trong truong hop phuc tap hon, dieu nay se gay loi khi tinh toan (so sanh gia, tinh trung binh, v.v.).
- **Null Values**: Ban ghi "Ghost Item" co `id = None` va `category = None`. Cac gia tri null co the gay loi hoac bi bo qua ma khong co canh bao, lam mat di du lieu quan trong.

Tat ca cac van de nay cho thay: du prompt co tot den dau, neu du lieu dau vao bi "poisoned", AI Agent van se dua ra ket qua sai va co the gay hai.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Dong y.

Thi nghiem nay chung minh rang du lieu chat luong cao quan trong hon viec co mot prompt tot. Voi Clean Data, Agent cho ket qua chinh xac 100% ma khong can thay doi gi o prompt. Voi Garbage Data, du prompt hoan toan giong nhau, Agent van tra ve ket qua sai lech nghiem trong do outlier. Do do, trong bat ky he thong AI nao, buoc validate va lam sach du lieu (ETL pipeline) la nen tang bat buoc truoc khi su dung du lieu cho bat ky mo hinh hay agent nao.
