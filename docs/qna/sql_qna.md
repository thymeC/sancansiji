- What's the difference between char, varchar and nvarchar in SQL Server?
https://stackoverflow.com/questions/176514/what-is-the-difference-between-char-nchar-varchar-and-nvarchar-in-sql-server  

Just to clear up... or sum up...  

1 char and nchar are fixed-length which will reserve storage space for number of characters you specify even if you don't use up all that space.

2 varchar and nvarchar are variable-length which will only use up spaces for the characters you store. It will not reserve storage like char or nchar.

3 nchar and nvarchar can store Unicode characters.

4 char and varchar cannot store Unicode characters.

nchar and nvarchar will take up twice as much storage space, so it may be wise to use them only if you need Unicode support.