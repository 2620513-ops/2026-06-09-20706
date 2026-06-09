import streamlit as st

# 앱 제목 설정
st.title("📱 스마트폰 배터리 관리 프로그램")
st.write("여러 스마트폰의 배터리 상태를 입력하고 평균 및 상태를 확인하세요.")

# 6. 입력할 배터리의 개수를 사용자에게 입력 받을 것 (기본값 3개)
count = st.number_input("관리할 배터리의 개수를 입력하세요:", min_value=1, max_value=10, value=3, step=1)

# 1. 리스트 사용 (배터리 잔량을 저장할 리스트 초기화)
battery_list = []

# 스트림릿에서 여러 입력을 한 번에 처리하기 위해 Form 사용
with st.form(key='battery_form'):
    st.write(f"각 스마트폰의 배터리 잔량(%)을 입력해주세요 (총 {count}개):")
    
    # 3. 반복문(for) 사용 & 4. 사용자 입력(input 대신 st.number_input) 사용
    # count 개수만큼 반복하여 입력 창 생성
    for i in range(int(count)):
        level = st.number_input(f"{i+1}번 스마트폰 배터리 잔량 (%)", min_value=0, max_value=100, value=50, key=f"battery_{i}")
        # 7. 반복문을 사용하여 입력 받은 배터리를 리스트에 저장할 것
        battery_list.append(level)
        
    # 제출 버튼
    submit_button = st.form_submit_button(label='배터리 상태 분석하기')

# 사용자가 버튼을 눌렀을 때 결과 출력
if submit_button:
    st.subheader("📊 분석 결과")
    
    # 8. 평균을 구할 것 (반복문을 사용하여 리스트의 총합 구하기)
    total_battery = 0
    for battery in battery_list:
        total_battery += battery
        
    average_battery = total_battery / len(battery_list)
    st.info(f"🔋 입력된 배터리의 평균 잔량은 **{average_battery:.1f}%** 입니다.")
    
    st.write("---")
    st.write("**📱 개별 배터리 상태 확인:**")
    
    # 2. 조건문(if) 사용 & 3. 반복문(for) 사용
    # 개별 배터리의 상태를 판단하여 출력
    for i, battery in enumerate(battery_list):
        # 9. 배터리가 20% 이하일 때와 초과일 때의 조건문
        if battery <= 20:
            status = "🔴 충전이 필요합니다"
            st.error(f"[{i+1}번 스마트폰] 잔량: {battery}% -> {status}")
        else:
            status = "🟢 정상 상태입니다"
            st.success(f"[{i+1}번 스마트폰] 잔량: {battery}% -> {status}")