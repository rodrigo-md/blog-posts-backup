## Comments are useful. You just need to learn when to use them

Perhaps you may have heard some of the following statements:
- "Code needs to be self-explanatory"
- "Comments are useless"
- "Don't write comments in your code"
- "Comments are a code-smell" 
- "Comments are an anti-pattern"

The thing is that writing comments aren't bad at all if you know in which cases they can help us and on which others they are a complete waste of time.


## Code needs to be self-explanatory

Code should be expressive and intention-revealing. Needing comments to explain the code means the code doesn't describe itself well enough.

Our code is read far more times than is written, so we should aim to make it as easier to read as we can.

Sometimes it's clear when a comment is redundant.

```javascript
// Calculate the square root of the sum of two numbers
const result = Math.sqrt(a + b);

// Timeout of 30 seconds
const timeout= 30000;
```
But other times, knowing if the use of comments is a good or bad idea, is confusing.

Consider the following example:

* Can you know in less than ten seconds, what the code does without reading the comments?

```javascript
/*
* Calculate the total amount to pay for a pizza going through each
* item in the order and obtain the cost of each ingredient of the
* price object searching by section and ingredient name to then add 
* each subtotal and return the total to pay
*
* @param {object} o: pizza order
* @param {object} p: ingredients prices
* @return {number} total: total price to pay for the order
*/
function total(o, p) {
    let total = 0;

    Object.keys(o).forEach((key) => {
        if (key !== 'ingredients') {
            total += p[key][o[key]];
        } else {
            o.ingredients.forEach((value) => {
                total += p.ingredients[value];
            });
        }
    });

    return total;
}
```
In the above code, we used JSDocs, which is a fancy way to create documentation automatically by reading the comments and special tags like ** `@ param` ** or **` @ return` ** that specify the parameters and the value returned by each function. Although JSDocs has justified use, when used for the sole purpose of describing what the code does, it becomes useless.

We can refactor the previous example using descriptive and meaningful variable names, moving some logic to a smaller function so that its name explains what the function does. After that, we will soon realize that all such comments are unnecessary.


```javascript

function calculatePriceToPay(pizzaOrder, tableOfPrices) {
    let totalPrice = 0;

    Object
        .entries(pizzaOrder)
        .forEach(([section, item]) => {
            totalPrice += getItemPrice(section, item, tableOfPrices);
     });

    return totalPrice;
}


function getItemPrice(section, item, tableOfPrices) {
    let price = 0;
    if (Array.isArray(item)) {
        price = item.reduce((accumulator, subItem) => {
             return accumulator +=  tableOfPrices[section][subItem];
        }, 0);
    } else {
        price = tableOfPrices[section][item];
    }
    return price;
}
```
The only reason to hesitate before removing those comments is if the JSDocs is actually been used to create documentation, if not, or if they were another type of comments we could remove them immediately.

See?. The `calculatePriceToPay` function is much simple to read after the refactoring even without the comments.

> When we write code with intention revealing and easy to understand, every previous comment whose sole purpose was to explain the code, lose completely its value, and therefore we can remove it

## Avoid using comments to explain "What"

Whenever you find comments used to explain what the code is doing I invite you to question if they are really necessary because in the majority of cases those comments aren't needed, and can be deleted.

We should understand what the code is doing after reading it line by line and not by reading a description.

Another reason is that code is alive, it changes more frequently than comments and even documentation, so it's more likely to see comments become obsolete sooner than you think.

As in everything, there are exceptions. There is one case in which we can use comments to describe the code; If we are building a really complex algorithm or performing complex operations, in non-trivial domains like statistics, physics, chemistry, healthcare. In those situations, we can give additional context and reinforce the self-described code through the use of comments.

Let's see an example:

```Python
 import math

EARTH_RADIUS_KM = 6371

def get_distance_km(src_lat, src_lng, dest_lat, dest_lng):
"""
Calculate the linear distance between two points in the earth using
the Haversine Formula

The points are GPS coordinates with a latitude and longitude 
represented as floats numbers
"""   
    
    DEGREES_TO_RADIANS = math.pi / 180.0

    # Convert latitude and longitude to
    # spherical coordinates in radians
    phi_src = (90.0 - src_lat) * DEGREES_TO_RADIANS
    phi_dest = (90.0 - dest_lat) * DEGREES_TO_RADIANS

    theta_src = src_lng * DEGREES_TO_RADIANS
    theta_dest = dest_lng * DEGREES_TO_RADIANS

    # Compute spherical distance from spherical coordinates.
    cos = (math.sin(phi_src) * math.sin(phi_dest) * math.cos(theta_src - theta_dest) +
           math.cos(phi_src) * math.cos(phi_dest))
    arc = math.acos(cos)

 
    distance_in_km = arc * EARTH_RADIUS_KM

    return distance

```

As you can see above, even with a clean code, it's difficult, near to impossible to understand what the code does. Why is that?, It's because it requires domain-specific knowledge. The variable names represent mathematical symbols and formulas but none of them give us context to understand what those formulas are needed, the use of comments, in this case, guides us through the steps and delivers us additional context. 

With the use of those comments, we can quickly search on google for "Haversine function" and "spherical coordinates" to get the missing information.

## Write comments to explain "Why"

Comments are a great tool to explain things that cannot be explained through the code.

They are good for express the reasons behind our decisions and provide additional context.

- Why we write the code in this way?
- Why we took that decision?
- Why we are using this library?

### Cases where is a good idea to use comments:
- Add links to issues that are affecting our code
```JavaScript
// Needed to handle Webpack and faux modules
// See https://github.com/fastify/fastify/issues/2356
// and https://github.com/fastify/fastify/discussions/2907.
```
- Explain exceptions to our code style
```JavaScript
// Typescript quirk - libraries exported with `export = ` need to be imported using `require()`.
// See: https://github.com/microsoft/TypeScript/blob/master/doc/spec.md#1135-export-assignments
import chaiShallowDeepEqual = require('chai-shallow-deep-equal');
```
- Redirect to an external source of information
```javascript
// Override timezone formatting for MSSQL
// link: https://stackoverflow.com/questions/47056395/how-to-pass-a-datetime-from-nodejs-sequelize-to-mssql
```
- Justify our choices when writing the code
```JavaScript
// Some errors contain not only line numbers in stack traces
// e.g. SyntaxErrors can contain snippets of code, and we don't
// want to trim those, because they may have pointers to the column/character
// which will get misaligned.
const trimPaths = (string: string) =>
  string.match(STACK_PATH_REGEXP) ? trim(string) : string;
```
- Build automatic documentation
```JavaScript
/**
 * Initialize a new `View` with the given `name`.
 *
 * Options:
 *
 *   - `defaultEngine` the default template engine name
 *   - `engines` template engine require() cache
 *   - `root` root path for view lookup
 *
 * @param {string} name
 * @param {object} options
 * @public
 */
```
- Prevent unexpected errors by providing warning messages
```JavaScript
// This file is autogenerated by build/build-validation.js, do not edit
```
- Explain business decisions
```JavaScript
/**
 * This module contains code that will be migrated into 
 * its own service
 * DO NOT ADD MORE CODE INTO THIS MODULE
 * see epic: https://jira.example.com/browse/PM-32145
**/
```

## What comments should we avoid?

Comments can lie, that is a fact that can create confusion because, unlike code that keeps evolving, comments are rarely updated because it's not our priority as developers

In addition to redundant comments, we should avoid those that are implicitly or explicitly time-limited.

For example, the famous TODO or FIXME comments are used to let everyone know that there is a debt in the code that we must pay in the future, but the future could never come, and no one will review the code when the team needs to prioritize work. Instead of those comments, create a task in your task manager and label it "Technical Debt" to track how much debt the project generates and how much is paid in each iteration.


## Conclusion

Comments are just another tool to solve a specific problem. The problem here is the lack of understanding after reading our code.

They aren't the bad of the movie, but they can become a waste of text so it's important to know when they are beneficial and when using them is nonsense.

As a general rule, write comments when you need to provide additional information that cannot be expressed through clean code.




If you like the article and want to support me to write more content like this