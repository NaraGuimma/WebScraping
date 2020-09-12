# WebScraping
Using python notebook to create an HTML scraper


# Python 

Basic Types
Anything inside the double or single quotes is a string

"Hello"
'Le Wagon'
type("Hello") # => <class 'str'>
⚠️ Any “text” value that you use in a program needs to come in quotes. Without quotes, Python will look for a variable or a method of that name, and will give you an error if it does not find it.

"hello" # OK
hello # NameError: name 'hello' is not defined 
Numbers can be either integers (whole numbers) or floats (numbers with a fractional part)

type(5) # => <class 'int'>
type(3.5) # => <class 'float'> 
True and False are special values of type bool that are used for logical operations.

Variables: Assignment
age = 21
name = "hello"
Variables are just a way programmers name values. They hold a reference to the underlying object.
We can use literal values and variables interchangeably:

type(21) # => <class 'int'>
age = 21 
type(age) # => <class 'int'> 
Variables: Re-assignment
age = 21
age = age + 1 # Now age holds the value 22

greeting = "Hello"
greeting = "Hola!" # the new value of `greeting` variable is "Hola!", "Hello" is lost
Objects and methods
Besides using basic types that come with the language, programmers can design their own types (classes), and create objects out of them.
Usually, objects have built-in methods that can be called with a dot notation:

greeting = "hello"
greeting.capitalize() # => "Hello"

# BeautifulSoup has its own types:

scraped = BeautifulSoup(html, "html.parser")
type(scraped) # => bs4.BeautifulSoup 

link = scraped.article.h3.a
type(link) # => bs4.element.Tag
.h1, .find(), find_all(), select() — are all methods that can be called on various BeautifulSoup objects

Lists, loops, Dictionaries
Python has a built-in List type for storing a collection of values.

numbers = [1, 5, 10, 15] # a list of ints
names = ["Ann", "Bob", "Charlie"] # a list of strings
In real-world programs the elements of the list are rarely known beforehand, so it is very common to start with an empty list and then populate it with elements. Lists have an append() method that takes any value as an argument.

numbers = []
numbers.append(1)
numbers.append(2)
# Now the list is [1, 2]
To do something with each element of the list, we need to iterate over it. Python has a special for... in... construct for that:

names = ["Ann", "Bob", "Charlie"] 
#    1.       2.
for name in names:
    print(name) # we can only use a `name` variable here, in a nested block of code.
    
 # 1. We can refer to each individual element on every iteration by that name
 # 2. The list we are iterating upon
This will print all the names in the list, individually.

It does not matter how we name first variable in the for... in..., but the second one has to be the one that holds a list.

for pizza in names:
    print(pizza)
    
# Same result!
Another common built-in data structure is called a Dictionary.

book = {"War and Peace": "A famous novel by Leo Tolstoi"}
Dictionaries are used to store “key-value” information. Here, “War and Peace” is the key, and the description is the value. So it works like a real… dictionary! 📖

You can have Lists of Dictionaries. Here’s how we can build one

food_prices = []

# Note the curly braces: 
food_prices.append({"Apple": 1})
food_prices.append({"Coco": 5})
Now the food_prices list will look like this: [{"Apple: 1"}, {"Coco": 5}]


________________________________________________________________________________________________________________________________________________________________________________

# BeautifulSoup

To get better at scraping, take time to consult the library’s official documentation.

Setting up
We need at least two Python libraries to write a scraper:

requests — to make HTTP requests from code
bs4 — BeautifulSoup, a library to parse HTML documents and navigate the element tree
On top of the file:

import requests
from bs4 import BeautifulSoup
These are the basic steps to pull page’s HTML code and parse it for further scraping:

url = "http://books.toscrape.com/index.html"
response = requests.get(url)
html = response.content
scraped = BeautifulSoup(html, 'html.parser')
Finding a single element
Use the tag name, in this case <title> is a tag we’re looking for:

scraped.find("title") or scraped.title

Getting text out of an element:

title_text = scraped.title.text.strip()
If the text is just numbers, you can convert it to int or float:

  price = scraped.article.find("p", class_="price_color").text() # => "£51.77"
  # convert to float 
  price =  float(price.text.lstrip("£"))
Sometimes the data we want is not inside the element’s text, but inside one of it’s attributes:

<a href="http://mywebsite.com/about.html">About</a<>
If we save this element into a link variable and call link.text — we will get “About”. If we wan’t the URL of the link instead — we will have to access it like that: a["href"].

As a rule of thumb:

tag_name.text — gives us the element’s text value
tag_name["attr_name"] — gives the value of the attribute
Collection of elements
Methods find_all and select will always return a list of elements (even if one element is found, it will still be a collection with a single element inside). Here’s how to iterate over a list in Python:

prices = scraped.select(".price_color")

for price in prices:
    print(price.text)
    




________________________________________________________________________________________________________________________________________________________________________________


# Complete scraper example

Here’s a code for a complete scraper that starts from the home page, scrapes all links to book titles, then navigates to the destination page of every link, and scrapes the description of a book from it:

import requests
from bs4 import BeautifulSoup

BASE_URL = "http://books.toscrape.com/"
response = requests.get(BASE_URL + "index.html")
html = response.content
scraped = BeautifulSoup(html, 'html.parser')

title_descriptions = []

articles = scraped.select(".product_pod")

for article in articles:
    title = article.h3.a["title"]
    title_url = article.h3.a["href"]
    
    product_response = requests.get(BASE_URL + title_url)
    product_html = product_response.content
    product_scraped = BeautifulSoup(product_html, 'html.parser')
    
    description = product_scraped.find("div", id="product_description").next_sibling.next_sibling
    
    title_descriptions.append({title: description.text.strip()})
    
print(title_descriptions)





    
------------------    
