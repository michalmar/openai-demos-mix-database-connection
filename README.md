# openai-demos-mix-database-connection

This repository contains a sample Azure Functions project that demonstrates how to manage products using HTTP triggers and SQL bindings.

## Project Structure
```
. 
├── .funcignore 
├── .gitignore 
├── .pylintrc 
├── .vscode/ 
│ ├── extensions.json 
│ ├── launch.json 
│ ├── settings.json 
│ └── tasks.json 
├── function_app.py 
├── host.json 
├── local.settings.json 
├── README.md 
├── requirements.txt
```

### Files

- `function_app.py`: Contains the Azure Functions and their bindings.
- `host.json`: Configuration file for the function app host.
- `local.settings.json`: Local settings for the function app, including connection strings.
- `requirements.txt`: Python dependencies for the project.
- `.vscode/`: Contains VS Code configuration files for debugging and tasks.

## Functions

### GetProducts

Fetches product details based on the provided product ID.

- **Route**: `getproducts/{id}`
- **SQL Input Binding**: Executes the query `SELECT ProductID, Name, ProductNumber FROM [SalesLT].[Product] WHERE ProductID = @ID` with the `id` parameter from the URL.

```python
@app.function_name(name="GetProducts")
@app.route(route="getproducts/{id}")
@app.sql_input(arg_name="products",
               command_text="SELECT ProductID, Name, ProductNumber FROM [SalesLT].[Product] WHERE ProductID = @ID",
               command_type="Text",
               parameters="@ID={id}",
               connection_string_setting="SqlConnectionString")
def get_products(req: func.HttpRequest, products: func.SqlRowList) -> func.HttpResponse:
    rows = list(map(lambda r: json.loads(r.to_json()), products))
    return func.HttpResponse(
        json.dumps(rows),
        status_code=200,
        mimetype="application/json"
    )

```

## Setup
1. Install Dependencies: Ensure you have Python and pip installed. Run the following command to install the required packages:

```shell
pip install -r requirements.txt
```
1. Configure Local Settings: Update local.settings.json with your SQL connection string and other necessary settings.

1. Run the Function App: Use the following command to start the function app locally:

```shell
func start
```

## Debugging
To debug the Azure Functions in Visual Studio Code, use the provided configuration in .vscode/launch.json. Ensure the Azure Functions Core Tools are installed.

## License
This project is licensed under the MIT License. See the LICENSE file for more details.

