
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

# Improve code readability  - Refactoring Specialist (Roslinda)

# Constants for fixed contributions
SSS_CONTRIBUTION = 1200
PAGIBIG_CONTRIBUTION = 100
FIXED_TAX = 1875  # Placeholder for simplicity

    def compute_deductions(salary):
        """Calculates total deductions and net salary based on given salary."""
    
        # Computed contributions
        philhealth = (salary * 0.05) / 2  # PhilHealth deduction
        tax = FIXED_TAX  

        # Total deductions
        total_deductions = SSS_CONTRIBUTION + philhealth + PAGIBIG_CONTRIBUTION + tax
        net_salary = salary - total_deductions

        return net_salary, total_deductions, philhealth, tax

    def display_salary_details(salary, net_salary, total_deductions, philhealth, tax):
        """Displays a detailed salary breakdown."""
        print("\n--- Salary Breakdown ---")
        print(f"Gross Salary: {salary:,.2f}")
        print(f"SSS Deduction: {SSS_CONTRIBUTION:,.2f}")
        print(f"PhilHealth Deduction: {philhealth:,.2f}")
        print(f"Pag-IBIG Deduction: {PAGIBIG_CONTRIBUTION:,.2f}")
        print(f"Tax Deduction: {tax:,.2f}")
        print(f"Total Deductions: {total_deductions:,.2f}")
        print(f"Net Salary: {net_salary:,.2f}")

 Get user input
try:
    salary = float(input("Enter your monthly salary: "))
    
    if salary <= 0:
        print("Error: Salary must be greater than zero.")
    else:
        net_salary, total_deductions, philhealth, tax = compute_deductions(salary)
        display_salary_details(salary, net_salary, total_deductions, philhealth, tax)

except ValueError:
    print("Error: Invalid input! Please enter a valid numeric value.")


    
# Key improvements:

 Constants for fixed values – makes the code easier to modify and understand.
 Formatted output – uses :,.2f for better number readability (e.g., 10,000.00 instead of 10000).
 Added function docstring – explains what the function does.
 Error handling for invalid input – prevents crashes from non-numeric values.
 Clear section headers (--- Salary Breakdown ---) – makes output more readable.


# Convert to Modular Functions - Lead Developer (Maulod)
def compute_deductions(salary):
    """Calculates deductions and net salary based on given salary."""
    
    # Define contribution constants
    SSS_CONTRIBUTION = 1200
    PHILHEALTH_RATE = 0.05
    PAGIBIG_CONTRIBUTION = 100
    FIXED_TAX = 1875  # Assuming a fixed tax for simplicity

    if salary <= 0:
        print("Error: Salary must be a positive value.")
        return None

    # Compute deductions
    philhealth = (salary * PHILHEALTH_RATE) / 2
    total_deductions = SSS_CONTRIBUTION + philhealth + PAGIBIG_CONTRIBUTION + FIXED_TAX
    net_salary = salary - total_deductions

    return {
        "Gross Salary": salary,
        "SSS Deduction": SSS_CONTRIBUTION,
        "PhilHealth Deduction": philhealth,
        "Pag-IBIG Deduction": PAGIBIG_CONTRIBUTION,
        "Tax Deduction": FIXED_TAX,
        "Total Deductions": total_deductions,
        "Net Salary": net_salary
    }

    def get_user_salary():
    """Handles user input and ensures it is a valid positive number."""
    while True:
        try:
            salary = float(input("Enter your monthly salary: "))
            if salary > 0:
                return salary
            print("Error: Please enter a valid positive salary.")
        except ValueError:
            print("Invalid input. Please enter a numeric value.")

    def display_salary_details(result):
    """Displays the computed salary breakdown in a structured format."""
    print("\n--- Salary Breakdown ---")
    for key, value in result.items():
        print(f"{key}: {value:,.2f}")

    def main():
    """Main function to execute the program."""
    salary = get_user_salary()
    result = compute_deductions(salary)
    
    if result:
        display_salary_details(result)

    Run the program
    if __name__ == "__main__":
    main()

# Key Improvements

Separate functions for each responsibility (computation, input handling, display).
 Better maintainability – any changes (e.g., new tax rates) can be made in a single function.
 Improved reusability – functions can be used independently in other programs.
 Error handling included – prevents crashes from invalid user input.
 Uses if __name__ == "__main__" – ensures proper execution when used as a script.



# Add input validation & error handling - Tester & Documentor (Maulod)

def compute_deductions(salary):
    """Calculates and returns salary deductions."""
    # Constants
    SSS_CONTRIBUTION = 1200
    PHILHEALTH_RATE = 0.05
    PAGIBIG_CONTRIBUTION = 100
    TAX = 1875  # Fixed tax for simplicity

    # Compute deductions
    philhealth = (salary * PHILHEALTH_RATE) / 2
    total_deductions = SSS_CONTRIBUTION + philhealth + PAGIBIG_CONTRIBUTION + TAX
    net_salary = salary - total_deductions

    return {
        "Gross Salary": salary,
        "SSS Deduction": SSS_CONTRIBUTION,
        "PhilHealth Deduction": round(philhealth, 2),
        "Pag-IBIG Deduction": PAGIBIG_CONTRIBUTION,
        "Tax Deduction": TAX,
        "Total Deductions": round(total_deductions, 2),
        "Net Salary": round(net_salary, 2)
    }

def get_valid_salary():
    """Prompts user for salary input with validation."""
    while True:
        try:
            salary = float(input("Enter your monthly salary: "))
            if salary <= 0:
                print("Error: Salary must be greater than zero. Please try again.")
            else:
                return salary
        except ValueError:
            print("Invalid input. Please enter a numeric value.")

    def display_deductions(deductions):
    """Displays salary breakdown in a readable format."""
    print("\n=== Salary Breakdown ===")
    for key, value in deductions.items():
        print(f"{key}: {value:,.2f}")

    def main():
    """Main function to execute the program."""
    salary = get_valid_salary()
    deductions = compute_deductions(salary)
    display_deductions(deductions)

    Run the program
    if __name__ == "__main__":
    main()
# Key improvements

Separated input validation (get_valid_salary())
 Clear function responsibilities (compute_deductions(), display_deductions())
 Formatted currency output (e.g., 1,500.00 instead of 1500.0)
 Error handling for zero or negative salary
























