# API-Data-Analysis-Using-Python-

# Day 1: API Data Analysis Using Python Structures

Fetches customer data from a public API, explores its nested structure, flattens it, counts customers by city and email domain, loads the result into a pandas DataFrame, exports CSVs and charts the distributions.

**Data source:** https://jsonplaceholder.typicode.com/users (public, no API key needed)

## Requirements

```bash
pip install requests pandas matplotlib
```

## How to run

Run the notebook/script top to bottom. The `__main__` block fetches the data and prints the result of each step. Two CSV files are written next to the script, and the charts are displayed on screen.

## Functions

| Function | Purpose |
|---|---|
| `get_customers(url)` | Fetches the customers with a 10-second timeout, checks the status code and returns the parsed list. On a network or HTTP error it prints a clear message and returns an empty list. |
| `explore(record, indent)` | Recursively prints every key of one record with its Python type, indented by depth. |
| `extract_attributes(customers)` | Returns one flat dict per customer: id, name, username, email, email domain, phone, city, zipcode, latitude, longitude, company, website. Latitude and longitude are floats. |
| `unique_cities(customers)` | Returns the set of distinct cities (set comprehension). |
| `city_counts(customers)` | Counts customers per city with a loop (no `Counter`, no pandas). |
| `email_domain_counts(customers)` | Counts customers per email top-level domain with a dict comprehension. |
| `to_dataframe(rows)` | Converts the flat dicts into a pandas DataFrame. |
| `plot_distribution(counts, title)` | Displays a labelled bar chart of a count dictionary. |

## Step 2: Nested attributes

Each customer record is a dictionary. Three attributes are nested:

- `address` (dict): contains `street`, `suite`, `city`, `zipcode` and `geo`
- `geo` (dict, inside `address`): contains `lat` and `lng`
- `company` (dict): contains `name`, `catchPhrase` and `bs`

The deepest attributes are `lat` and `lng`, three levels down:

```python
customer["address"]["geo"]["lat"]
customer["address"]["geo"]["lng"]
```

The API returns them as strings, so `extract_attributes` converts them with `float()`.

## Step 4: Unique cities vs customers

There are 10 unique cities for 10 customers, which means no two customers share a city, so city is too granular to segment customers for marketing.

## Step 6: pandas check

`value_counts()` on the DataFrame gives the same city and email-domain counts as the hand-written functions (both comparisons print `True`). The DataFrame's shape and dtypes are printed by the script; `latitude` and `longitude` are `float64`.

## Step 7: Exports

- `customers.csv`: one row per customer with all flat attributes
- `city_counts.csv`: columns `city`, `customers`

Both are written without the pandas index (`index=False`).

## Step 8: Charts

Two charts are displayed: customers per city, and customers per email domain (top-level domain).

**The email domain chart is more useful.** Every city appears exactly once, so the city chart is a row of equal bars and shows no pattern. The email domain chart has bars of different heights, so it shows which groups are larger and can support segmentation.

## Error handling

- A broken URL or HTTP error prints a short message and returns an empty list, with no traceback.
- The 10-second timeout stops the program from hanging if the server does not respond.
