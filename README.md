# Chatwork メンション短縮ブックマークレット「一列くん」v2

Chatworkのメッセージ入力欄で、複数行に分かれた `[To:uid]名前` や `[返信 aid=... to=...]名前` を、ワンクリックで1行にまとめて短縮できるブックマークレットです。  
新バージョンでは、**「カーソルで選択した範囲だけ」短縮することが可能**になりました。

---

## 特徴

- 複数の `[To:uid]名前`、`[返信 aid=... to=...]名前` を、1行 `[To:uid][To:uid2][返信 aid=... to=...][...]` 形式に自動変換
- **選択した範囲だけ短縮できる**（他の部分は変更されません）
- 名前部分は省略され、メンション・返信タグのみが連結されます
- Chatworkの入力欄（PCブラウザ専用、`<textarea id="_chatText">`）で動作
- ブラウザのブックマークバーからワンクリックで実行
- 外部サーバーやAPIにはアクセスせず、ローカル上のみで動作

## 使い方

1. 下記のコードをコピーし、ブラウザのブックマークとして登録してください
2. Chatworkのメッセージ入力欄で、**短縮したい範囲だけをドラッグ選択**してください（範囲には複数行を含めてもOK）
3. ブックマークをクリックすると、**選択範囲が自動的に短縮**されます（他の部分はそのまま残ります）

## コード例

```javascript
// ブックマークレットとして登録してください（必ず1行で）
javascript:(()=>{const ta=document.getElementById('_chatText');if(!ta){alert('入力欄が見つかりません');return;}const val=ta.value;const selStart=ta.selectionStart;const selEnd=ta.selectionEnd;if(selStart===selEnd){alert('短縮したい範囲を選択してください');return;}const before=val.slice(0,selStart);const selected=val.slice(selStart,selEnd);const after=val.slice(selEnd);const tags=Array.from(selected.matchAll(/\[To:\d+\]|\[返信\s+aid=\d+\s+to=[^\]]+\]/g)).map(m=>m[0]);const uniqueTags=[...new Set(tags)].join('');const newVal=before+uniqueTags+after;ta.value=newVal;const newSel=before.length+uniqueTags.length;ta.focus();ta.setSelectionRange(newSel,newSel);ta.dispatchEvent(new Event('input',{bubbles:true}));})();
```

## 動作イメージ
### 実行前
```
[To:1] name1
[To:2] name2
[To:3] name3
```

たとえば [To:2] name2\n[To:3] name3 の部分をドラッグ選択
### 実行後
```
[To:1] name1
[To:2][To:3]
```

## 注意事項
- 本ツールはChatwork公式の提供・推奨するものではありません。
- Chatworkのサービス仕様変更などにより、動作しなくなる場合があります。
- 本ツールの利用によるいかなる不利益や損害についても、作者は一切の責任を負いません。
- ご自身の責任においてご利用ください。
- 不具合・改善案などはIssue等でご報告いただけると幸いです。

---
## v1ログ
Chatworkのメッセージ入力欄で、複数行に分かれた `[To:uid]名前` や `[返信 aid=... to=...]名前` を、ワンクリックで1行にまとめて短縮するブックマークレットです。

## 特徴

- 複数の `[To:uid]名前`、`[返信 aid=... to=...]名前` を、1行 `[To:uid][To:uid2][返信 aid=... to=...][...]` 形式に自動変換
- 名前部分は省略され、メンション・返信タグのみが連結されます
- Chatworkの入力欄（PCブラウザ専用）で動作
- ブラウザのブックマークバーからワンクリックで実行
- 外部サーバーやAPIにはアクセスせず、ローカル上のみで動作

### 使い方

1. このリポジトリのコードを1行コピーし、ブラウザのブックマークとして登録します
2. Chatworkのメッセージ入力欄に複数メンション・複数返信が並んだ状態で、ブックマークをクリックしてください
3. 自動的に1行に短縮されます

### コード
ブックマークレットとして登録してください
```javascript
javascript:(()=>{const ta=document.getElementById('_chatText');if(!ta){alert('入力欄が見つかりません');return;}const lines=ta.value.split('\n');const tags=lines.map(line=>{let m=line.match(/\[To:\d+\]/);if(m)return m[0];m=line.match(/\[返信\s+aid=\d+\s+to=[^\]]+\]/);return m?m[0]:'';}).filter(Boolean);const result=[...new Set(tags)].join('');ta.value=result;ta.focus();ta.setSelectionRange(result.length,result.length);ta.dispatchEvent(new Event('input',{bubbles:true}));})();
```
