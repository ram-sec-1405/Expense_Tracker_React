# Expense Tracker (ReactJS)
## Date:24/05/2025

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM
## app.js
```
import React, { useState } from "react";
import "./App.css";

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState("");
  const [amount, setAmount] = useState("");

  const handleAddTransaction = (e) => {
    e.preventDefault();
    if (!description || !amount) return;

    const newTransaction = {
      id: Date.now(),
      description,
      amount: parseFloat(amount),
    };

    setTransactions([newTransaction, ...transactions]);
    setDescription("");
    setAmount("");
  };

  const handleDelete = (id) => {
    setTransactions(transactions.filter((t) => t.id !== id));
  };

  const balance = transactions.reduce((acc, transaction) => acc + transaction.amount, 0);

  const income = transactions
    .filter((t) => t.amount > 0)
    .reduce((acc, t) => acc + t.amount, 0)
    .toFixed(2);
  const expense = (
    transactions
      .filter((t) => t.amount < 0)
      .reduce((acc, t) => acc + t.amount, 0) * -1
  ).toFixed(2);

  return (
    <div className="container">
      <h2>Expense Tracker</h2>

      
      <div className="balance-box">
        <h3>Your Balance</h3>
        <h1>${balance.toFixed(2)}</h1>
      </div>


      <div className="summary">
        <div className="income">
          <h4>Income</h4>
          <p className="money plus">+${income}</p>
        </div>
        <div className="expense">
          <h4>Expense</h4>
          <p className="money minus">-${expense}</p>
        </div>
      </div>

      <h3 className="add-transaction-heading">Transaction</h3>

      <ul className="list">
        {transactions.map((t) => (
          <li
            key={t.id}
            className={t.amount < 0 ? "minus" : "plus"}
          >
            {t.description}
            <span>{t.amount < 0 ? "-" : "+"}${Math.abs(t.amount)}</span>
            <button onClick={() => handleDelete(t.id)} className="delete-btn">
              x
            </button>
          </li>
        ))}
      </ul>

      <h3 className="add-transaction-heading">Add New Transaction</h3>
      <form onSubmit={handleAddTransaction}>
        <div className="form-control">
          <label>Description</label>
          <input
            type="text"
            placeholder="Enter text..."
            value={description}
            onChange={(e) => setDescription(e.target.value)}
          />
        </div>
        <div className="form-control">
          <label>Amount <br /> (positive = income or negative = expense)</label>
          <input
            type="number"
            placeholder="Enter amount..."
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
          />
        </div>
        <button className="btn">Add Transaction by clicking here</button>
      </form>
    </div>
  );
}

export default App;
```
## app.css
```

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  background-color: rgb(118, 193, 220);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 20px;
}


.container {
  width: 100%;
  max-width: 600px;
  background: black;
  padding: 20px 18px;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(102, 57, 12, 0.15);
  display: grid;
  gap: 18px;
}



h2 {
  text-align: center;
  font-size: 28px;
  font-weight: bold;
  color: white;
  text-decoration: underline;
}

.balance-box {
  background: #f0f8ff;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  width: 100%; 
}
.balance-box {
  margin-top: 10px;
}


.balance-box h3 {
  color: black;
  font-size: 18px;
  margin-bottom: 5px;
}

.balance-box h1 {
  font-size: 36px;
  color: black;
}


.summary {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
  background: #f4f6fa;
  padding: 15px;
  border-radius: 12px;
  box-shadow: inset 0 0 5px rgb(219, 17, 17);
}

.income,
.expense {
  text-align: center;
  padding: 10px;
  border-radius: 8px;
}

.income {
  background: #e9f9f1;
  border-left: 5px solid green;
}

.expense {
  background: #fdecea;
  border-left: 5px solid red;
}

.money {
  font-size: 20px;
  font-weight: bold;
  margin-top: 5px;
  
}

.money.plus {
  color: #2ecc71;
}

.money.minus {
  color: #c0392b;
}


.list {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.list li {
  display: grid;
  grid-template-columns: 1fr auto auto;
  align-items: center;
  gap: 10px;
  background: #f9f9f9;
  color: black;
  padding: 12px 16px;
  border-radius: 10px;
  margin-bottom: 12px;
  border-left: 6px solid;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.list li.plus {
  border-color: green;
}

.list li.minus {
  border-color: red;
}

.list li span {
  font-weight: bold;
  color: white;
}


.delete-btn {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  font-size: 16px;
  cursor: pointer;
  transition: 0.3s ease;
}

.delete-btn:hover {
  background: #c0392b;
}


form {
  display: grid;
  gap: 15px;
}

.form-control label {
  display: block;
  margin-bottom: 6px;
  font-weight: bold;
  color : white;
}

.form-control input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.3s ease;
}

.form-control input:focus {
  border-color: #2e86de;
  outline: none;
}


.btn {
  width: 100%;
  padding: 12px;
  background: linear-gradient(to right, #6a11cb, #2575fc);
  color: black;
  font-size: 16px;
  border: none;
  border-radius: 10px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}

.btn:hover {
  background: linear-gradient(to right, #2575fc, #6a11cb);
}


@media (max-width: 480px) {
  .container {
    padding: 20px;
  }

  .summary {
    grid-template-columns: 1fr;
  }

  .list li {
    grid-template-columns: 1fr auto;
    grid-template-areas: 
      "desc amount"
      "btn btn";
  }

  .delete-btn {
    justify-self: end;
  }
}
.add-transaction-heading {
  color: white; /* Example: blue */
}

```

## OUTPUT

![ram12](https://github.com/user-attachments/assets/89f71913-81e5-4c8f-9fad-2a776bb86731)

![image](https://github.com/user-attachments/assets/476ab248-72c1-48f6-9b6a-5726928a86e9)





## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
