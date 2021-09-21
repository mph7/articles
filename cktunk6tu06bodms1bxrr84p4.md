## How to consume an API in Python

  
   Nowadays, there is a huge number of connecting, sending and receiving data services, with all sort of purposes. API's, the Application Programming Interfaces, are one the most common ways for those services to communicate, and today we will learn how to consume an API in Python.

[Portuguese Version/Versão em português](https://mph7.hashnode.dev/como-consumir-uma-api-em-python)

## How Will We Do That? 
    
To give your first experience in API's, we will use a service called [Zippopotamus.](https://www.zippopotam.us), It's an application that receives a zip code and return addrees data.
## Hands On Desk 
For starter, we need the 'request' lib. Then, we import it and build the basic struture for our code. It is like this: 

```python
import requests

def main():
    
    
if __name__ == '__main__':
    main()
```

​	All our code will be written inside the `main()` function.

​	In the context of our API, we need a postal code to search, and it will be written by the user:

```python
zip_code = input('Zip code searcher')
```

​	A valid zip code has five digits, something different won't work, so we can check if the zip code is valid to continue. If not, we request a new zip code until it is valid: 

```python
while 1:
	cep = input('Type the Zip Code for search: ')
    
    if len(cep) != 5:
        break
    print('Invalid digit quantity.')
```

​	Now that we have a valid zip code, we can start to communicate with our API in the [official website](https://www.zippopotam.us) 
Here is the URL format we will be using:
> http://api.zippopotam.us/us/90210

'90210' is the zip code being consulted, so we can change it to any valid zip code and do the request:

```python
request = requests.get("http://api.zippopotam.us/us/{}".format(zip_code))
```

​	The .get() function makes it possible to extract the file from the URL with the info, and the curly brackets mark the place that will be receiving the zip code with the format() function. 

​ Because our file is a json, we can tell Python that using the .json() function: 

```python
address_data = request.json()
```

​	A JSON file can be a dictionary in Python. This makes it easier to work with the values because we can inform the key as an index, so let's see the JSON file from the API with the postal code 90210: 

```json
{"post code": "90210",
 "country":  "United States",
 "country abbreviation": "US",
 "places": 
 [
     {"place name": "Beverly Hills", 
      "longitude": "-118.4065", 
      "state": "California", 
      "state abbreviation": "CA", 
      "latitude": "34.0901"}
 ]
}
```

​	In this case, we want to show to users data about the Zip Code, Country, State, State Abbreviation, and Place. Those are the keys of our dictionary, but we can only show it if the Zip Code is valid. If not, we will be receiving an empty dictionary: {}

​	So we will only display it if the dictionary is not empty. And to show the information in a better way: 

```python
if address_data != {}:
	print("### Zip Code Found ###")
    print("Zip Code: {}".format(address_data["post code"]))
    print("Country: {}".format(address_data["country"]))
  
```

The next data are inside the key "places" in the main dictionary, and inside a list, then we have the dictionary that we will be working on: 

```python
	print("State: {}".format(address_data["places"][0]["state"]))
    print(
    "State Abbreviation: {}".format(
                address_data["places"][0]["state abbreviation"]))
    print("City: {}".format(address_data["places"][0]["place name"]))
else:
    print("{}: Invalid Zip Code".format(zip_code))
```

​	And the "else" keyword if the dictionary is empty, telling us that the zip code is invalid. Now we just include an option to search again or quit, so the user can search for another zip code without the need to close and open again the program: 

```python
option = int(input("Do you want to make a new search?? \n1. Yes\n2. Exit\n"))
    if option == 1:
        main()
    else:
        print("Quitting...")
```

​	Calling the main() function, the process will start again, and you can modify the program in any way you want, like start the program with a header before inserting the zip code: 

```python
print("#" * 30)
print("#" * 7, " Zip Code Searcher ", "#" * 7)
print("#" * 30, "\n")
```

​	The program working:

![program working](https://cdn.hashnode.com/res/hashnode/image/upload/v1631827013963/8zl8Xad5o.png)


​	In the case of a zip code with less than five digits:


![less than five digits](https://cdn.hashnode.com/res/hashnode/image/upload/v1631827093849/6CPVXSIrS.png)

​	And in the case of an invalid zip code:


![invalid zipcode](https://cdn.hashnode.com/res/hashnode/image/upload/v1631827143414/fc7upSMwm.png)

------

​	And just like that we have a functional program that searches zip codes using an API. If you read until the end, I hope you learned a lot, and feel free to ask questions on the comments section bellow. You can also see the full code [here](https://github.com/mph7/CEPChecker), Thanks for reading it and follow me for more articles in the future!