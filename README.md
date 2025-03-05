
**Role Responsibilities**

Project Manager Oversees progress, ensures timely completion. --------------------

Lead Developer Implements major code improvements. -------------------------------

Refactoring Specialist Identifies technical debt and suggests improvements. -------  Maulod & Roslinda

Git Manager Manages Git operations (branching, commits, documentation). - ------

Tester/Documenter Writes test cases and updates documentation.--------


Poor Naming Conventions

The function name compute_deductions does not return anything; it only prints the results. A better name would be display_salary_deductions or calculate_and_display_deductions.
Variable names like sss, philhealth, and pagibig could be more descriptive, such as sss_contribution, philhealth_contribution, and pagibig_contribution for better clarity.
Lack of Modular Functions

The function mixes computation and printing. It should return values rather than directly printing them, making it reusable in different contexts.
Splitting into smaller functions (e.g., compute_sss_deduction(salary), compute_philhealth_deduction(salary)) would improve readability and maintainability.
Hardcoded Values Instead of Dynamic Inputs

Values like sss = 1200, pagibig = 100, and tax = 1875 are hardcoded. These should either be configurable (e.g., passed as parameters) or retrieved from a database/config file.
PhilHealth contribution is computed as (salary * 0.05) / 2, but the rate may change. A constant should be used instead of directly writing the formula in the function.
No Error Handling

No validation is done for the user input. If a non-numeric value is entered, the program will crash. Wrapping the input in a try-except block would help.
No checks for negative or zero salary, which could lead to incorrect calculations.
Code Duplication

The function compute_deductions(salary) is defined before salary is even assigned. Calling compute_deductions(salary) at the end forces repetition of logic.
The deductions could be handled within a return statement instead of repeating the calculation in multiple places.

#Improve code readability  - Refactoring Specialist (Roslinda)

def compute_deductions(salary):
    """Calculates total deductions and net salary based on given salary."""
    
    # Fixed contributions
    SSS_CONTRIBUTION = 1200
    PAGIBIG_CONTRIBUTION = 100

    # Computed contributions
    philhealth = (salary * 0.05) / 2  # PhilHealth deduction
    tax = 1875  # Fixed tax amount (should be dynamic in real cases)

    # Total deductions
    total_deductions = SSS_CONTRIBUTION + philhealth + PAGIBIG_CONTRIBUTION + tax
    net_salary = salary - total_deductions

    # Display salary breakdown
    print("\n--- Salary Breakdown ---")
    print(f"Gross Salary: {salary:,.2f}")
    print(f"SSS Deduction: {SSS_CONTRIBUTION:,.2f}")
    print(f"PhilHealth Deduction: {philhealth:,.2f}")
    print(f"Pag-IBIG Deduction: {PAGIBIG_CONTRIBUTION:,.2f}")
    print(f"Tax Deduction: {tax:,.2f}")
    print(f"Total Deductions: {total_deductions:,.2f}")
    print(f"Net Salary: {net_salary:,.2f}")

# Get user input
try:
    salary = float(input("Enter your monthly salary: "))
    
    if salary < 0:
        print("Error: Salary must be a positive value.")
    else:
        compute_deductions(salary)

except ValueError:
    print("Error: Invalid input! Please enter a valid numeric value.")
    
#Key improvements:

 Constants for fixed values – makes the code easier to modify and understand.
 Formatted output – uses :,.2f for better number readability (e.g., 10,000.00 instead of 10000).
 Added function docstring – explains what the function does.
 Error handling for invalid input – prevents crashes from non-numeric values.
 Clear section headers (--- Salary Breakdown ---) – makes output more readable.


#Convert to Modular Functions - Lead Developer (Maulod)
def get_user_salary():
    """Handles user input and ensures it's a valid positive number."""
    while True:
        try:
            salary = float(input("Enter your monthly salary: "))
            if salary < 0:
                print("Salary must be a positive value. Please try again.")
            else:
                return salary
        except ValueError:
            print("Invalid input! Please enter a numeric value.")

def compute_sss():
    """Returns the fixed SSS contribution."""
    return 1200

def compute_philhealth(salary):
    """Calculates the PhilHealth deduction based on salary."""
    return (salary * 0.05) / 2

def compute_pagibig():
    """Returns the fixed Pag-IBIG contribution."""
    return 100

def compute_tax(salary):
    """Computes tax based on salary brackets."""
    if salary <= 20000:
        return salary * 0.05
    elif salary <= 50000:
        return salary * 0.1
    else:
        return salary * 0.15

def compute_deductions(salary):
    """Calculates total deductions and net salary."""
    sss = compute_sss()
    philhealth = compute_philhealth(salary)
    pagibig = compute_pagibig()
    tax = compute_tax(salary)

    total_deductions = sss + philhealth + pagibig + tax
    net_salary = salary - total_deductions

    return net_salary, total_deductions, sss, philhealth, pagibig, tax

def display_salary_details(salary, net_salary, total_deductions, sss, philhealth, pagibig, tax):
    """Displays the salary breakdown."""
    print("\nSalary Breakdown:")
    print(f"Gross Salary: {salary}")
    print(f"SSS Deduction: {sss}")
    print(f"PhilHealth Deduction: {philhealth:.2f}")
    print(f"Pag-IBIG Deduction: {pagibig}")
    print(f"Tax Deduction: {tax:.2f}")
    print(f"Total Deductions: {total_deductions:.2f}")
    print(f"Net Salary: {net_salary:.2f}")

def main():
    """Main function to execute the program."""
    salary = get_user_salary()
    net_salary, total_deductions, sss, philhealth, pagibig, tax = compute_deductions(salary)
    display_salary_details(salary, net_salary, total_deductions, sss, philhealth, pagibig, tax)

# Run the program
if __name__ == "__main__":
    main()
























