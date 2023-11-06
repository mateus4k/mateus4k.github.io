+++
title = "Currency to number transformation in JavaScript"
date = "2023-11-06"
author = "Mateus Sampaio"
description = "Dealing with monetary values of different currencies is undoubtedly a big challenge, especially when you don't know the type of currency you will need to deal with. This is why currency-to-float was created, a tiny JavaScript library."
+++

# Currency to number transformation

Dealing with monetary values of different currencies is undoubtedly a big challenge, especially when you don't know the type of currency you will need to deal with. This is why [**currency-to-float**](https://www.npmjs.com/package/currency-to-float) was created, a tiny JavaScript library.

With this, you can easily transform currency strings into numeric values regardless of the currency format. It is a versatile tool that simplifies your financial and web development projects.

## Example

Here's how straightforward it is to use `currency-to-float`:

```ts
import currencyToFloat from 'currency-to-float';

// Sample array of expenses in USD
const expenses = ['$12.50 USD', '$8.75 USD', '$20 USD', '$15.25 USD'];

// Sum the expenses
const totalExpense = expenses.reduce(
  (acc, expense) => acc + currencyToFloat(expense),
  0
);

console.log(totalExpense); // Output: 56.5
```

In this example, we have an array of expenses in US dollars. By leveraging `currencyToFloat` function, we convert each expense string to a numeric value and calculate the total using the `reduce` function.

## Supported Currencies

| Currency                    |    Locale    |    Example    |
| --------------------------- | :----------: | :-----------: |
| US Dollar ($, USD)          |    en-US     |  $12.50 USD   |
| Brazilian Real (R$, BRL)    |    pt-BR     |   R$ 12,50    |
| Canadian Dollar ($, CAD)    |    en-CA     |  $12.50 CAD   |
| Canadian Dollar ($, CAD)    |    fr-CA     |  12,50 $ CAD  |
| Australian Dollar ($, AUD)  |    en-AU     |  $12.50 AUD   |
| Euro (€, EUR)               | de-DE, fr-FR |  12,50 € EUR  |
| Euro (€, EUR)               |    en-IE     |  €12.50 EUR   |
| Euro (€, EUR)               |    nl-NL     |  €12,50 EUR   |
| British Pounds (£, GBP)     |    en-GB     |  £12.50 GBP   |
| Japanese Yen (¥, JPY)       |    ja-JP     |   ¥1250 JPY   |
| New Zealand Dollar ($, NZD) |    en-NZ     |  $12.50 NZD   |
| Hong Kong Dollar ($, HKD)   |    zh-HK     |  $12.50 HKD   |
| Singapore Dollar ($, SGD)   |    zh-SG     |  $12.50 SGD   |
| Danish Krone (Kr, DKK)      |    da-DK     | 12,50 kr. DKK |

## Installation

```sh
npm install currency-to-float
```

## Useful links

GitHub: https://github.com/mateus4k/currency-to-float
