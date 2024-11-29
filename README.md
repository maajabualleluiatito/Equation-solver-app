#!/usr/bin/env python3
"""
A terminal-based application that solves quadratic equations and systems of equations 
with 2 and 3 unknowns using the Wolfram Alpha Short Answers API.
"""

import os
import re
import requests
from dotenv import load_dotenv

# Load environment variables from the .env file
load_dotenv()

# Constants
APP_ID = os.getenv("WOLFRAM_APP_ID")
BASE_URL = "http://api.wolframalpha.com/v1/result"


def fetch_solution(query):
    """
    Sends a query to the Wolfram Alpha Short Answers API and returns the solution.

    Args:
        query (str): The mathematical query to be solved.

    Returns:
        str: The solution or an error message.
    """
    try:
        response = requests.get(BASE_URL, params={'appid': APP_ID, 'i': query})
        if response.status_code == 200:
            return response.text.strip()
        else:
            return f"Error: {response.status_code}, {response.text.strip()}"
    except requests.exceptions.RequestException as e:
        return f"Request error: {e}"


def validate_numeric_input(value):
    """
    Validates that the input is a valid number (integer or float).

    Args:
        value (str): The input string.

    Returns:
        bool: True if valid, raises ValueError otherwise.
    """
    if not re.match(r"^-?\d+(\.\d+)?$", value):
        raise ValueError(f"Invalid numeric input: '{value}'. Please enter a valid number.")
    return True


def validate_equation_input(equation):
    """
    Validates that the input is a valid linear equation.

    Args:
        equation (str): The input string.

    Returns:
        bool: True if valid, raises ValueError otherwise.
    """
    pattern = r"^[\d+\-*/xXyYzZ\s=]+$"
    if not re.match(pattern, equation):
        raise ValueError(f"Invalid equation format: '{equation}'. Ensure the input is a valid equation.")
    return True


def solve_quadratic(a, b, c):
    """
    Solves a quadratic equation of the form ax^2 + bx + c = 0 using the Wolfram Alpha API.

    Args:
        a (str): Coefficient of x^2.
        b (str): Coefficient of x.
        c (str): Constant term.
    """
    query = f"solve {a}x^2 + {b}x + {c} = 0"
    result = fetch_solution(query)
    print(f"Solution: {result}")


def solve_two_unknowns(eq1, eq2):
    """
    Solves a system of two equations with two unknowns using the Wolfram Alpha API.

    Args:
        eq1 (str): The first equation (e.g., "2x + 3y = 5").
        eq2 (str): The second equation (e.g., "x - y = 1").
    """
    query = f"solve {eq1}, {eq2}"
    result = fetch_solution(query)
    print(f"Solution: {result}")


def solve_three_unknowns(eq1, eq2, eq3):
    """
    Solves a system of three equations with three unknowns using the Wolfram Alpha API.

    Args:
        eq1 (str): The first equation.
        eq2 (str): The second equation.
        eq3 (str): The third equation.
    """
    query = f"solve {eq1}, {eq2}, {eq3}"
    result = fetch_solution(query)
    print(f"Solution: {result}")


def show_instructions():
    """
    Displays instructions on how to use the app.
    """
    print("\n--- Instructions ---")
    print("1. Solve Quadratic Equation:")
    print("   - Input three coefficients (a, b, c).")
    print("   - The app will return solutions for ax^2 + bx + c = 0.")
    print("\n2. Solve System of 2 Equations:")
    print("   - Input two linear equations (e.g., '2x + 3y = 5' and 'x - y = 2').")
    print("   - The app will return the values of x and y.")
    print("\n3. Solve System of 3 Equations:")
    print("   - Input three linear equations (e.g., 'x + y + z = 6', 'x - y = 4', 'z = 3').")
    print("   - The app will return the values of x, y, and z.")
    print("\n4. Exit the application.")
    print("--------------------\n")


def main():
    """
    Main function to provide a menu for the user to choose and solve different types of equations.
    """
    while True:
        print("Choose a task:")
        print("0. Instructions")
        print("1. Solve Quadratic Equation (ax^2 + bx + c = 0)")
        print("2. Solve System of 2 Equations with 2 Unknowns")
        print("3. Solve System of 3 Equations with 3 Unknowns")
        print("4. Exit Application")

        choice = input("Enter your choice (0/1/2/3/4): ").strip()

        if choice == '0':
            show_instructions()

        elif choice == '1':
            try:
                a = input("Enter coefficient a: ").strip()
                validate_numeric_input(a)
                b = input("Enter coefficient b: ").strip()
                validate_numeric_input(b)
                c = input("Enter coefficient c: ").strip()
                validate_numeric_input(c)
                solve_quadratic(a, b, c)
            except ValueError as e:
                print(e)

        elif choice == '2':
            try:
                eq1 = input("Enter the first equation (e.g., 2x + 3y = 5): ").strip()
                validate_equation_input(eq1)
                eq2 = input("Enter the second equation (e.g., x - y = 2): ").strip()
                validate_equation_input(eq2)
                solve_two_unknowns(eq1, eq2)
            except ValueError as e:
                print(e)

        elif choice == '3':
            try:
                eq1 = input("Enter the first equation: ").strip()
                validate_equation_input(eq1)
                eq2 = input("Enter the second equation: ").strip()
                validate_equation_input(eq2)
                eq3 = input("Enter the third equation: ").strip()
                validate_equation_input(eq3)
                solve_three_unknowns(eq1, eq2, eq3)
            except ValueError as e:
                print(e)

        elif choice == '4':
            print("Exiting the application. Goodbye!")
            break

        else:
            print("Invalid choice! Please select 0, 1, 2, 3, or 4.")


if __name__ == "__main__":
    main()

