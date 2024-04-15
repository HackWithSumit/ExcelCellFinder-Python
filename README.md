import pandas as pd

def convert_date_format(input_path):
    try:
        # Read the Excel file
        df = pd.read_excel(input_path)
        
        # Find the column containing 'last login' or similar in the column name
        date_column = None
        for col in df.columns:
            if 'last login' in str(col).lower():
                date_column = col
                break
        
        if date_column is None:
            print("Column containing 'last login' not found in the Excel sheet.")
            return
        
        # Convert the date column to datetime format (handling different date formats)
        df[date_column] = pd.to_datetime(df[date_column], errors='coerce')
        
        # Convert the datetime column to desired date format (dd-mm-yyyy)
        df[date_column] = df[date_column].dt.strftime('%d-%m-%Y')
        
        # Save the updated DataFrame back to Excel
        output_path = input_path.split('.')[0] + '_formatted.xlsx'
        df.to_excel(output_path, index=False)
        
        print(f"Date column '{date_column}' formatted and saved to '{output_path}'.")
        
    except Exception as e:
        print(f"An error occurred: {e}")

# Ask user for Excel file path
excel_path = input("Enter the path of the Excel file: ")

# Call the function to convert date format
convert_date_format(excel_path)

