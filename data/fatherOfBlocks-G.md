**JBTiered721Delegate**
- L216/218 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L240 - It is less expensive to validate that uint != 0 than to validate uint > 0


**abstract/JB721Delegate**
- L116 - It is less expensive to validate that uint != 0 than to validate uint > 0


**libraries/JBIpfsDecoder**
- L47/49/51/68/76/84 - It is not necessary to set a variable to its default value when it is created, in this case all digit positions are 0, since it is uint8, so set it again at 0 is unnecessary. The same goes for other uint variables.

- L45/49/75/76/77/83/84 - When the length of an array is used more than once, it is less expensive to create a variable in length memory and use that variable.

- L57 - It is less expensive to validate that uint != 0 than to validate uint > 0

- L59/68/76/84 - It is less expensive to do ++i or --i, rather than i++, i-- or i - 1.


**libraries/JBTiered721DelegateStore**
- L1254 - It is less expensive to validate that uint != 0 than to validate uint > 0

