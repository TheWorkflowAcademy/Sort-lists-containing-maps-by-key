# Sort-lists-containing-maps-by-key
Deluge script for sorting lists containing maps by the specific key (by date-time/alphabetical order) in ascending/descending order.

## Core Idea
A list of maps is essentially a table. It has the headers (keys) and its respective values. How can you use Deluge to sort your dataset by any of the keys like sorting columns in Excel? This can be done with some string manipulation, additiona and by reassigning indexes.

## Example Dataset
Lets use the table below as an example. 

| Product  | Date of Sale | Quantity Sold |
| ------------- | ------------- | ------------- |
| Orange | 2020-04-08  | 20 |
| Apple | 2020-02-28  | 15 |
| Kiwi | 2020-05-15  | 50 |
| Peach | 2020-11-22  | 35 |
| Strawberry | 2020-12-24  | 10 |
| Watermelon | 2021-01-11  | 25 |

Here is the JSON string for the table.
```javascript
{"Products":"Orange","Date_of_Sale":"2020-04-18","Quantity_Sold":20},{"Products":"Apple","Date_of_Sale":"2020-02-28","Quantity_Sold":15},{"Products":"Kiwi","Date_of_Sale":"2020-05-15”,”Quantity_Sold":50},{"Products":"Peach","Date_of_Sale":"2020-11-22","Quantity_Sold":35},{"Products":"Strawberry","Date_of_Sale":"2020-12-24”,”Quantity_Sold":10},{"Products":"Watermelon","Date_of_Sale":"2021-01-11","Quantity_Sold”:25}
```

### Sort by Product
* *listvar* is your list variable. To ensure that it is read as a list, use a `toList()` function. 
* Then, set the variable *n* as "000" (this will acccount for a list of up to 1000 elements).
* `Loop` over the list, `get()` the key that you want to sort by and add *n* as the suffix.
* Convert *n* to a number, add 1, then convert it back to a string.
* If the length of *n* is one digit, we wanna add only two zeros at the front, if it's two, we should only add one zero. If it's three, zero does not need to be added.
* At the end of the loop, variable *sortkey* to the a new list (this list will only contain the keys that you want to sort your data by).

```javascript
originalList = listvar.toList();
n = "000";
newdatelist = List();
for each  l in originalList
{
	sortkey = l.get("Products") + n;
	n = n.toLong() + 1;
	n = n.toString();
	if(n.length() = 1)
	{
		n = "00" + n;
	}
	else if(n.length() = 2)
	{
		n = "0" + n;
	}
	newdatelist.add(sortkey);
}
```
