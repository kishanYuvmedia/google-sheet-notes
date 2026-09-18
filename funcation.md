/**
 * 1. Add two numbers
 * Example: =ADD_NUMBERS(A2,B2)
 */
function ADD_NUMBERS(a, b) {
  return Number(a) + Number(b);
}


/**
 * 2. Subtract two numbers
 * Example: =SUBTRACT(A2,B2)
 */
function SUBTRACT(a, b) {
  return Number(a) - Number(b);
}


/**
 * 3. Multiply two numbers
 * Example: =MULTIPLY(A2,B2)
 */
function MULTIPLY(a, b) {
  return Number(a) * Number(b);
}


/**
 * 4. Divide two numbers
 * Example: =DIVIDE(A2,B2)
 */
function DIVIDE(a, b) {
  if (Number(b) === 0) {
    return "Cannot divide by zero";
  }

  return Number(a) / Number(b);
}


/**
 * 5. Combine first and last name
 * Example: =FULL_NAME(A2,B2)
 */
function FULL_NAME(firstName, lastName) {
  return String(firstName).trim() + " " + String(lastName).trim();
}


/**
 * 6. Convert text to uppercase
 * Example: =UPPER_TEXT(A2)
 */
function UPPER_TEXT(text) {
  return String(text).toUpperCase();
}


/**
 * 7. Convert text to lowercase
 * Example: =LOWER_TEXT(A2)
 */
function LOWER_TEXT(text) {
  return String(text).toLowerCase();
}


/**
 * 8. Count words in a cell
 * Example: =COUNT_WORDS(A2)
 */
function COUNT_WORDS(text) {
  if (!text || String(text).trim() === "") {
    return 0;
  }

  return String(text).trim().split(/\s+/).length;
}


/**
 * 9. Calculate price after discount
 * Example: =DISCOUNT_PRICE(A2,B2)
 *
 * A2 = Original Price
 * B2 = Discount %
 */
function DISCOUNT_PRICE(price, discountPercent) {
  price = Number(price);
  discountPercent = Number(discountPercent);

  return price - (price * discountPercent / 100);
}


/**
 * 10. Calculate GST amount
 * Example: =GST_AMOUNT(A2,B2)
 *
 * A2 = Price
 * B2 = GST %
 */
function GST_AMOUNT(price, gstPercent) {
  price = Number(price);
  gstPercent = Number(gstPercent);

  return price * gstPercent / 100;
}
