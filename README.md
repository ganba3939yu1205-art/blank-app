# 🗓️ 1日の遊びプラン提案アプリ

## 概要
条件（日付・天気・人数・年齢層・予算など）を入力すると、  
その条件に合った **1日の遊びプランの方針を提案**し、  
**利用履歴を Supabase のデータベースに保存**する Web アプリです。

Streamlit を用いて UI を構築し、  
SQLite などの一時的なデータベースではなく **Supabase（PostgreSQL）** を利用することで、  
アプリ停止後もデータが永続的に保持されます。

---

## 使用技術
- Python
- Streamlit
- Supabase（PostgreSQL）
- GitHub / Streamlit Community Cloud

---

## 主な機能

### ① 遊びプランの提案
以下の条件を入力できます。

- 日付
- 天気（晴れ / 曇り / 雨）
- 都道府県
- 人数
- メンバー構成（友達・カップル・家族）
- 年齢層
- 1人あたりの予算

入力内容に応じて、  
年齢層を考慮した **プラン方針**（例：アクティブ、屋内中心など）を表示します。

---

### ② Supabase へのデータ保存
フォーム送信時に、以下の情報を **Supabase のテーブル `play_plans` に保存**します。

- 日付
- 天気
- 都道府県
- 人数
- メンバー構成
- 年齢層
- 予算
- プラン方針
- 作成日時（created_at）

これにより、アプリを再起動してもデータが保持されます。

---

### ③ 利用履歴の表示
Supabase に保存されたデータを取得し、  
**直近10件の利用履歴を一覧表示**します。

---

## データベース構成

- データベース：Supabase（PostgreSQL）
- テーブル名：`public.play_plans`
- RLS（Row Level Security）：無効化

---

## デプロイ先
本アプリは Streamlit Community Cloud にデプロイしています。

🔗 アプリURL  
https://blank-app-vzpxncxw0o.streamlit.app/

---

## 環境変数（Secrets）
Supabase への接続情報は、Streamlit の secrets 機能を利用して管理しています。

```toml
SUPABASE_URL = "https://xxxx.supabase.co"
SUPABASE_KEY = "your_anon_public_key"

