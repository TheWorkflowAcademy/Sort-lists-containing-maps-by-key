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

### Select the Key for Sort, Add a Suffix to the Key, then Add to a New List
* *listvar* is your list variable. To ensure that it is read as a list, use a `toList()` function. 
* Create a new list (*newList*).
* Then, set the variable *n* as "000" (this will acccount for a list of up to 1000 elements).
* `Loop` over the list, `get()` the key that you want to sort by and add *n* as the suffix.
	* To sort by **Products** (we'll use this for this demo)
		* sortkey = l.get("Products") + n;
	* To sort by **Date of Sale**
		* sortkey = l.get("Date_of_Sale").replaceAll("-","") + n;
	* To sort by **Quantity Sold**
		* sortkey = l.get("Quantity_Sold") + n;
* Convert *n* to a number, add 1, then convert it back to a string.
* If the length of *n* is one digit, we wanna add only two zeros at the front, if it's two, we should only add one zero. If it's three, zero does not need to be added.
* At the end of the loop, add *sortkey* to the *newList* variable (this list will contain only the selected key's values with the added suffix).

```javascript
originalList = listvar.toList();
n = "000";
newList = List();
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
	newList.add(sortkey);
}
```

### Sort the New List and Get the Indexes
* Use the `sort()` function to sort the values *newList* by ascending (true) or descending (false) order. 
* Create another list (this will be used to store the end result of the sorted list).
* `Loop` over variable `newList` and extract the index of each element by getting the numbers without the zero(s).
* At this point, you have the sorted list, and the respective index that acts as an "id" for each of the line items.
* All you gotta now is to create a new map by using `get(index)`function on the original list, then add the map to the new *sortedList*.
* By the end of the loop iteration, *sortedList* will contain the sorted output of the list.

```javascript
newList.sort(true);
sortedlist = List();
for each  n in newList
{
	if(n.right(3).left(2) = "00")
	{
		index = n.right(1);
	}
	else if(n.right(3).left(1) = "0" && n.right(3).left(2).right(1) != "0")
	{
		index = n.right(2);
	}
	else
	{
		index = n.right(3);
	}
	map = originalList.get(index);
	sortedlist.add(map);
}
info sortedlist;
```
