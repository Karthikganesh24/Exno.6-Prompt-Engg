# Exno.6 Development of Python Code Compatible with Multiple AI Tools

# Aim: 
Development of Python Code Compatible with Multiple AI Tools
# Algorithm: 
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights.
## Output:
## Objective
The objective of this experiment is to develop Python code that integrates with multiple AI tools (via their APIs), automates the process of interacting with these APIs, compares the outputs from the AI tools, and generates actionable insights based on these comparisons.
# Topic :
College Management System
# Output:
College Management System Python Programs
Here are Python implementations for several of the advanced college management system features you mentioned:

1. Smart Assistant Integration (Chatbot)
python
import re
from datetime import datetime

class CollegeAssistant:
    def __init__(self):
        self.schedule = {
            'tests': {
                'Math': datetime(2023, 11, 15),
                'Physics': datetime(2023, 11, 20)
            },
            'assignments': {
                'CS101': {'due': datetime(2023, 11, 10), 'submitted': False}
            },
            'timetable': {
                'Monday': ['Math 9-11', 'Physics Lab 2-4'],
                'Tuesday': ['CS101 10-12', 'English 3-5']
            }
        }
    
    def handle_query(self, query):
        query = query.lower()
        
        # Next test query
        if 'next test' in query:
            next_test = min(self.schedule['tests'].items(), key=lambda x: x[1])
            return f"Your next test is {next_test[0]} on {next_test[1].strftime('%B %d')}."
        
        # Assignment reminder
        elif 'submit' in query or 'assignment' in query:
            for course, details in self.schedule['assignments'].items():
                if not details['submitted']:
                    return f"Remember to submit {course} assignment by {details['due'].strftime('%B %d')}."
            return "No pending assignments found."
        
        # Schedule query
        elif 'schedule' in query or 'timetable' in query:
            day = None
            for d in ['monday', 'tuesday', 'wednesday', 'thursday', 'friday']:
                if d in query:
                    day = d.capitalize()
                    break
            if day:
                return f"Your {day} schedule: {', '.join(self.schedule['timetable'][day])}"
            return "Please specify a day (e.g., 'What's my schedule on Monday?')"
        
        return "I'm not sure I understand. You can ask about tests, assignments, or your schedule."

# Example usage
assistant = CollegeAssistant()
print(assistant.handle_query("When is my next test?"))
print(assistant.handle_query("Remind me to submit my assignment"))
print(assistant.handle_query("What's my schedule on Tuesday?"))
2. Predictive Analytics for Performance
python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

class PerformancePredictor:
    def __init__(self):
        # Sample dataset - in real scenario, load from database
        data = {
            'attendance': [90, 75, 60, 85, 45, 80, 95, 70],
            'assignment_avg': [85, 70, 50, 80, 40, 75, 90, 65],
            'midterm': [80, 65, 45, 75, 30, 70, 85, 60],
            'passed': [1, 1, 0, 1, 0, 1, 1, 0]  # 1=passed, 0=failed
        }
        self.df = pd.DataFrame(data)
        self.model = None
        
    def train_model(self):
        X = self.df[['attendance', 'assignment_avg', 'midterm']]
        y = self.df['passed']
        
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
        
        self.model = RandomForestClassifier()
        self.model.fit(X_train, y_train)
        
        # Evaluate
        y_pred = self.model.predict(X_test)
        print(f"Model accuracy: {accuracy_score(y_test, y_pred):.2f}")
    
    def predict_student(self, attendance, assignment_avg, midterm):
        if not self.model:
            self.train_model()
            
        prediction = self.model.predict([[attendance, assignment_avg, midterm]])
        proba = self.model.predict_proba([[attendance, assignment_avg, midterm]])
        
        if prediction[0] == 1:
            return f"High chance of passing ({proba[0][1]:.0%} probability)"
        else:
            return f"Risk of failing ({proba[0][0]:.0%} probability)"

# Example usage
predictor = PerformancePredictor()
print(predictor.predict_student(80, 75, 70))  # Good student
print(predictor.predict_student(50, 40, 35))  # At-risk student
3. Automated Timetable Generator
python
from itertools import product

class TimetableGenerator:
    def __init__(self):
        self.courses = []
        self.rooms = []
        self.timeslots = []
        self.faculty_availability = {}
    
    def add_course(self, name, faculty, students):
        self.courses.append({
            'name': name,
            'faculty': faculty,
            'students': students
        })
    
    def add_room(self, name, capacity):
        self.rooms.append({
            'name': name,
            'capacity': capacity
        })
    
    def add_timeslot(self, day, time):
        self.timeslots.append({
            'day': day,
            'time': time
        })
    
    def set_faculty_availability(self, faculty, available_slots):
        self.faculty_availability[faculty] = available_slots
    
    def generate(self):
        # This is a simplified version - real implementation would use more advanced algorithms
        timetable = []
        
        # Sort courses by number of students (descending) to place larger classes first
        sorted_courses = sorted(self.courses, key=lambda x: -x['students'])
        
        for course in sorted_courses:
            placed = False
            faculty = course['faculty']
            
            # Get available slots for this faculty
            available_slots = self.faculty_availability.get(faculty, self.timeslots)
            
            for slot in available_slots:
                for room in self.rooms:
                    if room['capacity'] >= course['students']:
                        # Check if room is available at this timeslot
                        conflict = False
                        for scheduled in timetable:
                            if (scheduled['room'] == room['name'] and 
                                scheduled['day'] == slot['day'] and 
                                scheduled['time'] == slot['time']):
                                conflict = True
                                break
                        
                        if not conflict:
                            timetable.append({
                                'course': course['name'],
                                'faculty': faculty,
                                'day': slot['day'],
                                'time': slot['time'],
                                'room': room['name']
                            })
                            placed = True
                            break
                if placed:
                    break
            
            if not placed:
                print(f"Warning: Could not place {course['name']}")
        
        return timetable

# Example usage
generator = TimetableGenerator()

# Add rooms
generator.add_room("A101", 50)
generator.add_room("B202", 30)
generator.add_room("C303", 70)

# Add timeslots
for day in ["Mon", "Tue", "Wed"]:
    for time in ["9-11", "11-1", "2-4", "4-6"]:
        generator.add_timeslot(day, time)

# Add courses
generator.add_course("Math 101", "Dr. Smith", 45)
generator.add_course("Physics 201", "Dr. Johnson", 35)
generator.add_course("CS 301", "Dr. Williams", 25)

# Set faculty availability
generator.set_faculty_availability("Dr. Smith", [{"day": "Mon", "time": "9-11"}, {"day": "Tue", "time": "2-4"}])
generator.set_faculty_availability("Dr. Johnson", [{"day": "Mon", "time": "11-1"}, {"day": "Wed", "time": "9-11"}])
generator.set_faculty_availability("Dr. Williams", [{"day": "Tue", "time": "11-1"}, {"day": "Wed", "time": "2-4"}])

# Generate timetable
timetable = generator.generate()
for entry in timetable:
    print(f"{entry['course']} with {entry['faculty']} on {entry['day']} {entry['time']} in {entry['room']}")

# Concluation:
The Python implementations provided for these college management system features demonstrate how modern technologies can transform educational administration. By integrating these advanced capabilities, institutions can:

Enhance Student Experience through AI-powered assistants that provide 24/7 support and predictive analytics that offer early intervention opportunities.

Optimize Administrative Efficiency with automated systems for timetable generation, digital notice distribution, and certificate issuance that reduce manual workloads.

Improve Decision-Making using data-driven insights from predictive analytics and comprehensive tracking systems.

Increase Engagement with role-specific dashboards and targeted communication channels that ensure relevant information reaches the right stakeholders.

Ensure Scalability through modular Python implementations that can integrate with existing systems or expand to accommodate growing institutional needs.




# Result: 
The corresponding Prompt is executed successfully
