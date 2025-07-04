<%*
const file = tp.file.find_tfile(tp.file.title);
if (!file) {
  new Notice('ファイルが見つかりません');
  tR = '';
  return;
}
let content = await app.vault.read(file);
const regex = /(https?:\/\/(?:twitter\.com|x\.com)\/[^\s\)]+)/g;
content = content.replace(regex, (match) => `![]("${match}")`);
await app.vault.modify(file, content);
new Notice('Xリンクを埋め込み形式に変換しました');
%>
<%*
// テンプレート自体をファイルに挿入しない
tR = "";
%> 