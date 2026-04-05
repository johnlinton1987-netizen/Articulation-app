# Articulation-appimport streamlit as st
import random
import time

# List of phrases - You can add more here later!
phrases = [
    "The sixth sick sheik's sixth sheep's sick.",
    "Specific Pacific Ocean requirements.",
    "He threw three free throws.",
    "Literally an adjustable aluminum anomaly.",
    "Regularly relevant rural murals.",
    "Is it statistically significant?",
    "A deteriorating decorative door."
]

st.set_page_config(page_title="Articulation Pro", page_icon="🗣️")

st.title("🗣️ Articulation Practice")
st.write("Read the phrase below using your phone's dictation (microphone) button.")

# Initialize the target phrase if it doesn't exist
if 'target' not in st.session_state:
    st.session_state.target = random.choice(phrases)
    st.session_state.start_time = time.time()

st.info(f"### {st.session_state.target}")

# The Input Box
user_input = st.text_input("Tap here, then hit your keyboard's microphone button to speak:")

if user_input:
    # Calculate Time
    end_time = time.time()
    duration = end_time - st.session_state.start_time
    
    # Logic to compare words
    target_clean = st.session_state.target.lower().replace(".", "").replace("'", "")
    user_clean = user_input.lower().replace(".", "").replace("'", "")
    
    target_words = target_clean.split()
    user_words = user_clean.split()
    
    # Scoring
    correct_words = [w for w in user_words if w in target_words]
    accuracy = (len(correct_words) / len(target_words)) * 100
    wpm = (len(user_words) / (duration / 60)) if duration > 0 else 0

    # Display Results
    st.divider()
    col1, col2 = st.columns(2)
    col1.metric("Accuracy", f"{int(accuracy)}%")
    col2.metric("Speed", f"{int(wpm)} WPM")

    if accuracy >= 90:
        st.success("Great job! Your articulation is clear.")
        st.balloons()
    elif accuracy >= 70:
        st.warning("Not bad, but a few words were missed. Try enunciating the consonants.")
    else:
        st.error("Hard to understand. Slow down and focus on each syllable.")

    st.write(f"**I heard:** _{user_input}_")

# Button to get a new phrase
if st.button("Next Phrase"):
    st.session_state.target = random.choice(phrases)
    st.session_state.start_time = time.time()
    st.rerun()
