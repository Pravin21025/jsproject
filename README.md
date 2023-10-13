const expenseList = document.querySelector("#expense-list");
const expenseForm = document.querySelector("#expense-form");
const expenseInput = document.querySelector("#expense-input");
const amountInput = document.querySelector("#amount-input");
const totalExpense = document.querySelector("#total-expense");

let expenses = [];
let total = 0;

// Add an expense to the list
function addExpense(description, amount) {
  const expense = {
    id: new Date().getTime(),
    description,
    amount: parseFloat(amount),
  };
  expenses.push(expense);
}

// Update the total expense
function updateTotal() {
  total = expenses.reduce((acc, expense) => acc + expense.amount, 0);
  totalExpense.textContent = `Total: $${total.toFixed(2)}`;
}

// Render the list of expenses
function renderExpenses() {
  expenseList.innerHTML = "";
  expenses.forEach((expense) => {
    const listItem = document.createElement("li");
    listItem.innerHTML = `
      ${expense.description}: $${expense.amount.toFixed(2)}
      <button data-id="${expense.id}" class="delete-button">Delete</button>
    `;
    expenseList.appendChild(listItem);
  });
  updateTotal();
}

// Handle form submission
expenseForm.addEventListener("submit", (e) => {
  e.preventDefault();
  const description = expenseInput.value;
  const amount = amountInput.value;
  if (description && amount) {
    addExpense(description, amount);
    renderExpenses();
    expenseInput.value = "";
    amountInput.value = "";
  }
});

// Handle expense deletion
expenseList.addEventListener("click", (e) => {
  if (e.target.classList.contains("delete-button")) {
    const id = parseInt(e.target.getAttribute("data-id"));
    expenses = expenses.filter((expense) => expense.id !== id);
    renderExpenses();
  }
});

// Initialize the app
renderExpenses();
