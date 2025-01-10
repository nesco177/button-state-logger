from openpyxl import Workbook, load_workbook
import os
import time
from datetime import datetime

EXCEL_FILE = "data.xlsx"

# Function to get the last state from the Excel file
def get_last_state():
    if not os.path.exists(EXCEL_FILE):
        # Create the file if it doesn't exist
        wb = Workbook()
        ws = wb.active
        ws.append(["Time", "State"])  # Add header
        ws.append(["", "On"])  # Default initial state
        wb.save(EXCEL_FILE)

    wb = load_workbook(EXCEL_FILE)
    ws = wb.active
    last_row = ws.max_row
    return ws.cell(row=last_row, column=2).value

# Function to log a new state into the Excel file
def log_state(new_state):
    wb = load_workbook(EXCEL_FILE)
    ws = wb.active
    current_time = datetime.now().strftime("%I:%M:%S %p")  # 12-hour time format
    ws.append([current_time, new_state])
    wb.save(EXCEL_FILE)
    print(f"Logged: {current_time}, {new_state}")

# Main function to toggle the state and log if changed
def main():
    last_state = get_last_state()

    # Toggle the state
    new_state = "Off" if last_state == "On" else "On"

    if new_state != last_state:
        log_state(new_state)

if __name__ == "__main__":
    main()
