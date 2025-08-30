# SUBSTRING

The SUBSTRING function in SQL (and also in many programming languages) is used to extract part of a string.
e.g Select title SUBSTRING(col,start,len) from books

# CONCAT

select author_fname, author_lname, CONCAT(author_fname,' ' , author_lname) from books;

# REPLACE

The REPLACE() function in MySQL is used to find and replace text inside a string.
select Replace(author_fname,'David', 'imran'), author_lname from books;

# Combining String Functions

select Concat(substring(author_fname, 1, 10), "...") from books;

# Char_length

select author_fname, Char_length(author_fname) from books;

# reverse

select author_fname, reverse(author_fname) from books;
