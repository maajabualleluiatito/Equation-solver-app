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
        value (str): The user input to validate.

    Raises:
        ValueError: If the input is not a valid number.
    """
    if not re.fullmatch(r'-?\d+(\.\d+)?', value):
        raise ValueError(f"Invalid input: '{value}' is not a valid number.")


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


def solve_two_unknowns_with_coefficients(a1, b1, c1, a2, b2, c2):
    """
    Solves a system of two equations with two unknowns using the Wolfram Alpha API.

    The equations are of the form:
    a1*x + b1*y = c1
    a2*x + b2*y = c2
    """
    eq1 = f"{a1}x + {b1}y = {c1}"
    eq2 = f"{a2}x + {b2}y = {c2}"
    query = f"solve {{ {eq1}, {eq2} }} for x, y"
    print(f"Debug: Query sent to API: {query}")
    result = fetch_solution(query)
    print(f"Solution: {result}")


def solve_three_unknowns_with_coefficients(a1, b1, c1, d1, a2, b2, c2, d2, a3, b3, c3, d3):
    """
    Solves a system of three equations with three unknowns using the Wolfram Alpha API.

    The equations are of the form:
    a1*x + b1*y + c1*z = d1
    a2*x + b2*y + c2*z = d2
    a3*x + b3*y + c3*z = d3
    """
    eq1 = f"{a1}x + {b1}y + {c1}z = {d1}"
    eq2 = f"{a2}x + {b2}y + {c2}z = {d2}"
    eq3 = f"{a3}x + {b3}y + {c3}z = {d3}"
    query = f"solve {{ {eq1}, {eq2}, {eq3} }} for x, y, z"
    print(f"Debug: Query sent to API: {query}")
    result = fetch_solution(query)
    print(f"Solution: {result}")


def show_instructions():
    """
    Displays instructions on how to use the application.
    """
    print("""
    Instructions:
    1. Solve Quadratic Equation:
       Input the coefficients a, b, and c for the equation ax^2 + bx + c = 0.
    
    2. Solve System of 2 Equations:
       Input the coefficients for two equations of the form:
       a1*x + b1*y = c1
       a2*x + b2*y = c2

    3. Solve System of 3 Equations:
       Input the coefficients for three equations of the form:
       a1*x + b1*y + c1*z = d1
       a2*x + b2*y + c2*z = d2
       a3*x + b3*y + c3*z = d3

    4. Exit the application by selecting option 5.
    """)


def main():
    """
    Main function to provide a menu for the user to choose and solve different types of equations.
    """
    while True:
        print("\nChoose a task:")
        print("1. Solve Quadratic Equation (ax^2 + bx + c = 0)")
        print("2. Solve System of 2 Equations with 2 Unknowns")
        print("3. Solve System of 3 Equations with 3 Unknowns")
        print("4. Instructions")
        print("5. Exit")

        choice = input("Enter your choice (1/2/3/4/5): ").strip()

        if choice == '1':
            try:
                a = input("Enter coefficient a: ").strip()
                b = input("Enter coefficient b: ").strip()
                c = input("Enter coefficient c: ").strip()
                validate_numeric_input(a)
                validate_numeric_input(b)
                validate_numeric_input(c)
                solve_quadratic(a, b, c)
            except ValueError as e:
                print(e)

        elif choice == '2':
            try:
                print("Enter coefficients for the equations in the form a1*x + b1*y = c1:")
                a1 = input("Enter a1: ").strip()
                b1 = input("Enter b1: ").strip()
                c1 = input("Enter c1: ").strip()
                a2 = input("Enter a2: ").strip()
                b2 = input("Enter b2: ").strip()
                c2 = input("Enter c2: ").strip()
                validate_numeric_input(a1)
                validate_numeric_input(b1)
                validate_numeric_input(c1)
                validate_numeric_input(a2)
                validate_numeric_input(b2)
                validate_numeric_input(c2)
                solve_two_unknowns_with_coefficients(a1, b1, c1, a2, b2, c2)
            except ValueError as e:
                print(e)

        elif choice == '3':
            try:
                print("Enter coefficients for the equations in the form a1*x + b1*y + c1*z = d1:")
                a1 = input("Enter a1: ").strip()
                b1 = input("Enter b1: ").strip()
                c1 = input("Enter c1: ").strip()
                d1 = input("Enter d1: ").strip()
                a2 = input("Enter a2: ").strip()
                b2 = input("Enter b2: ").strip()
                c2 = input("Enter c2: ").strip()
                d2 = input("Enter d2: ").strip()
                a3 = input("Enter a3: ").strip()
                b3 = input("Enter b3: ").strip()
                c3 = input("Enter c3: ").strip()
                d3 = input("Enter d3: ").strip()
                validate_numeric_input(a1)
                validate_numeric_input(b1)
                validate_numeric_input(c1)
                validate_numeric_input(d1)
                validate_numeric_input(a2)
                validate_numeric_input(b2)
                validate_numeric_input(c2)
                validate_numeric_input(d2)
                validate_numeric_input(a3)
                validate_numeric_input(b3)
                validate_numeric_input(c3)
                validate_numeric_input(d3)
                solve_three_unknowns_with_coefficients(a1, b1, c1, d1, a2, b2, c2, d2, a3, b3, c3, d3)
            except ValueError as e:
                print(e)

        elif choice == '4':
            show_instructions()

        elif choice == '5':
            print("Exiting the application. Goodbye!")
            break

        else:
            print("Invalid choice! Please select 1, 2, 3, 4, or 5.")


if __name__ == "__main__":
    main()
