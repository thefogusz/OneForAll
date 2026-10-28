# ติดตั้ง OneForAll ให้ตรง agent

OneForAll ใช้ไฟล์ `SKILL.md` เดียวกัน สิ่งที่ต่างคือที่วางไฟล์ วิธีเรียก และไฟล์คำสั่งที่ agent อ่านตอนเริ่มงาน

## เลือกบ้านให้สกิลกลาง

| Agent | สกิลส่วนตัว | เรียกตรง ๆ | สกิลโปรเจกต์ใหม่ |
| --- | --- | --- | --- |
| Codex | `~/.agents/skills/oneforall/` | `$oneforall` | `<project>/.agents/skills/<name>/` |
| Claude Code | `~/.claude/skills/oneforall/` | `/oneforall` | `<project>/.claude/skills/<name>/` |
| Agent อื่น | ตำแหน่งที่ agent นั้นรองรับ | คำสั่งของ agent หรือสั่งให้อ่าน `SKILL.md` | ตรวจจากเอกสารของ agent ก่อน |

`~` คือโฟลเดอร์ผู้ใช้ของคนติดตั้ง และ `<project>` คือรากโปรเจกต์ที่ทำงานอยู่ ไม่ใช่ชื่อโฟลเดอร์ให้พิมพ์ตาม บน PowerShell หา home ได้ด้วย `$HOME`; บน macOS/Linux ใช้ `echo "$HOME"`

บางโปรไฟล์ Codex มีสกิลที่ `$CODEX_HOME/skills` หรือ `~/.codex/skills` อยู่แล้ว ให้ใช้ตำแหน่งที่ Codex พบจริง ไม่ต้องติดตั้งชื่อเดียวกันสองที่

## ผ่าน Skills CLI

ต้องมี Node.js/npm และ Git:

```sh
npx skills add thefogusz/OneForAll --skill oneforall --agent codex --global
```

เปลี่ยน `codex` เป็น `claude-code` เพื่อใช้กับ Claude Code หรือเลือกทั้งคู่:

```sh
npx skills add thefogusz/OneForAll --skill oneforall --agent codex claude-code --global
```

คำสั่งนี้ติดตั้งเฉพาะสกิลชื่อ `oneforall` ตัว CLI อาจสร้าง symlink ตามวิธีที่เลือก ใช้ `--copy` ถ้าต้องการคัดลอกไฟล์แทน และเอา `--global` ออกเมื่อต้องการติดตั้งสกิลกลางเฉพาะโปรเจกต์

Agent อื่นให้เลือก identifier ที่ [Skills CLI รองรับ](https://github.com/vercel-labs/skills/blob/main/docs/agents.md) ความสำเร็จของคำสั่งติดตั้งหมายถึงวางไฟล์ได้ ไม่ได้แปลว่าทดลองพฤติกรรมใน agent นั้นแล้ว

## ติดตั้งด้วยไฟล์

Clone หรือดาวน์โหลด repo นี้ แล้วคัดลอก **ทั้งโฟลเดอร์** `skills/oneforall/` ไปยังโฟลเดอร์สกิลส่วนตัวในตารางด้านบน ต้องมี `SKILL.md`, `references/`, `assets/` และ `agents/` อยู่ด้วยกัน

ถ้ามี `oneforall` เดิมแล้ว ให้สำรองหรือเปรียบเทียบก่อนอัปเดต หลีกเลี่ยงการติดตั้งชื่อเดียวกันซ้ำ เปิด session ใหม่ถ้ายังไม่พบสกิล

## เปิดโหมดอัตโนมัติ

ทำครั้งเดียวในไฟล์คำสั่ง **ส่วนตัว** ถ้าต้องการให้ใช้ทุกโปรเจกต์:

- **Codex:** ไฟล์คำสั่งที่มีผลใต้ `$CODEX_HOME` ซึ่งปกติคือ `~/.codex` ใช้ `AGENTS.md` เว้นแต่มี `AGENTS.override.md` ที่ไม่ว่างอยู่แล้ว ให้เพิ่มในไฟล์ที่มีผลจริง
- **Claude Code:** `~/.claude/CLAUDE.md`
- **เฉพาะโปรเจกต์:** เพิ่มใน `AGENTS.md` ของโปรเจกต์สำหรับ Codex หรือ `CLAUDE.md` สำหรับ Claude Code

เพิ่มข้อความต่อไปนี้โดย **เก็บเนื้อหาเดิมไว้** ไม่ต้องแทนที่ไฟล์ทั้งก้อน:

```markdown
<!-- oneforall:start -->
## OneForAll

Before substantive project work, quietly assess whether current abilities
and project guidance are sufficient. If they are, do the task directly.
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

ถ้า agent ยังหาตัวสกิลไม่เจอ ให้เพิ่มพาธ `SKILL.md` ที่ติดตั้งจริงของ **เครื่องคุณเอง** ในประโยคแรก แล้วเริ่ม session ใหม่ สำหรับ agent อื่น ให้ใช้กลไกคำสั่งประจำที่เอกสารของ agent นั้นรองรับ ไม่เดาว่ามันอ่าน `AGENTS.md` หรือ `CLAUDE.md` เสมอ

## ลองใช้และดูว่าเกิดอะไร

```text
Codex:       $oneforall ตรวจและแก้ export ในโปรเจกต์นี้
Claude Code: /oneforall ตรวจและแก้ export ในโปรเจกต์นี้
```

ควรได้ผลของงานก่อน ถ้าความสามารถและแนวทางเดิมเพียงพอจะทำงานตรง ๆ โดยไม่มีการค้นหรือไฟล์สกิลเพิ่ม เมื่อพบจุดที่ควรรีเสิร์ชจึงค้นข้อมูลและสกิลแยกกัน แล้วเก็บเฉพาะวิธีที่มีประโยชน์ใช้ซ้ำ ไม่ต้องเห็นรายงานสกิลทุกครั้งเพื่อพิสูจน์ว่ามันทำงานอยู่

ถ้าต้องการสำรวจโดยตรง ให้สั่ง “หาสกิลที่ช่วยลดงานซ้ำของโปรเจกต์นี้ พร้อมเหตุผล” การขอค้นชัด ๆ จะเปิดขั้นตอนรีเสิร์ชแม้งานปัจจุบันทำได้อยู่แล้ว

สลับจาก Codex ไป Claude Code ในโปรเจกต์เดียวกันแล้ว OneForAll จะอ่านสกิลเดิมจากอีกโฟลเดอร์ได้โดยตรง การค้นพบอัตโนมัติของแต่ละ host ยังเป็นไปตามกติกาของ host นั้น ไม่มีการสร้างสำเนาหรือ symlink เพิ่มเอง

## พักหรือถอดออก

- **งานเดียว:** บอกว่า “งานนี้ไม่ใช้ OneForAll” หรือ “งานนี้อ่านอย่างเดียว ไม่แก้ไฟล์”
- **ปิดโหมดอัตโนมัติ:** ลบเฉพาะบล็อกระหว่าง `oneforall:start` กับ `oneforall:end` จากไฟล์ที่เพิ่มไว้
- **ถอดสกิลกลาง:** หลังปิดโหมดอัตโนมัติแล้ว ถอนด้วยตัวติดตั้งที่ใช้ หรือเอาเฉพาะโฟลเดอร์สกิลกลางออก สกิลที่สร้างในโปรเจกต์ยังคงอยู่ให้ตรวจและใช้ต่อ

## ขอบเขตการรองรับ

รูปแบบไฟล์รองรับ Agent Skills; ขั้นตอนต้องมีเครื่องมืออ่าน/เขียนไฟล์และค้นเว็บ หากเป็นแชตที่ไม่มี filesystem ถาวร มันทำได้เพียงร่างไฟล์ให้ ไม่สามารถอ้างว่าสกิลถูกเก็บถาวรแล้ว ส่วนโหมด cloud อาจใช้พื้นที่และการติดตั้งคนละชุดกับเครื่องของคุณ

ตรวจเอกสารเมื่อ 2026-10-28: [Codex skills](https://learn.chatgpt.com/docs/build-skills), [Codex instructions](https://learn.chatgpt.com/docs/agent-configuration/agents-md), [Claude Code skills](https://code.claude.com/docs/en/skills), [Claude Code memory](https://code.claude.com/docs/en/memory), [Skills CLI](https://github.com/vercel-labs/skills/blob/main/docs/commands.md)
