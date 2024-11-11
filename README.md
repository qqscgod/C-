using System;
using System.Collections.Generic;

namespace BudgetPlanner
{
    public class Budget
    {
        public double Limit { get; set; }
        private List<Expense> expenses;

        public Budget(double limit)
        {
            Limit = limit;
            expenses = new List<Expense>();
        }

        public void AddExpense(string category, double amount)
        {
            Expense newExpense = new Expense(category, amount, DateTime.Now);
            expenses.Add(newExpense);
        }

        public void DeleteExpense(int index)
        {
            if (index >= 0 && index < expenses.Count)
            {
                expenses.RemoveAt(index);
                Console.WriteLine("Expense removed successfully.");
            }
            else
            {
                Console.WriteLine("Invalid expense index.");
            }
        }

        public void ViewExpenses()
        {
            Console.WriteLine("\nExpenses:");
            for (int i = 0; i < expenses.Count; i++)
            {
                Console.WriteLine($"{i + 1}. {expenses[i]}");
            }
        }

        public double GetTotalExpenses()
        {
            double total = 0;
            foreach (var expense in expenses)
            {
                total += expense.Amount;
            }
            return total;
        }

        public double GetRemainingBudget()
        {
            return Limit - GetTotalExpenses();
        }
    }
}
using System;

namespace BudgetPlanner
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Welcome to the Budget Planner!");

            // Set initial budget limit
            Console.Write("Enter your budget limit: $");
            double budgetLimit = Convert.ToDouble(Console.ReadLine());
            Budget budget = new Budget(budgetLimit);

            while (true)
            {
                Console.Clear();
                Console.WriteLine("===== Budget Planner =====");
                Console.WriteLine("1. View Expenses");
                Console.WriteLine("2. Add Expense");
                Console.WriteLine("3. Delete Expense");
                Console.WriteLine("4. Check Remaining Budget");
                Console.WriteLine("5. Exit");
                Console.Write("Choose an option: ");
                
                string choice = Console.ReadLine();

                switch (choice)
                {
                    case "1":
                        budget.ViewExpenses();
                        Console.WriteLine($"\nTotal Expenses: ${budget.GetTotalExpenses()}");
                        Console.WriteLine($"Remaining Budget: ${budget.GetRemainingBudget()}");
                        Console.WriteLine("Press Enter to return to the menu...");
                        Console.ReadLine();
                        break;

                    case "2":
                        Console.Write("Enter the category of the expense: ");
                        string category = Console.ReadLine();
                        Console.Write("Enter the amount: $");
                        double amount = Convert.ToDouble(Console.ReadLine());

                        budget.AddExpense(category, amount);
                        Console.WriteLine("Expense added successfully!");
                        Console.WriteLine("Press Enter to return to the menu...");
                        Console.ReadLine();
                        break;

                    case "3":
                        budget.ViewExpenses();
                        Console.Write("Enter the number of the expense to delete: ");
                        int index = Convert.ToInt32(Console.ReadLine()) - 1;

                        budget.DeleteExpense(index);
                        Console.WriteLine("Press Enter to return to the menu...");
                        Console.ReadLine();
                        break;

                    case "4":
                        Console.WriteLine($"\nRemaining Budget: ${budget.GetRemainingBudget()}");
                        Console.WriteLine("Press Enter to return to the menu...");
                        Console.ReadLine();
                        break;

                    case "5":
                        Console.WriteLine("Thank you for using the Budget Planner!");
                        return;

                    default:
                        Console.WriteLine("Invalid option. Please try again.");
                        Console.ReadLine();
                        break;
                }
            }
        }
    }
}
