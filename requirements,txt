import streamlit as st
import random
import time

# -----------------------------------------------------------------------------
# 페이지 기본 설정
# -----------------------------------------------------------------------------
st.set_page_config(
    page_title="신나는 구구단 마스터 🎮",
    page_icon="🔢",
    layout="centered"
)

# -----------------------------------------------------------------------------
# 세션 상태(Session State) 초기화
# -----------------------------------------------------------------------------
if 'score' not in st.session_state:
    st.session_state.score = 0
if 'total_questions' not in st.session_state:
    st.session_state.total_questions = 0
if 'combo' not in st.session_state:
    st.session_state.combo = 0
if 'max_combo' not in st.session_state:
    st.session_state.max_combo = 0
if 'num1' not in st.session_state or 'num2' not in st.session_state:
    st.session_state.num1 = random.randint(2, 9)
    st.session_state.num2 = random.randint(1, 9)
if 'feedback' not in st.session_state:
    st.session_state.feedback = None
if 'start_time' not in st.session_state:
    st.session_state.start_time = time.time()

# -----------------------------------------------------------------------------
# 함수 정의
# -----------------------------------------------------------------------------
def generate_new_question(selected_dan=0):
    """새로운 구구단 문제를 생성합니다."""
    if selected_dan == 0:  # 전체 단 랜덤
        st.session_state.num1 = random.randint(2, 9)
    else:  # 특정 단 선택
        st.session_state.num1 = selected_dan
    
    st.session_state.num2 = random.randint(1, 9)
    st.session_state.user_answer = ""

def check_answer(user_ans):
    """사용자의 정답을 채점하고 세션 상태를 업데이트합니다."""
    correct_ans = st.session_state.num1 * st.session_state.num2
    st.session_state.total_questions += 1
    
    if user_ans == correct_ans:
        st.session_state.score += 10
        st.session_state.combo += 1
        if st.session_state.combo > st.session_state.max_combo:
            st.session_state.max_combo = st.session_state.combo
        st.session_state.feedback = ("correct", f"🎉 정답입니다! ({st.session_state.num1} × {st.session_state.num2} = {correct_ans})")
    else:
        st.session_state.combo = 0
        st.session_state.feedback = ("wrong", f"❌ 아쉽네요! 정답은 {correct_ans}입니다. ({st.session_state.num1} × {st.session_state.num2} = {correct_ans})")

def reset_stats():
    """모든 기록을 초기화합니다."""
    st.session_state.score = 0
    st.session_state.total_questions = 0
    st.session_state.combo = 0
    st.session_state.max_combo = 0
    st.session_state.feedback = None
    st.session_state.start_time = time.time()

# -----------------------------------------------------------------------------
# 메인 UI 레이아웃
# -----------------------------------------------------------------------------
st.title("🔢 신나는 구구단 마스터 🎮")
st.caption("베테랑 앱 개발자가 만든 재미있는 인터랙티브 수학 연습실!")

# 사이드바 - 게임 설정 및 모드 선택
st.sidebar.header("⚙️ 게임 설정")
mode = st.sidebar.radio("학습 모드 선택", ["🎯 선택 단 집중 연습", "🎲 전체 믹스 퀴즈"])

selected_dan = 0
if mode == "🎯 선택 단 집중 연습":
    selected_dan = st.sidebar.selectbox("연습할 단을 선택하세요", list(range(2, 10)), index=0)

if st.sidebar.button("🔄 기록 초기화", use_container_width=True):
    reset_stats()
    generate_new_question(selected_dan)
    st.rerun()

# 점수판 패널
col_score1, col_score2, col_score3 = st.columns(3)
with col_score1:
    st.metric(label="현재 점수", value=f"{st.session_state.score} 점")
with col_score2:
    st.metric(label="연속 정답 (Combo)", value=f"{st.session_state.combo} 회 🔥")
with col_score3:
    st.metric(label="최고 Combo", value=f"{st.session_state.max_combo} 회")

st.markdown("---")

# -----------------------------------------------------------------------------
# 문제 출력 및 입력 구역
# -----------------------------------------------------------------------------
st.subheader("💡 문제를 풀어보세요!")

# 문제 대형 텍스트 표시
st.markdown(
    f"""
    <div style="text-align: center; background-color: #f0f2f6; padding: 20px; border-radius: 15px; margin-bottom: 20px;">
        <h1 style="color: #1f77b4; font-size: 3.5rem; margin: 0;">
            {st.session_state.num1}  ×  {st.session_state.num2}  =  ?
        </h1>
    </div>
    """,
    unsafe_allow_html=True
)

# 사용자 정답 입력 폼
with st.form(key="answer_form", clear_on_submit=True):
    user_input = st.number_input(
        "정답을 입력하고 Enter를 누르거나 [정답 제출] 버튼을 클릭하세요",
        min_value=0,
        max_value=100,
        step=1,
        value=None,
        placeholder="숫자 입력...",
        key="user_input_field"
    )
    submit_button = st.form_submit_button(label="🚀 정답 제출", use_container_width=True)

if submit_button:
    if user_input is not None:
        check_answer(user_input)
        generate_new_question(selected_dan)
        st.rerun()
    else:
        st.warning("숫자를 입력해주세요!")

# -----------------------------------------------------------------------------
# 피드백 및 효과
# -----------------------------------------------------------------------------
if st.session_state.feedback:
    fb_type, fb_msg = st.session_state.feedback
    if fb_type == "correct":
        st.success(fb_msg)
        if st.session_state.combo > 0 and st.session_state.combo % 5 == 0:
            st.balloons()  # 5연속 정답마다 축하 풍선
    else:
        st.error(fb_msg)

# -----------------------------------------------------------------------------
# 시각적 원리 이해 도구 (Mathematical Grid Visualization)
# -----------------------------------------------------------------------------
with st.expander("🧩 구구단 원리 시각화 보기 (도움말)"):
    st.write(f"**{st.session_state.num1} × {st.session_state.num2}**는 **{st.session_state.num1}개씩 {st.session_state.num2}줄** 모인 것과 같습니다!")
    
    # 격자 형태로 원리 시각화
    grid_html = "<div style='line-height: 1.5; font-size: 1.2rem; text-align: center;'>"
    for _ in range(st.session_state.num2):
        grid_html += " ".join(["🟡"] * st.session_state.num1) + "<br>"
    grid_html += "</div>"
    
    st.markdown(grid_html, unsafe_allow_html=True)

# -----------------------------------------------------------------------------
# 전체 학습 통계
# -----------------------------------------------------------------------------
if st.session_state.total_questions > 0:
    st.markdown("---")
    st.write("📊 **오늘의 학습 결과**")
    accuracy = (st.session_state.score // 10) / st.session_state.total_questions * 100
    st.progress(accuracy / 100, text=f"정답률: {accuracy:.1f}% ({st.session_state.score // 10} / {st.session_state.total_questions} 문제)")
