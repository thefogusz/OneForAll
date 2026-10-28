# คู่มือติดตั้ง

[English](setup.md) | [ภาษาไทย](setup.th.md)

เลือกติดตั้งสำหรับ Codex หรือ Claude Code ตามที่ใช้งาน ทั้งสองใช้สกิลชุดเดียวกัน แต่เก็บไฟล์และเรียกใช้ต่างกัน

## ติดตั้งผ่าน Skills CLI

ต้องมี Node.js/npm และ Git สำหรับ Codex ใช้คำสั่งนี้:

```sh
npx skills add thefogusz/OneForAll --skill oneforall head-chef --agent codex --global
```

สำหรับ Claude Code เปลี่ยน `codex` เป็น `claude-code` หรือติดตั้งให้ทั้งคู่ในครั้งเดียว:

```sh
npx skills add thefogusz/OneForAll --skill oneforall head-chef --agent codex claude-code --global
```

`--skill oneforall head-chef` ติดตั้งทั้งสองสกิลใน repo นี้ ถ้าต้องการตัวเดียวให้ใส่แค่ `oneforall` หรือ `head-chef` ส่วน `--global` ทำให้ใช้ได้ในทุกโปรเจกต์ หากต้องการติดตั้งเฉพาะโปรเจกต์ ให้รันจากโฟลเดอร์นั้นและเอา `--global` ออก

ตัวติดตั้งอาจใช้ symlink เพื่อให้หลาย agent อ่านไฟล์ชุดเดียวกัน หากต้องการคัดลอกไฟล์แทน ให้เพิ่ม `--copy`

สำหรับ agent อื่น ให้เลือกชื่อจาก [รายการที่ Skills CLI รองรับ](https://github.com/vercel-labs/skills/blob/main/docs/agents.md) การติดตั้งสำเร็จยืนยันว่าไฟล์ถูกวางแล้ว ส่วนผลทดสอบการใช้งานแต่ละ agent ดูได้ที่ [ผลทดสอบ](verification.th.md)

## ตำแหน่งไฟล์และคำสั่งเรียก

| Agent | สกิลส่วนตัว | เรียกใช้ | สกิลโปรเจกต์ที่สร้างใหม่ |
| --- | --- | --- | --- |
| Codex | `~/.agents/skills/oneforall/` | `$oneforall` | `<project>/.agents/skills/<name>/` |
| Claude Code | `~/.claude/skills/oneforall/` | `/oneforall` | `<project>/.claude/skills/<name>/` |
| Agent อื่น | ใช้ตำแหน่งตามเอกสารของ agent | ใช้คำสั่งที่รองรับ หรือสั่งให้อ่าน `SKILL.md` | ตรวจตำแหน่งของ agent นั้นก่อน |

`~` หมายถึงโฟลเดอร์ผู้ใช้ และ `<project>` หมายถึงโฟลเดอร์หลักของโปรเจกต์ ให้แทนด้วยตำแหน่งบนเครื่องของคุณ บน PowerShell ดูโฟลเดอร์ผู้ใช้ได้ด้วย `$HOME`; บน macOS/Linux ใช้ `echo "$HOME"`

Codex บางชุดติดตั้งพบสกิลจาก `$CODEX_HOME/skills` หรือ `~/.codex/skills` ถ้ามี OneForAll ที่ใช้งานได้อยู่แล้ว ให้ใช้ตำแหน่งเดิมเพื่อไม่ให้มีหลายสำเนา

## ติดตั้งด้วยไฟล์

วิธีนี้ไม่ต้องใช้ Node.js ให้ clone หรือดาวน์โหลด repo แล้วคัดลอกทั้งโฟลเดอร์ `skills/oneforall/` ไปยังตำแหน่งสกิลส่วนตัวของ agent ตามตารางด้านบน โดยเก็บ `SKILL.md`, `references/`, `assets/` และ `agents/` ไว้ด้วยกัน

ถ้าเคยติดตั้งไว้แล้ว ให้สำรองหรือเปรียบเทียบกับชุดเดิมก่อนแทนที่ จากนั้นเปิด session ใหม่หากสกิลยังไม่แสดง

## เปิดโหมดอัตโนมัติ

หลังติดตั้ง คุณสามารถเรียก OneForAll เองได้ทันที หากต้องการให้ agent พิจารณาใช้เองเวลาเริ่มงาน ให้เพิ่มคำสั่งด้านล่างในไฟล์คำสั่งส่วนตัว:

- **Codex:** `AGENTS.md` ใต้ `$CODEX_HOME` ซึ่งปกติคือ `~/.codex` ถ้ามี `AGENTS.override.md` ที่ไม่ว่าง ให้เพิ่มในไฟล์นั้นเพราะมันมีผลแทน `AGENTS.md`
- **Claude Code:** `~/.claude/CLAUDE.md`
- **ใช้เฉพาะโปรเจกต์:** เพิ่มใน `AGENTS.md` ของโปรเจกต์สำหรับ Codex หรือ `CLAUDE.md` สำหรับ Claude Code

เพิ่มบล็อกนี้ต่อจากเนื้อหาเดิม โดยเก็บคำสั่งอื่นในไฟล์ไว้:

```markdown
<!-- oneforall:start -->
## OneForAll

On substantive project work, follow the work principles in the installed
oneforall skill: understand first, smallest complete change, root cause
before fixes, evidence before claims. Quietly assess whether current
abilities and project guidance are sufficient. If they are, do the task
directly.
Follow the installed oneforall skill when a concrete gap or a useful
specialist workflow warrants discovery, or when I request skill research.
On that route, research technical information and skills separately.
Respect explicit offline, read-only, or disabled-OneForAll requests.

I authorize automatic creation and maintenance of OneForAll-managed
skills inside the current project's native skill directory. Inspect
both .agents/skills and .claude/skills before creating anything, reuse
matching skills in place, and follow project and branch instructions.

Missing third-party skills are optional recommendations. Do not install
them without my selection or wait for that selection to continue the
main task. Keep project knowledge inside its own checkout. Surface skills
only when they materially help; skip skill chatter for routine work,
casual conversation, and status updates. Reuse suitable existing skills
without another search or an unnecessary rewrite.
<!-- oneforall:end -->
```

บล็อกนี้อนุญาตให้ OneForAll สร้างและปรับสกิลที่มันดูแลภายในโปรเจกต์ ส่วนการติดตั้งสกิลภายนอกที่แนะนำยังต้องรอให้คุณเลือก

ถ้า agent หาสกิลไม่เจอ ให้ระบุพาธของ `SKILL.md` ที่ติดตั้งไว้เพิ่มในบล็อก แล้วเริ่ม session ใหม่ สำหรับ agent อื่น ให้ใช้ไฟล์คำสั่งประจำตามเอกสารของ agent นั้น

## ทดลองใช้

```text
Codex:       $oneforall ตรวจและแก้ export ในโปรเจกต์นี้
Claude Code: /oneforall ตรวจและแก้ export ในโปรเจกต์นี้
```

ถ้า agent ทำงานนี้ได้ด้วยความสามารถและแนวทางเดิม ก็ทำต่อได้เลย อาจไม่มีการค้นหรือไฟล์สกิลใหม่เกิดขึ้น เมื่อมีเหตุให้ค้นเพิ่มจึงค้นข้อมูลและสกิลแยกกัน แล้วเก็บวิธีที่มีประโยชน์ไว้ในโปรเจกต์

หากต้องการค้นสกิลโดยตรง ลองสั่งว่า “หาสกิลที่ช่วยลดงานซ้ำของโปรเจกต์นี้ พร้อมเหตุผล”

เมื่อสลับระหว่าง Codex กับ Claude Code ในโปรเจกต์เดียวกัน OneForAll จะอ่านสกิลเดิมจากอีกโฟลเดอร์ได้โดยตรง จึงไม่ต้องสร้างสำเนาใหม่ แต่การค้นพบสกิลอัตโนมัติของแต่ละ agent ยังขึ้นกับตำแหน่งที่ agent นั้นรองรับ

## ปิดใช้งานหรือถอนการติดตั้ง

- **ปิดสำหรับงานเดียว:** บอกว่า “งานนี้ไม่ใช้ OneForAll” หรือระบุขอบเขต เช่น “งานนี้อ่านอย่างเดียว ไม่แก้ไฟล์”
- **ปิดโหมดอัตโนมัติ:** ลบเฉพาะบล็อกตั้งแต่ `oneforall:start` ถึง `oneforall:end` ออกจากไฟล์ที่เพิ่มไว้
- **ถอนการติดตั้ง:** ปิดโหมดอัตโนมัติก่อน แล้วถอนด้วยตัวติดตั้งที่ใช้ หรือเอาโฟลเดอร์สกิลกลางออก สกิลที่สร้างไว้ในโปรเจกต์ยังอยู่ให้ใช้ต่อได้

## ใช้กับสภาพแวดล้อมอื่น

OneForAll ใช้รูปแบบ Agent Skills และต้องอาศัยเครื่องมืออ่าน/เขียนไฟล์ รวมถึงเครื่องมือเว็บเมื่อต้องรีเสิร์ช หากแชตนั้นไม่มีพื้นที่เก็บไฟล์ถาวร จะทำได้เพียงร่างสกิลให้คุณนำไปบันทึกเอง ส่วนการใช้บน cloud อาจต้องติดตั้งแยกจากเครื่องของคุณ

ตำแหน่งไฟล์และคำสั่งในคู่มือนี้ตรวจจากเอกสารเมื่อ 2026-10-28: [Codex skills](https://learn.chatgpt.com/docs/build-skills), [Codex instructions](https://learn.chatgpt.com/docs/agent-configuration/agents-md), [Claude Code skills](https://code.claude.com/docs/en/skills), [Claude Code memory](https://code.claude.com/docs/en/memory), [Skills CLI](https://github.com/vercel-labs/skills/blob/main/docs/commands.md)
