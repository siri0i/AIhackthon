# AIhackthon
AI bot to summaries chats and tasks, which not only saves time but also give better answers
# SmartAI Assistant
A chatbot that uses AI to summarize text and automate tasks.

## Features
- Chat-based AI responses
- Task automation
- Built using Python + ChatGPT API

## How to Run
1. Clone this repo
2. Run `python app.py`

## Team
Created by Siri Chandana for AI Hackathon 2025

import openai
import pyttsx3
import webbrowser
import datetime

# Initialize Text-to-Speech engine
engine = pyttsx3.init()

# --- YOUR OPENAI API KEY ---
openai.api_key = "your_api_key_here"  # Replace with your own key

def speak(text):
    """Convert text to voice"""
    engine.say(text)
    engine.runAndWait()

def summarize_text(text):
    """Summarize large text using OpenAI model"""
    prompt = f"Summarize the following text in simple terms:\n\n{text}"
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}]
    )
    summary = response.choices[0].message["content"]
    return summary

def handle_task(command):
    """Automate simple tasks"""
    command = command.lower()
    
    if "open youtube" in command:
        webbrowser.open("https://www.youtube.com")
        return "Opening YouTube..."
    
    elif "open google" in command:
        webbrowser.open("https://www.google.com")
        return "Opening Google..."
    
    elif "time" in command:
        now = datetime.datetime.now().strftime("%H:%M:%S")
        return f"The current time is {now}"
    
    elif "remind me" in command:
        reminder = command.replace("remind me", "").strip()
        return f"Reminder noted: {reminder}"
    
    elif "summarize" in command:
        print("Enter the text to summarize:")
        text = input("📝 Text: ")
        summary = summarize_text(text)
        return f"Here’s the summary:\n{summary}"
    
    else:
        # Default ChatGPT-style response
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[{"role": "user", "content": command}]
        )
        answer = response.choices[0].message["content"]
        return answer


def main():
    print("🤖 SmartAI Assistant — Your AI helper is ready!")
    speak("Hi Siri, I'm your Smart AI Assistant. How can I help you today?")
    
    while True:
        user_input = input("\nYou: ")
        if user_input.lower() in ["exit", "quit", "bye"]:
            speak("Goodbye! Have a great day.")
            print("👋 Exiting...")
            break
        
        response = handle_task(user_input)
        print("AI:", response)
        speak(response)

if __name__ == "__main__":
    main()
