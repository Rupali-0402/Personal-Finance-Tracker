def main():
    tracker = FinanceTracker()

    print("Welcome to Personal Finance Tracker!")
    filename = "finance_data.json"

    # Try loading existing data
    try:
        tracker.load_from_file(filename)
        print(f"Loaded {len(tracker.transactions)} transactions from {filename}.")
    except FileNotFoundError:
        print(f"No existing data file found, starting fresh.")

    while True:
        print("\nOptions:")
        print("1. Add Transaction")
        print("2. Show Expenses Over $100")
        print("3. Show All Transactions Sorted by Date")
        print("4. Search Transactions")
        print("5. Show Monthly Spending Bar Chart")
        print("6. Save and Exit")
        choice = input("Choose an option (1-6): ")

        if choice == '1':
            date = input("Enter date (YYYY-MM-DD): ")
            category = input("Enter category (e.g., Food, Salary): ")
            amount = float(input("Enter amount: "))
            trans_type = input("Enter type (income/expense): ").lower()
            description = input("Enter description (optional): ")
            tracker.add_transaction(Transaction(date, category, amount, trans_type, description))
            print("Transaction added.")

        elif choice == '2':
            filtered = tracker.filter_expenses_over(100)
            if filtered:
                print("Expenses over $100:")
                for t in filtered:
                    print(f"{t.date} - {t.category} - ${t.amount:.2f} - {t.description}")
            else:
                print("No expenses over $100 found.")

        elif choice == '3':
            tracker.sort_transactions_by_date()
            for t in tracker.transactions:
                print(f"{t.date} | {t.trans_type} | {t.category} | ${t.amount:.2f} | {t.description}")

        elif choice == '4':
            keyword = input("Enter keyword to search: ")
            results = tracker.search_transactions(keyword)
            if results:
                for t in results:
                    print(f"{t.date} | {t.trans_type} | {t.category} | ${t.amount:.2f} | {t.description}")
            else:
                print("No matching transactions found.")

        elif choice == '5':
            tracker.display_ascii_bar_chart()

        elif choice == '6':
            tracker.save_to_file(filename)
            print(f"Data saved to {filename}. Goodbye!")
            break

        else:
            print("Invalid option. Try again.")

if __name__ == "__main__":
    main()
