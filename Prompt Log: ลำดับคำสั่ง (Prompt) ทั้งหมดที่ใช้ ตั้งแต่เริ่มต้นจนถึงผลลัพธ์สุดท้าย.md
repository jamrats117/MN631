## **Prompt Log**

### 🔷 ขั้นตอนที่ 1: Data Preparation

ผู้จัดทำได้ดำเนินการดังนี้:

* ดึงข้อมูลจาก YouTube lecture จำนวน 3 คลิป
* แปลงเสียงเป็นข้อความ (transcription)
* ทำการแบ่งข้อความ (chunking) โดย:

  * 1 video ≈ 8 chunks
  * ขนาด chunk ≈ 2,000–3,000 คำ
* เพื่อให้เหมาะสมกับข้อจำกัดของโมเดลและเพิ่มความแม่นยำในการประมวลผล

---

### 🔷 ขั้นตอนที่ 2: Knowledge Extraction (Chunk-level)

#### Prompt สำหรับ Chunk ที่ 1

```text
You are a teaching assistant.

From this lecture transcript chunk:

Extract:
- Key concepts
- Important formulas
- Definitions
- Examples

Do NOT summarize.
Keep all important details.
```

#### Prompt สำหรับ Chunk ที่ 2–7

```text
This is part of a longer lecture transcript.

From this chunk:
Extract:
- Key concepts
- Important formulas
- Definitions
- Examples

Do NOT summarize.
Keep all important details.
```

#### Prompt สำหรับ Chunk ที่ 8 (ตอนสุดท้าย)

```text
This is the last part of a longer lecture transcript.

From this chunk:
Extract:
- Key concepts
- Important formulas
- Definitions
- Examples

Do NOT summarize.
Keep all important details.
```

📌 แนวคิด:

* ใช้ **context-aware prompting** เพื่อให้ AI เข้าใจตำแหน่งของข้อมูลใน lecture
* ลดปัญหาการตีความผิดจากการแบ่งข้อมูล

---

### 🔷 ขั้นตอนที่ 3: Aggregation (รวมข้อมูลทั้งหมด)

```text
Combine and organize the extracted content vdo3_1 to vdo3_8 into:

1. Key concepts (merged, no duplicates)
2. Important formulas (with explanation)
3. Examples
4. Definitions

Remove redundancy and keep it structured.
```

📌 แนวคิด:

* ลด redundancy
* รวมองค์ความรู้ให้เป็นภาพรวมเดียว (global understanding)

---

### 🔷 ขั้นตอนที่ 4: Transformation → Exam Cheat Sheet

```text
You are preparing an exam cheat sheet.

Transform this into:
- Structured summary
- Key formulas (highlight + when to use)
- Quick examples
- Exam tips
- Common mistakes

Constraint:
This must be the ONLY document allowed in an open-book exam.
```

📌 แนวคิด:

* เปลี่ยนจาก raw knowledge → usable knowledge
* เน้น “ใช้สอบได้จริง”

---

### 🔷 ขั้นตอนที่ 5: Self-Evaluation & Refinement

```text
Check this cheat sheet:
- Are any key concepts missing?
- Is it sufficient for exam use?
- Improve clarity and completeness
```

📌 แนวคิด:

* ใช้ **self-reflection prompting**
* เพิ่มความครบถ้วนและความน่าเชื่อถือของผลลัพธ์

---

### 🔷 ขั้นตอนที่ 6: Output Formatting

```text
please show result as .md format
```

📌 แนวคิด:

* ใช้ Markdown (.md) เพื่อ:

  * ความเป็นมาตรฐานในสาย developer
  * อ่านง่าย / นำไปใช้งานต่อได้ทันที

---
