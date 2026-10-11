# manifest.json
extension for English teaching
{
  "manifest_version": 3,
  "name": "English 3-Forms Verbs Learner",
  "version": "1.0",
  "description": "เรียนรู้คำศัพท์กริยา 3 ช่อง 100 คำ พร้อมฟังเสียงอ่านภาษาอังกฤษ",
  "action": {
    "default_popup": "popup.html"
  },
  "permissions": []
}
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>Verbs 3 ช่อง</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h2>🇬🇧 กริยา 3 ช่อง (100 คำ)</h2>
  <div class="search-box">
    <input type="text" id="searchInput" placeholder="ค้นหาคำศัพท์...">
  </div>
  <div class="table-container">
    <table>
      <thead>
        <tr>
          <th>Base (V1)</th>
          <th>Past (V2)</th>
          <th>Past Participle (V3)</th>
          <th>คำแปล</th>
        </tr>
      </thead>
      <tbody id="verbTableBody">
        <!-- รายการคำศัพท์จะถูกสร้างขึ้นด้วย JS -->
      </tbody>
    </table>
  </div>
  <script src="popup.js"></script>
</body>
</html>
body {
  width: 480px;
  height: 500px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 0;
  padding: 12px;
  background-color: #f8f9fa;
  box-sizing: border-box;
}

h2 {
  margin-top: 0;
  color: #1a73e8;
  font-size: 18px;
  text-align: center;
}

.search-box {
  margin-bottom: 10px;
}

.search-box input {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  box-sizing: border-box;
  font-size: 14px;
}

.table-container {
  max-height: 410px;
  overflow-y: auto;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  background-color: #ffffff;
}

table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  font-size: 13px;
}

th {
  background-color: #f1f3f4;
  position: sticky;
  top: 0;
  padding: 8px;
  color: #3c4043;
  border-bottom: 2px solid #e0e0e0;
}

td {
  padding: 8px;
  border-bottom: 1px solid #f0f0f0;
}

tr:hover {
  background-color: #f5f5f5;
}

.verb-btn {
  background: none;
  border: none;
  color: #1a73e8;
  cursor: pointer;
  font-size: 13px;
  font-weight: 500;
  padding: 2px 4px;
  border-radius: 4px;
  transition: background-color 0.2s;
  display: inline-flex;
  align-items: center;
  gap: 4px;
}

.verb-btn:hover {
  background-color: #e8f0fe;
  text-decoration: underline;
}

.meaning {
  color: #5f6368;
}
// ฐานข้อมูลกริยา 3 ช่อง ทั้งหมด 100 คำ
const verbs = [
  { v1: "abide", v2: "abode", v3: "abode", meaning: "ปฏิบัติตาม" },
  { v1: "arise", v2: "arose", v3: "arisen", meaning: "เกิดขึ้น" },
  { v1: "awake", v2: "awoke", v3: "awoken", meaning: "ตื่น" },
  { v1: "bear", v2: "bore", v3: "borne", meaning: "อดทน/ให้กำเนิด" },
  { v1: "beat", v2: "beat", v3: "beaten", meaning: "ตี/ชนะ" },
  { v1: "become", v2: "became", v3: "become", meaning: "กลายเป็น" },
  { v1: "begin", v2: "began", v3: "begun", meaning: "เริ่ม" },
  { v1: "bend", v2: "bent", v3: "bent", meaning: "งอ" },
  { v1: "bet", v2: "bet", v3: "bet", meaning: "พนัน" },
  { v1: "bid", v2: "bid", v3: "bid", meaning: "ประมูล" },
  { v1: "bind", v2: "bound", v3: "bound", meaning: "ผูก" },
  { v1: "bite", v2: "bit", v3: "bitten", meaning: "กัด" },
  { v1: "bleed", v2: "bled", v3: "bled", meaning: "เลือดออก" },
  { v1: "blow", v2: "blew", v3: "blown", meaning: "เป่า" },
  { v1: "break", v2: "broke", v3: "broken", meaning: "แตก/หัก" },
  { v1: "bring", v2: "brought", v3: "brought", meaning: "นำมา" },
  { v1: "build", v2: "built", v3: "built", meaning: "สร้าง" },
  { v1: "burn", v2: "burnt", v3: "burnt", meaning: "เผา" },
  { v1: "buy", v2: "bought", v3: "bought", meaning: "ซื้อ" },
  { v1: "catch", v2: "caught", v3: "caught", meaning: "จับ" },
  { v1: "choose", v2: "chose", v3: "chosen", meaning: "เลือก" },
  { v1: "cling", v2: "clung", v3: "clung", meaning: "เกาะ" },
  { v1: "come", v2: "came", v3: "come", meaning: "มา" },
  { v1: "cost", v2: "cost", v3: "cost", meaning: "มีราคา" },
  { v1: "creep", v2: "crept", v3: "crept", meaning: "คืบคลาน" },
  { v1: "cut", v2: "cut", v3: "cut", meaning: "ตัด" },
  { v1: "deal", v2: "dealt", v3: "dealt", meaning: "จัดการ" },
  { v1: "dig", v2: "dug", v3: "dug", meaning: "ขุด" },
  { v1: "do", v2: "did", v3: "done", meaning: "ทำ" },
  { v1: "draw", v2: "drew", v3: "drawn", meaning: "วาด/ดึง" },
  { v1: "dream", v2: "dreamt", v3: "dreamt", meaning: "ฝัน" },
  { v1: "drink", v2: "drank", v3: "drunk", meaning: "ดื่ม" },
  { v1: "drive", v2: "drove", v3: "driven", meaning: "ขับรถ" },
  { v1: "eat", v2: "ate", v3: "eaten", meaning: "กิน" },
  { v1: "fall", v2: "fell", v3: "fallen", meaning: "ตก/ล้ม" },
  { v1: "feed", v2: "fed", v3: "fed", meaning: "ให้อาหาร" },
  { v1: "feel", v2: "felt", v3: "felt", meaning: "รู้สึก" },
  { v1: "fight", v2: "fought", v3: "fought", meaning: "ต่อสู้" },
  { v1: "find", v2: "found", v3: "found", meaning: "พบ" },
  { v1: "fly", v2: "flew", v3: "flown", meaning: "บิน" },
  { v1: "forbid", v2: "forbade", v3: "forbidden", meaning: "ห้าม" },
  { v1: "forget", v2: "forgot", v3: "forgotten", meaning: "ลืม" },
  { v1: "forgive", v2: "forgave", v3: "forgiven", meaning: "ให้อภัย" },
  { v1: "freeze", v2: "froze", v3: "frozen", meaning: "แช่แข็ง" },
  { v1: "get", v2: "got", v3: "gotten", meaning: "ได้รับ" },
  { v1: "give", v2: "gave", v3: "given", meaning: "ให้" },
  { v1: "go", v2: "went", v3: "gone", meaning: "ไป" },
  { v1: "grow", v2: "grew", v3: "grown", meaning: "เติบโต" },
  { v1: "hang", v2: "hung", v3: "hung", meaning: "แขวน" },
  { v1: "have", v2: "had", v3: "had", meaning: "มี" },
  { v1: "hear", v2: "heard", v3: "heard", meaning: "ได้ยิน" },
  { v1: "hide", v2: "hid", v3: "hidden", meaning: "ซ่อน" },
  { v1: "hit", v2: "hit", v3: "hit", meaning: "ชน/ตี" },
  { v1: "hold", v2: "held", v3: "held", meaning: "ถือ/จัดงาน" },
  { v1: "hurt", v2: "hurt", v3: "hurt", meaning: "เจ็บ/ทำร้าย" },
  { v1: "keep", v2: "kept", v3: "kept", meaning: "เก็บ/รักษา" },
  { v1: "know", v2: "knew", v3: "known", meaning: "รู้" },
  { v1: "lay", v2: "laid", v3: "laid", meaning: "วาง/วางไข่" },
  { v1: "lead", v2: "led", v3: "led", meaning: "นำ" },
  { v1: "lean", v2: "leant", v3: "leant", meaning: "พิง" },
  { v1: "leap", v2: "leapt", v3: "leapt", meaning: "กระโดด" },
  { v1: "learn", v2: "learnt", v3: "learnt", meaning: "เรียนรู้" },
  { v1: "leave", v2: "left", v3: "left", meaning: "ออกจาก/ทิ้ง" },
  { v1: "lend", v2: "lent", v3: "lent", meaning: "ให้ยืม" },
  { v1: "let", v2: "let", v3: "let", meaning: "อนุญาต" },
  { v1: "lie", v2: "lay", v3: "lain", meaning: "นอนลง" },
  { v1: "light", v2: "lit", v3: "lit", meaning: "จุดไฟ" },
  { v1: "lose", v2: "lost", v3: "lost", meaning: "ทำหาย/แพ้" },
  { v1: "make", v2: "made", v3: "made", meaning: "ทำ/สร้าง" },
  { v1: "mean", v2: "meant", v3: "meant", meaning: "หมายถึง" },
  { v1: "meet", v2: "met", v3: "met", meaning: "พบ" },
  { v1: "pay", v2: "paid", v3: "paid", meaning: "จ่าย" },
  { v1: "put", v2: "put", v3: "put", meaning: "วาง" },
  { v1: "read", v2: "read", v3: "read", meaning: "อ่าน" },
  { v1: "ride", v2: "rode", v3: "ridden", meaning: "ขี่" },
  { v1: "ring", v2: "rang", v3: "rung", meaning: "สั่นกระดิ่ง" },
  { v1: "rise", v2: "rose", v3: "risen", meaning: "ขึ้น/เพิ่ม" },
  { v1: "run", v2: "ran", v3: "run", meaning: "วิ่ง" },
  { v1: "say", v2: "said", v3: "said", meaning: "พูด" },
  { v1: "see", v2: "saw", v3: "seen", meaning: "เห็น" },
  { v1: "seek", v2: "sought", v3: "sought", meaning: "ค้นหา" },
  { v1: "sell", v2: "sold", v3: "sold", meaning: "ขาย" },
  { v1: "send", v2: "sent", v3: "sent", meaning: "ส่ง" },
  { v1: "set", v2: "set", v3: "set", meaning: "จัด/ตั้ง" },
  { v1: "shake", v2: "shook", v3: "shaken", meaning: "เขย่า" },
  { v1: "shine", v2: "shone", v3: "shone", meaning: "ส่องสว่าง" },
  { v1: "shoot", v2: "shot", v3: "shot", meaning: "ยิง" },
  { v1: "show", v2: "showed", v3: "shown", meaning: "แสดง" },
  { v1: "shut", v2: "shut", v3: "shut", meaning: "ปิด" },
  { v1: "sing", v2: "sang", v3: "sung", meaning: "ร้องเพลง" },
  { v1: "sink", v2: "sank", v3: "sunk", meaning: "จม" },
  { v1: "sit", v2: "sat", v3: "sat", meaning: "นั่ง" },
  { v1: "sleep", v2: "slept", v3: "slept", meaning: "นอน" },
  { v1: "speak", v2: "spoke", v3: "spoken", meaning: "พูด" },
  { v1: "spend", v2: "spent", v3: "spent", meaning: "ใช้จ่าย/ใช้เวลา" },
  { v1: "stand", v2: "stood", v3: "stood", meaning: "ยืน" },
  { v1: "swim", v2: "swam", v3: "swum", meaning: "ว่ายน้ำ" },
  { v1: "take", v2: "took", v3: "taken", meaning: "เอาไป/นำ" },
  { v1: "teach", v2: "taught", v3: "taught", meaning: "สอน" },
  { v1: "write", v2: "wrote", v3: "written", meaning: "เขียน" }
];

// ฟังก์ชันเล่นเสียงอ่านภาษาอังกฤษ
function speakWord(word) {
  window.speechSynthesis.cancel(); // ยกเลิกเสียงเดิมก่อนถ้ามี
  const utterance = new SpeechSynthesisUtterance(word);
  utterance.lang = 'en-US'; // กำหนดสำเนียงภาษาอังกฤษ
  utterance.rate = 0.9;     // ความเร็วเสียงอ่าน
  window.speechSynthesis.speak(utterance);
}

// ฟังก์ชันสำหรับ Render ตาราง
function renderTable(data) {
  const tbody = document.getElementById('verbTableBody');
  tbody.innerHTML = '';

  data.forEach(item => {
    const tr = document.createElement('tr');

    tr.innerHTML = `
      <td><button class="verb-btn" data-word="${item.v1}">🔊 ${item.v1}</button></td>
      <td><button class="verb-btn" data-word="${item.v2}">🔊 ${item.v2}</button></td>
      <td><button class="verb-btn" data-word="${item.v3}">🔊 ${item.v3}</button></td>
      <td class="meaning">${item.meaning}</td>
    `;

    tbody.appendChild(tr);
  });

  // ผูก Event Listeners สำหรับการกดเล่นเสียง
  document.querySelectorAll('.verb-btn').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const word = e.currentTarget.getAttribute('data-word');
      speakWord(word);
    });
  });
}

// ระบบค้นหาคำศัพท์
document.getElementById('searchInput').addEventListener('input', (e) => {
  const searchTerm = e.target.value.toLowerCase();
  const filtered = verbs.filter(item => 
    item.v1.toLowerCase().includes(searchTerm) ||
    item.v2.toLowerCase().includes(searchTerm) ||
    item.v3.toLowerCase().includes(searchTerm) ||
    item.meaning.includes(searchTerm)
  );
  renderTable(filtered);
});

// Render ตารางครั้งแรกเมื่อเปิด Extension
document.addEventListener('DOMContentLoaded', () => {
  renderTable(verbs);
});
