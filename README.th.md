# yindee-skills — แนวทางเขียนโค้ดสำหรับ Claude Code (สไตล์ Karpathy)

> **Fork จาก [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)** และปรับให้เป็นของตัวเอง
> หลักการต้นฉบับมาจาก [observations ของ Andrej Karpathy](https://x.com/karpathy/status/2015883857489522876) เรื่องนิสัยพังๆ ของ LLM ตอนเขียนโค้ด

🌐 **ภาษา:** [English](./README.md) | **ภาษาไทย** | [简体中文](./README.zh.md)

Plugin สำหรับ Claude Code (หรือไฟล์ `CLAUDE.md` เดียวๆ) ที่ทำให้ Claude เขียนโค้ดดีขึ้น แพ็กเป็น `yindee-skills`

## ปัญหาที่พบบ่อย

จาก post ของ Andrej:

> "โมเดลมักเดา assumption ผิดแทนเรา แล้วลุยต่อโดยไม่ตรวจสอบ ไม่จัดการความสับสนของตัวเอง ไม่ขอ clarification ไม่บอก inconsistency ไม่นำเสนอ tradeoff ไม่ push back ทั้งที่ควร"

> "ชอบทำให้โค้ดและ API ซับซ้อนเกินจำเป็น พอง abstraction ไม่ลบ dead code... สร้างของ 1000 บรรทัดทั้งที่ 100 บรรทัดก็พอ"

> "บางทีก็แก้/ลบ comment กับโค้ดที่ไม่ได้เข้าใจดีพอ เป็น side effect แม้จะไม่เกี่ยวกับ task เลย"

## วิธีแก้

**4 หลักการ** ในไฟล์เดียว ที่จัดการปัญหาด้านบนตรงๆ:

| หลักการ | แก้ปัญหา |
|---|---|
| **Think Before Coding** | เดา assumption ผิด, ซ่อนความสับสน, ไม่บอก tradeoff |
| **Simplicity First** | ทำซับซ้อนเกินจำเป็น, abstraction บวม |
| **Surgical Changes** | แก้ของที่ไม่เกี่ยว, เปลี่ยนโค้ดที่ไม่ควรแตะ |
| **Goal-Driven Execution** | ใช้ test-first + success criteria ที่ตรวจวัดได้ |

## รายละเอียด 4 หลักการ

### 1. Think Before Coding (คิดก่อนโค้ด)

**อย่าเดา อย่าปิดบังความสับสน นำเสนอ tradeoff**

LLM มักเลือกตีความแบบเงียบๆ แล้วลุยเลย หลักการนี้บังคับให้คิดให้ชัด:

- **ระบุ assumption ให้ชัด** — ถ้าไม่แน่ใจ ถาม ไม่ใช่เดา
- **เสนอหลายตัวเลือก** — ถ้ามีหลายการตีความ อย่าเลือกเงียบๆ
- **Push back เมื่อจำเป็น** — ถ้ามีทางง่ายกว่า บอกออกมา
- **หยุดเมื่อสับสน** — บอกตรงๆ ว่าอะไรไม่เคลียร์ แล้วถาม

### 2. Simplicity First (ง่ายไว้ก่อน)

**โค้ดน้อยที่สุดที่แก้ปัญหาได้ ไม่เผื่ออนาคต**

ต้านนิสัย over-engineering:

- ไม่เพิ่ม feature ที่ไม่ได้ขอ
- ไม่สร้าง abstraction ให้โค้ดที่ใช้ครั้งเดียว
- ไม่ทำ "flexibility" หรือ "configurability" ถ้าไม่ได้ขอ
- ไม่ใส่ error handling สำหรับเคสที่เกิดไม่ได้
- ถ้า 200 บรรทัดทำได้ใน 50 → เขียนใหม่

**ทดสอบ:** senior engineer จะบอกว่ามันซับซ้อนเกินไหม? ถ้าใช่ → ทำให้ง่ายลง

### 3. Surgical Changes (แก้แบบศัลยแพทย์)

**แตะเฉพาะที่ต้อง เก็บกวาดเฉพาะที่ตัวเองทำ**

ตอนแก้โค้ดที่มีอยู่:

- อย่า "ปรับปรุง" โค้ด/comment/format ข้างเคียง
- อย่า refactor สิ่งที่ไม่ได้พัง
- ตามสไตล์เดิม ถึงแม้คุณจะทำคนละแบบ
- ถ้าเจอ dead code ที่ไม่เกี่ยว → บอก อย่าลบเอง

ตอนการแก้ของคุณสร้าง orphan:

- ลบ import/variable/function ที่ **การแก้ของคุณ** ทำให้ไม่ใช้
- อย่าลบ dead code เก่าๆ ที่มีอยู่ก่อน เว้นแต่ถูกขอ

**ทดสอบ:** ทุกบรรทัดที่แก้ต้องสาวกลับไปหาคำขอของ user ได้

### 4. Goal-Driven Execution (ทำงานตามเป้า)

**กำหนด success criteria แล้ว loop จนผ่าน**

แปลง task แบบสั่งให้เป็นเป้าหมายที่ตรวจวัดได้:

| แทนที่จะ... | เปลี่ยนเป็น... |
|---|---|
| "เพิ่ม validation" | "เขียน test สำหรับ invalid input ก่อน แล้วทำให้ผ่าน" |
| "Fix bug" | "เขียน test ที่ reproduce bug ก่อน แล้วทำให้ผ่าน" |
| "Refactor X" | "ให้ test ผ่านทั้งก่อนและหลัง" |

สำหรับ task หลายขั้น ระบุแผนสั้นๆ:

```
1. [ขั้นที่ 1] → verify: [วิธีตรวจ]
2. [ขั้นที่ 2] → verify: [วิธีตรวจ]
3. [ขั้นที่ 3] → verify: [วิธีตรวจ]
```

Success criteria ที่แข็งแรงทำให้ LLM loop ได้เอง ส่วนเป้าหลวมๆ ("ทำให้มันใช้ได้") ต้องคอย clarify ตลอด

## ติดตั้ง

**ตัวเลือก A: Claude Code Plugin (แนะนำ)**

ใน Claude Code:

```
/plugin marketplace add YINDEEINDY/yindee-skills
/plugin install yindee-skills@yindee-skills
```

ติดตั้ง guideline เป็น plugin → ใช้ได้ทุกโปรเจกต์

**ตัวเลือก B: CLAUDE.md (ราย project)**

โปรเจกต์ใหม่:
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/YINDEEINDY/yindee-skills/main/CLAUDE.md
```

โปรเจกต์เดิม (เพิ่มต่อท้าย):
```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/YINDEEINDY/yindee-skills/main/CLAUDE.md >> CLAUDE.md
```

## ใช้กับ Cursor

repo นี้มี rule ของ Cursor ([`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc)) อยู่แล้ว — เปิดโปรเจกต์ใน Cursor ก็ใช้ guideline เดียวกันได้ ดูรายละเอียดที่ **[CURSOR.md](CURSOR.md)**

## Insight สำคัญ

จาก Andrej:

> "LLM เก่งมากเรื่อง loop จนกว่าจะถึงเป้า... อย่าบอกมันว่าให้ทำอะไร ให้บอก success criteria แล้วดูมันทำ"

หลักการ "Goal-Driven Execution" จับ insight นี้ไว้ — แปลงคำสั่งบังคับให้เป็นเป้าหมาย declarative ที่ verify ได้

## รู้ได้ยังไงว่ามันได้ผล

guideline นี้ใช้ได้ผลถ้าเห็น:

- **diff มีการเปลี่ยนแปลงน้อยลง** — แค่ที่ขอเท่านั้น
- **rewrite น้อยลงเพราะไม่ได้ over-engineer** — โค้ดง่ายตั้งแต่ครั้งแรก
- **คำถามเคลียร์ context มาก่อนเริ่มเขียน** — ไม่ใช่หลังพังแล้ว
- **PR สะอาด เล็ก** — ไม่มี refactor แถมหรือ "ปรับปรุง" ที่ไม่ขอ

## ปรับให้เข้าโปรเจกต์ตัวเอง

guideline นี้ออกแบบให้ merge กับคำสั่งเฉพาะโปรเจกต์ได้ — เพิ่มลงใน `CLAUDE.md` ที่มีอยู่ หรือสร้างใหม่

ตัวอย่างเพิ่มกฎเฉพาะโปรเจกต์:

```markdown
## Project-Specific Guidelines

- ใช้ TypeScript strict mode
- API endpoint ทุกตัวต้องมี test
- ตามรูปแบบ error handling ที่ `src/utils/errors.ts`
```

## หมายเหตุเรื่อง Tradeoff

guideline นี้เอน **ความระมัดระวังมากกว่าความเร็ว** สำหรับงานเล็กๆ (แก้ typo, แก้ 1 บรรทัดที่ชัดเจน) ใช้วิจารณญาณ — ไม่ใช่ทุกการเปลี่ยนต้องเข้มข้น

เป้าหมายคือลด mistake ที่แพงในงานสำคัญ ไม่ใช่ทำงานเล็กให้ช้าลง

## เครดิต

- **Repo ต้นฉบับ:** [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)
- **แรงบันดาลใจ:** [X post ของ Andrej Karpathy](https://x.com/karpathy/status/2015883857489522876)
- **ดูแลโดย:** [@YINDEEINDY](https://github.com/YINDEEINDY)

## License

MIT
