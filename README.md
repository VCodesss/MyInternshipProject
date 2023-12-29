import tkinter as tk
from tkinter import messagebox

class QuizApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Quiz Game")
        self.root.geometry("500x300")
        self.score = 0
        self.current_question = 0

        # Questions, options, and correct answers
        self.questions = [
            {
                "question": "Which country has the highest population in the world?",
                "options": ["China", "India", "USA", "Russia"],
                "correct_answer": "India"
            },
            {
                "question": "What is the hottest planet in our solar system?",
                "options": ["Mars", "Jupiter", "Venus", "Saturn"],
                "correct_answer": "Venus"
            },
            {
                "question": "Who wrote 'Romeo and Juliet'?",
                "options": ["Charles Dickens", "William Shakespeare", "Jane Austen", "Mark Twain"],
                "correct_answer": "William Shakespeare"
            },
            {
                "question": "What is the largest mammal in the world?",
                "options": ["Elephant", "Blue Whale", "Giraffe", "Hippopotamus"],
                "correct_answer": "Blue Whale"
            },
            {
                "question": "What is the capital of Japan?",
                "options": ["Seoul", "Tokyo", "Beijing", "Bangkok"],
                "correct_answer": "Tokyo"
            }

        ]

        # GUI elements
        self.question_label = tk.Label(root, text="", font=("Helvetica", 14), fg="white", bg="#add8e6")
        self.question_label.pack(pady=20)

        self.radio_var = tk.StringVar()
        self.radio_var.set("")

        self.option_buttons = []
        for i in range(4):
            button = tk.Radiobutton(root, text="", variable=self.radio_var, value="", font=("Helvetica", 12), fg="white", bg="#add8e6")
            button.pack(pady=5)
            self.option_buttons.append(button)

        self.next_button = tk.Button(root, text="Next", command=self.next_question, font=("Helvetica", 12), fg="white", bg="#add8e6")
        self.next_button.pack(pady=20)

        # Initialize the first question
        self.load_question()

    def load_question(self):
        if self.current_question < len(self.questions):
            question_data = self.questions[self.current_question]
            self.question_label.config(text=question_data["question"])

            for i in range(4):
                self.option_buttons[i].config(text=question_data["options"][i], value=question_data["options"][i])

    def next_question(self):
        if self.radio_var.get() == "":
            messagebox.showinfo("Error", "Please select an answer.")
        else:
            selected_answer = self.radio_var.get()
            correct_answer = self.questions[self.current_question]["correct_answer"]

            if selected_answer == correct_answer:
                self.score += 1

            feedback = f"Your answer is {'correct' if selected_answer == correct_answer else 'incorrect'}. The correct answer is {correct_answer}."
            messagebox.showinfo("Feedback", feedback)

            self.current_question += 1
            self.radio_var.set("")
            if self.current_question < len(self.questions):
                self.load_question()
            else:
                self.show_final_score()

    def show_final_score(self):
        messagebox.showinfo("Quiz Completed", f"Your final score is {self.score}/{len(self.questions)}.")

if __name__ == "__main__":
    root = tk.Tk()
    root.configure(bg="#add8e6")
    app = QuizApp(root)
    root.mainloop()
