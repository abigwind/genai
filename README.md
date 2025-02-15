
# Order Processing System
This project is designed to automatically process email order requests from customers, check product availability, update stock levels, and log the order status into a Google Sheet. The system processes orders by extracting relevant information (product ID and quantity) from email messages and determining whether the requested products are available in stock.

## Features
* Extract product information (ID and quantity) from customer emails.
* Verify product availability in stock.
* Fulfill orders and adjust stock levels.
* Log each order's status (created or out of stock) into a Google Sheet for tracking.
* Handle edge cases, such as missing product IDs or insufficient stock.

## Requirements

Python Packages:
* pandas: For handling data manipulation.
* re: To perform regular expression-based parsing of product information.
* gspread: For working with Google Sheets.
* oauth2client: To handle authentication with Google Sheets.
* numpy: For numerical operations (if needed in stock handling).
* faiss-cpu: For any similarity or vector search needs (optional for order request expansion).

You can install the required packages via pip:

pip install pandas gspread oauth2client numpy faiss-cpu

## Google Sheets API Setup:
* You'll need to create a Google API project and enable the Google Sheets API.
* Download your credentials JSON file and rename it (e.g., credentials.json).
* Share your Google Sheet with the service account email generated by Google.

## Usage
## 1. Setup
* Create a Google Sheet with two tabs:

    * Stock sheet: This should have product information, including product IDs and stock levels.
    * Order-status sheet: This will store the result of the processed orders with columns:
        * email_id: Unique identifier for each email.
        * product_id: The ID of the ordered product.
        * quantity: Quantity requested.
        * status: Either "created" (if the order was fulfilled) or "out of stock" (if stock was insufficient).
* Ensure you have a DataFrame called emails_df containing the following columns:

email_id: Unique identifier for the email.
subject: Subject of the email.
message: Body of the email, where product requests are detailed.

## 2. Code Structure

## Key Functions:
* extract_product_info(message): Uses regex to extract product ID and quantity from the email body.
* process_order(email_id, subject, message): Processes the order by verifying stock and updating order status.
* order_status_sheet.append_row([email_id, product_id, quantity, status]): Updates the Google Sheet with the order processing result.
Example Data:
python
Copy code
###    Example of an email in the emails_df
    emails_df = pd.DataFrame({
    'email_id': ['E001'],
    'subject': ['Leather Wallets'],
    'message': ['Hi there, I want to order all the remaining LTH0976 Leather Bifold Wallets you have in stock.']
})

##

###   Example of stock data in stock_df
    stock_df = pd.DataFrame({
    'product_id': ['LTH0976'],
    'stock': [10]
})
## 3. Running the Script
Once the environment is set up and the stock sheet is loaded into stock_df, you can run the script to process emails:


### Example loop to process emails
    for index, email in emails_df.iterrows():
        email_id = email['email_id']
        subject = email['subject']
        message = email['message']
    
        process_order(email_id, subject, message)
The system will log the order status in the order-status sheet on Google Sheets.

## 4. Error Handling
* If a product ID cannot be found in the email, the order is skipped and a message is printed.
* If a product is out of stock, the system logs it as "out of stock" in the Google Sheet.


## Customization
* You can modify the regex inside extract_product_info to fit your product ID format better.
* Adapt the stock update logic if your inventory handling differs from the basic decrement model.

## Future Improvements
* Add support for multiple products per email.
* Implement notifications for out-of-stock products.
* Integrate with other systems (e.g., order management systems or ERPs).