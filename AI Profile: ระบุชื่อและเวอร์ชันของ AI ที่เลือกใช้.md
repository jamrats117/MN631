## **AI Profile**

* AI หลัก: ChatGPT (GPT-5.4 Thinking mode)
* วัตถุประสงค์การใช้งาน:

  * ประมวลผลข้อความจาก lecture transcript ที่มีความยาวมาก
  * สกัดองค์ความรู้เชิงโครงสร้าง (structured knowledge extraction)
  * สร้างเอกสารสรุปในรูปแบบ **exam-oriented cheat sheet**
* แนวทาง: ใช้ **multi-step prompting pipeline** เพื่อเพิ่มความครบถ้วนและความแม่นยำของผลลัพธ์

---

## **สรุปแนวทาง (Workflow Overview)**

กระบวนการทั้งหมดสามารถสรุปได้ดังนี้:

1. Transcription จากวิดีโอ
2. Chunking เพื่อจัดการข้อมูลขนาดใหญ่
3. Extraction (ระดับ chunk)
4. Aggregation (รวมข้อมูลทั้งหมด)
5. Transformation → Cheat Sheet
6. Self-evaluation
7. Export เป็น Markdown

---
