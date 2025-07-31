---
description: 'Comprehensive Python coding conventions and best practices'
applyTo: '**/*.py'
---

# Python Coding Conventions & Best Practices

## Core Programming Principles

### Code Clarity and Readability
- **Prioritize readability over cleverness** - code is read more often than it's written
- Write self-documenting code with descriptive variable and function names
- Use clear, concise comments to explain the "why", not the "what"
- Structure code logically with consistent patterns throughout the project

### Function Design
- Write **pure functions** when possible (no side effects, predictable outputs)
- Follow the **Single Responsibility Principle** - each function should do one thing well
- Keep functions small and focused (ideally under 20 lines)
- Use descriptive names that clearly indicate the function's purpose
- Always include comprehensive type hints for parameters and return values

### Documentation Standards
- Provide docstrings following **PEP 257** conventions for all public functions and classes
- Include parameter descriptions, return value details, and usage examples
- Document any exceptions that may be raised
- Add inline comments for complex logic or business rules

## Type Annotations and Modern Python

### Type Hints Best Practices
- Use the `typing` module for complex type annotations
- Prefer built-in generics for Python 3.9+ (e.g., `list[str]` instead of `List[str]`)
- Use `Union` types sparingly; consider redesigning if you have many unions
- Always annotate function parameters and return types
- Use `Optional[T]` for nullable values

```python
from typing import Optional, Dict, List, Union
from datetime import datetime

def process_user_data(
    users: List[Dict[str, Union[str, int]]], 
    filter_active: bool = True
) -> Optional[Dict[str, int]]:
    """Process user data and return summary statistics."""
    pass
```

### Error Handling and Validation
- Use specific exception types rather than generic `Exception`
- Implement proper error handling with meaningful error messages
- Validate inputs early and fail fast
- Use `assert` statements for debugging, not for user input validation
- Consider using libraries like `pydantic` for data validation

## Code Style and Formatting

### PEP 8 Compliance
- Follow the **PEP 8** style guide strictly for consistency
- Use 4 spaces for indentation (never tabs)
- Limit lines to **88 characters** (Black formatter standard) or 79 for strict PEP 8
- Use meaningful blank lines to separate logical sections

### Naming Conventions
- **Variables and functions**: `snake_case` (e.g., `user_name`, `calculate_total`)
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `MAX_RETRIES`, `DEFAULT_TIMEOUT`)
- **Classes**: `PascalCase` (e.g., `UserManager`, `DataProcessor`)
- **Private methods/attributes**: prefix with single underscore `_private_method`
- **Module-level "private" items**: prefix with single underscore

### Import Organization
```python
# Standard library imports
import os
import sys
from datetime import datetime
from pathlib import Path

# Third-party imports
import requests
import pandas as pd
from sqlalchemy import create_engine

# Local application imports
from .models import User
from .utils import format_date
```

### String Formatting
- Prefer **f-strings** for string formatting (Python 3.6+)
- Use `.format()` for complex formatting or when f-strings aren't suitable
- Avoid old-style `%` formatting

```python
# Preferred
name = "Alice"
age = 30
message = f"Hello {name}, you are {age} years old"

# Complex formatting
template = "Value: {value:.2f}, Status: {status!r}"
result = template.format(value=3.14159, status="active")
```

## Documentation and Examples

### Comprehensive Function Documentation

```python
from typing import List, Optional, Dict, Any
from datetime import datetime

def process_user_transactions(
    user_id: int,
    transactions: List[Dict[str, Any]],
    start_date: Optional[datetime] = None,
    include_pending: bool = False
) -> Dict[str, float]:
    """
    Process and aggregate user transactions within a specified timeframe.
    
    This function calculates total amounts, fees, and net values for a user's
    transactions. It can filter by date range and include/exclude pending
    transactions based on business requirements.
    
    Args:
        user_id (int): The unique identifier for the user
        transactions (List[Dict[str, Any]]): List of transaction dictionaries
            containing 'amount', 'fee', 'status', and 'date' keys
        start_date (Optional[datetime], optional): Filter transactions from this
            date onwards. If None, includes all transactions. Defaults to None.
        include_pending (bool, optional): Whether to include transactions with
            'pending' status. Defaults to False.
    
    Returns:
        Dict[str, float]: Dictionary containing:
            - 'total_amount': Sum of all transaction amounts
            - 'total_fees': Sum of all fees
            - 'net_amount': Total amount minus total fees
            - 'transaction_count': Number of processed transactions
    
    Raises:
        ValueError: If user_id is not positive or transactions list is empty
        KeyError: If required transaction fields are missing
        TypeError: If transaction amounts cannot be converted to float
    
    Example:
        >>> from datetime import datetime
        >>> transactions = [
        ...     {'amount': 100.0, 'fee': 2.5, 'status': 'completed', 'date': datetime.now()},
        ...     {'amount': 50.0, 'fee': 1.0, 'status': 'completed', 'date': datetime.now()}
        ... ]
        >>> result = process_user_transactions(123, transactions)
        >>> print(result)
        {'total_amount': 150.0, 'total_fees': 3.5, 'net_amount': 146.5, 'transaction_count': 2}
    
    Note:
        This function assumes all monetary values are in the same currency.
        Date filtering uses datetime objects for precise comparison.
    """
    if user_id <= 0:
        raise ValueError("User ID must be a positive integer")
    
    if not transactions:
        raise ValueError("Transactions list cannot be empty")
    
    # Filter transactions based on criteria
    filtered_transactions = []
    for transaction in transactions:
        # Validate required fields
        required_fields = ['amount', 'fee', 'status', 'date']
        if not all(field in transaction for field in required_fields):
            raise KeyError(f"Transaction missing required fields: {required_fields}")
        
        # Apply date filter
        if start_date and transaction['date'] < start_date:
            continue
            
        # Apply status filter
        if not include_pending and transaction['status'] == 'pending':
            continue
            
        filtered_transactions.append(transaction)
    
    # Calculate aggregations
    try:
        total_amount = sum(float(t['amount']) for t in filtered_transactions)
        total_fees = sum(float(t['fee']) for t in filtered_transactions)
    except (ValueError, TypeError) as e:
        raise TypeError(f"Invalid amount or fee value: {e}")
    
    return {
        'total_amount': total_amount,
        'total_fees': total_fees,
        'net_amount': total_amount - total_fees,
        'transaction_count': len(filtered_transactions)
    }
```

### Class Documentation Example

```python
class DataProcessor:
    """
    A utility class for processing and transforming data from various sources.
    
    This class provides methods for cleaning, validating, and transforming
    data while maintaining processing statistics and error logs.
    
    Attributes:
        processed_count (int): Number of successfully processed records
        error_count (int): Number of records that failed processing
        errors (List[str]): List of error messages encountered during processing
    
    Example:
        >>> processor = DataProcessor()
        >>> cleaned_data = processor.clean_data(raw_data)
        >>> print(f"Processed: {processor.processed_count}, Errors: {processor.error_count}")
    """
    
    def __init__(self) -> None:
        """Initialize the DataProcessor with empty statistics."""
        self.processed_count: int = 0
        self.error_count: int = 0
        self.errors: List[str] = []
    
    def reset_statistics(self) -> None:
        """Reset all processing statistics and error logs."""
        self.processed_count = 0
        self.error_count = 0
        self.errors.clear()
```

## Performance and Best Practices

### Memory and Performance Optimization
- Use **generators** for large datasets instead of creating lists in memory
- Leverage **list comprehensions** and **generator expressions** for concise, readable code
- Use **set operations** for membership testing and deduplication
- Consider **dataclasses** or **NamedTuple** for structured data

```python
# Generator for memory efficiency
def process_large_dataset(data_source):
    """Process large dataset efficiently using generators."""
    for item in data_source:
        if item.is_valid():
            yield transform_item(item)

# List comprehension for readability
valid_items = [item for item in data if item.status == 'active']

# Set operations for performance
user_ids = {user.id for user in users}
if target_id in user_ids:  # O(1) lookup
    process_user(target_id)
```

### Dependency Management
- Use **virtual environments** (venv, conda, poetry) for project isolation
- Pin dependency versions in `requirements.txt` or `pyproject.toml`
- Document external dependencies and their purposes
- Regularly update dependencies and test for compatibility

### Security Considerations
- **Never hardcode secrets** - use environment variables or secret management
- **Validate all user inputs** to prevent injection attacks
- **Use parameterized queries** for database operations
- **Log security events** without exposing sensitive information

