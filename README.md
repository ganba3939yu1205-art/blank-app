import streamlit as st
import datetime

# =====================
# 基本設定
# =====================
st.set_page_config(
    page_title="1日の遊びプラン提案アプリ",
    page_icon="🗓️",
    layout="centered"
)

st.title("🗓️ 1日の遊びプラン提案アプリ")
st.write("条件を入力すると、あなたに合った1日の遊びプランを提案します。")

# =====================
# 入力フォーム
# =====================
with st.form("plan_form"):
    date = st.date_input("日付")
    weather = st.selectbox("天気", ["晴れ", "曇り", "雨"])
    pref = st.selectbox("都道府県", ["東京", "大阪"])
    people = st.number_input("人数", min_value=1, max_value=10, value=2)

    member = st.selectbox(
        "メンバー構成",
        ["友達", "カップル", "家族"]
    )

    age_group = st.selectbox(
        "年齢層",
        ["10代", "20代", "30代", "40代以上", "子ども含む"]
    )

    budget = st.slider(
        "1人あたりの予算（円）",
        min_value=1000,
        max_value=30000,
        step=1000,
        value=5000
    )

    submit = st.form_submit_button("プランを提案")

# =====================
# プラン生成ロジック
# =====================
def get_budget_level(budget):
    if budget < 3000:
        return "low"
    elif budget < 10000:
        return "mid"
    else:
        return "high"


def generate_plan(weather, member, age_group, budget_level):
    # 屋内・屋外判定
    place_type = "indoor" if weather == "雨" else "outdoor"

    # 基本方針
    if age_group == "子ども含む":
        policy = "安全・短距離・屋内多め"
    elif age_group in ["10代", "20代"]:
        policy = "アクティブ・体験重視"
    elif age_group == "40代以上":
        policy = "落ち着いた・移動少なめ"
    else:
        policy = "バランス型"

    # タイムテーブル
    if place_type == "indoor":
        plan = [
            "午前：屋内施設でゆったり過ごす",
            "昼：アクセスの良いレストランで食事",
            "午後：体験型・展示系スポット",
            "夜：早めに解散またはカフェ"
        ]
    else:
        plan = [
            "午前：屋外スポットを散策",
            "昼：人気エリアでランチ",
            "午後：アクティビティ体験",
            "夜：夜景やご飯を楽しむ"
        ]

    # 予算調整
    if budget_level == "low":
        plan = [p.replace("体験", "無料スポット") for p in plan]
    elif budget_level == "high":
        plan[2] = "午後：特別な体験型レジャー"

    return plan, policy


# =====================
# 出力
# =====================
if submit:
    is_weekend = date.weekday() >= 5
    budget_level = get_budget_level(budget)

    plan, policy = generate_plan(
        weather,
        member,
        age_group,
        budget_level
    )

    st.subheader("📋 今日のプラン")
    for p in plan:
        st.write("•", p)

    st.subheader("🧠 プランのポイント")
    st.write(f"- 年齢層：**{age_group}**")
    st.write(f"- 予算帯：**{budget_level.upper()}**")
    st.write(f"- 方針：**{policy}**")
    st.write(f"- {'休日' if is_weekend else '平日'}想定のプランです")

    st.success("プランを作成しました！")



