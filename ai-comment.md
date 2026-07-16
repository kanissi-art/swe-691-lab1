จำนวน bug: 2 จุด
ไม่มีการตรวจสอบชนิดข้อมูล → ถ้า scores มีค่าที่ไม่ใช่ตัวเลข เช่น "abc" หรือ None จะทำให้เกิด TypeError ตอนบวกหรือหาร

ไม่มีการตรวจสอบช่วงคะแนน → ถ้าคะแนนติดลบ หรือเกิน 100 จะยังถูกนำไปคำนวณและตีเกรดตามเงื่อนไข ซึ่งไม่สมเหตุสมผล

📋 รายละเอียด bug แต่ละจุด
Bug ที่ 1: for score in scores: total = total + score → ถ้า score ไม่ใช่ตัวเลข จะ crash ทันที

Bug ที่ 2: if average >= 80: → ใช้ได้เฉพาะกรณีคะแนนอยู่ในช่วง 0–100 แต่ถ้าเจอ -5 หรือ 120 ก็ยังตีเกรดผิดพลาด

def calculate_grade(scores):
    def score_to_grade(score):
        if not isinstance(score, (int, float)):
            return "Invalid"
        if score < 0 or score > 100:
            return "Invalid"
        elif score >= 80:
            return "A"
        elif score >= 70:
            return "B"
        elif score >= 60:
            return "C"
        elif score >= 50:
            return "D"
        else:
            return "F"

    # กรองเฉพาะตัวเลขที่อยู่ในช่วง 0–100
    valid_scores = [s for s in scores if isinstance(s, (int, float)) and 0 <= s <= 100]

    if not valid_scores:
        print("ไม่มีคะแนนที่ถูกต้องเลย")
        return None

    total = sum(valid_scores)
    average = total / len(valid_scores)

    overall_grade = score_to_grade(average)

    detailed_grades = [(score, score_to_grade(score)) for score in scores]

    print(f"จำนวนจุดทั้งหมด: {len(scores)}")
    print("รายละเอียดแต่ละจุด:")
    for score, grade in detailed_grades:
        print(f"- {score} → {grade}")
    print(f"\nค่าเฉลี่ย: {average:.2f}")
    print(f"เกรดรวม: {overall_grade}")
    return overall_grade, average, detailed_grades


# ตัวอย่างทดสอบ
scores = [85, "abc", -5, 92, 110, 70]
calculate_grade(scores)

จำนวนจุดทั้งหมด: 6
รายละเอียดแต่ละจุด:
- 85 → A
- abc → Invalid
- -5 → Invalid
- 92 → A
- 110 → Invalid
- 70 → B

ค่าเฉลี่ย: 82.33
เกรดรวม: A
เข้าใจแล้วครับ ตารางที่คุณต้องการคือการวัด **คุณภาพและความน่าเชื่อถือของตัวโค้ด (Software Reliability)** เพื่อดูว่าโค้ดที่เราเขียนอยู่ในระดับไหน ตั้งแต่แค่พอทำงานได้ ไปจนถึงระดับมืออาชีพที่พร้อมใช้งานจริง

นี่คือตาราง **Trust Level สำหรับโค้ดคำนวณเกรด** ตามแบบที่คุณต้องการครับ:

### ตาราง Trust Level ของซอฟต์แวร์ (Software Reliability)

| ระดับความเชื่อถือ | ชื่อระดับ (Status) | คำอธิบาย (Description) | ตัวอย่างจากโค้ดที่คุณให้มา |
| :--- | :--- | :--- | :--- |
| **Level 0** | **Unstable (อันตราย)** | โค้ดทำงานผิดพลาดบ่อย หรือค้าง (Crash) เมื่อเจอข้อมูลผิดปกติ | **โค้ดเดิมของคุณ:** หากใส่ List ว่าง `[]` โปรแกรมจะพังทันทีเพราะหารด้วย 0 (ZeroDivisionError) |
| **Level 1** | **Functional (ใช้งานได้)** | ทำงานตามตรรกะได้ถูกต้องในสภาวะปกติ แต่ไม่มีระบบป้องกันความผิดพลาด | **โค้ดเดิมของคุณ:** คำนวณเกรดได้ถูกต้องถ้าคะแนนที่ใส่มาเป็นตัวเลขและครบถ้วน |
| **Level 2** | **Reliable (เชื่อถือได้)** | มีการเช็คข้อผิดพลาดพื้นฐาน (Error Handling) ป้องกันไม่ให้โปรแกรมค้าง | **โค้ดที่แก้ไขแล้ว:** มีการเช็ค `if not scores:` เพื่อไม่ให้โปรแกรมพังเมื่อไม่มีข้อมูลคะแนน |
| **Level 3** | **Robust (แข็งแกร่ง)** | รองรับข้อมูลที่หลากหลาย และมีการจัดการ Error ที่ซับซ้อน (เช่น ข้อมูลไม่ใช่ตัวเลข) | **โค้ดเวอร์ชันปรับปรุง:** มีการใช้ `try...except` เพื่อดักจับกรณีมี String ปนมาในลิสต์คะแนน |
| **Level 4** | **Production Ready (พร้อมใช้งานจริง)** | โค้ดสะอาด (Clean), ทำงานเร็ว (Optimized), และมีคำอธิบายชัดเจนตามมาตรฐาน | **โค้ดที่สมบูรณ์:** ใช้ฟังก์ชัน `sum()`, มี Type Hinting, มี Docstring, และเขียนตามมาตรฐาน PEP8 |

---

### วิธีขยับ Trust Level ให้โค้ดของคุณไปถึง "Level 4"
หากคุณต้องการให้โค้ดของคุณอยู่ในระดับมืออาชีพ (Production Ready) นี่คือโค้ดเวอร์ชันที่ปรับปรุงแล้วครับ:

```python
def calculate_grade(scores: list) -> tuple:
    """
    คำนวณเกรดเฉลี่ยจากรายการคะแนนที่ได้รับ
    Return: (grade, average)
    """
    # Level 2 & 3: Error Handling & Validation
    if not scores:
        return "N/A", 0.0
    
    try:
        # Level 4: Efficiency (Use sum instead of loop)
        average = sum(scores) / len(scores)
    except TypeError:
        return "Error: ข้อมูลในลิสต์ต้องเป็นตัวเลขเท่านั้น", 0.0

    # Logic การตัดเกรด
    if average >= 80: grade = "A"
    elif average >= 70: grade = "B"
    elif average >= 60: grade = "C"
    elif average >= 50: grade = "D"
    else: grade = "F"

    return grade, round(average, 2)

# --- การใช้งาน ---
scores = [85, 92, 78, 88, 95]
grade, avg = calculate_grade(scores)
print(f"ผลลัพธ์: เกรด {grade} (คะแนนเฉลี่ย {avg})")
```

**สรุปสิ่งที่เพิ่มเข้ามาเพื่อให้ได้ Level 4:**
1.  **Type Hinting (`scores: list`)**: บอกชัดเจนว่าฟังก์ชันรับค่าอะไร
2.  **Docstring (`"""..."""`)**: คำอธิบายหน้าที่ของฟังก์ชัน
3.  **Efficiency**: ใช้ `sum()` เพื่อให้ทำงานเร็วขึ้น
4.  **Robustness**: ใช้ `try...except` ป้องกันกรณีคนใส่ข้อมูลผิดประเภท
5.  **Clean Output**: ใช้ `round()` เพื่อให้ทศนิยมไม่ยาวจนเกินไปครับ
