# 📊 SQL Business/Data Analyst Case Study: Customer Retention & Sales Trends

## 🏢 Business Scenario | โจทย์ทางธุรกิจ

**TrendCloth** เป็นบริษัทค้าปลีกเสื้อผ้าออนไลน์ที่ให้บริการลูกค้าทั่วสหรัฐอเมริกา โดยบริษัทต้องการเพิ่มอัตราการกลับมาซื้อซ้ำของลูกค้า (**Customer Retention**) และทำความเข้าใจแนวโน้มการซื้อสินค้าในช่วงเวลาต่าง ๆ

ในฐานะ **Business/Data Analyst** หน้าที่ของเราคือวิเคราะห์ข้อมูลจากฐานข้อมูล SQL เพื่อค้นหา Insight ที่สามารถนำไปใช้สนับสนุนการตัดสินใจของทีม **Marketing** และ **Product** ได้

---

## 📁 Data Tables | โครงสร้างข้อมูล

ข้อมูลที่ใช้ในการวิเคราะห์ประกอบด้วย 3 ตารางหลัก ได้แก่ `customers`, `orders` และ `order_items`

### `customers`

เก็บข้อมูลพื้นฐานของลูกค้าและวันที่สมัครใช้งาน

| customer_id | first_name | last_name | signup_date | city    | state |
|-------------|------------|-----------|-------------|---------|-------|
| 1001        | Jamie      | Christian | 2023-01-15  | Raleigh | NC    |

### `orders`

เก็บข้อมูลคำสั่งซื้อของลูกค้า

| order_id | customer_id | order_date | total_amount | status    |
|----------|-------------|------------|--------------|-----------|
| 5001     | 1001        | 2024-03-20 | 75.50        | Completed |

### `order_items`

เก็บรายละเอียดสินค้าในแต่ละคำสั่งซื้อ

| item_id | order_id | product_name     | quantity | unit_price |
|---------|----------|------------------|----------|------------|
| 8001    | 5001     | Graphic T-Shirt  | 2        | 15.00      |

---

## 📌 Objectives | เป้าหมายการวิเคราะห์

### 1. Retention Analysis
วิเคราะห์พฤติกรรมการกลับมาซื้อซ้ำของลูกค้า

- มีลูกค้ากี่รายที่ซื้อสินค้ามากกว่า 1 ครั้ง (**Repeat Customers**)?
- ระยะเวลาเฉลี่ยระหว่างการสั่งซื้อครั้งแรกและครั้งที่สองของลูกค้าเป็นเท่าไร?

### 2. Sales Insights
วิเคราะห์ยอดขายและสินค้าที่สร้างรายได้ให้กับธุรกิจ

- สินค้า 5 อันดับแรกที่สร้างรายได้สูงสุดคืออะไร?
- **Average Order Value (AOV)** ในช่วง 12 เดือนที่ผ่านมาอยู่ที่เท่าไร?

### 3. Churn Risk Indicators
ค้นหากลุ่มลูกค้าที่มีความเสี่ยงที่จะไม่กลับมาซื้อสินค้าอีก

- ลูกค้าคนใดสมัครมานานกว่า 6 เดือน แต่มีคำสั่งซื้อเพียง 1 ครั้ง?
- รัฐใดมีสัดส่วนของ **One-time Customers** สูงที่สุด?

---

## 🧠 SQL Analysis

### 1. Repeat Customers

ค้นหาลูกค้าที่มีคำสั่งซื้อมากกว่า 1 ครั้ง เพื่อระบุกลุ่มลูกค้าที่กลับมาซื้อซ้ำ

```sql
SELECT customer_id, COUNT(*) AS total_orders
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

### 2. Average Order Value

คำนวณมูลค่าคำสั่งซื้อเฉลี่ย (**Average Order Value**) ในช่วง 12 เดือนที่ผ่านมา

```sql
SELECT AVG(total_amount) AS avg_order_value
FROM orders
WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 12 MONTH);
```

### 3. Top Products by Revenue

ค้นหาสินค้าที่สร้างรายได้สูงสุด 5 อันดับแรกจากยอดขายทั้งหมด

```sql
SELECT product_name, SUM(quantity * unit_price) AS total_revenue
FROM order_items
GROUP BY product_name
ORDER BY total_revenue DESC
LIMIT 5;
```

---

## 📈 Key Findings | ผลลัพธ์จากการวิเคราะห์

จากการวิเคราะห์ข้อมูลคำสั่งซื้อย้อนหลัง 12 เดือน พบว่า:

- **28% of customers** มีการกลับมาซื้อสินค้าซ้ำ
- **Graphic T-Shirts** เป็นสินค้าที่สร้างรายได้สูงที่สุด
- **Average Order Value (AOV)** อยู่ที่ **$58.10**
- **37% of one-time customers** กระจุกตัวอยู่ในรัฐ **Texas และ Georgia**

---

## 💡 Business Recommendations | ข้อเสนอแนะทางธุรกิจ

จาก Insight ที่ได้ สามารถเสนอแนวทางให้กับทีมธุรกิจได้ดังนี้:

- ทำ **Targeted Retention Campaigns** สำหรับลูกค้าที่เพิ่งซื้อสินค้าครั้งแรก โดยเฉพาะในรัฐที่มีสัดส่วน One-time Customers สูง
- ใช้โปรโมชั่นหรือส่วนลดเพื่อกระตุ้นให้ลูกค้ากลับมาซื้อครั้งที่สอง
- สร้าง **Product Bundles** โดยใช้สินค้าที่มียอดขายสูง เช่น Graphic T-Shirts เพื่อเพิ่มมูลค่าต่อคำสั่งซื้อ
- ติดตามกลุ่มลูกค้าที่ไม่มีการซื้อซ้ำเป็นเวลานาน เพื่อใช้เป็นกลุ่มเป้าหมายสำหรับ **Re-engagement Campaigns**
