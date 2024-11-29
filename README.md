#!/usr/bin/env python3
"""
A terminal-based application that solves quadratic equations and systems of equations 
with 2 and 3 unknowns using the Wolfram Alpha Short Answers API.
"""

import os
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


def main():
    """
    Main function to provide a menu for the user to choose and solve different types of equations.
    """
    print("Choose a task:")
    print("1. Solve Quadratic Equation (ax^2 + bx + c = 0)")
    print("2. Solve System of 2 Equations with 2 Unknowns")
    print("3. Solve System of 3 Equations with 3 Unknowns")

    choice = input("Enter your choice (1/2/3): ").strip()

    if choice == '1':
        a = input("Enter coefficient a: ").strip()
        b = input("Enter coefficient b: ").strip()
        c = input("Enter coefficient c: ").strip()
        solve_quadratic(a, b, c)

    elif choice == '2':
        eq1 = input("Enter the first equation (e.g., 2x + 3y = 5): ").strip()
        eq2 = input("Enter the second equation (e.g., x - y = 2): ").strip()
        solve_two_unknowns(eq1, eq2)

    elif choice == '3':
        eq1 = input("Enter the first equation: ").strip()
        eq2 = input("Enter the second equation: ").strip()
        eq3 = input("Enter the third equation: ").strip()
        solve_three_unknowns(eq1, eq2, eq3)

    else:
        print("Invalid choice! Please select 1, 2, or 3.")


if __name__ == "__main__":
    main()

