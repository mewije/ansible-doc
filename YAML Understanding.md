## WHAT IS YAML ?

YAML is one of the most popular languages for writing configuration files.In this article, you will learn how YAML compares to XML and JSON - two languages also used for creating configuration files.

You will also learn some of the rules and features of the language, along with its basic syntax.

YAML stands for YAML Ain't Markup Language, but it originally stood for Yet Another Markup Language.

YAML is a human-readable data serialization language, just like XML and JSON.
Serialization is a process where one application or service that has different data structures and is written in a different set of technologies can transfer data to another application using a standard format.

In other words, serialization is about translating, converting, and wrapping up a data structure in another format.

The data in the new format can be stored in a file or transmitted to another application or service over a network.

YAML is a widely used format for writing configuration files for different DevOps tools, programs, and applications because of its human-readable and intuitive syntax.

## XML VS JSON VS YAML - What's The Difference?
XML, JSON, and YAML are all used for creating configuration files and transferring data between applications.

### XML, which stands for Extensible Markup Language
```
<Employees>
    <Employee>
        <name> John Doe </name>
        <department> Engineering </department>
        <country> INDIA </country>
    </Employee>
     <Employee>
        <name> FOO </name>
        <department> IT Support </department>
        <country> USA </country>
    </Employee>
</Employees>
```
### JSON Stands for JavaScript Object Notation
```
{
	"Employees": [
    {
			"name": "John Doe",
			"department": "Engineering",
			"country": "INDIA"
		},

		{
			"name": "FOO",
			"department": "IT support",
			"country": "USA"
		}
	]
}
```

### YAML Yet Another Markup Language, now stands for YAML Ain't Markup Language.
```
Employees:
- name: John Doe
  department: Engineering
  country: INDIA
- name: FOO
  department: IT support
  country: USA
```
# How to Create a YAML File
To create a YAML file, use either the .yaml or .yml file extension.

Before writing any YAML code, you can add three dashes ```(---)``` at the start of the file. You can also use three dots ```(...)``` to mark the end of the document

# Indentation in YAML
In YAML, there is an emphasis on indentation and line separation to denote levels and structure in data. The indentation system is quite similar to the one Python uses.

YAML *doesn't* use symbols such as curly braces, square brackets, or opening or closing tags - just indentation.

# Tabs in YAML
YAML *doesn't* allow you to use any tabs when creating indentation - use spaces instead.

# Write A Comment in YAML
To add a comment to comment out a line of code, use the # character:
```
---
#Employees in my Company
Employees:
- name: John Doe
  department: Engineering
  country: INDIA
- name: FOO
  department: IT support
  country: USA
```

> [!NOTE]
> Please Convert JSON to YAML online https://www.json2yaml.com/

# Key-value Pair
```
---
Fruit: Apple
Vegetable: Carrot
Liquid: water
Meat: chicken
```
# Array/Lists
List is an ordered Collections
```
---
- vegetables
- rice
- meat
- beverages
```
# Dictionary
Dictionary is an unordered Collections
```
---
vegetables:
rice:
biriyani:
meat:
beverages:
```

# Dictionary in Dictionary

```
---
color: Blue
Model:
	Name: Ford
  year: 1995
```
# Lists of Dictionary [starts with -]
```
---
- color: Blue
	Model:
		Name: Ford
  	year: 1995
  Transition: Manual
  Prize: $20,000

- color: grey
	Model:
		Name: benz
  	year: 1995
  Transition: Manual
  Prize: $30,000
```

# Array of Lists
```
---
Fruit:
- Apple
-	Orange
Vegetable:
- Carrot
Liquid: water
Meat:
- chicken
- Mutton
```
