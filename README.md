# Quiz_App
import streamlit as st
import random

# Initialize state variables on first run
if 'questions' not in st.session_state:
    st.session_state.questions = [
        {
            "question": "What is the capital of France?",
            "options": ["London", "Paris", "Berlin", "Madrid"],
            "answer": "Paris"
        },
        {
            "question": "Which planet is known as the Red Planet?",
            "options": ["Venus", "Mars", "Jupiter", "Saturn"],
            "answer": "Mars"
        },
        {
            "question": "What is the largest mammal?",
            "options": ["Elephant", "Blue Whale", "Giraffe", "Hippopotamus"],
            "answer": "Blue Whale"
        },
        {
            "question": "In which year did World War II end?",
            "options": ["1943", "1945", "1947", "1950"],
            "answer": "1945"
        },
        {
            "question": "What is the chemical symbol for gold?",
            "options": ["Go", "Gd", "Au", "Ag"],
            "answer": "Au"
        },
        {
            "question": "Who painted the Mona Lisa?",
            "options": ["Vincent van Gogh", "Pablo Picasso", "Leonardo da Vinci", "Michelangelo"],
            "answer": "Leonardo da Vinci"
        },
        {
            "question": "What is the largest ocean on Earth?",
            "options": ["Atlantic Ocean", "Indian Ocean", "Arctic Ocean", "Pacific Ocean"],
            "answer": "Pacific Ocean"
        },
        {
            "question": "Which language has the most native speakers?",
            "options": ["English", "Hindi", "Spanish", "Mandarin Chinese"],
            "answer": "Mandarin Chinese"
        },
        {
            "question": "What is the square root of 64?",
            "options": ["4", "6", "8", "10"],
            "answer": "8"
        },
        {
            "question": "Which element has the atomic number 1?",
            "options": ["Helium", "Hydrogen", "Oxygen", "Carbon"],
            "answer": "Hydrogen"
        }
    ]
    st.session_state.score = 0
    st.session_state.asked = []
    st.session_state.current_q = None
    st.session_state.show_result = False
    st.session_state.num_questions = 5

# Function to get new question
def get_new_question():
    available = [q for q in st.session_state.questions if q not in st.session_state.asked]
    if available:
        q = random.choice(available)
        st.session_state.asked.append(q)
        st.session_state.current_q = q
    else:
        st.session_state.current_q = None
        st.session_state.show_result = True

# Main app logic
st.title("🧠 Quiz App")
st.write(f"You will be asked *{st.session_state.num_questions}* random questions. Good luck!")

if not st.session_state.current_q and not st.session_state.show_result:
    get_new_question()

# Show question if not done yet
if st.session_state.current_q and not st.session_state.show_result:
    q = st.session_state.current_q
    st.subheader(q["question"])
    option = st.radio("Choose your answer:", q["options"], key=f"opt_{len(st.session_state.asked)}")

    if st.button("Submit"):
        if option == q["answer"]:
            st.success("Correct!")
            st.session_state.score += 1
        else:
            st.error(f"Wrong! Correct answer: *{q['answer']}*")

        if len(st.session_state.asked) >= st.session_state.num_questions:
            st.session_state.show_result = True
        else:
            st.button("Next Question", on_click=get_new_question)

# Final result
if st.session_state.show_result:
    st.markdown("---")
    st.subheader("🎉 Quiz Finished!")
    total = len(st.session_state.asked)
    score = st.session_state.score
    percentage = (score / total) * 100
    st.write(f"*Score: {score} / {total} ({percentage:.1f}%)*")

    if percentage >= 80:
        st.balloons()
        st.success("Excellent work! You're a quiz master!")
    elif percentage >= 60:
        st.info("Good job! A little more practice and you'll be at the top.")
    elif percentage >= 40:
        st.warning("Fair effort. Keep learning and improving!")
    else:
        st.error("Don't worry! Every expert was once a beginner. Keep studying!")

    # Reset button
    if st.button("Play Again"):
        for key in st.session_state.keys():
            del st.session_state[key]
        st.experimental_rerun()
#streamlit run quiz.py ---run this command .
