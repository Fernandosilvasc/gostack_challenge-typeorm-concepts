<img alt="GoStack" src="https://storage.googleapis.com/golden-wind/bootcamp-gostack/header-desafios-new.png" />

<h3 align="center">
  Challenge 06: Database and file upload on Node.js
</h3>

<p align="center">
 <img src="https://img.shields.io/static/v1?label=PRs&message=welcome&color=#FE7F2D&labelColor=#FE7F2D" alt="PRs welcome!" />

  <img alt="License" src="https://img.shields.io/static/v1?label=license&message=MIT&color=#FE7F2D&labelColor=#FE7F2D">
</p>


<p align="center">
  <a href="#rocket-about-the-challenge">About the challenge</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#memo-licence">Licence</a>
</p>


## :rocket: About the challenge

This challenge is a complement of the last challenge I have done before using NodeJS and TypeScript. The link to access it will be available below.

In this challenge was necessary implement the database with TypeORM and upload files using Multer as handler!

This will be an application that should store incoming and outgoing financial transactions and allow the registration and listing of these transactions, in addition to allowing the creation of new records in the database by sending a csv file.


### Application Routes

This was requested through that challenge.

- **`POST / transactions`**: The route must receive` title`, `value`,` type`, and `category` within the request body, with` type` being the type of the transaction, which must be income for incoming (deposits) and outcome for outgoing (withdrawn). When registering a new transaction, it must be stored within your database, with the fields `id`,` title`, `value`,` type`, `category_id`,` created_at`, `updated_at`.

**Tip**: For the category, you must create a new table, which will have the fields `id`,` title`, `created_at`,` updated_at`.

**Tip 2**: Before creating a new category, always check if a category with the same title already exists. If it exists, use the `id` already in the database.

```json
{
  "id": "uuid",
  "title": "Paycheck",
  "value": 3000,
  "type": "income",
  "category": "Salary"
}
```

- **`GET / transactions`**: This route should return a listing with all the transactions that you have registered so far, together with the sum of the entries, withdrawals and total credit. This route must return an object in the following format:

```json
{
  "transactions": [
    {
      "id": "uuid",
      "title": "Paycheck",
      "value": 4000,
      "type": "income",
      "category": {
        "id": "uuid",
        "title": "Salary",
        "created_at": "2020-04-20T00:00:49.620Z",
        "updated_at": "2020-04-20T00:00:49.620Z"
      },
      "created_at": "2020-04-20T00:00:49.620Z",
      "updated_at": "2020-04-20T00:00:49.620Z"
    },
    {
      "id": "uuid",
      "title": "Freelance Project",
      "value": 2000,
      "type": "income",
      "category": {
        "id": "uuid",
        "title": "Others",
        "created_at": "2020-04-20T00:00:49.620Z",
        "updated_at": "2020-04-20T00:00:49.620Z"
      },
      "created_at": "2020-04-20T00:00:49.620Z",
      "updated_at": "2020-04-20T00:00:49.620Z"
    },
    {
      "id": "uuid",
      "title": "Bills",
      "value": 4000,
      "type": "outcome",
      "category": {
        "id": "uuid",
        "title": "Others",
        "created_at": "2020-04-20T00:00:49.620Z",
        "updated_at": "2020-04-20T00:00:49.620Z"
      },
      "created_at": "2020-04-20T00:00:49.620Z",
      "updated_at": "2020-04-20T00:00:49.620Z"
    },
    {
      "id": "uuid",
      "title": "Gamer Chair",
      "value": 1200,
      "type": "outcome",
      "category": {
        "id": "uuid",
        "title": "extra",
        "created_at": "2020-04-20T00:00:49.620Z",
        "updated_at": "2020-04-20T00:00:49.620Z"
      },
      "created_at": "2020-04-20T00:00:49.620Z",
      "updated_at": "2020-04-20T00:00:49.620Z"
    }
  ],
  "balance": {
    "income": 6000,
    "outcome": 5200,
    "total": 800
  }
}
```

**Tip**: Within balance, income is the sum of all transaction values with `type` income. The outcome is the sum of all transaction values with `type` outcome, and the total is the value of `income - outcome`.

**Tip 2**: To sum up the values, you can use the function [reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) to group transactions by the `type` property, so you will be able to add up all the values easily and get the return of` balance`.

- **`DELETE / transactions /: id`**: The route must delete a transaction with the `id` present in the route parameters;

- **`POST / transactions / import`**: The route must allow the import of a file in `.csv` format containing the same information needed to create a transaction `id`,`title`,`value`, `type`,` category_id`, `created_at`,` updated_at`, where each line of the CSV file must be a new record for the database, and finally return all the `transactions` that have been imported into your database . The csv file, must follow the following [template](./assets/file.csv)


### Tests Specification

To complete this challenge was necessary to follow these rules:

<h4 align="center">
  ⚠️  Before running the tests, create a database with the name "gostack_desafio06_tests" so that all tests can run correctly  ⚠️
</h4>

- **`should be able to create a new transaction`**: For this test to pass, your application must allow a transaction to be created, and return a json with the created transaction.

- **`should create tags when inserting new transactions`**: For this test to pass, your application must allow that when creating a new transaction with a category that does not exist, it is created and inserted in the category_id field of the transaction with the `id` just created.

- **`should not create tags when they already exists`**: For this test to pass, your application must allow when creating a new transaction with a category that already exists, to be assigned to the category_id field of the transaction with the `id` that existing category, not allowing the creation of categories with the same `title`.

- **`should be able to list the transactions`**: For this test to pass, your application must allow an array of objects to be returned containing all transactions along with the balance of income, outcome and total transactions that have been created so far.

- **`should not be able to create outcome transaction without a valid balance`**: For this test to pass, your application must not allow a transaction of the`outcome` type to exceed the total amount that the user has in cash ( total income), returning a response with HTTP 400 code and an error message in the following format: `{error: string}`.

- **`should be able to delete a transaction`**: In order for this test to pass, you must allow your delete route to delete a transaction, and when doing the deletion, it returns an empty response, with status 204.

- **`should be able to import transactions`**: For this test to pass, your application must allow a csv file to be imported, containing the following [template](./assets/file.csv). With the imported file, you must allow all records and categories that were present in that file to be created in the database, and return all transactions that were imported.

## :memo: Licence

This project is licensed under the MIT License - see the [LICENSE](LICENSE.md) file for more information.

---

👨🏻‍💻 - Made by Fernando Corrêa da Silva.


