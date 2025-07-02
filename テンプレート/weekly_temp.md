---
create_dt: <% tp.file.creation_date("YYYY-MM-DD") %>
<%*
// 今日の日付を取得
const now = new Date();

// 今週の月曜日の日付を計算（0=日曜日, 1=月曜日, ..., 6=土曜日）
const dayOfWeek = now.getDay();
const diff = dayOfWeek === 0 ? -6 : 1 - dayOfWeek; // 日曜日なら-6、それ以外なら1-曜日
const monday = new Date(now);
monday.setDate(now.getDate() + diff);

// 今週の日曜日の日付を計算
const sunday = new Date(monday);
sunday.setDate(monday.getDate() + 6);

// フォーマット関数
function formatDate(date) {
  const year = date.getFullYear();
  const month = String(date.getMonth() + 1).padStart(2, '0');
  const day = String(date.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
}

// 週番号を計算
function getWeekNumber(date) {
  const firstDayOfYear = new Date(date.getFullYear(), 0, 1);
  const pastDaysOfYear = (date - firstDayOfYear) / 86400000;
  return Math.ceil((pastDaysOfYear + firstDayOfYear.getDay() + 1) / 7);
}

const weekNum = getWeekNumber(monday);
const startDateStr = formatDate(monday);
const endDateStr = formatDate(sunday);
-%>
start_date: <% startDateStr %>
end_date: <% endDateStr %>
week_of: <% monday.getFullYear() %>-W<% String(weekNum).padStart(2, '0') %>
tags: [weekly, plan]
---

# 🗓 Weekly Planning
*<% startDateStr %> 〜 <% endDateStr %>*

## 🎯 Weekly Goals
*今週達成したい重要な目標*

### 💼 仕事
- 

### 📚 学習
- 

### 💪 健康
- 

### 🏠 プライベート
- 

---

## 📅 Daily Schedule
*曜日別の具体的なタスク*

<%*
// 日本の曜日（月曜始まり）
const days = ["月", "火", "水", "木", "金", "土", "日"];

// 各曜日の日付とタスクを出力
for (let i = 0; i < 7; i++) {
  const current = new Date(monday);
  current.setDate(monday.getDate() + i);
  const dateStr = formatDate(current);
  tR += `### ${days[i]}曜日（${dateStr}）\n- [ ] \n- [ ] \n\n`;
}
%>

---

## ✅ Weekly Tasks
*今週の全体的なやることリスト*

- [ ] 
- [ ] 
- [ ] 

---

## 🔄 Weekly Review
*一週間の振り返りと改善*

### ✨ 良かったこと
*今週うまくいったこと・成果*

- 

### 🔍 課題・改善点
*今週の課題や次週への改善点*

- 

### 💡 学んだこと・気づき
*新しい発見や重要な気づき*

- 

---

## 📖 Daily TODO Overview
*一週間のTODO一覧*

<%*
// 一週間分のTODOセクションリンクを生成
const weekDays = ["月", "火", "水", "木", "金", "土", "日"];
let dailyTodoLinks = [];

for (let i = 0; i < 7; i++) {
  const current = new Date(monday);
  current.setDate(monday.getDate() + i);
  const dateStr = formatDate(current);
  const dayName = weekDays[i];
  const fileName = `Daily/${dateStr} (${dayName})`;
  
      // TODOセクションのみを参照（実際のヘッダー名に合わせる）
    dailyTodoLinks.push(`### ${dayName}曜日(${dateStr})\n![[${fileName}#✅ TODO]]`);
}

tR += dailyTodoLinks.join("\n\n");
%> 

---

## 🚀 Next Week Preparation
*来週に向けた準備とメモ*

### 🎯 来週の重点項目
- 

### 📝 引き継ぎ事項
- 

### 🔖 メモ・補足
- 