# IntroToProg-Python-Mod06


import json

# Constants
MENU = """---- Course Registration Program ----
Select from the following menu:  
1. Register a Student for a Course
2. Show current data  
3. Save data to a file
4. Exit the program
-----------------------------------------"""

FILE_NAME = "Enrollments.json"

# Variables
menu_choice = ""
students = []


class FileProcessor:
    """Class to handle file processing operations."""

    @staticmethod
    def read_data_from_file(file_name: str, student_data: list):
        """Reads data from a JSON file and populates the student_data list."""
        try:
            with open(file_name, 'r') as file:
                data = json.load(file)
                student_data.extend(data)
        except FileNotFoundError:
            print("File not found. Starting with an empty list.")
        except json.JSONDecodeError as e:
            output_error_messages("Error decoding JSON.", e)

    @staticmethod
    def write_data_to_file(file_name: str, student_data: list):
        """Writes student data to a JSON file."""
        try:
            with open(file_name, 'w') as file:
                json.dump(student_data, file)
            print(f"Data saved to {file_name}.")
        except Exception as e:
            output_error_messages("Error saving data.", e)


class IO:
    """Class to handle input and output operations."""

    @staticmethod
    def output_error_messages(message: str, error: Exception = None):
        """Outputs error messages to the user."""
        print(f"Error: {message}")
        if error:
            print(f"Details: {error}")

    @staticmethod
    def output_menu(menu: str):
        """Displays the menu to the user."""
        print(menu)

    @staticmethod
    def input_menu_choice() -> str:
        """Prompts the user for a menu choice and returns it."""
        return input("Enter your choice: ")

    @staticmethod
    def output_student_courses(student_data: list):
        """Displays the current student registrations."""
        if not student_data:
            print("No student registrations found.")
        else:
            print("Current Student Registrations:")
            for student in student_data:
                print(f"{student['first_name']} {student['last_name']} - {student['course']}")

    @staticmethod
    def input_student_data(student_data: list):
        """Prompts the user for student data and adds it to the student_data list."""
        try:
            first_name = input("Enter the student's first name: ")
            if not first_name.strip():
                raise ValueError("First name cannot be empty.")

            last_name = input("Enter the student's last name: ")
            if not last_name.strip():
                raise ValueError("Last name cannot be empty.")

            course_name = input("Enter the course name: ")

            # Create a dictionary for the student data
            student_info = {
                "first_name": first_name,
                "last_name": last_name,
                "course": course_name
            }
            student_data.append(student_info)
            print(f"Registered: {first_name} {last_name} for {course_name}")

        except ValueError as e:
            IO.output_error_messages(str(e))


# Main program logic
def main():
    global menu_choice
    FileProcessor.read_data_from_file(FILE_NAME, students)  # Load existing data

    while True:
        IO.output_menu(MENU)
        menu_choice = IO.input_menu_choice()

        if menu_choice == '1':
            IO.input_student_data(students)
        elif menu_choice == '2':
            IO.output_student_courses(students)
        elif menu_choice == '3':
            FileProcessor.write_data_to_file(FILE_NAME, students)
        elif menu_choice == '4':
            print("Exiting the program.")
            break
        else:
            IO.output_error_messages("Invalid choice. Please select a valid option.")


if __name__ == "__main__":
    main()

